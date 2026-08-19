# Lecture Script: Docker Basics & Containerization
**Duration:** 110 minutes | **Tools:** Docker Desktop, Terminal, FastAPI app, Browser | **Language:** Dockerfile + Python

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Works on my machine |
| Why Does This Matter? | 12 min | Cloud runs images |
| What Is the Concept? | 22 min | Image, container, Dockerfile |
| How Do We Apply It? (LOs) | 55 min | Write, build, run, smoke |
| Live lab | 8 min | Every student hits /docs |
| Recap | 5 min | CI/CD next |

---

## Session Opening (8 min)

**[Script:]** "Your FastAPI works in a venv. A teammate has Python 3.11. You have 3.12. Production has neither. **Containers** freeze the runtime with the app. Today we explain the problem, write a **simple Dockerfile**, **build**, **run locally**, and **smoke-test** the API. That is the whole mountain. Not clusters."

**Real-world hook:** Show Docker Hub page for `python` official image. "Industry stands on these bases."

🎯 **Instructor Note:** Docker Desktop must be running. Have a tiny `requirements.txt` (`fastapi`, `uvicorn`) ready. Skip multi-stage builds.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask two students their `python --version`. Use the mismatch as the hook.

**[Script:]** "When **Spotify** deploys, they do not SSH and pip install by hand. They run a container image. **Render** and **Railway** will ask you for a Dockerfile or will invent one. If you cannot read a five-line Dockerfile, you cannot debug a failed deploy. This session is employable operations literacy."

**Pain if misunderstood:**
- `CMD` using `--host 127.0.0.1` — port map useless
- Copying venv into the image — huge, broken
- Never smoke-testing — “build succeeded” is not “API works”

> **In the Real World:** **Paytm** and bank IT teams still fight environment drift. Containers are how modern teams reduce that fight.

---

## What Is the Concept?

**Container:** isolated process with its own files and declared ports.

**Image:** snapshot used to start containers.

**Dockerfile:** build instructions.

**Python vs JS:** `node:20` + `npm start` is the same pattern as `python:3.12` + `uvicorn`.

**Common mistake:** Confusing “Docker is a VM.” It is lighter. Do not teach hypervisor internals.

---

## How Do We Apply It?

### LO 1: Explain what containers solve

**Problem:** Dependency drift across laptop, TA machine, cloud.

**Translate logic:** Ship app + runtime together.

**Predict:** Does a container replace Neon? (No — database still external; container runs the API.)

---

### LO 2: Write a simple Dockerfile for FastAPI

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Predict before running:** Why copy `requirements.txt` before the rest of the code? (Faster rebuilds when only app code changes — mention lightly.)

**Explain result:** Each line is a layer. Keep it readable.

🎯 **Instructor Note:** Add `.dockerignore` with `.venv` and `.env` — hygiene, not a new topic.

---

### LO 3: Build a Docker image

```bash
docker build -t notes-api .
```

**Predict before running:** First build slow or fast? (Slow — downloads base image.)

**Explain result:** `docker images` shows `notes-api`.

---

### LO 4: Run the container locally

```bash
docker run --rm -p 8000:8000 notes-api
```

**Predict before running:** If port 8000 is already used by local Uvicorn, what happens? (Bind error — stop the other process.)

**Explain result:** Logs show Uvicorn inside the container.

---

### LO 5: Smoke-test the containerised API

Browser: `http://localhost:8000/docs`. Or:

```bash
curl -s http://localhost:8000/notes
```

**Predict:** Swagger from the container should match the app you copied in.

**Live:** Change a route, rebuild, rerun, confirm smoke test sees the change.

> **In the Real World:** **GitHub** Actions and cloud hosts run a similar “start then HTTP check.” You are doing it by hand.

---

## Live Lab (8 min)

Build, run, screenshot `/docs` from the container. TAs unblock Docker Desktop permissions.

---

## Recap (5 min)

**[Script:]** "Next session: **GitHub Actions** on push, then **Render or Railway**. The image idea travels with you."

---

## Lecture Summary

- **Containers** fix environment drift by packaging app and runtime
- A **Dockerfile** declares how to install and start FastAPI
- **docker build** produces an image you can name and reuse
- **docker run** starts a local container with port mapping
- A **smoke-test** proves the API answers, not only that build finished
- **Practical value:** You can run the same FastAPI the way cloud platforms expect
