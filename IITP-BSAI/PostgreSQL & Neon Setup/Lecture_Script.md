# Lecture Script: PostgreSQL & Neon Setup
**Duration:** 110 minutes | **Tools:** Browser, Neon console, notes | **Language:** SQL (PostgreSQL)

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Restart deleted the demo |
| Why Does This Matter? | 12 min | Persistence, deploys, audits |
| What Is the Concept? | 22 min | Tables, types, PK, Neon |
| How Do We Apply It? (LOs) | 53 min | Five LOs live |
| Live lab | 8 min | Create + inspect `items` |
| Recap | 7 min | Queries next; ORM later |

---

## Session Opening (8 min)

**[Script:]** "We posted items from React. Then we restarted Uvicorn and the list reset. That is not a bug in FastAPI. **RAM is not a database.** Today we create **PostgreSQL** on **Neon**, run **CREATE TABLE**, set a **primary key**, and **inspect** the table in the console. We are not wiring FastAPI to Neon yet."

**Problem hook:** Imagine deploy every Friday. If orders live in memory, Friday wipes Saturday's sales.

🎯 **Instructor Note:** Accounts and MFA will eat time. Have a backup shared read-only project if a student is locked out. No ORM today.

---

## Why Does This Matter?

🎯 **Instructor Note:** Poll: "Who still has last week's POSTed items in the API?" Expect zero. That is the hook.

**[Script:]** "Companies store users, payments, and grades in databases because servers restart, scale out, and crash. If you skip a primary key, you cannot point at 'that row' in an update. If you pick TEXT for qty, sums become painful later."

> **In the Real World:** Banks and campus ERPs survive app restarts. Neon is how we get PostgreSQL without installing Postgres on every laptop today.

**Pain if misunderstood:**

- "I'll just write a JSON file" — fine for a toy, not the course path
- No primary key — duplicate identity
- Creating tables in Python RAM again and calling it done

| In-memory list | PostgreSQL table |
|----------------|------------------|
| Dies on restart | Persists |
| Types are hints | Column types |
| Index in a list | Primary key `id` |

---

## What Is the Concept?

**Persistent database:** data independent of the API process.

**Neon project:** hosted Postgres instance + console.

**CREATE TABLE:** name the grid and columns.

**Column types:** INTEGER, TEXT, BOOLEAN.

**Primary key:** unique row id.

**Inspect:** UI proof the table exists.

🎯 **Instructor Note:** Draw one table as a grid on the board before SQL.

---

## How Do We Apply It?

### LO 1: Explain why apps need a persistent database

**Problem:** Product data disappeared after a restart.

**Translate logic:** API process ≠ storage. Storage must outlive the process.

**Write code:** None. Board comparison table (memory vs database).

**Predict before running: What will happen?**

**Predict:** After restart, GET list is empty starter data only.

**Explain result:** Students felt this last session. Database is the designed fix.

---

### LO 2: Create a Neon PostgreSQL project

**Problem:** No server on the laptop to hold tables.

**Translate logic:** Neon provisions Postgres in the cloud. One project per class lab.

**Write code:** Click path: New project → region → create. (No application code.)

**Predict before running: What will happen?**

**Predict:** Dashboard shows a database ready. SQL editor opens.

**Explain result:** You now have a real PostgreSQL endpoint. FastAPI will use it in a later session.

🎯 **Instructor Note:** Help password/connection string copy into a note — they will need it later. Do not paste secrets into Git.

---

### LO 3: Create a table with appropriate column types

**Problem:** Need a place for items: id, name, qty.

**Translate logic:** INTEGER for id and qty. TEXT for name.

**Write code:**

```sql
CREATE TABLE items (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  qty INTEGER
);
```

**Predict before running: What will happen?**

**Predict:** Success message. Table `items` exists.

**Explain result:** Columns are typed. `NOT NULL` protects empty names.

---

### LO 4: Define a primary key

**Problem:** GET by id must mean one row.

**Translate logic:** `id INTEGER PRIMARY KEY` — unique, identifies the row.

**Write code:** Same `CREATE TABLE` — highlight PRIMARY KEY. Optionally fail a second create with duplicate id if you INSERT twice.

**Predict before running: What will happen?**

**Predict:** Duplicate `id` insert is rejected by Postgres.

**Explain result:** The key is a database rule, not a Python habit.

🎯 **Instructor Note:** If INSERT is extra, keep it to one duplicate demo.

---

### LO 5: Inspect the table in the Neon console

**Problem:** Students do not trust SQL they cannot see.

**Translate logic:** Tables UI or schema view. Read column list and PK badge.

**Write code:** None — click inspect.

**Predict before running: What will happen?**

**Predict:** `items` visible with `id`, `name`, `qty`.

**Explain result:** Console is the source of truth. Use it when FastAPI and SQL disagree later.

---

## Live Lab (8 min)

Every student: project exists, `items` (or `tasks`) created, screenshot of table schema.

> **In the Real World:** First day on a data team: open the console, confirm the table, then query. You practised the first two steps.

---

## Recap (7 min)

🎯 **Instructor Note:** "Does FastAPI already read Neon?" Not yet. "What did PRIMARY KEY buy us?" Unique id.

**[Script:]** "You explained persistence, created Neon, typed a table, set a primary key, and inspected it. Next lab is FastAPI CRUD practice. Then SQL SELECT and JOIN on this data."

---

## Lecture Summary

- **Why a database:** persist beyond process restart
- **Neon project:** hosted PostgreSQL
- **CREATE TABLE** with INTEGER / TEXT (and BOOLEAN if used)
- **Primary key** uniquely identifies rows
- **Neon console** confirms schema
- **Practical value:** Storage is now real; queries and ORM come next
