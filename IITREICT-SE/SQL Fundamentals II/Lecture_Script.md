# Lecture Script: SQL Fundamentals II
**Duration:** 110 minutes | **Tools:** SQLite + last session `campus.db` | **Language:** SQL

**Agenda:** Opening 7 · Why 12 · Concepts 18 · LO walkthroughs 50 · Live demo 13 · Recap 10

---

## Session Opening (7 min)

**[Script:]** "Yesterday we created and inserted. Today we change and remove — with **WHERE** glued on. Then we name relationships so your next ORM models are not a guess."

**Problem:** Intern runs `DELETE FROM students;` to "clear a test user." Production empty. Horror story, then we practice the seatbelt.

---

## Why Does This Matter?

🎯 **Instructor Note:** Run SELECT COUNT before a demo UPDATE. After missing WHERE, COUNT still 3 but all names identical. Pause.

**[Script:]** "SQL interviews always ask WHERE. **CRED**, **Razorpay**, **IRCTC** — one wrong UPDATE is a war room. Relationship types are how you design **Instagram** followers (many-to-many) versus a passport (one-to-one)."

- **Real-world use:** Address corrections, cancel order, deactivate user
- **Pain if misunderstood:** Table wipes; treating many-to-many as a single FK

---

## What Is the Concept?

**UPDATE** changes columns. **DELETE** removes rows. **WHERE** restricts.

**1-1 / 1-many / many-many** describe how entities link.

**Mental model:** UPDATE is edit cell. DELETE is remove row. WHERE is the filter in Excel.

**Common mistakes:** UPDATE without WHERE; DELETE then "undo" (there is no undo without backup/transaction); stuffing many clubs into a comma-separated TEXT column.

---

## How Do We Apply It?

Use seed data from SQL I.

### LO 1: Write UPDATE

**Write code:**

```sql
UPDATE students SET name = 'Asha Rao' WHERE id = 1;
SELECT id, name FROM students WHERE id = 1;
```

**Predict before running:** Only id 1 changes.

**Explain result:** SET names columns. WHERE picks rows.

---

### LO 2: DELETE with WHERE

**Write code:**

```sql
SELECT * FROM students WHERE id = 3;
DELETE FROM students WHERE id = 3;
SELECT * FROM students WHERE id = 3;
```

**Predict before running:** Second SELECT empty.

**Explain result:** WHERE is mandatory in class policy even when SQL allows skip.

---

### LO 3: Filter SELECT with WHERE

**Write code:**

```sql
SELECT name FROM students WHERE club_id = 1;
SELECT name FROM clubs WHERE name = 'Robotics';
```

**Predict before running:** Only matching rows.

**Explain result:** Same WHERE idea as UPDATE/DELETE, read-only.

---

### LO 4: Explain 1-1, 1-many, many-many

**Walkthrough board:**

- 1-1: `id_cards.student_id UNIQUE`
- 1-many: `students.club_id` → `clubs.id`
- many-many: `memberships` two FKs

**Predict:** Can Asha join Robotics and Drama with only `club_id` on students? No — need link table.

**Explain result:** Schema follows relationship type.

---

### LO 5: Combine WHERE with UPDATE and DELETE safely

**Problem:** Rename one club; remove one membership.

**Translate logic:** Identify keys with SELECT. Mutate with same WHERE.

**Write code:**

```sql
SELECT id FROM clubs WHERE name = 'Drama';
UPDATE clubs SET name = 'Theatre' WHERE id = 2;
DELETE FROM memberships WHERE student_id = 1 AND club_id = 2;
```

**Predict before running:** Other clubs untouched.

**Demo:** Show `EXPLAIN`? Skip — stay beginner. Show COUNT before/after.

🎯 **Instructor Note:** "If WHERE matches 0 rows, UPDATE succeeds with 0 changes — not always an error. Check counts."

---

## Live Demo Block (13 min)

Seed 1-many data. Add `memberships` for many-many. UPDATE one name. DELETE one membership. Attempt DELETE FROM clubs WHERE id=1 while students still point — discuss FK behavior.

**[Script:]** "SELECT, then mutate. Say it with me."

---

## Recap (10 min)

🎯 **Instructor Note:** Draw three relationship icons. Students shout examples.

---

## Lecture Summary

- **UPDATE** modifies existing records
- **DELETE ... WHERE** removes targeted rows
- **SELECT ... WHERE** filters reads
- **1-1, 1-many, many-many** drive table design (including link tables)
- **WHERE on writes** is the safe habit for production SQL
- **Practical value:** You can change data without destroying it — next masterclass explains transactions when one business action is several SQL steps
