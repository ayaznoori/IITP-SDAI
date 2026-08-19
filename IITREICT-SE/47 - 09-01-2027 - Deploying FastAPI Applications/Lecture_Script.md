# Lecture Script: Deploying FastAPI Applications
**Duration:** 110 minutes | **Tools:** VS Code, Docker Desktop or equivalent, FastAPI AI app | **Context:** Module 3–5 project with env-based OpenAI key

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Works on my machine |
| Why Does This Matter? | 12 min | Platforms, leaks |
| What Is the Concept? | 23 min | Layout, env, Docker |
| How Do We Apply It? (LOs) | 52 min | Live image run |
| Secret hunt | 13 min | Grep the image story |
| Recap | 5 min | LLMOps next |

---

## Session Opening (5 min)

**[Script:]** "Your classifier runs in VS Code. Recruiters and users need a **URL**, not a laptop. Today we **package** FastAPI, separate **local vs production** secrets, write a **basic Dockerfile**, **run the container** with stable config, and **never bake keys into images**."

**Problem hook:** `git grep sk-` on a dummy file. "If this is in Docker Hub, assume it is public."

🎯 **Instructor Note:** Docker install issues: demo on instructor machine; students follow file changes even if build waits.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask who has used a `.env` that they almost committed.

**[Script:]** "**Netflix**, **Uber**, and Indian clouds on **AWS** run **containers**. **Razorpay** will not accept a key in GitHub. Capstone quality gate later asks for deployment-like setup. This session is that muscle at beginner level — not Kubernetes."

**Pain if misunderstood:**
- `venv` in the image (huge, fragile)
- Key in Dockerfile
- App bound to localhost inside container
- Different flags every run → "it failed in prod only"

---

## What Is the Concept?

### Layout

App package, requirements, Dockerfile, README.

### Env Vars

Code reads names. Values come from the **environment**. Production injects them.

### Dockerfile

Layered recipe. Copy requirements first for cache (mention lightly).

### Container Run

Image is the snapshot. Run is a process with **runtime** env.

### No Hardcoded Secrets

Git, image layers, and logs are leaky.

---

## How Do We Apply It?

### LO 1: Package FastAPI with clear layout

**Walkthrough:** Move `main.py` under `app/` if needed. Confirm `uvicorn app.main:app`. `requirements.txt` includes `fastapi`, `uvicorn`, `openai`, etc.

**Predict:** Can Docker use a venv from the host?

**Explain result:** No. Image installs its own packages from requirements.

> **In the Real World:** **Heroku**-style `Procfile` is another start command. Docker `CMD` is our version.

---

### LO 2: Manage env vars and secrets local vs production

**Local:** `.env` gitignored; `export` in terminal.  
**Production:** dashboard "Environment" — same **names**.

**Demo table on board.** Code stays `os.environ["OPENAI_API_KEY"]`.

**Predict:** If production forgets the var, should the container start and crash later on first request?

**Explain result:** Fail fast at startup if you can check the var. Clearer ops.

---

### LO 3: Basic Dockerfile for FastAPI

**Write the Dockerfile live** (short). Point out:
- `0.0.0.0`
- no `.env` copy
- `CMD` uvicorn

**Predict before build:** Will `COPY . .` include `.env` if we forget `.dockerignore`?

**Explain result:** Yes risk. Add `.dockerignore` with `.env`, `venv`, `.git`.

---

### LO 4: Deploy or run containerised app with stable config

**Build tag** `bookstore-ai:1`. **Run** `-p 8000:8000 -e OPENAI_API_KEY=$OPENAI_API_KEY`. Hit `/docs`.

**Predict:** Does `/docs` inside the container need the key for Swagger UI to load?

**Explain result:** UI loads without calling OpenAI. Classify still needs the key. Stable: always port 8000, always same image tag for the demo.

If a cloud deploy is available in class, map the same env names. If not, **run container** satisfies the LO.

---

### LO 5: Avoid hardcoding secrets

**Anti-demo:** `ENV OPENAI_API_KEY=sk-test` — then explain `docker history` can reveal. Delete it. Use `-e` only.

**Predict:** Is a test key in GitHub "fine because it is test"?

**Explain result:** Still a leak pattern. Use placeholders in README: `export OPENAI_API_KEY=your_key`.

---

## Secret Hunt (13 min)

Pairs: checklist `.gitignore`, `.dockerignore`, Dockerfile, `main.py`, README. No real keys. Report one risk.

---

## Recap (5 min)

**[Script:]** "The box runs. Next we treat **prompts like code**: versions, evals, cache, **cost and latency budgets** — LLMOps foundations."

---

## Lecture Summary

- **Package** with a clear layout and `requirements.txt`
- **Secrets** via env: local files vs production stores, same code
- A **basic Dockerfile** runs uvicorn on `0.0.0.0`
- **Run the container** with documented ports and runtime env
- **Never hardcode** keys in source or images
- **Practical value:** Your AI API can leave your laptop without leaking the kitchen keys
