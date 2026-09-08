# Lecture Script: Packaging and Deploying FastAPI
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Packaging a FastAPI Application | 22 min |
| 3 | Environment Management | 22 min |
| 4 | Secrets Handling | 22 min |
| 5 | FastAPI Deployment | 20 min |
| 6 | Stable Production Configs | 11 min |
| 7 | Lecture Summary and Recap | 5 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has built a full-featured FastAPI application — database integration, authentication, AI API calls — that currently only runs with `uvicorn main:app --reload` on their own machine. This session addresses the gap between "runs on my machine" and "runs reliably for real users, on a real server, without leaking secrets." Open with that gap concretely. Wait after the opening question.

**[Script:]**

"Everything you have built runs perfectly when you type `uvicorn main:app --reload` on your own laptop. That command will not work for anyone else, and it should not — `--reload` exists specifically for development, watching your files and restarting on every change, which is exactly the wrong behavior for a server actually serving real traffic. Your application currently depends on packages installed in your own Python environment, a `.env` file sitting on your own hard drive with your own API keys, and a database file that only exists on your machine. None of that travels with your code.

Getting an application from 'works on my machine' to 'runs reliably in production' requires several distinct pieces, each solving a different problem. Packaging: bundling your application and its exact dependencies into something that can run somewhere else, identically. Environment management: making sure your application knows whether it is running in development, staging, or production, and behaves appropriately for each. Secrets handling: getting your API keys, database credentials, and JWT signing keys into production safely, without ever putting them in your code or your version control history. And stable production configuration: running your application with a setup that survives real traffic, restarts cleanly after a crash, and does not silently misbehave under load the way a development server would.

Get any one of these wrong, and the consequences are real: a leaked API key racking up unexpected charges, a crash that takes your entire application down with no automatic recovery, a `.env` file accidentally committed to a public GitHub repository. Today covers all four of these pieces, building directly on the FastAPI, authentication, and API integration work from earlier sessions, so that the application you have already built can actually go live safely."

---

## Block 2 — Packaging a FastAPI Application

### 2A — Why "It Works Locally" Is Not Enough

**[Script:]**

"Your application currently depends on a specific set of installed Python packages, at specific versions, that happen to be present in your local environment. If you handed your `main.py` file to someone else right now, with none of that surrounding context, it would fail immediately — missing packages, or worse, subtly different behavior from a different installed version of a package you depend on.

Packaging solves this by capturing exactly what your application needs to run, in a form that can be recreated identically anywhere else — another developer's machine, a staging server, a production server."

---

### 2B — Declaring Dependencies Precisely

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If your `requirements.txt` just lists `fastapi` with no version number, and six months from now a new, incompatible version of FastAPI is released, what happens when someone sets up your project fresh?" Answer: they get the newest available version at that time, which might behave differently than the version you actually developed and tested against — potentially breaking the application in ways that have nothing to do with any change you made yourself.

**Demo 1 — A precise requirements file (whiteboard-friendly)**

```
# requirements.txt
fastapi==0.115.0
uvicorn==0.32.0
sqlalchemy==2.0.36
pydantic==2.9.2
python-jose==3.3.0
bcrypt==4.2.0
python-dotenv==1.0.1
```

**[Script:]**

"Each line pins an exact version with `==`, rather than leaving it open-ended. This guarantees that anyone — including a production server — installing from this file gets the exact same versions you developed and tested against, eliminating an entire category of 'it works on my machine but not in production' problems caused by version drift.

`pip freeze > requirements.txt` generates this file automatically from your currently installed packages, capturing exact versions of everything installed in your environment at that moment — this is the standard way to produce it, rather than writing version numbers by hand."

> 🎯 **Instructor Note:** Ask: "Why might pinning exact versions also occasionally cause a problem, rather than only preventing problems?" Answer: pinned versions mean you do not automatically receive security patches or bug fixes released for a dependency after you pinned it — version pinning trades automatic updates for reproducibility, which is generally the right trade for production stability, but does mean dependencies need to be deliberately, periodically reviewed and updated rather than being left frozen forever.

---

### 2C — Containerizing with Docker

**[Script:]**

"A `requirements.txt` file solves dependency versions, but it does not solve everything — it assumes the target machine already has the correct Python version installed, the correct operating system libraries, and so on. Docker solves this more completely: it packages your application, its dependencies, the correct Python version, and enough of an operating system environment to run all of it, into a single, portable unit called a container image, which behaves identically wherever it runs."

> 🎯 **Instructor Note:** Draw this layered picture on the board.

```
Docker image contains, bundled together:
  - A base operating system layer
  - The specific Python version your app needs
  - Every dependency from requirements.txt, at pinned versions
  - Your actual application code

Result: the exact same image runs identically on your laptop, 
a teammate's laptop, and a production server — no "it depends 
on what's already installed" uncertainty.
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Based on what a Docker image needs to contain, what do you expect the first few lines of a Dockerfile for a FastAPI app to actually do, before any of your own application code is even mentioned?" Guide toward: specify a base Python image, then install dependencies from requirements.txt — establishing the foundation before your own code enters the picture at all.

**Demo 2 — A Dockerfile for a FastAPI application (whiteboard-friendly)**

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**[Script:]**

"`FROM python:3.12-slim` starts from a minimal, official Python base image — this fixes the Python version, eliminating any ambiguity about which version is actually running. `WORKDIR /app` sets the working directory inside the container. `COPY requirements.txt .` followed by `RUN pip install ...` installs dependencies before copying the rest of the application code — this ordering is deliberate: Docker caches each step, so if only your application code changes and dependencies stay the same, rebuilding skips the slow dependency installation step entirely, reusing the cached layer from before.

`COPY . .` copies the rest of your application code in. `CMD [...]` specifies what actually runs when a container starts from this image — notably `--host 0.0.0.0` instead of the default `127.0.0.1`, since the container needs to accept connections from outside itself, not just from within its own isolated environment. Notice `--reload` is deliberately absent here — this is a production command, not the local development command from Block 1."

> 🎯 **Instructor Note:** Ask: "Why does the order of COPY and RUN commands in this Dockerfile matter for how quickly you can rebuild the image during development?" Answer: Docker builds images in layers and caches each layer; if `requirements.txt` has not changed since the last build, Docker reuses the cached dependency-installation layer instead of reinstalling everything from scratch, only re-running the steps after the first actual change — which is why dependencies are copied and installed before the rest of the application code, which changes far more frequently.

**Recap of Block 2 before moving on:**

- Packaging captures exactly what an application needs to run, so it can be recreated identically elsewhere, rather than depending on whatever happens to already be installed locally
- Pinning exact dependency versions in `requirements.txt` prevents version drift between development and production, at the cost of needing deliberate, periodic dependency updates
- Docker bundles the operating system layer, Python version, dependencies, and application code into one portable image that runs identically anywhere
- Dockerfile instruction order matters for build speed — dependencies are installed before application code is copied in, so Docker's layer caching skips reinstalling dependencies when only application code changes

---

## Block 3 — Environment Management

### 3A — Why One Configuration Does Not Fit Every Stage

**[Script:]**

"Your application typically runs in at least three distinct contexts across its life: development, on your own machine, with fast iteration and verbose error messages; staging, a environment that mimics production closely, used for final testing before real release; and production, serving actual real users, where verbose error details and debug tooling should never be exposed. Each of these needs different configuration — a different database connection, different logging verbosity, different allowed origins for CORS — while running from the exact same application code."

> 🎯 **Instructor Note:** Write this three-environment distinction on the board.

```
Development    → local database, verbose errors, auto-reload, 
                  permissive CORS for local testing

Staging        → production-like database and settings, 
                  used to catch issues before real release

Production     → real database, minimal error detail exposed 
                  to users, strict CORS, no auto-reload
```

---

### 3B — Driving Configuration from Environment Variables

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If your application's database connection string were hardcoded directly in your Python code, what would you need to do differently to deploy the exact same code to development, staging, and production?" Answer: you would need to actually edit the source code itself for each environment, which is fragile, error-prone, and risks accidentally deploying the wrong environment's settings, or committing one environment's specific configuration into version control where it does not belong.

**Demo 3 — Environment-driven configuration (whiteboard-friendly)**

```python
import os

class Settings:
    environment: str = os.environ.get("ENVIRONMENT", "development")
    database_url: str = os.environ.get("DATABASE_URL", "sqlite:///./dev.db")
    debug: bool = environment == "development"

settings = Settings()

if settings.debug:
    print("Running in development mode with verbose logging")
```

**[Script:]**

"`Settings` reads its actual values from environment variables at startup, with sensible defaults for local development so nothing breaks if a variable happens to be unset while developing. `ENVIRONMENT` itself becomes an environment variable, and other settings — like `debug` — can be derived directly from it.

The critical shift here: your application code stays completely identical across development, staging, and production. What changes is only the environment variables present when it starts up — this is precisely what makes the same Docker image from Block 2 usable, unmodified, across all three environments, with each deployment simply supplying different environment variables at startup."

> 🎯 **Instructor Note:** Connect this directly to earlier sessions. Say: "This is the exact same pattern as loading your OpenAI API key from an environment variable in the AI API integration session — that was one specific instance of a much more general principle: configuration that varies by environment belongs in environment variables, never hardcoded in your application code."

---

### 3C — Using .env Files for Local Development

**[Script:]**

"Manually setting environment variables in your terminal every time you start your application locally is tedious. A `.env` file, combined with `python-dotenv`, lets you define these variables in a file that is automatically loaded at startup — purely for local development convenience. In staging and production, environment variables are typically set directly by the hosting platform itself, not by a `.env` file at all — we will return to exactly why in Block 4."

**Recap of Block 3 before moving on:**

- Applications typically run across at least three distinct environments — development, staging, and production — each needing different configuration from identical application code
- Environment variables, read at startup with sensible local defaults, are the standard way to drive this configuration without editing source code per environment
- The same Docker image can run identically across every environment, since only the environment variables supplied at startup actually change
- `.env` files are a local development convenience; staging and production typically receive environment variables directly from the hosting platform

---

## Block 4 — Secrets Handling

### 4A — What Counts as a Secret

**[Script:]**

"A secret is any piece of configuration that would cause real harm if exposed — API keys, database passwords, the JWT signing key from the authentication session, OAuth client secrets. You have already practiced never hardcoding these directly in source code, across the authentication and AI API integration sessions. This block generalizes that discipline specifically to the production deployment context, where the stakes and the number of places a mistake could happen both increase."

---

### 4B — Never Committing Secrets to Version Control

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If a `.env` file containing a real production database password is committed to a Git repository, and later deleted in a subsequent commit, is that password actually safe now?" Answer: no — this is the exact same point from the AI API integration session, worth reinforcing here at higher stakes. The password remains fully visible in the repository's commit history, recoverable by anyone with access to that history, even though it no longer appears in the current, latest version of the file.

**Demo 4 — Protecting secrets from version control (whiteboard-friendly)**

```
# .gitignore
.env
.env.local
.env.production
*.db
__pycache__/
```

**[Script:]**

"`.gitignore` tells Git to never track these specific files at all — they simply never enter version control in the first place, which is the only fully reliable protection. Once a secret has actually been committed, even once, the only genuinely safe remedy is to treat that secret as compromised: revoke it and generate a completely new one, rather than assuming removing it from a later commit is sufficient."

> 🎯 **Instructor Note:** Emphasize this as a hard rule: "Add `.env` to `.gitignore` before you ever create the `.env` file itself, as the very first step of setting up a new project — not as a fix you remember to apply after the fact."

---

### 4C — Secrets in Production: Beyond a .env File

**[Script:]**

"In production, secrets typically do not come from a `.env` file sitting on the server at all — most hosting platforms provide a dedicated secrets management feature specifically for this: a secure interface where you enter your API keys and passwords, which the platform then injects as environment variables into your running application at startup, without those values ever needing to exist as a plain-text file anywhere on disk."

> 🎯 **Instructor Note:** Draw this contrast on the board.

```
Local development:
  .env file on your machine → loaded by python-dotenv → 
  available as environment variables

Production (typical hosting platform):
  Secrets entered into the platform's secure dashboard or CLI → 
  injected directly as environment variables at container startup → 
  no plain-text secrets file exists on the server at all
```

**[Script:]**

"Your application code does not need to know or care which of these two mechanisms actually supplied the environment variable — `os.environ.get('DATABASE_URL')` works identically either way. This is precisely the value of the environment-variable-driven configuration pattern from Block 3: the exact same code that reads a `.env` file locally reads platform-injected secrets in production, with zero code changes required between the two."

> 🎯 **Instructor Note:** Ask: "Why is a platform's dedicated secrets management feature generally considered more secure than a `.env` file sitting directly on the production server's disk?" Answer: a `.env` file on a server's disk is a plain-text file that could potentially be read if the server itself were compromised, accidentally included in a backup, or exposed through a misconfiguration; a dedicated secrets manager typically encrypts values at rest and provides tighter, auditable access control over who and what can actually retrieve them.

**Recap of Block 4 before moving on:**

- A secret is any configuration value that would cause real harm if exposed — API keys, database credentials, JWT signing keys, OAuth secrets
- `.gitignore` must exclude `.env` and similar files from the very start of a project, before secrets are ever written to them
- A secret committed to version control, even briefly, must be treated as compromised and rotated — removal from a later commit is not sufficient
- Production environments typically inject secrets directly as environment variables through the hosting platform's own secrets management feature, rather than relying on a plain-text `.env` file on the server

---

## Block 5 — FastAPI Deployment

### 5A — From Development Server to Production Server

**[Script:]**

"`uvicorn main:app --reload` is a development server — it is single-process, watches for file changes, and is explicitly not designed to handle real production traffic reliably. Production deployment requires a genuinely different running configuration, and typically a process that manages multiple worker processes rather than a single one."

---

### 5B — Running with Multiple Workers

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If your application receives one hundred simultaneous requests, but is running as a single process, what happens to requests 2 through 100 while request 1 is still being processed?" Answer: depending on what request 1 is doing, they may need to wait, since a single process has limited capacity to genuinely handle many requests in true parallel — this motivates why production deployments typically run multiple worker processes rather than just one.

**Demo 5 — Running with Gunicorn managing multiple Uvicorn workers (whiteboard-friendly)**

```
gunicorn main:app --workers 4 --worker-class uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

**[Script:]**

"Gunicorn is a production-grade process manager — here, it runs four separate worker processes, each one an instance of Uvicorn actually serving your FastAPI application, distributing incoming requests across all four rather than relying on just one. If one worker process crashes for any reason, Gunicorn detects this and restarts it automatically, while the other three workers continue serving requests uninterrupted — a resilience property the single-process development server does not provide at all.

The number of workers is typically chosen based on available CPU cores on the production server — a common starting guideline is roughly `(2 × number of cores) + 1`, though the actual right number depends on your specific application's workload and should be tuned based on real observed performance."

> 🎯 **Instructor Note:** Ask: "What would happen to your application's availability if you ran it with a single worker in production, and that one worker process crashed due to an unexpected bug in a rarely-hit code path?" Answer: the entire application would go down completely until something manually restarted it — with multiple workers under Gunicorn's management, a single worker crashing is automatically recovered from, and the application keeps serving requests through its remaining workers in the meantime.

---

### 5C — Deployment Platforms and the General Pattern

**[Script:]**

"Specific hosting platforms differ in their exact setup steps, but the general deployment pattern is consistent across nearly all of them: push your packaged application, typically as a Docker image from Block 2, to the platform; the platform builds and runs it, supplying environment variables and secrets as covered in Blocks 3 and 4; and the platform typically handles routing real internet traffic to your running application, often including HTTPS automatically."

> 🎯 **Instructor Note:** Keep this section deliberately platform-agnostic, since specific platforms and their exact interfaces change over time. Note directly: "The exact steps for any specific hosting platform are worth checking in that platform's current documentation when you actually deploy — what matters today is understanding the underlying pieces being configured: your packaged application, your environment variables, your secrets, and your production server configuration, since that understanding transfers regardless of which specific platform you end up using."

**Recap of Block 5 before moving on:**

- The `--reload` development server is not suitable for production; production deployments require a process manager running multiple worker processes
- Gunicorn managing multiple Uvicorn workers distributes requests across processes and automatically restarts any worker that crashes, improving both throughput and resilience
- Worker count is typically guided by available CPU cores, then tuned based on observed real performance
- The general deployment pattern — push a packaged application, supply environment variables and secrets, let the platform route traffic — is consistent across most hosting platforms, even though specific setup steps differ

---

## Block 6 — Stable Production Configs

### 6A — What "Stable" Actually Requires

**[Script:]**

"Beyond simply running with multiple workers, a genuinely stable production configuration includes a few additional, deliberate pieces: proper logging that captures what is actually happening in production without exposing sensitive details, a health check endpoint the hosting platform can use to confirm your application is actually alive and responding, and appropriate error handling that never leaks internal details to end users."

---

### 6B — A Health Check Endpoint

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If your hosting platform has no way to check whether your application is actually running correctly, beyond just confirming the process has not crashed outright, what problem could go undetected?" Answer: the process could still be technically running while being unable to actually serve real requests correctly — for instance, if it lost its connection to the database — and without an active check of genuine functionality, the platform would have no way to detect this and automatically restart or replace the unhealthy instance.

**Demo 6 — A basic health check endpoint (whiteboard-friendly)**

```python
@app.get("/health")
def health_check():
    return {"status": "ok"}
```

**[Script:]**

"Most hosting platforms periodically request this endpoint automatically, and if it stops responding successfully, the platform can automatically restart the affected instance, or stop routing traffic to it until it recovers. A more thorough version might also verify the database connection is genuinely working, not just that the web process itself is running — since a process that is technically alive but cannot reach its database is not actually healthy in any way that matters to real users."

---

### 6C — Hiding Internal Errors from Users

**[Script:]**

"In development, seeing a full error traceback directly in your browser is genuinely useful for debugging. In production, that same detailed traceback exposed to an end user is a real security risk — it can reveal internal file paths, library versions, and details about your application's internal structure that should never be visible to an outside user. This connects directly to the environment-driven configuration from Block 3: debug mode, including detailed error pages, should be tied to the `ENVIRONMENT` setting and firmly disabled in production."

> 🎯 **Instructor Note:** Ask a closing synthesis question: "How does this specific point connect back to the environment management concept from Block 3?" Answer: this is a direct, concrete application of environment-driven configuration — the exact same application code should show detailed errors in development, where that is useful for debugging, and generic, non-revealing errors in production, where the same detail becomes a security liability, with the difference driven entirely by the `ENVIRONMENT` variable rather than by different code.

**Recap of Block 6 before moving on:**

- A stable production configuration includes logging, a genuine health check, and error handling appropriate to the environment — not just multiple worker processes alone
- A health check endpoint lets the hosting platform detect and automatically respond to an unhealthy instance, ideally verifying real functionality like database connectivity, not just that the process is technically alive
- Detailed error tracebacks are useful in development and a security risk in production; this behavior should be driven by the environment configuration from Block 3, not by separate code paths

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "Why pin exact dependency versions instead of leaving them open-ended? Why does the same application code work across development, staging, and production without being edited? What must happen to a secret that was ever committed to version control, even if later removed? Why does production need multiple worker processes instead of one? What must be disabled in production that is useful in development?"

**Packaging a FastAPI Application**

- Packaging captures exactly what an application needs to run, so it can be recreated identically elsewhere
- Pinned dependency versions in `requirements.txt` prevent version drift, at the cost of needing periodic deliberate updates
- Docker bundles the OS layer, Python version, dependencies, and code into a portable image that runs identically anywhere
- Dockerfile instruction order matters for build speed, since Docker caches unchanged layers like dependency installation

**Environment Management**

- Applications typically run across development, staging, and production, each needing different configuration from identical code
- Environment variables, read at startup, drive this configuration without editing source code per environment
- `.env` files are a local development convenience; production typically receives variables from the hosting platform directly

**Secrets Handling**

- A secret is any value that would cause real harm if exposed — API keys, database credentials, signing keys, OAuth secrets
- `.gitignore` must exclude secret files from the very start of a project
- A secret ever committed to version control must be treated as compromised and rotated, not just removed from a later commit
- Production secrets typically come from a platform's dedicated secrets management feature, injected as environment variables, rather than a plain-text file on the server

**FastAPI Deployment**

- The development server is unsuitable for production; a process manager running multiple worker processes is required instead
- Gunicorn managing multiple Uvicorn workers distributes load and automatically restarts crashed workers
- The general deployment pattern — packaged application, environment variables, secrets, platform-managed traffic routing — is consistent across most hosting platforms

**Stable Production Configs**

- Stability requires more than multiple workers alone: proper logging, a genuine health check, and environment-appropriate error handling
- A health check endpoint should ideally verify real functionality, like database connectivity, not just that the process is alive
- Detailed error tracebacks belong in development only; production must show generic errors, driven by environment configuration

**Why All of This Matters Together**

- Packaging, environment management, secrets handling, and stable production configuration are four connected pieces of the same underlying goal: making the application already built across this entire course actually runnable, safely and reliably, somewhere other than your own laptop — the environment-variable pattern that drives configuration is the same one used to load an API key back in the AI integration session, the secrets discipline is the same discipline from the authentication session applied at production scale, and the result of doing all four correctly is the difference between a project that only ever worked in a classroom demo and one that could genuinely serve real users

---

*End of script.*
