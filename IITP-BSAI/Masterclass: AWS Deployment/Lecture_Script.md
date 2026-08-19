# Lecture Script: Masterclass — AWS Deployment
**Duration:** 110 minutes | **Tools:** Whiteboard, AWS console (demo only), student Render/Railway URLs | **Tone:** Professor / mentor masterclass

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 10 min | Cloud as a map, not a badge |
| Why Does This Matter? | 15 min | Jobs, bills, leaked keys |
| What Is the Concept? | 25 min | AWS pieces vs PaaS |
| How Do We Apply It? (LOs) | 48 min | Outline, secrets, compare, checklist |
| Clinic | 7 min | Apply checklist to live student URLs |
| Recap | 5 min | AI module next |

---

## Session Opening (10 min)

**[Script:]** "You shipped a backend on a platform that hid the servers. That was the correct engineering choice for speed. Today we zoom out. **Amazon Web Services** is not a single deploy button. It is a city of services. I will teach you a **beginner map**, an **outline** for a containerised API, **secret hygiene**, an honest **comparison** with Render and Railway, and a **short production checklist**. We will not pretend you are certified. We will make you literate."

**Real-world hook:** Show an AWS homepage service grid for five seconds, then hide it. "If that grid feels loud, good. Loud is the lesson. Experts pick three pieces, not fifty."

🎯 **Instructor Note:** Mentor + professor voice. Discourage live account creation for everyone if time is tight. One guided console walk is enough. No IAM policy JSON workshops.

---

## Why Does This Matter?

🎯 **Instructor Note:** Open a job description that lists AWS *and* FastAPI. Read it aloud. Then ask: "Does this mean day-one EC2?" Discuss.

**[Script:]** "Companies like **Amazon** itself, **Netflix**, and large Indian product teams run on public clouds. They hired people who can **reason about** compute, images, env vars, and failure. They did not hire people who memorised every acronym. If you leak a cloud key, the failure is financial and reputational. If you over-claim AWS on a resume, the failure is the first interview. Both matter."

**Pain if misunderstood:**
- Equating “I opened the console” with “I operate production”
- Copying access keys into GitHub
- Abandoning a working Railway app to chase logos

> **In the Real World:** Startups often **start on Render/Railway/Fly**, then **move to AWS/GCP** when compliance, scale, or enterprise customers demand it. Sequence is a strategy, not a moral ranking.

---

## What Is the Concept?

**Mental model:** You rent **compute** to run the container, **storage/network** to reach it, and **config** to inject secrets.

**PaaS vs IaaS (plain words):** Platform does the furniture. AWS often makes you choose furniture.

**Container path (story only):** image → registry → runner → URL → logs.

**Common mistakes:** Teaching Lambda + API Gateway + ECS + EKS in one hour. Pick **one outline** and stay there.

**Python vs JS:** Same cloud. Same image idea. Runtime is just the `CMD`.

---

## How Do We Apply It?

### LO 1: Explain AWS at a beginner high level

**Problem:** Students think AWS is “Amazon’s Vercel.”

**Translate logic:** Catalog of APIs for computers, data, and delivery.

**Socratic:** Name two things you already outsourced (GitHub, Neon). AWS is more of that, with sharper edges.

**Predict:** Do you need AWS to pass this course’s deploy LO from Friday? (No — you already deployed.)

---

### LO 2: Outline deploying a containerised backend on AWS

**Whiteboard five boxes only:**

1. Build image (you did this)
2. Store image in a registry
3. Run the image on a container-aware compute service
4. Inject env vars
5. Expose HTTPS and read logs

**Predict before a console click:** Which step did Render do for you automatically? (Most of 2–5.)

**Demo (mentor):** Click-path at high level. No students following every checkbox unless extra time.

**[Script:]** "If you can narrate these five steps in an interview, you are ahead of many applicants who only say the word Kubernetes."

---

### LO 3: Manage environment variables and secrets safely

**Rules of the house:**

- Git never sees passwords
- Chat never sees production strings
- Rotate if leaked
- Prefer platform secret storage as you grow

**Predict:** Is `DEBUG=true` as dangerous as `DATABASE_URL`? (Usually less, still do not leak internals.)

> **In the Real World:** **Capital One** and others had famous cloud security incidents. The academic point: **identity and secrets** are part of deploy, not an optional extra.

---

### LO 4: Compare Render/Railway with AWS for early projects

**Discussion table (fill live):** time-to-URL, cost surprise risk, resume signal, team size.

**Mentor take:** Ship on PaaS now. Learn AWS names now. Migrate when a **reason** appears (customer, scale, policy).

**Predict:** A two-person student team with one FastAPI service — default? (PaaS.)

---

### LO 5: Apply a short production deploy checklist

Project the checklist. Students audit **their existing live API**:

- Starts reliably
- Secrets off Git
- HTTPS URL works
- Logs exist
- Access to env vars is limited

**Predict:** Free-tier sleep — is that a checklist fail? (Note it; not always a fail for class, but say it in a hiring demo.)

---

## Clinic (7 min)

Pairs grade each other’s README: live URL + “how to set env vars” without pasting secrets.

---

## Recap (5 min)

**[Script:]** "You are allowed to be beginners on AWS. You are not allowed to be careless with secrets. Next week we enter **LLMs** as builders — prompts first, then APIs on the backend you just learned to ship."

---

## Lecture Summary

- **AWS** is a large service catalog; beginners need a map, not every service
- A **containerised backend** on AWS follows image, registry, run, config, URL
- **Secrets** stay out of Git and casual chat; leaks have real cost
- **Render/Railway** remain the right default for early projects; AWS is the next landscape
- A **short checklist** turns “I deployed” into something you can defend
- **Practical value:** You can talk about production like a junior engineer, not a tourist in the console
