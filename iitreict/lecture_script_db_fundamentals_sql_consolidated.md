# Lecture Script: Database Fundamentals and SQL
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Introduction to Relational Databases | 15 min |
| 3 | Table Creation and Constraints | 20 min |
| 4 | Primary Keys and Foreign Keys | 15 min |
| 5 | SQL Basics — SELECT, INSERT, UPDATE, DELETE | 30 min |
| 6 | Database Normalization Principles | 12 min |
| 7 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** Open with the persistence problem — the gap between what learners have been building and what production applications require. This audience has built FastAPI routes and Pydantic models. They know how to receive and validate data. The hook is: where does that data actually go? Wait a moment after the opening question before continuing.

**[Script:]**

"You have a FastAPI application. It accepts POST requests, validates data with Pydantic, and returns responses. Where does the data go?

Right now, if you have not used a database, the answer is: nowhere permanent. It lives in a Python variable for the duration of the request and then it is gone. Every time the server restarts, every piece of data disappears. No users persist. No orders survive. No records carry forward.

That is fine for learning routing and validation. It is not fine for any application a real user depends on.

Databases solve exactly this: data that persists when the program stops, survives server restarts, handles multiple simultaneous users safely, and can be queried, updated, and related across many different entities.

And there is a second problem. Even once you have a database, how you structure the data matters enormously. Store the same information in three places and you spend the rest of the project fixing inconsistencies when one copy changes and the others do not. Define a foreign key incorrectly and your application crashes on records that reference deleted rows. Write an update without a WHERE clause and you change every row in the table simultaneously.

Today covers the full foundation: what relational databases are, how to design tables correctly, how primary and foreign keys work, how to write the four fundamental SQL operations, and the basic principles that keep data from becoming a mess over time. This is the ground floor everything else is built on."

---

## Block 2 — Introduction to Relational Databases

### 2A — What a Relational Database Is

**[Script:]**

"A relational database stores data in tables. A table is a two-dimensional structure: columns define what kind of data each record holds, and rows are the individual records. One column per attribute, one row per entity.

The word relational comes from the mathematical concept of a relation — essentially, a set with a defined structure. In practice it means two things: tables have strict structure enforced by the database, and tables can be linked to each other through defined relationships. That second part is what makes relational databases powerful for multi-entity applications."

> 🎯 **Instructor Note:** Draw this on the board immediately. Keep it minimal — three columns, three rows. Label columns and rows explicitly before saying another word.

```
Table: users

| id | name    | email               |
|----|---------|---------------------|
| 1  | Alice   | alice@example.com   |
| 2  | Bob     | bob@example.com     |
| 3  | Carol   | carol@example.com   |
```

**[Script:]**

"Each column has a data type — `id` is an integer, `name` and `email` are text. The database enforces these types on every write. You cannot put a number where an email belongs. You cannot leave out a required column. This is already more protection than a Python dictionary gives you — a dict has no enforced schema.

Each row is one user. The database gives you tools to add rows, retrieve specific rows, change existing rows, and remove rows. All of that is SQL — Structured Query Language.

SQL is declarative. You describe what data you want or what change to make. The database determines how to execute it. This is a different mental model from Python, where you write step-by-step instructions. In SQL you write intent, not procedure."

---

### 2B — Why SQLite for This Session

**[Script:]**

"We will use SQLite throughout. SQLite stores an entire database in a single file on disk. It does not require a running server process — it is a library, embedded directly in your application. This makes it ideal for learning: no installation, no configuration, no server management.

The SQL we write is standard SQL. The concepts — table structure, constraints, keys, relationships — transfer directly to PostgreSQL and MySQL, which you will encounter in production environments. The dialect differences are minor and specific; the foundations are identical."

> 🎯 **Instructor Note:** If learners ask about PostgreSQL vs MySQL vs SQLite: name the difference briefly — SQLite is file-based and embedded, the others are server-based and used in production — and redirect. The SQL fundamentals they are learning today are the same across all three. Do not spend time on database comparison.

**[Script:]**

"One important SQLite-specific note you will need today: SQLite does not enforce foreign keys by default. You activate it with one command at the start of your connection. We will cover this when we reach foreign keys."

**Recap of Block 2 before moving on:**

- A relational database stores data in tables: columns define structure and type, rows hold records
- The database enforces column types on every write — stricter than a Python dictionary
- SQL is declarative — you state what you want; the database determines how to get it
- Relational means tables have structure and can be linked to each other through defined relationships
- SQLite stores data in a file, requires no server, and is suitable for learning; concepts transfer to production databases

---

## Block 3 — Table Creation and Constraints

### 3A — CREATE TABLE and Data Types

**[Script:]**

"To create a table in SQL you write `CREATE TABLE`, give it a name, and list each column with its name, data type, and any constraints. Constraints are rules the database enforces automatically on every insert and update. They run before the data is stored — if a constraint fails, the row is rejected and the data never gets in.

The most common data types in SQLite:"

> 🎯 **Instructor Note:** Write these on the board as you say them. Four types cover the majority of beginner use cases.

```
INTEGER   — whole numbers: IDs, counts, ages
TEXT      — strings of any length: names, emails, descriptions
REAL      — decimal numbers: prices, coordinates
BOOLEAN   — true or false (stored as 1/0 in SQLite)
```

---

### 3B — Constraints

**[Script:]**

"Constraints are what separate a real database table from a Python list of dictionaries. They are rules the database enforces on every row, from every application that connects, automatically. You do not have to write Python code to check them — the database handles it."

> 🎯 **Instructor Note:** Write each constraint and its definition on the board. Leave it visible through the rest of Block 3.

```
NOT NULL    — this column must always have a value
UNIQUE      — no two rows can have the same value in this column
DEFAULT     — if no value is provided, use this value instead
CHECK       — value must satisfy a condition you define
```

**[Script:]**

"`NOT NULL` means you cannot create a user with no email. `UNIQUE` means you cannot have two users with the same username. `DEFAULT` means a column can have a fallback value if none is provided. `CHECK` lets you write a condition — age must be non-negative, status must be one of a known set of values.

These run on every write, always. If you add a second application, a data import script, or a manual SQL shell — all of them face the same constraints. The protection is in the database, not in any single piece of application code."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I try to insert a user with no email into a table where email is NOT NULL, what happens?" Answer: the database rejects the insert with a constraint error. The row is not stored. Then ask: "If I insert two users with the same email into a table where email is UNIQUE?" Answer: the second insert fails with a unique constraint error.

**Demo 1 — CREATE TABLE with constraints (whiteboard-friendly)**

```sql
CREATE TABLE IF NOT EXISTS users (
    id    INTEGER PRIMARY KEY,
    name  TEXT    NOT NULL,
    email TEXT    NOT NULL UNIQUE,
    age   INTEGER CHECK(age >= 0)
);
```

**[Script:]**

"Read this top to bottom. `IF NOT EXISTS` means: create this table only if it does not already exist — safe to run every time your application starts. `id` is the primary key — we will cover that fully in Block 4. `name` is required. `email` is required and unique. `age` must be zero or greater if provided.

The database now enforces all four of these rules on every insert and update, from every source, automatically."

> 🎯 **Instructor Note:** Ask: "What happens if you run this CREATE TABLE statement a second time?" Without `IF NOT EXISTS` — error. With it — silently does nothing. This is why `IF NOT EXISTS` belongs in any setup code that runs on application start.

**Recap of Block 3 before moving on:**

- `CREATE TABLE` defines column names, data types, and constraints
- Constraints — `NOT NULL`, `UNIQUE`, `DEFAULT`, `CHECK` — are enforced automatically on every write
- Constraints protect data integrity across all applications and access paths, not just one piece of code
- `CREATE TABLE IF NOT EXISTS` is the safe form for application setup code

---

## Block 4 — Primary Keys and Foreign Keys

### 4A — Primary Keys

**[Script:]**

"Every table should have a primary key. A primary key uniquely identifies each row. No two rows can share a primary key value. A primary key column can never be NULL.

The most common pattern is an integer primary key that auto-increments — the database assigns the next available integer each time a row is inserted. You do not manage it. You do not supply it on insert. It is always unique and always set.

In SQLite, declaring a column as `INTEGER PRIMARY KEY` activates this behavior automatically."

> 🎯 **Instructor Note:** Ask: "Could we use the user's email as the primary key instead of an integer ID?" Let learners respond. Guide them to: emails can change; email is longer and slower to index and join on; an integer ID is stable, compact, and never changes. The integer ID pattern exists for good reasons.

**[Script:]**

"You already saw `id INTEGER PRIMARY KEY` in the users table. That column is automatically assigned, always unique, never null. When you insert a user and omit the `id`, SQLite assigns the next available integer. When you need to reference that user from another table, you use this ID."

---

### 4B — Foreign Keys

**[Script:]**

"A foreign key is a column in one table that references the primary key of another table. It creates a link between rows in two different tables. And the database can enforce that link — you cannot create an order for a user that does not exist.

The table with the foreign key is the many side. The table being referenced is the one side. One user can have many orders. The orders table holds the foreign key — `user_id` — pointing back to users."

> 🎯 **Instructor Note:** Draw this on the board and leave it up. The visual is clearer than any verbal description.

```
users                           orders
+----+-------+                  +----+----------+---------+
| id | name  |                  | id | item     | user_id |
+----+-------+                  +----+----------+---------+
| 1  | Alice |  ←─────────────  | 1  | Keyboard | 1       |
| 2  | Bob   |  ←─────────────  | 2  | Monitor  | 1       |
+----+-------+                  | 3  | Mouse    | 2       |
                                +----+----------+---------+
                                        FK → users.id
```

**[Script:]**

"The foreign key `user_id` in orders references `id` in users. This enforces referential integrity: you cannot insert an order with a `user_id` that does not exist in users. You cannot delete a user who has orders without deciding what to do with those orders.

`ON DELETE CASCADE` is one option — when a user is deleted, all their orders are deleted automatically. Other options exist. You choose the behavior that matches your application's rules."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I insert an order with `user_id = 99` and no user has `id = 99`, what should happen?" Answer: foreign key constraint failure — the insert is rejected. Then mention: "In SQLite, you must activate foreign key enforcement with one command. Without it, SQLite accepts the bad data silently." Show the pragma.

**Demo 2 — Foreign Key (whiteboard-friendly)**

```sql
PRAGMA foreign_keys = ON;

CREATE TABLE orders (
    id      INTEGER PRIMARY KEY,
    item    TEXT    NOT NULL,
    user_id INTEGER NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

INSERT INTO orders (item, user_id) VALUES ('Keyboard', 1);

-- This will fail: user 99 does not exist
INSERT INTO orders (item, user_id) VALUES ('Monitor', 99);
```

**[Script:]**

"The first insert succeeds — user 1 exists. The second is rejected — foreign key constraint failed. The bad data never gets in.

`PRAGMA foreign_keys = ON` must be run once per connection in SQLite. It is not needed in PostgreSQL — that database enforces foreign keys by default. Make it the first line of any SQLite setup code you write."

> 🎯 **Instructor Note:** This pragma omission is the single most common SQLite mistake. Learners test foreign key violations, see no error, and conclude foreign keys do not work. The pragma is the cause every time. Name it clearly and suggest they treat it as non-optional.

**Recap of Block 4 before moving on:**

- Primary key uniquely identifies each row; `INTEGER PRIMARY KEY` in SQLite auto-increments
- Foreign key is a column that references the primary key of another table
- Foreign keys enforce referential integrity — rows cannot reference non-existent rows in the referenced table
- `ON DELETE CASCADE` deletes dependent rows when the referenced row is deleted
- In SQLite, `PRAGMA foreign_keys = ON` is required to activate enforcement — it is off by default

---

## Block 5 — SQL Basics: SELECT, INSERT, UPDATE, DELETE

> 🎯 **Instructor Note:** Before starting Block 5, insert enough data to make the demos meaningful. Do this live or have it ready. The demos reference this exact data set.

```sql
INSERT INTO users (name, email, age) VALUES ('Alice', 'alice@example.com', 28);
INSERT INTO users (name, email, age) VALUES ('Bob',   'bob@example.com',   34);
INSERT INTO users (name, email, age) VALUES ('Carol', 'carol@example.com', 22);
```

---

### 5A — INSERT

**[Script:]**

"INSERT adds rows. There are two forms: positional, where values match column order exactly, and named, where you list the columns you are providing values for. Always use the named form in real code — it is explicit, does not break when columns are added, and is far easier to read."

```sql
-- Named form (always prefer this)
INSERT INTO users (name, email, age) VALUES ('Dave', 'dave@example.com', 30);
```

**[Script:]**

"Omit `id` — SQLite assigns it. Omit any column with a DEFAULT or that allows NULL. Any NOT NULL column without a DEFAULT must be provided or the insert fails with a constraint error."

---

### 5B — SELECT

**[Script:]**

"`SELECT` retrieves rows. `SELECT *` returns all columns — every column, every row. Prefer named columns in application code so unexpected schema changes do not break your results. `WHERE` filters which rows come back."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** With three users in the table, ask: "What does `SELECT name FROM users WHERE age > 25` return?" Answer: Alice (28) and Bob (34). Carol at 22 does not pass the filter.

**Demo 3 — SELECT with WHERE (whiteboard-friendly)**

```sql
-- All rows, all columns
SELECT * FROM users;

-- Specific columns
SELECT name, email FROM users;

-- Filtered rows
SELECT name, age FROM users WHERE age > 25;

-- Exact match
SELECT * FROM users WHERE email = 'alice@example.com';
```

**[Script:]**

"Four queries, four different result shapes. The last returns at most one row because email is UNIQUE — only one user can have that email.

`AND` and `OR` combine conditions. `ORDER BY` sorts. `LIMIT` caps the number of rows."

```sql
SELECT name, age FROM users WHERE age > 20 AND age < 35 ORDER BY age DESC;
```

**[Script:]**

"Returns Alice and Carol — both between 20 and 35 exclusive — sorted youngest first since `DESC` on age puts lower numbers last. Wait — `DESC` sorts highest first. Alice at 28 comes before Carol at 22. Bob at 34 is excluded by `age < 35`."

> 🎯 **Instructor Note:** Walk through this slowly. Ask learners which rows pass the WHERE filter before asking about the ORDER BY result. Separating the two steps helps learners debug their own queries later.

---

### 5C — UPDATE

**[Script:]**

"UPDATE modifies existing rows. It has three parts: the table, the `SET` clause naming what changes, and the `WHERE` clause identifying which rows. Without WHERE, every row is updated.

Write this on the board. Leave it visible."

> 🎯 **Instructor Note:** Write this in large letters and do not erase it.

```
No WHERE clause = every row is affected.
This applies to both UPDATE and DELETE.
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I run `UPDATE users SET age = 99` with no WHERE, how many rows change?" Answer: all three. Every user now has age 99. Ask: "How do I update only Alice?" Answer: `WHERE id = 1` or `WHERE email = 'alice@example.com'`.

**Demo 4 — UPDATE with and without WHERE (whiteboard-friendly)**

```sql
-- Safe: update one row by primary key
UPDATE users SET age = 29 WHERE id = 1;

-- Safe: update rows matching a condition
UPDATE users SET age = age + 1 WHERE age < 25;

-- Dangerous: no WHERE — every row changes
UPDATE users SET age = 99;
```

**[Script:]**

"The first updates Alice only. The second increments anyone under 25 — Carol. The third sets age to 99 on every row in the table. The database does not ask for confirmation. It executes.

A practical habit: write the WHERE clause first. Run a SELECT with the same WHERE to preview which rows will change before you run the UPDATE. On production data, this habit prevents the most common accidental mass-update."

---

### 5D — DELETE

**[Script:]**

"`DELETE` removes rows. Same rule as UPDATE: no WHERE means every row is deleted. The table itself remains — its structure, columns, and constraints — but every row is gone."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What is the difference between `DELETE FROM users` and `DROP TABLE users`?" Answer: DELETE removes all rows, table survives. DROP TABLE removes the table entirely — structure, data, constraints. Both are dangerous without a WHERE clause or a backup. Both are irreversible without a restore.

**Demo 5 — DELETE (whiteboard-friendly)**

```sql
-- Safe: delete one row by primary key
DELETE FROM users WHERE id = 2;

-- Safe: delete by condition
DELETE FROM users WHERE age > 30;

-- Dangerous: no WHERE — empties the table
DELETE FROM users;
```

**[Script:]**

"The first removes Bob. The second removes anyone over 30 — after the first delete, only Alice at 28 and Carol at 22 remain, so neither qualifies. The third deletes everything.

Same discipline as UPDATE: write WHERE first, SELECT to preview, then DELETE. On data you care about, there is no undo without a backup."

**Recap of Block 5 before moving on:**

- `INSERT INTO table (columns) VALUES (values)` — always use the named column form
- `SELECT columns FROM table WHERE condition` — use named columns in application code, not `*`
- `UPDATE table SET column = value WHERE condition` — without WHERE, every row is changed
- `DELETE FROM table WHERE condition` — without WHERE, every row is deleted
- `DELETE FROM table` removes all rows, table structure survives; `DROP TABLE` removes the table entirely
- Write WHERE first; SELECT to preview which rows are affected before running UPDATE or DELETE

---

## Block 6 — Database Normalization Principles

### 6A — What Is Normalization and Why Does It Exist?

**[Script:]**

"Normalization is the process of structuring tables to reduce data redundancy and prevent inconsistency. When the same data is stored in multiple places, those copies can diverge — one is updated, the others are not. That is an update anomaly, and it means your database contains contradictory information.

There are formal numbered normal forms. We are covering the three that solve the problems you will actually encounter as a beginner. You do not need to memorize the names to apply the ideas."

---

### 6B — First Normal Form: One Value Per Cell

**[Script:]**

"First normal form says each column holds exactly one value. No lists. No comma-separated strings. No arrays hidden inside a text column.

The violation looks like this:"

> 🎯 **Instructor Note:** Draw both the bad design and the corrected design on the board side by side.

```
VIOLATION — multiple values in one column:
+----+-------+---------------------------+
| id | name  | emails                    |
+----+-------+---------------------------+
| 1  | Alice | alice@work.com,alice@home |
+----+-------+---------------------------+

CORRECT — one value per cell:
+----+-------+----------------+----------+
| id | name  | email          | type     |
+----+-------+----------------+----------+
| 1  | Alice | alice@work.com | work     |
| 2  | Alice | alice@home.com | personal |
+----+-------+----------------+----------+
```

**[Script:]**

"The first design makes it impossible to query by a specific email — you cannot write `WHERE email = 'alice@work.com'` when the email is buried in a comma-separated string. You also cannot enforce UNIQUE on the column. The fix is to give each value its own row."

---

### 6C — Second Normal Form: No Partial Dependencies

**[Script:]**

"Second normal form applies to tables with composite primary keys — primary keys made of more than one column. It says every non-key column must depend on the entire primary key, not just part of it.

The practical lesson for beginners without composite keys: do not store data in a table that belongs to a different entity. An orders table should not store the customer's name and email. Those belong to the users table. The order should store only the user ID — a foreign key."

```
VIOLATION — customer data stored in orders:
+----------+-----------+-------------------+----------+
| order_id | user_id   | customer_name     | item     |
+----------+-----------+-------------------+----------+
| 1        | 1         | Alice             | Keyboard |
+----------+-----------+-------------------+----------+

CORRECT — customer name lives in users table:
orders: order_id, user_id (FK), item
users:  id, name, email
```

**[Script:]**

"In the violation, if Alice changes her name, you must update every order row that contains it — potentially thousands of rows — and if you miss one, your database now contains two different names for the same person. With the correct design, name lives in one place. Change it once. Every query that joins orders to users automatically sees the updated name."

---

### 6D — Third Normal Form: No Transitive Dependencies

**[Script:]**

"Third normal form says non-key columns must depend on the primary key directly — not on another non-key column.

The practical version: if a column's value can be derived from another non-key column, it belongs in a separate table."

```
VIOLATION — city depends on zip_code, not on user_id:
+----+-------+---------+-----------+
| id | name  | zip_code| city      |
+----+-------+---------+-----------+
| 1  | Alice | 94102   | San Francisco |
| 2  | Bob   | 94102   | San Francisco |
+----+-------+---------+-----------+

CORRECT — zip codes in their own table:
users:     id, name, zip_code (FK)
zip_codes: zip_code (PK), city
```

**[Script:]**

"In the violation, every user with the same zip code stores the same city name. If the city assignment for that zip code changes — a rezoning, a data correction — you must update every user row with that zip. Miss one and you have contradictions. With the correct design, city is stored once in the zip_codes table."

> 🎯 **Instructor Note:** Pause here and ask: "What is the single theme connecting all three normal forms?" Let learners respond. The answer is: store each piece of information in exactly one place. If a fact needs to change, it changes in one row, one table, and every query that touches it immediately sees the correct version. This is the principle behind normalization — not a set of formal rules to memorize, but a discipline of keeping data non-redundant.

**[Script:]**

"You do not need to pass a normalization exam. You need to recognize the smell: the same data appearing in multiple rows, a table that stores information about two different things at once, columns that depend on other columns rather than on the key. When you smell that, you know the design needs rethinking."

**Recap of Block 6 before moving on:**

- Normalization structures tables to reduce redundancy and prevent update anomalies
- First normal form: one value per cell — no lists, no comma-separated strings in columns
- Second normal form: every column depends on the entire primary key — do not store one entity's data in another entity's table
- Third normal form: every column depends on the key directly — not on another non-key column
- The unifying principle: store each piece of information in exactly one place

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall — ask before confirming. "What does NOT NULL enforce? What is the difference between DELETE and DROP TABLE? Where does the foreign key go in a one-to-many relationship? What is the first normal form violation called when you store a list in a column? What must you always write before UPDATE or DELETE?" Keep pace up — this is consolidation.

**Introduction to Relational Databases**

- A relational database stores data persistently in tables: columns define structure and type, rows are individual records
- The database enforces column types on every write — this is stricter and more reliable than application-level checking
- SQL is declarative — you describe what you want; the database determines how to get it
- Relational means tables have defined relationships; data across tables is linked through keys, not assembled manually in application code

**Table Creation and Constraints**

- `CREATE TABLE IF NOT EXISTS` defines column names, types, and constraints; `IF NOT EXISTS` makes it safe to run on startup
- `NOT NULL` requires a value; `UNIQUE` prevents duplicate values; `DEFAULT` provides a fallback; `CHECK` enforces a condition
- Constraints run on every write from every application — the protection is in the database, not in any one piece of code

**Primary Keys and Foreign Keys**

- Primary key uniquely identifies each row; `INTEGER PRIMARY KEY` in SQLite auto-increments and never requires manual assignment
- Foreign key is a column that references the primary key of another table — it links rows across tables
- Foreign keys enforce referential integrity: rows cannot reference non-existent rows in the referenced table
- In SQLite, `PRAGMA foreign_keys = ON` must be run per connection to activate enforcement

**SQL Basics — SELECT, INSERT, UPDATE, DELETE**

- `INSERT INTO table (columns) VALUES (values)` — always name the columns; omit auto-increment primary keys
- `SELECT columns FROM table WHERE condition ORDER BY column LIMIT n` — use named columns, filter precisely
- `UPDATE table SET column = value WHERE condition` — without WHERE, every row changes; write WHERE first
- `DELETE FROM table WHERE condition` — without WHERE, every row is deleted; `DROP TABLE` removes the table itself
- The safest habit before any UPDATE or DELETE: run a SELECT with the same WHERE clause to preview affected rows

**Database Normalization Principles**

- First normal form: one value per cell — no lists or comma-separated strings inside columns
- Second normal form: every column depends on the full primary key — do not store one entity's attributes in another entity's table
- Third normal form: every column depends on the key directly — not on another non-key column
- The unifying principle: store each fact in exactly one place; a fact that lives in one row, in one table, can be updated once and stay consistent everywhere

**Why All of This Matters Together**

- The FastAPI routes and Pydantic models already built provide validation and API structure; a database provides the persistence layer those routes need — without it, every restart wipes the state and no real application can function
- Table design decisions made now — which constraints to set, where to put foreign keys, how normalized the schema is — determine whether data stays consistent automatically or requires constant defensive code to patch around a poorly structured schema; getting the fundamentals right the first time is far cheaper than fixing data integrity problems after a system is in production

---

*End of script.*
