# Lecture Script: ORM, Neon Connection & Schema Design
**Duration:** 110 minutes | **Tools:** VS Code, FastAPI, Neon console, SQLAlchemy | **Language:** Python

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | In-memory data dies on restart |
| Why Does This Matter? | 12 min | Industry ORM + hosted Postgres |
| What Is the Concept? | 25 min | ORM, model, URL, session, sketch |
| How Do We Apply It? (LOs) | 55 min | One model, Neon connect, GET read |
| Live lab | 5 min | Students hit GET in Swagger |
| Recap | 5 min | Preview persistent CRUD |

---

## Session Opening (8 min)

**[Script:]** "Your FastAPI CRUD still lives in a Python list. Restart Uvicorn and the data is gone. Last sessions you put tables on **Neon**. Today we connect FastAPI to that database the way industry backends do it: an **ORM**, one **SQLAlchemy model**, a **database URL**, and one real **GET** that reads a row."

**Real-world hook:** Show a Neon dashboard with a `notes` table, then show empty in-memory list after restart. "Products like **Linear** and **Notion** cannot afford RAM as the source of truth."

🎯 **Instructor Note:** Confirm every student has a Neon project and `DATABASE_URL` from PostgreSQL & Neon Setup. Do not paste URLs into Slack.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask: "Who lost API data after restart?" Hands up. That pain is today's hook.

**[Script:]** "At **Swiggy**, restaurant menus are not stored in a FastAPI list. They live in PostgreSQL. Engineers map tables to Python classes with an ORM so routes stay readable. If you only know raw SQL strings inside routes, you will struggle in code review. If you only know ORM and cannot read the SQL it emits, you will struggle in production incidents. Today you get both: you already wrote SQL; now you add SQLAlchemy as the translator."

**Real-world use:**

| Team | Pattern |
|------|---------|
| Early startup | SQLAlchemy + Neon/Supabase URL |
| **Instagram** (historical) | Django ORM on Postgres |
| Many FastAPI shops | SQLAlchemy 2.x + Pydantic schemas |

**Pain if misunderstood:**
- Committing `DATABASE_URL` with the password — leaked Neon project
- One giant model for the whole product — unreadable schema
- Skipping the paper sketch — wrong columns shipped to production

---

## What Is the Concept?

### ORM vs Raw SQL

**Definition:** An **ORM** maps Python classes to tables and objects to rows.

**Mental model:** Class `Note` = table `notes`. Instance = one row.

**Python vs JavaScript (when relevant):** Django/SQLAlchemy are Python. Node teams use Prisma or Drizzle. Same idea: objects ↔ tables.

**Common mistake:** Thinking the ORM replaces SQL forever. You still use SQL for joins and EXPLAIN.

### Engine, Session, Model

```
DATABASE_URL → create_engine → Session → Note model → row
```

**Session** is a short conversation. Open, query, close. Do not keep one global session forever in beginners' code without a dependency.

### Schema Sketch

Boxes first. Code second. One table today.

---

## How Do We Apply It?

### LO 1: Explain why ORM helps backend development

**Problem:** Five routes each paste slightly different SQL. A column rename breaks three of them.

**Translate logic:** One model owns column names. Routes use attributes.

**Walkthrough:** Whiteboard `SELECT title FROM notes` vs `note.title`.

**[Script:]** "ORM help is not magic speed. It is consistency and less copy-paste SQL in Python."

**Predict before running:** If we rename a column only in SQL and not in the model, will the GET still work? (No — mapping must match.)

---

### LO 2: Define a SQLAlchemy model for one table

**Problem:** Represent `notes(id, title)` in Python.

**Write code (demo, keep short):**

```python
class Note(Base):
    __tablename__ = "notes"
    id: Mapped[int] = mapped_column(primary_key=True)
    title: Mapped[str] = mapped_column(String(100))
```

**Predict before running:** What SQL type is `id`? (Integer primary key.)

**Explain result:** SQLAlchemy can emit `CREATE TABLE` or bind to a table you already created on Neon.

🎯 **Instructor Note:** Prefer matching a table students already created in Neon. Avoid Alembic in this session — out of LO.

> **In the Real World:** **Stripe** internal services still start many features with one core entity table before adding relations.

---

### LO 3: Connect FastAPI to Neon with a database URL

**Problem:** FastAPI must reach hosted Postgres, not a list.

**Translate logic:** Read `os.getenv("DATABASE_URL")` → `create_engine`.

```python
engine = create_engine(os.getenv("DATABASE_URL"))
```

**Predict before running:** If `DATABASE_URL` is missing, what happens? (Error at startup — good, fail fast.)

**Demo:** `.env` locally (gitignored) vs Neon connection string. Show `sslmode` if Neon requires it.

**[Script:]** "Neon is just Postgres on the internet. The URL is the handshake."

🎯 **Instructor Note:** If SSL errors appear, add the Neon-documented query param. Do not invent extra cloud providers.

---

### LO 4: Sketch a simple schema for a sample app

**Problem:** Students jump to five tables.

**Walkthrough:** 4-minute paper sketch — `notes` only: `id` PK, `title` text.

**Predict:** Do we need a `users` table today? (No — stay inside one-table LO.)

**Explain result:** A schema sketch is a product decision, not decoration.

> **In the Real World:** **Airbnb** listing pages sit on many tables. Interns still sketch one entity first in design docs.

---

### LO 5: Run one ORM read through a FastAPI GET route

**Problem:** Prove the wire works: browser → API → Neon → JSON.

**Write code:**

```python
@app.get("/notes/{note_id}")
def read_note(note_id: int, db: Session = Depends(get_db)):
    note = db.get(Note, note_id)
    return {"id": note.id, "title": note.title}
```

**Predict before running:** What if `note_id` does not exist? (Attribute error unless we check — mention 404 as next-session hygiene, keep GET success path today.)

**Live demo:** Insert one row in Neon console. Hit GET in Swagger. Show JSON.

**Explain result:** Restart Uvicorn. Hit GET again. Row still there. That is the win versus in-memory lists.

---

## Live Lab (5 min)

Students: insert a row in Neon, call GET, screenshot Swagger + Neon table.

---

## Recap (5 min)

**[Script:]** "Next session we persist CREATE and READ fully, plus one UPDATE or DELETE. Same session and model. More verbs."

---

## Lecture Summary

- **ORM** lets backend code use Python objects instead of pasted SQL in every route
- **SQLAlchemy model** maps one class to one table with a primary key
- **Database URL** is how FastAPI reaches **Neon** Postgres
- **Schema sketch** comes before extra tables and extra columns
- **GET via ORM** proves the app can read a real row
- **Practical value:** Your API can survive a server restart because the source of truth is the database
