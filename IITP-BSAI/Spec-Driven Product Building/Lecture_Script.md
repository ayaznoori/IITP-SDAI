# Lecture Script: Spec-Driven Product Building
**Duration:** 110 minutes | **Tools:** Whiteboard, Markdown spec, existing CRUD app | **Tone:** Product + engineering mentor

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Fluent wrong apps |
| Why Does This Matter? | 12 min | AI, teams, capstone |
| What Is the Concept? | 20 min | Stories, AC, model |
| How Do We Apply It? (LOs) | 55 min | One feature spec |
| Live lab | 10 min | Spec a tiny slice |
| Recap | 5 min | Human-AI workflow next |

---

## Session Opening (8 min)

**[Script:]** "You can generate a UI in minutes. That is dangerous if you never defined **done**. Today: **when to prototype fast** versus **specify first**. You will write **user stories**, **acceptance criteria**, a **data model sketch**, and a **short spec** you could actually implement."

**Real-world hook:** Show two READMEs — a vibe dump vs a one-page spec. Ask which a TA can grade.

🎯 **Instructor Note:** Do not teach full Agile certification. One feature only. Tie examples to their tasks app.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask Cursor to “build a clone of Trello.” Watch scope explode. Pause. "That is why specs exist."

**[Script:]** "**Atlassian** (Jira) and **Linear** exist because teams disagree about done. **Amazon** working backwards starts with the press release. You will write a student version: stories plus criteria. With LLMs, the spec is also the prompt. Garbage spec, garbage generation."

**Pain if misunderstood:**
- Infinite features
- Frontend fields that never existed in the API
- Demos that cannot be tested

> **In the Real World:** **Razorpay** payment checkouts have brutal acceptance tests. Money makes criteria serious. Your tasks app trains the muscle.

---

## What Is the Concept?

**User story:** user-facing slice.

**Acceptance criteria:** observable checks.

**Data model:** fields and id.

**Prototype vs spec:** unknown UX vs known contract.

**Common mistake:** Stories that are tasks for developers (“add CORS”) — rewrite as user value.

---

## How Do We Apply It?

### LO 1: Explain when to prototype fast vs specify first

**Rule of thumb:** unknown feel → prototype; multiple builders or AI bulk-gen → spec.

**Predict:** Adding `done` to an existing tasks API — which mode? (Spec first; the product is known.)

---

### LO 2: Write user stories for one small feature

```text
As a student, I want to mark a task done, so that I can see what remains.
```

**Predict:** Is “as a developer I want a boolean column” a user story? (No.)

---

### LO 3: Define acceptance criteria that make done clear

```text
Given a task is listed, when I click Done, then it shows as completed after reload.
Given I click Done on a missing id, then I see a not-found message.
```

**Predict before coding:** Could Pytest express the first line later? (Yes — that is the point.)

---

### LO 4: Sketch a simple data model

Box `Task` — `id`, `title`, `done`. No `User` table today.

**Walkthrough:** If AC needs timestamps, add `updated_at` now, not in panic during the demo.

---

### LO 5: Turn a rough idea into a short implementable spec

Live Markdown:

- Problem
- Stories
- AC
- Model
- Out of scope: auth, mobile app, AI rewrite

**[Script:]** "If it does not fit one page, you are not slicing."

> **In the Real World:** **Basecamp** Shape Up pitches are short on purpose. Length is a smell.

---

## Live Lab (10 min)

Pairs spec “edit title” for their CRUD app. Mentor stamps “implementable” or “too big.”

---

## Recap (5 min)

**[Script:]** "Next you will **verify AI output**, polish **README and commits**, and package a **hiring conversation** around a live URL."

---

## Lecture Summary

- **Prototype** explores; a **spec** locks a slice for building and for AI
- **User stories** describe value for a role
- **Acceptance criteria** make done testable
- A **data model sketch** keeps FastAPI and React aligned
- A **short spec** is an implementable one-pager with out-of-scope
- **Practical value:** You can steer yourself, a teammate, or a model without drowning in features
