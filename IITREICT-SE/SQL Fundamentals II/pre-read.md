# Pre-Read: SQL Fundamentals II

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **UPDATE** existing rows
- How to **DELETE with WHERE** so you do not wipe a table
- How to **filter SELECT with WHERE**
- What **one-to-one**, **one-to-many**, and **many-to-many** mean
- How to **combine WHERE with UPDATE and DELETE** for safe changes

---

## 2. Detailed Explanation

### WHERE Is a Seatbelt

`UPDATE students SET name = 'X'` without WHERE renames **everyone**. `DELETE FROM students` without WHERE deletes **everyone**.

**WHERE** limits which rows match.

**Analogy:** A highlighter on a register. "Only roll 14." Never "all pages."

> **In the Real World:** A support engineer at **Flipkart** fixing one order's address must UPDATE that `order_id`. A missing WHERE is a legendary outage story.

**Why It Matters**

- Safe mutations
- SELECT becomes useful (not the whole table every time)
- Relationships explain how tables join in the next sessions

### SELECT ... WHERE

```sql
SELECT name, email FROM students WHERE club_id = 1;
SELECT name FROM students WHERE email LIKE '%@campus.edu';
```

### UPDATE with WHERE

```sql
UPDATE students
SET name = 'Asha Rao'
WHERE id = 1;
```

Always show the SELECT first: "which rows will this touch?" Then UPDATE.

### DELETE with WHERE

```sql
DELETE FROM students WHERE id = 1;
```

**Safe habit:** `SELECT * FROM students WHERE id = 1;` then delete.

### Relationship Types

| Type | Meaning | Campus example |
|------|---------|----------------|
| **One-to-one** | A row in A matches at most one row in B | One student, one `id_cards` row (`student_id` UNIQUE FK) |
| **One-to-many** | One A, many B | One club, many students (`students.club_id`) |
| **Many-to-many** | Many A, many B | Students in many clubs — needs a **link table** `memberships(student_id, club_id)` |

```sql
CREATE TABLE memberships (
  student_id INTEGER,
  club_id INTEGER,
  PRIMARY KEY (student_id, club_id),
  FOREIGN KEY (student_id) REFERENCES students(id),
  FOREIGN KEY (club_id) REFERENCES clubs(id)
);
```

Many-to-many is two one-to-many relationships through the middle table.

### Messy to Clear

**Messy:** Delete a club and leave students pointing at a ghost `club_id`.

**Clear:** Know the relationship. Update or delete children first, or use DB rules later (transactions next masterclass).

---

## 3. Practice Exercises

**Exercise 1 — Predict UPDATE (3 min)**  
`UPDATE clubs SET name = 'Robo'` with no WHERE. How many rows change if 3 clubs exist?

**Exercise 2 — Safe DELETE (4 min)**  
Write SELECT then DELETE for student id 2. Why two statements?

**Exercise 3 — WHERE SELECT (3 min)**  
Write SELECT for students whose `club_id` is 1. List the columns you need for a roll call.

**Exercise 4 — Relationship label (4 min)**  
Label: user ↔ profile; author ↔ posts; students ↔ courses. Which is 1-1, 1-many, many-many?

**Exercise 5 — Real-world (4 min)**  
**Netflix:** one profile has many watch-history rows. A user can have many profiles. Sketch tables and relationship types in two bullets.
