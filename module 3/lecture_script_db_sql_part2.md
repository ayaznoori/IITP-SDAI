# Lecture Script: Database Fundamentals and SQL (Part II)
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | SQL Fundamentals — UPDATE and DELETE | 25 min |
| 3 | Filtering Data — WHERE Clause in Depth | 25 min |
| 4 | Database Relationships — One-to-Many and Many-to-Many | 40 min |
| 5 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** Open with a concrete failure scenario, not a list of topics. The goal is to make learners feel the cost of misunderstanding before they learn the concept. Wait a full ten seconds after the opening question. Silence is productive here — it signals that the problem is worth thinking about.

**[Script:]**

"Here is a situation that ends careers at startups.

A developer needs to update the status of one specific order in the database. They write an `UPDATE` statement, run it, and forget to add a `WHERE` clause. Every single order in the table is now marked as delivered. Ten thousand rows, all wrong. The undo is painful, slow, and sometimes impossible if there is no backup.

Here is a second scenario. A developer deletes a user from the users table. But there are fifty orders in the orders table that reference that user. The database either throws an error and refuses, or — if relationships are not defined — silently leaves fifty orphaned rows pointing to a user that no longer exists. Now the application crashes every time it tries to load those orders.

Both of these are real mistakes made by real developers, often in their first month of working with a database. They are not caused by bad intentions — they are caused by not understanding how `WHERE` filters work, and not understanding how relationships between tables must be defined before the database can protect them.

Today we are going to cover all of it. `UPDATE` and `DELETE` with proper filtering. The `WHERE` clause used carefully and precisely. And the two most important relationship patterns in relational database design — one-to-many and many-to-many. These are the tools that make the difference between a database that actively protects your data and one that quietly lets your data go wrong."

---

## Block 2 — SQL Fundamentals: UPDATE and DELETE

### 2A — What Are UPDATE and DELETE?

**[Script:]**

"We can already create tables and insert rows. But data changes. Users update their email addresses. Orders change status. Items get removed from a catalog. `UPDATE` changes existing rows. `DELETE` removes them.

Both statements follow the same critical rule: without a `WHERE` clause, they affect every row in the table. Every single one. This is not a warning — it is a design fact of SQL. The database assumes that if you did not specify which rows, you meant all of them.

This is the opposite of what most beginners expect. In Python, a function that modifies something usually operates on one object at a time. In SQL, a statement operates on a set of rows — and if you do not define the set, the set is the entire table."

> 🎯 **Instructor Note:** Write this on the board in large letters before going further. Leave it up for the entire block.

```
No WHERE clause = every row is affected.
```

**[Script:]**

"Keep that visible. We will come back to it with every example."

---

### 2B — UPDATE

**[Script:]**

"The `UPDATE` statement has three parts. The table you are updating. The `SET` clause that names which columns change and what their new values are. And the `WHERE` clause that identifies which rows to change.

The syntax is:

```
UPDATE table SET column = value WHERE condition;
```

Let us look at a concrete example with the users table from earlier."

> 🎯 **Instructor Note:** Assume the room has the following users table in memory from the previous context, or draw it on the board now. You will reference specific rows throughout this block.

```
users
| id | name  | email               | age |
|----|-------|---------------------|-----|
| 1  | Alice | alice@example.com   | 28  |
| 2  | Bob   | bob@example.com     | 34  |
| 3  | Carol | carol@example.com   | 22  |
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before showing the demo, ask the room: "If I run `UPDATE users SET age = 99`, with no `WHERE` clause, what happens to the table?" Wait for answers. Some learners will say only one row changes. Correct answer: all three rows now have `age = 99`. Make sure this lands before you continue.

**Demo 1 — UPDATE with and without WHERE (whiteboard-friendly)**

```sql
-- Safe: update one specific user
UPDATE users SET age = 29 WHERE id = 1;

-- Safe: update multiple rows matching a condition
UPDATE users SET age = age + 1 WHERE age < 30;

-- Dangerous: no WHERE clause — updates every row
UPDATE users SET age = 99;
```

**[Script:]**

"Run the first statement. Only Alice changes — her age goes from 28 to 29. The `WHERE id = 1` clause means exactly one row matches, because `id` is the primary key and is unique.

Run the second statement. `age + 1` is an expression — SQL can reference the current column value in the new value. Every user with an age below 30 gets incremented by one. In our data that is Alice at 29 and Carol at 22, so they both go up by one. Bob at 34 is unaffected.

Now look at the third statement. Do not run it against data you care about. No `WHERE` clause. Every row gets `age = 99`. All three users. The database does not ask for confirmation. It executes immediately."

> 🎯 **Instructor Note:** Pause here. Ask: "How do you protect yourself from accidentally running an UPDATE without a WHERE clause?" Let the room respond. Good answers include: always write the WHERE clause first, review the statement before executing, use transactions so you can roll back. Name all of these and validate them. The habit of writing WHERE before SET is worth teaching explicitly.

**[Script:]**

"One practical habit: when writing an UPDATE, write the `WHERE` clause before you write the `SET` clause, even if you have to go back and fill it in. Some developers do a `SELECT` with the same `WHERE` clause first — they look at which rows will be affected before they change anything. That is a good discipline, especially when working on a production database."

---

### 2C — DELETE

**[Script:]**

"`DELETE` removes rows from a table. The syntax is simpler than `UPDATE` — there is no `SET` clause, just the table name and the `WHERE` condition.

```
DELETE FROM table WHERE condition;
```

And the same rule applies: no `WHERE` clause means every row is deleted. The table still exists — its structure, columns, and constraints remain — but every row is gone."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "I run `DELETE FROM users WHERE id = 2`. How many rows are left in the table? Which ones?" Answer: two rows — Alice and Carol. Bob is gone. Then ask: "What happens if I run `DELETE FROM users` with no WHERE?" Answer: zero rows remain. The table is empty but still exists.

**Demo 2 — DELETE with and without WHERE (whiteboard-friendly)**

```sql
-- Safe: delete one specific row
DELETE FROM users WHERE id = 2;

-- Safe: delete rows matching a condition
DELETE FROM users WHERE age > 30;

-- Dangerous: no WHERE clause — deletes every row
DELETE FROM users;
```

**[Script:]**

"The first statement deletes Bob — one row, because we filtered by primary key. The second deletes everyone over 30 — in our current data that would be Bob if he were still there, but after the first statement only Alice and Carol remain, and neither is over 30, so nothing is deleted. The third empties the entire table.

Notice that `DELETE FROM users` is different from `DROP TABLE users`. `DELETE` removes the rows. `DROP TABLE` removes the table itself — its structure, its data, its constraints, everything. Do not confuse them."

> 🎯 **Instructor Note:** This DELETE vs DROP confusion is very common. Write both on the board with their effects:

```
DELETE FROM users;       -- removes all rows, table still exists
DROP TABLE users;        -- removes the table entirely, structure and data gone
```

**[Script:]**

"If you accidentally drop a table in a production database, restoring it requires a backup. If you accidentally delete all rows, same situation. The behavior is different but the damage is equally severe. The protection in both cases is the same: know exactly what your `WHERE` clause targets before you execute."

**Recap of Block 2 before moving on:**

- `UPDATE table SET column = value WHERE condition` modifies existing rows
- `DELETE FROM table WHERE condition` removes rows
- Without a `WHERE` clause, both statements affect every row in the table — this is by design, not a bug
- Write the `WHERE` clause first; do a `SELECT` with the same condition to preview affected rows
- `DELETE FROM table` empties all rows; `DROP TABLE` removes the table structure entirely

---

## Block 3 — Filtering Data: The WHERE Clause in Depth

### 3A — Why WHERE Deserves Its Own Focus

**[Script:]**

"We have used `WHERE` already — in `SELECT`, in `UPDATE`, in `DELETE`. But we have kept the conditions simple: `id = 1`, `age > 25`. Real queries need more expressive filtering. And getting filtering wrong means either returning too much data, too little data, or operating on the wrong rows when you update or delete.

The `WHERE` clause is the single most important part of most SQL statements. It is where most of the logic lives. It is also where most of the mistakes happen."

---

### 3B — Comparison Operators

**[Script:]**

"You already know the basic comparison operators. Let us name them clearly so the vocabulary is solid."

> 🎯 **Instructor Note:** Write this table on the board.

```
=        equal to
!=  <>   not equal to (both work; != is more common)
>        greater than
<        less than
>=       greater than or equal to
<=       less than or equal to
```

**[Script:]**

"One trap: in SQL, equality is a single equals sign `=`, not double `==` as in Python. Beginners write `WHERE id == 1` constantly — it either throws an error or behaves unexpectedly depending on the database. Single equals sign for comparison in SQL."

> 🎯 **Instructor Note:** This is a genuine and recurring mistake. Name it explicitly and ask the room to repeat it back: "Equality in SQL is single equals. Comparison in Python is double equals." The repetition builds the habit.

---

### 3C — AND, OR, NOT

**[Script:]**

"You can combine multiple conditions with `AND`, `OR`, and `NOT`. The logic is identical to Python's boolean operators — but the precedence rules in SQL mean you should use parentheses whenever you combine `AND` and `OR` in the same condition. Without parentheses, `AND` binds more tightly than `OR`, which produces results that look correct but are subtly wrong."

> 🎯 **Instructor Note:** This is one of the most common logic bugs in SQL. Use the Python comparison to make it concrete before the demo.

**[Script:]**

"Compare these two in Python:

```python
age > 25 and age < 35 or name == 'Carol'
# Python evaluates: (age > 25 and age < 35) or (name == 'Carol')

(age > 25) and (age < 35 or name == 'Carol')
# Completely different result
```

The same ambiguity exists in SQL. Use parentheses to make your intent explicit."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, restore the users table to its original state: Alice (28), Bob (34), Carol (22). Write the table on the board. Then ask: "What does `WHERE age > 25 AND age < 35` return? What does `WHERE age > 25 OR age < 25` return?" Let learners work through both before you run anything.

**Demo 3 — AND, OR, NOT in WHERE (whiteboard-friendly)**

```sql
-- Both conditions must be true
SELECT name FROM users WHERE age > 25 AND age < 35;

-- Either condition is enough
SELECT name FROM users WHERE age < 25 OR name = 'Bob';

-- Negate a condition
SELECT name FROM users WHERE NOT age > 25;

-- Parentheses make intent explicit
SELECT name FROM users WHERE (age > 20 AND age < 30) OR name = 'Bob';
```

**[Script:]**

"First query: Alice at 28 and Bob at 34 both satisfy `age > 25`. But only Alice satisfies `age < 35` — Bob is 34 which is less than 35, so Bob comes back too. Both Alice and Bob are returned.

Second query: Carol at 22 satisfies `age < 25`. Bob satisfies `name = 'Bob'`. Alice satisfies neither. Carol and Bob come back.

Third query: `NOT age > 25` means age is 25 or less. Only Carol at 22 satisfies this. One row.

Fourth query: the parentheses clarify that we want users between 20 and 30, or specifically Bob. Alice at 28 satisfies the age range. Bob is named explicitly. Carol at 22 also satisfies the age range. All three rows come back."

> 🎯 **Instructor Note:** Pause after the fourth query. Ask: "If I remove the parentheses from that last query — `WHERE age > 20 AND age < 30 OR name = 'Bob'` — do I get the same result?" Walk through the operator precedence: `AND` binds first, so it reads as `(age > 20 AND age < 30) OR name = 'Bob'`. In this specific case the result happens to be the same — but make clear that this is coincidental and learners should not rely on it. Always use parentheses when mixing `AND` and `OR`.

---

### 3D — LIKE, IN, BETWEEN, IS NULL

**[Script:]**

"Four more filtering tools that you will use regularly.

`LIKE` does pattern matching on text columns. The `%` character matches any sequence of characters — zero or more. The `_` character matches exactly one character. This is not the same as Python regex — much simpler, much less powerful, but sufficient for common filtering needs."

```sql
SELECT * FROM users WHERE email LIKE '%@example.com';
SELECT * FROM users WHERE name LIKE 'A%';
```

**[Script:]**

"The first returns every user whose email ends in `@example.com`. The `%` at the start matches anything before that suffix. The second returns users whose name starts with the letter A — Alice in our data.

`IN` lets you match against a list of values. It is cleaner than chaining multiple `OR` conditions."

```sql
-- These are equivalent
SELECT * FROM users WHERE name = 'Alice' OR name = 'Bob';
SELECT * FROM users WHERE name IN ('Alice', 'Bob');
```

**[Script:]**

"`BETWEEN` checks whether a value falls within a range, inclusive on both ends. This is cleaner than writing `>= lower AND <= upper`."

```sql
SELECT * FROM users WHERE age BETWEEN 22 AND 30;
```

**[Script:]**

"`IS NULL` and `IS NOT NULL` check for the absence or presence of a value. In SQL, NULL means no value — it is not zero, it is not an empty string. It is the absence of a value entirely. And critically, you cannot test for NULL with `= NULL`. That always returns false. You must use `IS NULL`."

> 🎯 **Instructor Note:** This NULL mistake is extremely common. Write both forms on the board:

```sql
WHERE age = NULL      -- WRONG: always returns false, never matches
WHERE age IS NULL     -- CORRECT: matches rows where age has no value
```

**[Script:]**

"In Python, you would write `if value is None`. In SQL, you write `WHERE column IS NULL`. The idea is the same — None in Python and NULL in SQL both represent the absence of a value — but the syntax is different and `= NULL` silently produces wrong results instead of throwing an error. Burn `IS NULL` into your memory."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Add a user with no age to the table: `INSERT INTO users (name, email) VALUES ('Dave', 'dave@example.com');` — Dave has no age value, so his age is NULL. Then ask: "If I run `SELECT * FROM users WHERE age = NULL`, how many rows come back?" Answer: zero. None. Even Dave's row does not come back, because `= NULL` never evaluates to true. Then ask: "What do I write to get Dave's row?" Answer: `WHERE age IS NULL`.

**Demo 4 — LIKE, IN, BETWEEN, IS NULL (whiteboard-friendly)**

```sql
-- Pattern matching
SELECT name FROM users WHERE email LIKE '%@example.com';

-- Match against a list
SELECT name FROM users WHERE name IN ('Alice', 'Carol');

-- Range check
SELECT name, age FROM users WHERE age BETWEEN 22 AND 30;

-- NULL check — correct form
SELECT name FROM users WHERE age IS NULL;
```

**[Script:]**

"Run all four. First query: everyone in our table has an `@example.com` email, so all rows come back. Second: Alice and Carol by name. Third: Alice at 28 and Carol at 22 — both fall within 22 to 30 inclusive; Bob at 34 does not. Fourth: Dave, whose age was left out of the insert and is therefore NULL."

**Recap of Block 3 before moving on:**

- Single `=` is equality in SQL — not double `==` as in Python
- `AND` requires both conditions to be true; `OR` requires either; `NOT` negates a condition
- Use parentheses whenever mixing `AND` and `OR` — `AND` binds more tightly than `OR` without them
- `LIKE` with `%` and `_` matches text patterns; `IN` matches against a value list; `BETWEEN` checks an inclusive range
- NULL is the absence of a value — test for it with `IS NULL`, never with `= NULL`
- `IS NOT NULL` returns rows where a column has any value; `IS NULL` returns rows where it has none

---

## Block 4 — Database Relationships: One-to-Many and Many-to-Many

### 4A — Why Relationships Exist

**[Script:]**

"We have one table so far — users. Real applications have dozens of tables. An e-commerce application has users, products, orders, order items, categories, addresses, payment methods. A social platform has users, posts, comments, likes, followers.

These tables are not independent. An order belongs to a user. A comment belongs to a post and belongs to a user. A product belongs to multiple categories and a category contains multiple products.

The relationships between tables are not just helpful — they are the structure that makes the data meaningful. Without defined relationships, you have a collection of isolated tables. With them, you have a system that can answer real questions: which orders belong to this user, which products are in this category, which users have liked this post.

There are two relationships we are covering today: one-to-many and many-to-many. These two patterns cover the overwhelming majority of real-world data structures."

---

### 4B — One-to-Many Relationships

**[Script:]**

"A one-to-many relationship is the most common relationship in relational databases. It means: one row in table A corresponds to many rows in table B. One user can have many orders. One author can write many posts. One category can contain many products.

The implementation always follows the same pattern: the table on the many side gets a foreign key column that references the primary key of the table on the one side."

> 🎯 **Instructor Note:** Draw this diagram on the board before writing any SQL. The visual makes the foreign key placement obvious. Spend a moment on it — learners who see the diagram first find the SQL much easier to read.

```
users (ONE side)                orders (MANY side)
+----+-------+                  +----+----------+---------+
| id | name  |                  | id | item     | user_id |
+----+-------+                  +----+----------+---------+
| 1  | Alice |  <--- 1 to many  | 1  | Keyboard | 1       |
| 2  | Bob   |                  | 2  | Monitor  | 1       |
+----+-------+                  | 3  | Mouse    | 2       |
                                +----+----------+---------+
                                       FK → users.id
```

**[Script:]**

"Alice has two orders — the keyboard and the monitor. Bob has one order — the mouse. Each order row stores the `user_id` of the user who placed it. That `user_id` column is the foreign key.

Notice where the foreign key lives: on the many side. The orders table has the foreign key. The users table has no reference to orders at all. This is always the rule for one-to-many: the foreign key goes on the many side."

> 🎯 **Instructor Note:** Ask the room: "Why does the foreign key go on the many side and not the one side?" Let learners reason through it. The answer is: if we put order IDs on the users side, we would need a column for each order — and a user can have any number of orders. That does not work. A single `user_id` column on the orders table scales to any number of orders per user.

**[Script:]**

"If you tried to store the relationship on the users side — a list of order IDs — you would need to know in advance how many orders each user might have. You would need `order_1`, `order_2`, `order_3` as separate columns. That is a design that breaks the moment a user has more orders than you anticipated. The foreign key on the many side solves this completely."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I insert an order with `user_id = 99` and there is no user with `id = 99`, what happens?" Answer: the database rejects it with a foreign key constraint violation — but only if `PRAGMA foreign_keys = ON` is set in SQLite. Make sure learners have that pragma running before this demo.

**Demo 5 — One-to-Many Relationship (whiteboard-friendly)**

```sql
PRAGMA foreign_keys = ON;

CREATE TABLE orders (
    id      INTEGER PRIMARY KEY,
    item    TEXT    NOT NULL,
    user_id INTEGER NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

INSERT INTO orders (item, user_id) VALUES ('Keyboard', 1);
INSERT INTO orders (item, user_id) VALUES ('Monitor',  1);
INSERT INTO orders (item, user_id) VALUES ('Mouse',    2);

-- This should fail: user 99 does not exist
INSERT INTO orders (item, user_id) VALUES ('Desk', 99);
```

**[Script:]**

"Run the first three inserts. All succeed — user 1 and user 2 exist. Now run the fourth. The database rejects it: foreign key constraint failed. User 99 does not exist. The bad data never gets in.

Now let us query across the relationship. To get all orders for a specific user, you filter on `user_id`."

```sql
SELECT item FROM orders WHERE user_id = 1;
```

**[Script:]**

"This returns the keyboard and the monitor — the two orders belonging to Alice. This is the practical payoff of the relationship: you can retrieve all the related rows instantly by filtering on the foreign key.

We will not cover `JOIN` today — joining tables to combine their columns in one result set is a separate topic. What matters right now is understanding that the foreign key creates the connection and that you can already query across it by filtering on the foreign key column."

> 🎯 **Instructor Note:** If learners ask about JOIN, acknowledge it briefly: "Joining tables combines data from both sides into one result — that is a topic on its own. What you have here is already powerful: you can get all orders for a user, or verify that a user_id is valid, purely by filtering on the foreign key."

---

### 4C — Many-to-Many Relationships

**[Script:]**

"A many-to-many relationship is where things get interesting. It means: one row in table A can relate to many rows in table B, and one row in table B can also relate to many rows in table A.

Think about students and courses. One student can enroll in many courses. One course can have many students enrolled. Neither side is the one side — both sides are many.

Or think about blog posts and tags. One post can have multiple tags. One tag can apply to multiple posts.

You cannot implement a many-to-many relationship with a foreign key on either of the two main tables. If you put a `course_id` on the students table, one student can only be in one course. If you put a `student_id` on the courses table, one course can only have one student. Neither works."

> 🎯 **Instructor Note:** Draw the problem on the board explicitly before drawing the solution. The visual of the broken design makes the junction table feel like a natural solution rather than an arbitrary convention.

```
WRONG: student_id on courses
courses
| id | title   | student_id |    ← one course can only have ONE student
|----|---------|------------|

WRONG: course_id on students
students
| id | name  | course_id |    ← one student can only be in ONE course
|----|-------|-----------|
```

**[Script:]**

"Neither of these works. The solution is a third table — a junction table, also called an association table or a linking table. This table has no real data of its own. Its entire purpose is to record which rows from table A are connected to which rows from table B."

> 🎯 **Instructor Note:** Draw the correct three-table design now. Take time with this diagram — it is the conceptual heart of Block 4.

```
students                   enrollments (junction)       courses
+----+-------+             +------------+-----------+   +----+----------+
| id | name  |             | student_id | course_id |   | id | title    |
+----+-------+             +------------+-----------+   +----+----------+
| 1  | Alice |  <--------- | 1          | 1         |   | 1  | Math     |
| 2  | Bob   |  <--------- | 1          | 2         |   | 2  | History  |
+----+-------+             | 2          | 1         |   +----+----------+
                           +------------+-----------+
                               FK → students.id
                               FK → courses.id
```

**[Script:]**

"Alice is enrolled in Math and History — two rows in the enrollments table, both with `student_id = 1`. Bob is enrolled in Math — one row with `student_id = 2` and `course_id = 1`.

The junction table has two foreign keys: one pointing to students, one pointing to courses. Together, those two columns form the primary key of the junction table — the combination of `student_id` and `course_id` is unique. The same student cannot be enrolled in the same course twice.

This is the standard pattern for many-to-many. Two tables for the real entities, one junction table to record the connections."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask two questions. First: "If I try to insert a row into enrollments with a `student_id` that does not exist, what happens?" Answer: foreign key constraint failure — same as the one-to-many case. Second: "If I insert the same `student_id` and `course_id` combination twice, what happens?" Answer: primary key or unique constraint failure, because that combination is the primary key of the junction table.

**Demo 6 — Many-to-Many Relationship (whiteboard-friendly)**

```sql
CREATE TABLE students (
    id   INTEGER PRIMARY KEY,
    name TEXT    NOT NULL
);

CREATE TABLE courses (
    id    INTEGER PRIMARY KEY,
    title TEXT    NOT NULL
);

CREATE TABLE enrollments (
    student_id INTEGER NOT NULL,
    course_id  INTEGER NOT NULL,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (course_id)  REFERENCES courses(id)
);
```

**[Script:]**

"Look at the `enrollments` table carefully. It has no `id` column of its own. Its primary key is the combination of `student_id` and `course_id` — written as `PRIMARY KEY (student_id, course_id)`. This is called a composite primary key. It guarantees that the same student-course pair cannot appear twice.

It has two foreign key constraints — one for each side of the relationship. Both must reference existing rows in their respective tables. Insert a row with a valid student and a valid course and it works. Invalid student or invalid course — the database rejects it."

```sql
INSERT INTO students (name) VALUES ('Alice'), ('Bob');
INSERT INTO courses  (title) VALUES ('Math'), ('History');

INSERT INTO enrollments (student_id, course_id) VALUES (1, 1);
INSERT INTO enrollments (student_id, course_id) VALUES (1, 2);
INSERT INTO enrollments (student_id, course_id) VALUES (2, 1);
```

**[Script:]**

"Alice is now enrolled in Math and History. Bob is enrolled in Math. Now let us query: which courses is Alice in?"

```sql
SELECT course_id FROM enrollments WHERE student_id = 1;
```

**[Script:]**

"This returns course IDs 1 and 2 — Math and History. We are filtering the junction table on one side to find all the connected rows on the other side. Same pattern as querying a one-to-many — filter on the foreign key column."

> 🎯 **Instructor Note:** Pause here and ask: "What query would you write to find all students enrolled in Math — course ID 1?" Let learners write it before you show it. Answer: `SELECT student_id FROM enrollments WHERE course_id = 1;` — this returns student IDs 1 and 2, Alice and Bob. The junction table works symmetrically in both directions.

---

### 4D — One-to-Many vs Many-to-Many: How to Choose

**[Script:]**

"How do you know which relationship type to use when designing a table? Ask yourself two questions.

First: can a row on the A side belong to more than one row on the B side, or only one? If a user can only belong to one company, that is one-to-many. If a user can belong to multiple companies, that is many-to-many.

Second: can a row on the B side belong to more than one row on the A side, or only one? If an order can only belong to one user, that is one-to-many. If a course can have many students and a student can have many courses, that is many-to-many.

Work through both directions. If either direction is many, you have a many-to-many."

> 🎯 **Instructor Note:** Ask the room to classify these relationships. Give them 60 seconds to think, then discuss. Do not give the answers immediately.

```
1. A blog post and its author       — one-to-many or many-to-many?
2. A product and its tags           — one-to-many or many-to-many?
3. An employee and their department — one-to-many or many-to-many?
4. A book and its authors           — one-to-many or many-to-many?
```

**[Script:]**

"Let us work through them.

Blog post and author: a post has one author, an author has many posts. One-to-many. Foreign key `author_id` goes on the posts table.

Product and tags: a product can have many tags, a tag can apply to many products. Many-to-many. Junction table needed.

Employee and department: depends on the business rule. If an employee can only be in one department, it is one-to-many. If they can be in multiple, it is many-to-many. The data model reflects the business rule, not the other way around.

Book and authors: a book can have multiple authors, an author can write multiple books. Many-to-many. Junction table needed."

> 🎯 **Instructor Note:** The employee-department case is worth lingering on. The point is that the correct data model depends on the real-world rules of the system, not on assumptions. Always clarify the business rules before you write the SQL.

**Recap of Block 4 before moving on:**

- Relationships connect tables and make data meaningful across the system
- One-to-many: one row in A corresponds to many rows in B; the foreign key always goes on the many side
- Many-to-many: both sides can have multiple relationships; requires a junction table with two foreign keys and a composite primary key
- The junction table has no data of its own — its purpose is to record which rows from each side are connected
- To query a relationship, filter on the foreign key column; the junction table can be queried from either direction
- Choose the relationship type by asking whether either direction allows multiple connections

---

## Block 5 — Lecture Summary

> 🎯 **Instructor Note:** Use this block as active recall, not passive reading. For each section, ask learners to answer before you confirm. "What happens to an UPDATE with no WHERE clause? How do you test for NULL? Where does the foreign key go in a one-to-many relationship? What does a junction table's primary key look like?" Keep the pace brisk — you have covered a lot of ground and this block should consolidate it with energy, not slow it down.

**SQL Fundamentals — UPDATE and DELETE**

- `UPDATE table SET column = value WHERE condition` modifies existing rows; the `WHERE` clause determines which ones
- `DELETE FROM table WHERE condition` removes rows matching the condition
- Without a `WHERE` clause, `UPDATE` and `DELETE` affect every row in the table — this is expected SQL behavior, not a bug
- `DELETE FROM table` removes all rows but leaves the table structure intact; `DROP TABLE` removes the table entirely
- A safe practice: write the `WHERE` clause first; run a `SELECT` with the same condition to preview which rows will be affected

**Filtering Data — WHERE Clause**

- Single `=` is equality in SQL; double `==` is a Python convention that does not apply in SQL
- `AND` requires all conditions to be true; `OR` requires any one condition to be true; `NOT` negates a condition
- `AND` binds more tightly than `OR` — use parentheses whenever you mix them to make precedence explicit
- `LIKE` with `%` (any sequence) and `_` (one character) matches text patterns
- `IN (list)` matches any value in the list; `BETWEEN lower AND upper` checks an inclusive range
- `IS NULL` tests for missing values; `= NULL` always evaluates to false and should never be used
- `IS NOT NULL` returns rows where the column has any value

**Database Relationships — One-to-Many and Many-to-Many**

- A one-to-many relationship means one row in table A connects to many rows in table B
- In a one-to-many relationship, the foreign key always lives on the many side, referencing the primary key of the one side
- A many-to-many relationship means both sides can connect to multiple rows on the other side
- Many-to-many requires a junction table — a third table with two foreign key columns, one for each side
- The junction table's primary key is composite: the combination of both foreign key columns, ensuring no duplicate pairs
- To query a relationship, filter on the foreign key column in the junction table or the related table
- Choose the relationship type by asking whether either direction of the relationship allows multiple connections

**Why All of This Matters Together**

- `UPDATE` and `DELETE` without `WHERE` are among the most destructive accidental operations in database work — every developer needs the habit of writing and verifying the `WHERE` clause before executing either statement
- The `WHERE` clause is the core of SQL filtering: every query that reads, modifies, or deletes specific data depends on writing precise, correctly-precedenced conditions
- Correctly modeled relationships — putting foreign keys in the right place, using junction tables for many-to-many — are what allow the database to enforce referential integrity automatically, eliminating an entire category of application-level bugs that arise when data references rows that do not exist
- Together, these three topics form the operational and structural foundation of working with a relational database in any backend application: you can modify data safely, retrieve it precisely, and trust that the relationships between your tables are enforced consistently

---

*End of script.*
