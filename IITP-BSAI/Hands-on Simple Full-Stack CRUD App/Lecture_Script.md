# Lecture Script: Hands-on Simple Full-Stack CRUD App
**Duration:** 110 minutes | **Tools:** VS Code, FastAPI, React (Vite), Swagger, Browser Network tab | **Format:** TA-led lab

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Contract on a sticky note |
| Why Does This Matter? | 10 min | Product tickets are full-stack |
| What Is the Concept? | 12 min | Shared schema, fetch loop |
| How Do We Apply It? (LOs) | 70 min | Schema → API → UI → fetch → sync |
| Recap | 10 min | Demo checklist |

---

## Session Opening (8 min)

**[Script:]** "Today is not a new framework. It is one **resource** living in **Postgres**, **FastAPI**, and **React** at the same time. If those three disagree, users see ghosts. We will define a schema, match CRUD routes, build list/create/edit/delete screens, wire **fetch**, and **verify sync**."

**Real-world hook:** Open **Trello** (or a simple tasks app). "Every drag is a PATCH. Every card is a row. You will build a baby version."

🎯 **Instructor Note:** Lab pacing. Frozen FastAPI CRUD from prior sessions is OK. Do not add auth. One resource only.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show a mismatch bug: API returns `title`, UI reads `name`. Empty cards. Class diagnoses.

**[Script:]** "At **Freshworks** or **Zoho**, a ticket is rarely 'make a button.' It is 'field X in UI must match API and DB.' This lab is that ticket. Recruiters ask: have you connected React to your own API? Today the answer becomes yes."

**Pain if misunderstood:**
- Two sources of truth (local array + server)
- Forgetting refetch after delete
- CORS ignored until the demo

> **In the Real World:** **Airbnb** search UI is useless if the API schema drifts. Contracts first.

---

## What Is the Concept?

**Shared schema:** same field names, types, and id.

**CRUD screens:** four user jobs, four HTTP verbs.

**Sync:** after every successful fetch, UI equals GET list.

**Common mistake:** Optimistic UI without confirming the response — skip it today; wait for the server.

---

## How Do We Apply It?

### LO 1: Define one schema → matching FastAPI CRUD

**Problem:** Pick `Task`: `id: int`, `title: str`, `done: bool`.

**Translate logic:** Pydantic in/out models match SQLAlchemy columns.

**Walkthrough:** Sticky note on the projector. Routes: `GET/POST /tasks`, `PATCH /tasks/{id}`, `DELETE /tasks/{id}`.

**Predict before running:** If Pydantic uses `done` and the column is `is_done`, what breaks? (Mapping / validation.)

**Live:** Confirm Swagger schema matches the sticky note.

---

### LO 2: React CRUD screens

**Problem:** Four thin components or one page with four actions.

**Walkthrough:** `TaskList`, create form, edit form (or inline), delete button.

**Predict:** Should the list render before fetch returns? (Show empty or loading — keep it simple.)

**Write code (list idea):**

```jsx
{tasks.map((t) => (
  <li key={t.id}>{t.title}</li>
))}
```

**Explain result:** Keys use `id` from the database, not array index.

---

### LO 3: Wire fetch

**Problem:** Vite on 5173, API on 8000.

```javascript
await fetch("http://localhost:8000/tasks", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ title, done: false }),
});
```

**Predict before running:** What if CORS is off? (Browser blocks; Swagger still works.)

🎯 **Instructor Note:** Network tab is the debugger of this lab. Status and payload first.

> **In the Real World:** **Postman** collections at companies match this same fetch contract.

---

### LO 4: Verify FE/BE stay in sync

**Ritual (do not skip):**

1. Create in UI → GET in Swagger
2. Edit in UI → row in Neon
3. Delete in UI → both lists empty of that id
4. Browser refresh → same list

**Predict:** Refresh restores a deleted item — which layer failed?

**Explain result:** Sync is a procedure, not a hope.

---

## Live Lab (embedded in 70 min)

Checkpoints every 15 minutes: schema, Swagger green, list fetch, mutations, sync ritual.

Pairs demo 60 seconds at the end.

---

## Recap (10 min)

**[Script:]** "You now have a tiny product. Next week we **containerise** the API so it runs the same on your machine and in the cloud."

---

## Lecture Summary

- **Shared schema** is the contract between React and FastAPI
- **FastAPI CRUD** implements list, create, edit, and delete for one resource
- **React screens** expose those four jobs to a human
- **fetch** is the wire; Network tab tells the truth
- **Sync checks** prove UI, API, and database agree
- **Practical value:** This is the smallest full-stack story you can put on a resume with a straight face
