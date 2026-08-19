# Lecture Script: Human-AI Workflows & Portfolio Readiness
**Duration:** 110 minutes | **Tools:** GitHub, live URL, Cursor, README | **Tone:** Career mentor

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Repos that cannot be explained |
| Why Does This Matter? | 12 min | Hiring is a demo of judgement |
| What Is the Concept? | 18 min | Verify loop, hygiene, pitch |
| How Do We Apply It? (LOs) | 55 min | Review, fix, polish, URL, package |
| Clinic | 12 min | Live audits in pairs |
| Recap | 5 min | RAG masterclass teaser |

---

## Session Opening (8 min)

**[Script:]** "You have FastAPI, React, a database, maybe an LLM wrapper, and a live URL. None of that helps if you cannot **defend** it. Today’s workflow: **review AI at each step**, **catch one mistake**, **polish README and commits**, **confirm the live URL**, **package a hiring story**."

**Real-world hook:** Scroll a student GitHub with `update` × 20 and a 404 deploy. Class winces. "That is fixable in one session."

🎯 **Instructor Note:** Prepare a small AI-buggy snippet (wrong JSON key) for the catch-and-fix LO. Do not shame students; audit kindly.

---

## Why Does This Matter?

🎯 **Instructor Note:** Role-play 90 seconds: "Walk me through this repo." Reward structure, punish mystery files.

**[Script:]** "Recruiters at **Amazon**, **Flipkart**, and startups open GitHub before they open your marks. They look for a **live demo**, a **README**, and whether commits look human. Companies also fear juniors who paste Copilot into production. Your verification loop is the counter-story."

**Pain if misunderstood:**
- Dead links in applications
- AI-generated README that mentions the wrong stack
- Cannot explain a function you “wrote”

> **In the Real World:** **Vercel**’s own career pages show shipped work. URL is the new attachment.

---

## What Is the Concept?

**Verification loop:** after every AI step, human gate.

**Portfolio package:** README + history + URL + spoken story.

**Common mistake:** Rewriting everything the night before. Polish the existing slice.

---

## How Do We Apply It?

### LO 1: Review AI output at each step before moving on

**Live:** Ask Cursor for a change. Read the diff aloud. Run it. Only then commit.

**Predict:** What if the diff includes `.env`? (Reject immediately.)

---

### LO 2: Catch and fix at least one AI mistake

**Exercise:** AI returns `name` but the schema is `title`. UI blank.

**Fix:** align field, rerun GET, commit `Fix task title field to match API schema`.

**Predict before running:** Would Pytest have caught this if it asserted `title`? (Yes — connect to Module 8.)

---

### LO 3: Polish README and commit history

README sections: What, Stack, Run, Env **names**, Live URL, Screenshots optional.

```markdown
## Live
https://your-app.example.com
```

**Commits:** squash culture is advanced; today just **future messages are meaningful**. Optionally add 2–3 good commits if history is empty.

🎯 **Instructor Note:** Do not force `git rebase` on the whole class.

---

### LO 4: Confirm a live deployment URL works

Incognito click. Record sleep time. Fix env vars if 500.

**Predict:** CORS on production frontend origin — might fail even if `/docs` works. Note both URLs if they have two hosts.

---

### LO 5: Package the project for a hiring conversation

Template on the board:

1. Problem (one line)
2. Users
3. Stack
4. What I implemented (CRUD / AI wrapper)
5. URL
6. Next improvement (one only)

Students speak it. Peers time 60 seconds.

> **In the Real World:** **Y Combinator** demo day is a tighter version of this. Practise now.

---

## Clinic (12 min)

Pair audit: README, last five commits, live URL, “explain this file.”

---

## Recap (5 min)

**[Script:]** "Saturday professor session: **RAG** — why models need **your** documents, in ideas not in heavy libraries."

---

## Lecture Summary

- **Review each AI step** so the repo stays yours
- **Catching one AI mistake** proves you are the engineer in the loop
- **README and commits** are the written handshake
- A **working live URL** is the demo
- A **short pitch** packages the work for hiring
- **Practical value:** You can walk into a conversation with evidence, not only certificates
