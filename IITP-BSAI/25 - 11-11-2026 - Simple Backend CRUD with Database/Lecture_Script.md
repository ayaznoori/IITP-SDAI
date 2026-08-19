# Lecture Script: Simple Backend CRUD with Database
**Duration:** 110 minutes | **Tools:** VS Code, FastAPI, Swagger, Neon | **Language:** Python

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Persistence vs RAM |
| Why Does This Matter? | 12 min | Products keep user writes |
| What Is the Concept? | 22 min | Session per request, commit |
| How Do We Apply It? (LOs) | 55 min | CREATE, READ, one mutation |
| Live lab | 8 min | Restart proof + Swagger |
| Recap | 5 min | Testing next |

---

## Session Opening (8 min)

**[Script:]** "Last time you read one row with the ORM. Today we **write**. CREATE and READ must hit Neon. Then we add **one** UPDATE or DELETE. Finally we **kill the server** and prove the row is still there. That restart test is the difference between a demo and a backend."

**Real-world hook:** Restart a student's old in-memory API on screen. Data gone. "Would **PhonePe** ship that wallet?"

🎯 **Instructor Note:** Reuse the same `Note` (or `Task`) model from the ORM session. Do not introduce a second table.

---

## Why Does This Matter?

🎯 **Instructor Note:** Poll — "Who still has a Python list as the database?" Pair them with someone on Neon.

**[Script:]** "When **Zomato** marks an order placed, that write must survive a deploy at 2 AM. ORM sessions, `commit`, and status codes are how junior backend tickets look in the first month. Swagger is how you show a reviewer the API before frontend exists."

**Pain if misunderstood:**
- Forget `commit` — Swagger looks fine, Neon empty
- One global session — weird bugs under two requests
- Skip 404 — UI shows crashes instead of “not found”

> **In the Real World:** **Shopify** admin APIs are boring on purpose: create resource, list resource, update or delete. You are practising that boring, hireable shape.

---

## What Is the Concept?

**Session in a route:** borrow `db`, use it, give it back.

**Unit of work:** changes sit in memory until `commit`.

**Python vs JS:** Express + Prisma `await prisma.note.create` is the same story as `db.add` + `commit`.

**Common mistakes:** Returning the object before commit; mixing in-memory dict with ORM.

---

## How Do We Apply It?

### LO 1: Wire FastAPI routes to ORM sessions

**Problem:** Routes need `db` without copy-pasting engine code.

**Translate logic:** `get_db` generator + `Depends`.

```python
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

**Predict before running:** After the response, is the session still open? (No — `finally` closes it.)

**Explain result:** Each HTTP request gets a clean session.

---

### LO 2: Implement persistent CREATE and READ

**Problem:** POST a title, GET it back by id and as a list.

**Write code:**

```python
@app.get("/notes")
def list_notes(db: Session = Depends(get_db)):
    return db.scalars(select(Note)).all()
```

**Predict before running:** Empty table — what JSON? (`[]`)

**Walkthrough POST:** body → `Note` → `add` → `commit` → `refresh` → 201.

> **In the Real World:** **Notion** “New page” is CREATE. Refreshing the sidebar is READ. Same pair.

🎯 **Instructor Note:** Show SQL in Neon after POST. Make the round-trip visible.

---

### LO 3: Add one UPDATE or DELETE against the database

**Problem:** Pick one verb as a class. Recommended: DELETE for fewer fields, or PATCH title.

```python
@app.delete("/notes/{note_id}")
def delete_note(note_id: int, db: Session = Depends(get_db)):
    note = db.get(Note, note_id)
    if not note:
        raise HTTPException(404, "Not found")
    db.delete(note)
    db.commit()
```

**Predict before running:** Second DELETE of same id? (404)

**Explain result:** Row gone in Neon. List GET shrinks.

---

### LO 4: Verify data remains after server restart

**Problem:** Students do not trust persistence until they see it.

**Live ritual:** POST → stop process → start → GET same id.

**Predict before running:** Will id stay the same? (Yes, serial PK.)

**Explain result:** Postgres, not Uvicorn, owns the data.

---

### LO 5: Test the flow in Swagger UI

**Problem:** Frontend is not the first test tool.

**Walkthrough:** `/docs` → Try it out → POST → GET list → DELETE or PATCH → GET 404.

**[Script:]** "If Swagger fails, do not debug React. Fix the API first."

---

## Live Lab (8 min)

Pairs run the restart ritual and screenshot Neon + Swagger.

---

## Recap (5 min)

**[Script:]** "Next: **Pytest** so you do not click Swagger for every change."

---

## Lecture Summary

- **ORM sessions** are injected into routes and closed per request
- **Persistent CREATE and READ** write and load Neon rows
- **One UPDATE or DELETE** completes a minimal mutation set
- **Restart check** proves the database is the source of truth
- **Swagger** is the first customer of your API
- **Practical value:** You can demo a backend that keeps user data after crash or redeploy
