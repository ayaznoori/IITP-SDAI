# Lecture Script: GitHub Actions CI/CD & Cloud Deploy
**Duration:** 110 minutes | **Tools:** GitHub, Actions YAML, Render or Railway, Browser | **Language:** YAML + FastAPI

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Push should tell the truth |
| Why Does This Matter? | 12 min | Hiring URLs and safety |
| What Is the Concept? | 20 min | CI/CD, YAML, env vars |
| How Do We Apply It? (LOs) | 58 min | Workflow, deploy, verify |
| Live lab | 7 min | Green check + live /docs |
| Recap | 5 min | AWS masterclass teaser |

---

## Session Opening (8 min)

**[Script:]** "Docker proved the API can run in a box. Today we connect GitHub to **checks** and a **cloud host**. The story is three verbs: **push**, **check**, **deploy**. You will write a small **GitHub Actions** workflow, deploy FastAPI to **Render or Railway**, set **env vars**, and open a **live URL**."

**Real-world hook:** Show a green check on `facebook/react` Actions, then a student repo with none. "That gap is today’s job."

🎯 **Instructor Note:** Pick **one** host for the room (Render **or** Railway) to cut chaos. Have a fallback if free-tier sleep delays demos.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask who can share a public API URL right now. Most cannot. That is the pain.

**[Script:]** "Recruiters will not SSH into your laptop. **Naukri** and LinkedIn profiles with a live `/docs` look like builders. Internally, **Razorpay** and **CRED** will not merge to main if CI is red. You are learning both the public URL and the red/green habit."

**Pain if misunderstood:**
- Deploy with secrets in the repo
- Workflow that never runs (wrong folder path)
- Trusting “build succeeded” without opening the URL

> **In the Real World:** **GitHub** itself is deployed through pipelines. Your YAML is a toy cousin of that idea.

---

## What Is the Concept?

**CI:** automatic test/build on new commits.

**CD:** getting that build onto a URL.

**Workflow file:** must live under `.github/workflows/`.

**Env vars:** named slots the process reads with `os.getenv`.

**Common mistake:** `on: pull_request` only when you meant `push` — stay with `on: [push]` as the LO.

---

## How Do We Apply It?

### LO 1: Explain CI/CD as push → check → deploy

**Whiteboard the three boxes.** Student commit is the only trigger they control.

**Predict:** If tests fail, should we still deploy? (No — that is the point of check.)

---

### LO 2: Write a simple GitHub Actions workflow on push

```yaml
name: CI
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "CI ran"
```

**Predict before running:** Will this run on a local save without push? (No.)

**Upgrade live:** add Python setup + `pytest` if requirements allow. Keep YAML short.

🎯 **Instructor Note:** First run often fails on path or missing `requirements.txt`. Debug the log together — that is the lesson.

---

### LO 3: Deploy the backend to Render or Railway

**Walkthrough:** New Web Service → connect repo → Docker or Uvicorn start command → deploy.

**Predict:** First deploy is slow. Why? (Build + cold start.)

**Explain result:** Host gives a `*.onrender.com` or Railway domain.

> **In the Real World:** **Stripe** docs show “deploy a sample.” Same connect-repo pattern on PaaS.

---

### LO 4: Configure basic environment variables

Dashboard → Environment → `DATABASE_URL` = Neon string.

**Predict before running:** If the var is missing, will FastAPI start? (Likely crash — good.)

**[Script:]** "Names match local `.env`. Values are production. Never screenshot the secret."

---

### LO 5: Verify the live URL responds

Open `/docs`. Run GET. Optional: `curl` the health or list route.

**Predict:** Free tier may spin down — first request is slow, not always dead.

**Explain result:** A 200 on a public URL is the definition of deployed for this course.

---

## Live Lab (7 min)

Every student: Actions run visible, live `/docs` screenshot. TAs chase failed YAML indents.

---

## Recap (5 min)

**[Script:]** "Saturday masterclass: **AWS** at a high level. You will compare it to Render/Railway, not rebuild your app on EC2 today."

---

## Lecture Summary

- **CI/CD** is push, then check, then deploy
- A **GitHub Actions** YAML on push is your first factory
- **Render or Railway** hosts the FastAPI service from GitHub
- **Env vars** keep Neon URLs and secrets off Git
- A **live URL** is how you prove the backend exists in the world
- **Practical value:** You can send a mentor a link instead of a zip file
