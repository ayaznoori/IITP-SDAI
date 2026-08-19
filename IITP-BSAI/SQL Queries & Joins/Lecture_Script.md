# Lecture Script: SQL Queries & Joins
**Duration:** 110 minutes | **Tools:** Neon SQL editor | **Language:** SQL (PostgreSQL)

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Tables exist; now ask them |
| Why Does This Matter? | 12 min | Interviews, ERPs, safety of WHERE |
| What Is the Concept? | 22 min | CRUD SQL, FK, INNER JOIN |
| How Do We Apply It? (LOs) | 53 min | Five LOs on Neon |
| Live lab | 8 min | Two-table JOIN |
| Recap | 7 min | ORM next |

---

## Session Opening (8 min)

**[Script:]** "Neon holds a table. FastAPI does not magically search it yet. You will. Today: **SELECT INSERT UPDATE DELETE**, **WHERE** and **ORDER BY**, the idea of a **foreign key**, one **INNER JOIN**, all on **sample Neon data**. No SQLAlchemy today. No FastAPI routes required."

**Problem hook:** Product question: "Who ordered the lab kit?" That is two tables. JOIN, not a Python double loop — for this course we learn the SQL shape first.

🎯 **Instructor Note:** Seed `students` and `orders` (or `items` + `item_notes`) before class. Paste seed SQL in chat. Warn: DELETE without WHERE.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show a DELETE without WHERE on a copy table (or talk through it). Students should look uncomfortable.

**[Script:]** "If you only know Python lists, you will pull the whole database into memory. SQL pushes filter and sort to Postgres. Foreign keys stop orphan orders. JOIN is how reports are born."

> **In the Real World:** Support tools and Metabase dashboards are SQL. Backend ORMs still emit JOIN. You will read that SQL in logs.

**Pain if misunderstood:**

- UPDATE all rows to the same qty
- JOIN without ON — a messy product (cartesian surprise) — keep ON mandatory
- Explaining FK as "a primary key on the second table" (wrong)

| SQL | Everyday |
|-----|----------|
| WHERE | Filter |
| ORDER BY | Sort |
| FK | Stamp linking two forms |
| INNER JOIN | Matched pairs only |

---

## What Is the Concept?

**SELECT** reads. **INSERT** adds. **UPDATE** changes. **DELETE** removes.

**WHERE** filters. **ORDER BY** sorts.

**Foreign key:** column referencing another table's PK.

**INNER JOIN:** rows with a match on the join condition.

🎯 **Instructor Note:** Draw two tables and a line from `orders.student_id` to `students.id` before any JOIN syntax.

---

## How Do We Apply It?

### LO 1: Write SELECT, INSERT, UPDATE, and DELETE statements

**Problem:** Need to read and change sample rows by hand.

**Translate logic:** Four statements on `students`.

**Write code:**

```sql
SELECT id, name FROM students;
INSERT INTO students (id, name) VALUES (3, 'Asha');
UPDATE students SET name = 'Asha K' WHERE id = 3;
DELETE FROM students WHERE id = 3;
```

**Predict before running: What will happen?**

**Predict:** SELECT lists rows. INSERT adds. UPDATE renames id 3. DELETE removes id 3.

**Explain result:** Each verb is explicit. WHERE on UPDATE/DELETE scoped the change.

🎯 **Instructor Note:** After INSERT, SELECT before UPDATE so they see the row.

---

### LO 2: Filter and sort with WHERE and ORDER BY

**Problem:** Too many rows to scan by eye.

**Translate logic:** Keep year = 2. Sort by name.

**Write code:**

```sql
SELECT name, year
FROM students
WHERE year = 2
ORDER BY name;
```

**Predict before running: What will happen?**

**Predict:** Only year-2 names, alphabetical.

**Explain result:** Postgres filtered and sorted. The client gets a small result.

---

### LO 3: Explain a foreign key relationship between two tables

**Problem:** Orders must belong to a real student.

**Translate logic:** `orders.student_id` references `students.id`.

**Write code:** (schema already seeded)

```sql
-- orders.student_id  -->  students.id
SELECT * FROM orders;
```

**Predict before running: What will happen?**

**Predict:** Every `student_id` in orders exists in students (if constraint holds).

**Explain result:** FK is a relationship rule, not a JOIN by itself. JOIN *uses* that link.

🎯 **Instructor Note:** Optional: INSERT order with fake student_id and show the error if FK is defined.

---

### LO 4: Write one INNER JOIN for a two-table query

**Problem:** Need student name next to item name.

**Translate logic:** INNER JOIN ON the FK = PK.

**Write code:**

```sql
SELECT students.name, orders.item_name
FROM students
INNER JOIN orders ON students.id = orders.student_id;
```

**Predict before running: What will happen?**

**Predict:** One result row per matching order. Students without orders absent.

**Explain result:** INNER = intersection of keys. That is the only JOIN type today.

---

### LO 5: Practise the queries on sample Neon data

**Problem:** Paper SQL is not muscle memory.

**Translate logic:** Re-run SELECT/WHERE/JOIN on the shared seed. Change one INSERT. SELECT again.

**Write code:** Students' own variants in Neon — same verbs, their WHERE.

**Predict before running: What will happen?**

**Predict:** Neon grid updates. Refresh SELECT to verify.

**Explain result:** The console is the lab. Save working SQL in a notes file (no secrets).

---

## Live Lab (8 min)

Pair: one JOIN screenshot plus one WHERE/ORDER BY.

> **In the Real World:** First analytics request is "list X for Y." That is JOIN + WHERE.

---

## Recap (7 min)

🎯 **Instructor Note:** "INNER JOIN drops unmatched?" Yes. "FK vs JOIN?" FK is the rule; JOIN is the query.

**[Script:]** "You wrote SQL CRUD, filtered and sorted, explained foreign keys, ran one INNER JOIN, and practised on Neon. Next: ORM connects FastAPI to this database so Python does the talking."

---

## Lecture Summary

- **SELECT / INSERT / UPDATE / DELETE** change and read tables
- **WHERE** filters; **ORDER BY** sorts
- **Foreign key** links a column to another table's PK
- **INNER JOIN** returns matched rows only
- **Neon practice** makes the syntax real
- **Practical value:** You can inspect and shape data before the ORM hides SQL
