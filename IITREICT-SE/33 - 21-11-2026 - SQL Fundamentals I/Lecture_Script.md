# Lecture Script: SQL Fundamentals I
**Duration:** 110 minutes | **Tools:** SQLite (CLI or sqliteonline.com), VS Code | **Data:** Campus clubs schema

**Agenda:** Opening 8 · Why 12 · Concepts 18 · LO walkthroughs 50 · Live demo 12 · Recap 10

---

## Session Opening (8 min)

**[Script:]** "Our API is a clever goldfish. Restart, memory gone. Today we put data in **tables**. SQL is the language every backend job still asks for."

**Problem:** Demo POST three books. Kill Uvicorn. GET empty. Class groans. "That is why databases exist."

---

## Why Does This Matter?

🎯 **Instructor Note:** Show a simplified **UPI** ledger: two tables, ids, no duplicated names everywhere.

**[Script:]** "If you only know Python lists, you cannot reason about **HDFC** accounts or **Swiggy** orders. Relational DBs give durable storage and **constraints** so fake emails and orphan rows fail at insert time, not at 2 a.m. in production."

- **Real-world use:** Inventory, tickets, users, orders
- **Pain if misunderstood:** No PK → duplicate chaos; no FK → orders for deleted users

---

## What Is the Concept?

**Relational DB:** data in tables with typed columns and keys.

**PK:** unique row id. **FK:** must match a PK (or be NULL if allowed).

**SELECT** reads. **INSERT** adds. **CREATE TABLE** defines schema.

**Mental model:** Labeled cabinets. PK is the folder number. FK is a sticky note "see cabinet Clubs, folder 1."

**Common mistakes:** Selecting `*` blindly in production habits (ok in class, warn); INSERT without listing columns; TEXT for everything with no UNIQUE on email.

Stay in SQL this session — no ORM.

---

## How Do We Apply It?

### LO 1: Explain why relational databases are used

**Problem:** Shared, durable, structured data.

**Translate logic:** Disk + engine + constraints > JSON file + hope.

**Walkthrough:** One API instance vs two. Lists do not sync. DB does.

**Predict:** After "crash," SELECT still shows INSERTs. (Commit in SQLite file.)

---

### LO 2: Design tables with PK, FK, constraints

**Write code:**

```sql
CREATE TABLE clubs (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL UNIQUE
);
CREATE TABLE students (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT NOT NULL UNIQUE,
  club_id INTEGER,
  FOREIGN KEY (club_id) REFERENCES clubs(id)
);
```

**Predict before running:** INSERT student with `club_id=99` fails if FK enforced.

**Explain result:** Constraints protect the schema.

---

### LO 3: Write SELECT

**Write code:**

```sql
SELECT id, name FROM clubs;
SELECT name, email FROM students;
```

**Predict before running:** Empty tables → empty result, not an error.

**Explain result:** SELECT is safe read. Column list is explicit.

---

### LO 4: Write INSERT

**Write code:**

```sql
INSERT INTO clubs (id, name) VALUES (1, 'Robotics');
INSERT INTO students (id, name, email, club_id)
VALUES (1, 'Asha', 'asha@campus.edu', 1);
```

**Predict before running:** Second INSERT with same email → UNIQUE fail.

**Explain result:** Constraints fire on write.

---

### LO 5: Simple schema for a sample app

**Problem:** Mini "campus clubs" product.

**Walkthrough:** Entities: Club, Student. Fields. Keys. One FK.

**Demo:** Run CREATE, INSERT, SELECT in one file `schema.sql`.

**Predict:** `SELECT name FROM clubs;` → `Robotics`.

🎯 **Instructor Note:** Students sketch on paper first, then type SQL.

---

## Live Demo Block (12 min)

Create SQLite file `campus.db`. Run schema. Insert two clubs, three students. SELECT all students. Show UNIQUE email failure.

**[Script:]** "This file is the source of truth. FastAPI will talk to it through an ORM soon. First you must speak SQL."

---

## Recap (10 min)

🎯 **Instructor Note:** "PK of students? FK? Why UNIQUE email?"

---

## Lecture Summary

- **Relational DBs** give durable, shared, constrained data for backends
- **Tables** use **PK, FK, and constraints** to protect shape
- **SELECT** retrieves columns and rows
- **INSERT** adds records
- A **sample schema** (clubs + students) models a small app
- **Practical value:** You can design storage before wiring FastAPI — next: UPDATE, DELETE, WHERE, and relationship types
