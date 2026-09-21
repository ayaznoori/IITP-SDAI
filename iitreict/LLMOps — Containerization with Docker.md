# Lecture Script: LLMOps — Containerization with Docker
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Docker Set-Up | 15 min |
| 3 | Containerization — Core Concepts | 25 min |
| 4 | Dockerfile Creation | 30 min |
| 5 | Docker Hub | 22 min |
| 6 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience briefly encountered Docker in the packaging and deployment session — a Dockerfile was shown as one piece of getting a FastAPI application production-ready. This session goes considerably deeper: actually installing Docker, understanding what a container fundamentally is, writing a real Dockerfile deliberately, and distributing an image through Docker Hub. Frame this as depth on something already introduced, specifically for LLM-powered applications, not entirely new material. Wait after the opening question.

**[Script:]**

"You have already seen a Dockerfile — a handful of lines that bundled a FastAPI application into something portable. Today we go much deeper into what is actually happening when you do that, and why it matters even more specifically for the LLM-powered applications you have been building throughout this course.

Your applications depend on specific package versions — `openai`, `fastapi`, `sqlalchemy` — at exact versions you tested against. They depend on environment variables holding API keys. They may depend on specific system-level libraries your Python packages need underneath them. Every one of these is a way your application can behave differently, or fail outright, on a machine that is not your own. A teammate trying to run your LLM-powered feature locally, a staging server, a production server — each is a different environment, and 'it works on my machine' is not a deployment strategy.

Containerization solves this by packaging your application together with everything it needs to run, into a single, portable unit that behaves identically no matter where it runs. Today covers four connected practical skills: getting Docker actually installed and working on your machine, understanding what containerization fundamentally is — not just how to write the file, but what a container actually is and why it works — writing a real, well-structured Dockerfile deliberately rather than by copying a template, and using Docker Hub to actually share and distribute your built images, the same way you already share code through GitHub.

By the end of today, you will be able to take any application you have built in this course and package it into something that runs identically on any machine with Docker installed — your laptop, a teammate's laptop, or a real production server."

---

## Block 2 — Docker Set-Up

### 2A — Installing Docker

**[Script:]**

"Docker Desktop is the standard way to get Docker running on a development machine — it includes the Docker engine itself, along with a graphical interface for inspecting running containers and images. Let us get this installed and verified together, step by step."

> 🎯 **Instructor Note:** Walk through this live, and confirm every learner has a working installation before moving on — nothing in the rest of the session works without Docker actually running.

**Demo 1 — Installing and verifying Docker (whiteboard-friendly)**

```
Step 1: Download Docker Desktop for your operating system from 
        docker.com

Step 2: Run the installer and follow the setup prompts

Step 3: Launch Docker Desktop and wait for it to report 
        "Docker is running" (a status indicator in the app)

Step 4: Open a terminal and verify the installation:

docker --version
docker run hello-world
```

**[Script:]**

"`docker --version` confirms the command-line tool is correctly installed and reports its version. `docker run hello-world` is the standard verification step — it downloads a small, purpose-built test image and runs it, and a successful run prints a confirmation message explaining that your installation is working correctly. If you see that message, Docker is fully operational on your machine."

> 🎯 **Instructor Note:** Ask the room directly: "Please run these two commands now and confirm you see the hello-world confirmation message. Raise a hand if you do not." Do not proceed to Block 3 until every learner has a confirmed working installation.

---

### 2B — Core Commands You Will Use Constantly

**[Script:]**

"A small set of Docker commands will account for the large majority of what you actually type throughout this session and beyond."

> 🎯 **Instructor Note:** Write this command reference on the board and keep it visible for the rest of the session.

```
docker build -t <name> .     — build an image from a Dockerfile
docker run <name>            — run a container from an image
docker ps                    — list currently running containers
docker images                — list images available locally
docker stop <container>      — stop a running container
```

**[Script:]**

"We will use each of these directly as the session progresses — `docker build` and `docker run` especially, starting in Block 4 once you have an actual Dockerfile to build from."

**Recap of Block 2 before moving on:**

- Docker Desktop provides the Docker engine and a graphical interface for local development
- `docker --version` and `docker run hello-world` are the standard verification steps confirming a working installation
- `docker build`, `docker run`, `docker ps`, `docker images`, and `docker stop` are the core commands used constantly throughout practical Docker work

---

## Block 3 — Containerization: Core Concepts

### 3A — What a Container Actually Is

**[Script:]**

"Before writing another Dockerfile, it is worth understanding precisely what a container is, since this understanding is what will let you reason about problems later, rather than just following steps. A container is a running instance of an image, isolated from the host machine and from other containers, but sharing the host machine's operating system kernel underneath — this is the detail that makes containers meaningfully different from a full virtual machine."

> 🎯 **Instructor Note:** Draw this comparison on the board — this distinction is genuinely important and frequently confused.

```
Virtual machine:
  Full guest operating system, fully virtualized hardware
  Heavy — gigabytes in size, slow to start (minutes)

Container:
  Shares the host machine's OS kernel
  Isolated processes, filesystem, and network, but NOT a full OS
  Light — megabytes in size, fast to start (seconds)
```

**[Script:]**

"This is why containers start in roughly a second, while a virtual machine typically takes minutes — a container is not booting an entire operating system from scratch, it is starting an isolated process that shares the host's already-running kernel. This is also why a container image can be dramatically smaller than a full virtual machine image — it only needs to bundle what is different from the host, not an entire independent operating system."

> 🎯 **Instructor Note:** Ask: "Given that a container shares the host's kernel rather than running a full separate operating system, what does this imply about running a Linux container on your own laptop, if your laptop runs Windows or macOS?" Answer: Docker Desktop actually runs a lightweight Linux virtual machine in the background specifically to provide that shared Linux kernel, since containers built from standard Linux-based images need a Linux kernel underneath them; this is transparent to you as the user, but it explains why Docker Desktop itself has some virtual-machine-like overhead even though the containers running inside it are lightweight.

---

### 3B — Images Versus Containers

**[Script:]**

"An image and a container are related but distinct concepts, and the distinction matters for how you actually work with Docker day to day. An image is a static, read-only template — the packaged application and everything it needs, built once. A container is a running instance created from that image — you can start multiple containers from the same single image, each one an independent, isolated running instance."

> 🎯 **Instructor Note:** Draw this relationship on the board using a familiar analogy.

```
Image  = a class definition (a blueprint)
Container = an instance of that class (a running object)

One image → can produce many independent, running containers
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If you run `docker run my-app` three separate times using the same image, how many independent containers do you end up with, and do they share any state with each other by default?" Answer: three separate, independent containers, each with its own isolated filesystem and process space by default — changes made inside one do not affect the others, exactly like three separate objects instantiated from the same class do not share instance state with each other.

---

### 3C — Why Isolation Matters for LLM-Powered Applications

**[Script:]**

"This isolation is specifically valuable for the applications you have built in this course. Your LLM-powered FastAPI application depends on a specific Python version, specific pinned package versions from `requirements.txt`, and environment variables holding API keys. Inside a container, all of this is isolated from whatever happens to already be installed on the host machine — no version conflicts with some other, completely unrelated Python project also on that machine, no accidental interference between your application's dependencies and anything else running on the same server."

> 🎯 **Instructor Note:** Ask a synthesis question: "How does this isolation connect back to the version-pinning discipline from the packaging and deployment session?" Answer: version pinning in `requirements.txt` guarantees exact dependency versions get installed; container isolation guarantees those exact pinned versions are the only versions present at all inside that container's environment, with no possibility of the host machine's own separately-installed packages interfering — the two practices reinforce each other toward the same goal of eliminating "works on my machine" uncertainty.

**Recap of Block 3 before moving on:**

- A container shares the host machine's OS kernel rather than virtualizing a full separate operating system, which is why containers are lightweight and start quickly compared to virtual machines
- An image is a static, read-only template; a container is a running, isolated instance created from that image, and one image can produce many independent containers
- Container isolation protects an LLM-powered application's specific dependencies and environment from interference by anything else on the host machine
- This isolation reinforces the version-pinning discipline from the packaging and deployment session toward the same underlying goal: eliminating environment-dependent, hard-to-reproduce failures

---

## Block 4 — Dockerfile Creation

### 4A — Building a Dockerfile Deliberately, Instruction by Instruction

**[Script:]**

"You have seen a complete Dockerfile before. Today, build one deliberately, understanding exactly what each instruction actually does and why it appears in that specific order — this is what lets you write and debug a Dockerfile confidently for a new application, rather than only being able to copy an existing template."

---

### 4B — Choosing a Base Image

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If you choose a full, general-purpose base image versus a 'slim' variant of the same Python version, what tradeoff do you expect between the two?" Answer: the full image includes more pre-installed tools and libraries, which can be convenient but makes the resulting image considerably larger; the slim variant is smaller and faster to build and transfer, at the cost of needing to explicitly install anything extra your specific application actually requires beyond the Python essentials.

**Demo 2 — Selecting a base image for an LLM-powered application (whiteboard-friendly)**

```dockerfile
FROM python:3.12-slim
```

**[Script:]**

"This line establishes the foundation everything else builds on: a specific, pinned Python version — exactly the same discipline as pinning package versions in `requirements.txt`, now applied to the Python runtime itself — using the `slim` variant, which includes the Python interpreter and essential tools without unnecessary extras, keeping the final image meaningfully smaller."

> 🎯 **Instructor Note:** Ask: "Why pin the exact Python version here, `3.12`, rather than using an unpinned or 'latest' tag?" Answer: exactly the same reasoning as pinning package versions — an unpinned base image could resolve to a different, newer Python version at some point in the future, potentially introducing compatibility issues with your pinned package versions that have nothing to do with any change you actually made to your own application.

---

### 4C — Structuring Instructions for Efficient Builds

**Demo 3 — A complete, deliberately ordered Dockerfile (whiteboard-friendly)**

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV PYTHONUNBUFFERED=1

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**[Script:]**

"`WORKDIR /app` sets the working directory inside the image for every subsequent instruction. `COPY requirements.txt .` followed by `RUN pip install` — this specific ordering, dependencies before application code, is deliberate: Docker caches each instruction as a layer, and if only your application code changes on a later build while `requirements.txt` stays the same, Docker reuses the cached dependency-installation layer entirely, skipping that slow step rather than reinstalling everything from scratch.

`COPY . .` copies the rest of your application code in, after dependencies are already installed. `ENV PYTHONUNBUFFERED=1` ensures Python's output — including your application's logs — is not buffered internally, so log messages appear immediately rather than being delayed, which matters for actually seeing what an LLM-powered application is doing in something like real time when monitoring it. `EXPOSE 8000` documents which port the container listens on — this is informational, helping anyone reading the Dockerfile understand the container's networking, though it does not by itself make the port accessible from outside without also being mapped when the container actually runs. `CMD` specifies what runs when a container starts from this image."

> 🎯 **Instructor Note:** Ask: "If you changed a single line in your application's `main.py` and rebuilt this image, which specific instructions would Docker need to actually re-run, and which would it reuse from cache?" Answer: `RUN pip install` and everything before it would be reused from cache, since `requirements.txt` did not change; only `COPY . .` and everything after it would actually re-run, since that is the first instruction after the point where something genuinely changed. This concretely demonstrates why the ordering directly affects how fast your iterative rebuilds actually are.

---

### 4D — Building and Running the Image

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "After running `docker build -t my-llm-app .`, and then `docker run -p 8000:8000 my-llm-app`, what do you expect the `-p 8000:8000` flag specifically to accomplish, given what EXPOSE alone does not do?" Answer: `-p 8000:8000` maps port 8000 on the host machine to port 8000 inside the container, which is what actually makes the application reachable from outside the container — this is the piece that `EXPOSE` alone, being purely documentation, does not provide.

**Demo 4 — Building and running the image (whiteboard-friendly)**

```
docker build -t my-llm-app .

docker run -p 8000:8000 --env-file .env my-llm-app
```

**[Script:]**

"`docker build -t my-llm-app .` builds an image from the Dockerfile in the current directory, tagging it with the name `my-llm-app`. `docker run -p 8000:8000 --env-file .env my-llm-app` starts a container from that image, mapping the port so it is actually reachable, and — critically for an LLM-powered application — `--env-file .env` supplies your environment variables, including your API key, at container startup, exactly the environment-variable-driven configuration pattern from the packaging and deployment session, now supplied specifically through this flag rather than a locally loaded `.env` file inside the container itself."

> 🎯 **Instructor Note:** Reinforce this directly: "Notice the `.env` file itself was never copied into the image in the Dockerfile — that would be a serious mistake, baking a secret directly into a portable, potentially shared image. `--env-file` supplies it only at runtime, to this specific running container, exactly the secrets-handling discipline from the deployment session."

**Recap of Block 4 before moving on:**

- A base image should pin a specific version, exactly like pinning package dependencies, and a slim variant reduces image size when the extra tools in a full image are not needed
- Instruction ordering directly affects build speed through Docker's layer caching — dependencies should be copied and installed before application code, since application code changes far more often
- `EXPOSE` documents a container's port but does not itself make it reachable; `-p` at `docker run` time is what actually maps the port to the host
- Secrets like API keys should never be copied into the image itself; `--env-file` or equivalent supplies them only at container runtime, consistent with the secrets-handling discipline from the deployment session

---

## Block 5 — Docker Hub

### 5A — Why You Need a Registry

**[Script:]**

"An image built locally with `docker build` exists only on your own machine so far. To actually deploy it to a server, or to share it with a teammate, that image needs to go somewhere both parties, or your production server, can actually access it. Docker Hub is the most widely used public registry for exactly this purpose — a place to push built images to, and pull them from, the same fundamental role GitHub plays for code, but for built container images instead of source code."

> 🎯 **Instructor Note:** Draw this parallel on the board — it is a strong, accurate mental model for this entire block.

```
GitHub         : source code    :: Docker Hub : built images
git push       : share code     :: docker push : share an image
git pull/clone : get code       :: docker pull : get an image
```

---

### 5B — Pushing an Image to Docker Hub

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Based on the GitHub parallel just drawn, what do you expect you need to do before you can actually push an image to Docker Hub, analogous to needing a GitHub account and being logged in before you can push code there?" Answer: create a Docker Hub account and log in from the command line, exactly analogous to needing a GitHub account and authenticated Git credentials before pushing code.

**Demo 5 — Pushing an image to Docker Hub (whiteboard-friendly)**

```
docker login

docker tag my-llm-app yourusername/my-llm-app:v1

docker push yourusername/my-llm-app:v1
```

**[Script:]**

"`docker login` authenticates your command line with your Docker Hub account. `docker tag` renames your locally built image to include your Docker Hub username and a specific version tag — `yourusername/my-llm-app:v1` — since Docker Hub organizes images by account namespace, exactly like a GitHub repository is namespaced under a specific account or organization. `docker push` then actually uploads that tagged image to Docker Hub, making it available for anyone with appropriate access to pull.

The `:v1` tag deserves particular attention — this is version tagging for images, directly analogous to the prompt versioning discipline from the LLMOps evaluation session, now applied to entire packaged application images rather than individual prompt strings. A specific, meaningful tag lets you and your team know exactly which version of your application a given running container is actually running, and lets you roll back cleanly to a known-good previous version if a new one causes a problem."

> 🎯 **Instructor Note:** Ask: "If you push a new image using the tag `latest` every single time, instead of meaningful version tags like `v1`, `v2`, and so on, what capability do you lose?" Answer: you lose the ability to know exactly which version is currently running anywhere, and you lose the ability to cleanly roll back to a specific prior version, since `latest` always points to whatever was most recently pushed — this is directly analogous to the problems with unpinned dependency versions from Block 4, now at the level of entire application images rather than individual packages.

---

### 5C — Pulling and Running an Image from Docker Hub

**Demo 6 — Pulling and running a published image (whiteboard-friendly)**

```
docker pull yourusername/my-llm-app:v1

docker run -p 8000:8000 --env-file .env yourusername/my-llm-app:v1
```

**[Script:]**

"This is what a teammate, or a production server, would actually run — `docker pull` retrieves the exact image you pushed, and `docker run` starts it, supplying environment variables at runtime exactly as before. Notice this teammate never needed your source code, your specific local Python setup, or your exact locally installed package versions at all — the pulled image already contains everything needed to run identically to how it ran on your own machine, which is the entire point of everything covered across this session."

> 🎯 **Instructor Note:** Close with a synthesis question connecting the whole session together. Ask: "Trace the full path an LLM-powered application takes across this entire session, from source code on your laptop to running on a teammate's machine or a production server — name every step." Guide the room through: write the Dockerfile deliberately (Block 4), build a local image with `docker build`, tag it with a meaningful version (Block 5), push it to Docker Hub, and then anyone with access pulls that exact image and runs it, supplying their own environment variables and secrets at runtime — the same portable, isolated container, built once, running identically everywhere it goes.

**Recap of Block 5 before moving on:**

- Docker Hub is a registry for built images, playing the same role for images that GitHub plays for source code
- `docker login`, `docker tag`, and `docker push` authenticate, namespace, and upload a locally built image to Docker Hub
- Meaningful version tags, rather than always using `latest`, allow tracking exactly which version is running and enable clean rollback — directly analogous to prompt versioning discipline applied to entire application images
- `docker pull` followed by `docker run` retrieves and runs a published image identically anywhere, without the receiving machine needing the original source code or local development setup at all

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "Why do containers start faster than virtual machines? What is the actual difference between an image and a container? Why does Dockerfile instruction order affect build speed? Why should secrets never be copied into an image itself? What does a meaningful version tag on Docker Hub actually let you do?"

**Docker Set-Up**

- Docker Desktop provides the Docker engine and a local development interface
- `docker --version` and `docker run hello-world` verify a working installation
- A small core set of commands — build, run, ps, images, stop — accounts for most day-to-day Docker usage

**Containerization — Core Concepts**

- A container shares the host machine's OS kernel rather than virtualizing a full separate operating system, making it lightweight and fast to start compared to a virtual machine
- An image is a static, read-only template; a container is a running, isolated instance created from it, and one image can produce many independent containers
- Container isolation protects an application's specific dependencies and environment, reinforcing the version-pinning discipline from the packaging and deployment session

**Dockerfile Creation**

- A base image should pin a specific version, exactly like pinning package dependencies in `requirements.txt`
- Instruction order affects build speed through layer caching — dependencies should be installed before application code is copied in
- Secrets should never be copied into an image; they are supplied only at container runtime, consistent with the secrets-handling discipline from the deployment session

**Docker Hub**

- Docker Hub is a registry for built images, playing the same role for images that GitHub plays for source code
- `docker tag` and `docker push` publish a local image; `docker pull` retrieves it anywhere
- Meaningful version tags enable tracking exactly what is running and clean rollback, directly analogous to prompt versioning applied to entire application images

**Why All of This Matters Together**

- Docker set-up, containerization concepts, Dockerfile creation, and Docker Hub together form the complete path an LLM-powered application takes from source code on a single developer's laptop to running identically anywhere it needs to run — a teammate's machine, a staging server, production — and every discipline from earlier in this course reappears here in a new form: version pinning becomes a pinned base image, environment-variable configuration becomes runtime-supplied secrets, and the versioning discipline from prompt management becomes meaningful image tags on Docker Hub, all converging on the same underlying goal of eliminating "it works on my machine" as a source of real production failures

---

*End of script.*
