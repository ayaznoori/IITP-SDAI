# Lecture Script: Masterclass — Database Transactions & ACID
**Duration:** 110 minutes | **Format:** Professor-led conceptual masterclass | **Tools:** Whiteboard, optional SQLite demo of BEGIN/COMMIT/ROLLBACK only

**Agenda:** Opening 10 · Why 15 · Concepts 25 · LO walkthroughs 40 · Guided thought demo 10 · Recap 10

**Tone:** Fewer recipes. More systems thinking. Connect to SQL I–II. Preview ORM `commit` without teaching SQLAlchemy APIs in depth.

---

## Session Opening (10 min)

**[Script:]** "You can UPDATE one row. Industry does not pay you for one row. Industry pays you to move **money, seats, and stock** as a single truth. Today is not extra syntax. Today is why databases are more than spreadsheets."

**Problem hook:** Narrate a transfer: debit Asha, crash, credit Ravi never runs. Ask: "Who is poorer? Who is richer? Who is the bank?" Let silence sit.

🎯 **Instructor Note:** No laptops required for first 25 minutes. Phones down. This is a reasoning class.

---

## Why Does This Matter?

🎯 **Instructor Note:** Poll: "Have you ever seen 'payment taken, order not placed'?" Collect hands. That is missing atomicity from the user's view.

**[Script:]** "If you treat the database as a dumb file, you will ship FastAPI routes that INSERT then UPDATE and hope. Hope is not a strategy. **UPI**, **NSE**, **airline PNR** systems exist because transactions exist. Misunderstand ACID and you will debug 'impossible' balances for weeks. Understand it and ORM `commit` will stop looking like magic."

- **Real-world use:** Payments, inventory, ticketing, user+profile signup
- **Pain if misunderstood:** Partial writes, double spend of last item, "success" JSON after data that vanished on crash (durability)

---

## What Is the Concept?

### Multi-step updates without a transaction

Several statements, each auto-committed, are **independent**. Failure in the middle leaves a **partial world**.

**Demerit:** Business invariant broken (money conserved, seat unique) even though each statement was "valid SQL."

### Why databases need transactions

The engine must offer a **unit of work** matching a **business operation**, not a single line of SQL.

### Commit and rollback

- **BEGIN** — start the unit (SQLite: `BEGIN;`)
- **COMMIT** — persist the unit
- **ROLLBACK** — discard the unit

**Mental model:** Exam answer sheet in pencil until you submit (commit). Walk out early and tear it (rollback). Invigilator never grades a half-erased sheet as final.

### ACID

Teach with one story per letter. Avoid vendor-specific isolation catalogs.

**Atomicity** — all or nothing.  
**Consistency** — constraints and invariants hold after commit.  
**Isolation** — concurrent transactions do not produce nonsense combinations.  
**Durability** — committed data survives process death.

**Common conceptual mistakes:** Thinking rollback undoes someone else's committed work; thinking 200 OK from FastAPI means committed; thinking JSON files are ACID.

**Python vs DB:** A Python `try/except` can skip the second line. Without a DB transaction, the first INSERT may already be durable. Exception handling ≠ atomicity.

---

## How Do We Apply It?

(Application here is **reasoning + tiny SQL illustration**, not a new framework.)

### LO 1: Demerit of multi-step updates without protection

**Problem:** Debit then credit as two auto-committed UPDATEs.

**Translate logic:** Two commits = two independent worlds.

**Whiteboard:**

```text
UPDATE accounts SET balance = balance - 500 WHERE id = 1;  -- committed
-- crash
UPDATE accounts SET balance = balance + 500 WHERE id = 2;  -- never ran
```

**Predict:** Asha -500, Ravi +0. Invariant "total money" broken.

**Explain result:** Each statement succeeded or never ran; the **business** failed.

---

### LO 2: Why databases need transactions

**Problem:** Map "transfer" to one unit.

**Translate logic:** BEGIN … both UPDATEs … COMMIT.

**Walkthrough:** Same two UPDATEs inside one transaction. Crash before commit → rollback path → both balances original.

**Predict:** After crash, SELECT shows original balances.

**Explain result:** The DB is the coordinator. The API must use that coordinator.

---

### LO 3: Commit and rollback concepts

**Optional demo (≤10 lines of SQL):**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 500 WHERE id = 1;
UPDATE accounts SET balance = balance + 500 WHERE id = 2;
ROLLBACK;
SELECT id, balance FROM accounts;
```

**Predict before running:** Balances unchanged after ROLLBACK.

Then repeat with COMMIT. Predict: balances changed and stay changed after reconnect.

**Explain result:** Commit = success handling. Rollback = failure handling.

🎯 **Instructor Note:** If no SQLite, act it with two sticky notes "pending" vs "stamped."

---

### LO 4: ACID properties and why each matters

**Story board (five minutes per letter if needed, keep total in LO block):**

| Property | Failure if missing | Real-life sting |
|----------|--------------------|-----------------|
| Atomicity | Half transfer | Customer rage + legal risk |
| Consistency | Negative stock, broken FK | Reports lie |
| Isolation | Two users, one last seat | Double booking |
| Durability | Commit then power cut | "We charged you" but row gone |

**Predict:** Isolation does not mean "one user at a time globally." It means **controlled** interleaving. Keep that one sentence; do not open isolation-level charts.

---

### LO 5: Why APIs and ORMs rely on transactions

**Problem:** FastAPI POST `/register` creates `users` row and `profiles` row.

**Translate logic:** Two INSERTs, one HTTP request, one transaction.

**Walkthrough:** ORM session = typically one transaction boundary. Success → `commit()`. Exception → `rollback()`. Client gets 500 or 400; DB not half-updated.

**Predict:** If profile INSERT fails and you forgot rollback, user row may remain (depending on autocommit). That is why frameworks wire sessions for you.

**Explain result:** You will not "invent" ACID in Python. You will **use** it.

**[Script:]** "When you see `db.commit()` next session, hear this lecture. It is not save-for-fun. It is the end of a transaction."

---

## Guided Thought Demo (10 min)

Walk **IRCTC-like** three writes: wallet, seat, ticket. Ask at each crash point: atomic? durable? Then contrast "log in a text file after commit" (app concern) vs DB durability.

No new libraries. No distributed transactions (2PC). Stay inside LOs.

---

## Recap (10 min)

🎯 **Instructor Note:** Students define ACID in their own words, one letter each, around the room.

**[Script:]** "You are not junior because you use an ORM. You are junior if you `commit` without knowing what you just promised."

---

## Lecture Summary

- **Unprotected multi-step updates** can leave broken business state
- **Transactions** exist so one business operation is one database unit
- **Commit** makes the unit permanent; **rollback** discards it on failure
- **ACID** explains all-or-nothing, valid state, safe concurrency, and crash survival
- **APIs and ORMs** wrap routes in transactions so FastAPI writes stay consistent
- **Practical value:** You can judge whether a backend path is safe before you touch SQLAlchemy — and you will recognize `commit`/`rollback` as ACID, not boilerplate
