# Lecture Script: Database Fundamentals and SQL (Part I)
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | Relational Databases — What They Are and Why They Exist | 25 min |
| 3 | Table Design — Creation, Constraints, Keys | 35 min |
| 4 | SQL Fundamentals — SELECT and INSERT | 30 min |
| 5 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** Do not start with a definition. Start with a problem the learners have almost certainly felt. Keep the room quiet for a moment after the opening question — let discomfort land before you relieve it. If the group is shy, rephrase: "Has anyone built something in Python where they just saved data to a variable or a list? What happened to that data when the program stopped?" That usually opens them up.

**[Script:]**

"Let me ask you a question before we write a single line of SQL.

You build a Python program. Users can register, log in, post messages, whatever. Where does all of that data go? Right now, if you are just starting out, the answer is probably: a Python list, or a dictionary, or maybe a file. And the moment your program stops running, all of it disappears.

That is the problem databases solve. Persistence — the data survives when the program is not running. It survives when the server restarts. It survives when a hundred users are hitting the system simultaneously and two of them are trying to update the same record at the same time.

But databases are not just storage. A file can store data. What makes a relational database different is that it gives you structure, relationships, guarantees, and a standardized language to query any of it — SQL.

Every serious backend application you have ever used — social media, banking, e-commerce, healthcare records — has a relational database behind it. Not because it is fashionable. Because the problems databases solve are real and recurring, and they show up the moment your application has more than one user and more than one table of data.

If you misunderstand how databases work — if you design your tables poorly, if you miss what primary keys and foreign keys actually enforce, if you write queries that do not match the structure of your data — you will spend weeks debugging data integrity problems that never needed to exist. We are going to make sure that does not happen."

---

## Block 2 — Relational Databases: Introduction and Role in Backend Development

### 2A — What Is a Relational Database?

**[Script:]**

"A database is an organized collection of data that persists independently of any application. A relational database specifically organizes that data into tables — and defines relationships between those tables.

Let us break down what a table actually is, because the word gets used loosely.

A table is a two-dimensional structure. It has columns, which define what kind of data each record holds — a name, an age, an email address. And it has rows, where each row is one complete record — one user, one product, one order.

You have seen this structure before. It looks like a spreadsheet. But a spreadsheet is just a display tool. A relational database is a system that stores, enforces, indexes, and queries that data according to explicit rules. That distinction matters."

> 🎯 **Instructor Note:** Draw this on the whiteboard before continuing. Keep it minimal — three columns, three rows. Label the columns and rows explicitly.

```
Table: users

| id | name    | email               |
|----|---------|---------------------|
| 1  | Alice   | alice@example.com   |
| 2  | Bob     | bob@example.com     |
| 3  | Carol   | carol@example.com   |
```

**[Script:]**

"Each column has a data type. `id` is an integer. `name` is text. `email` is text. The database enforces these types — you cannot put a number into the `email` column accidentally. This is already more protection than a Python dictionary gives you.

Each row is one user. The database gives us tools to find specific rows, add new rows, update existing ones, and delete them. All of that is SQL — Structured Query Language.

SQL is not a general-purpose programming language. It does not have loops in the way Python does. It is a declarative language — you describe what you want, and the database figures out how to get it. That is a different mental model from Python, and it takes a little adjustment."

---

### 2B — Why Relational Databases in Backend Development?

**[Script:]**

"When you write a backend API — whether it is FastAPI, Django, Express, anything — your application is stateless. The Python process has no memory between requests. When a user logs in, your application needs to look up their credentials somewhere that persists. When they place an order, the order needs to be recorded somewhere reliable.

That somewhere is the database. The application and the database are two separate systems that communicate. The application sends SQL queries, the database executes them and returns results.

This separation is deliberate. The application can be restarted, scaled to multiple servers, or rewritten in a different language. The database stays running, holds the state, and answers queries from whatever application connects to it."

> 🎯 **Instructor Note:** Ask the room: "In a web application, the server handles HTTP requests. If you have ten servers all handling requests from the same users, where does the user data live?" The answer is the database — shared, central, separate from the application layer. This is why the separation matters.

**[Script:]**

"The word relational comes from the mathematical concept of a relation — essentially, a table. But in practice, what it means is that tables can relate to each other. A `users` table and an `orders` table can be connected — each order knows which user placed it. That connection is not just a convention; the database can enforce it, query across it, and protect you from broken references.

That is what makes relational databases powerful for backend development. Not just storage — structured, related, enforced storage."

**Common mistakes — name them early:**

> 🎯 **Instructor Note:** These are the mistakes that cause the most confusion later. Say them out loud now so learners can recognize them when they appear.

**[Script:]**

"Two mental model mistakes that come up constantly.

First: thinking of a database table as a Python list of dictionaries. It is structurally similar — a list of rows, each row has the same keys. But a Python list has no types, no constraints, no enforcement. A database table has all of those things. The enforcements are not optional extras — they are the point.

Second: thinking SQL is part of Python. It is not. SQL is its own language that you send to the database as a string. The database executes it. Python just sends the query and receives the result. When something goes wrong with a SQL query, the error comes from the database, not from Python."

**Recap of Block 2 before moving on:**

- A relational database stores data in tables: columns define structure, rows hold records
- Each column has an enforced data type
- SQL is a declarative language — you describe what you want, the database figures out how
- The database is a separate system from the application; they communicate via SQL queries
- Relational means tables can be linked to each other through defined relationships

---

## Block 3 — Table Design: Creation, Constraints, Primary Keys, and Foreign Keys

### 3A — CREATE TABLE and Data Types

**[Script:]**

"Now let us write actual SQL. The first thing you need to be able to do is create a table. This is the `CREATE TABLE` statement.

When you create a table, you define its name and each of its columns. For every column, you give three things: the column name, the data type, and any constraints — rules that the database enforces on that column.

Let us look at the most common data types you will use as a beginner. These are standard SQL types that work in SQLite, PostgreSQL, and MySQL with minor variations."

> 🎯 **Instructor Note:** Write these on the board as you say them. Do not overwhelm — keep it to these four. They cover the majority of real use cases at this level.

```
INTEGER   — whole numbers: user IDs, counts, ages
TEXT      — strings of any length: names, emails, messages
REAL      — decimal numbers: prices, coordinates
BOOLEAN   — true or false (stored as 1/0 in SQLite)
```

**[Script:]**

"For this session we will use SQLite. SQLite is a relational database that stores everything in a single file on disk. It does not require a server, which makes it ideal for learning. The SQL we write is standard SQL — the same concepts and most of the same syntax transfer directly to PostgreSQL or MySQL when you use them in production."

---

### 3B — Constraints

**[Script:]**

"Constraints are rules attached to columns that the database enforces automatically. This is one of the most important ideas in table design. Every time you insert or update a row, the database checks all the constraints on that table. If any constraint is violated, the operation fails with an error. The bad data never gets in.

There are four constraints you need to know right now."

> 🎯 **Instructor Note:** Write each constraint name and a one-line definition on the board before the demo. Learners will see these in code shortly — the board gives them the vocabulary to read the code when it appears.

```
NOT NULL      — this column must always have a value; NULL is not allowed
UNIQUE        — no two rows can have the same value in this column
DEFAULT       — if no value is provided on insert, use this value instead
CHECK         — value must satisfy a specific condition you define
```

**[Script:]**

"These constraints sound simple, but they prevent entire categories of bugs. `NOT NULL` means you cannot accidentally create a user with no email address. `UNIQUE` means you cannot have two users with the same username. The database enforces these rules every time, from every application that connects to it — not just from your Python code. That matters when you have multiple services or multiple developers working on the same database."

---

### 3C — Primary Keys

**[Script:]**

"Every table should have a primary key. A primary key is a column — or in some cases a combination of columns — that uniquely identifies each row. No two rows can have the same primary key value, and a primary key column can never be NULL.

The most common pattern, and the one we will use throughout this session, is an integer primary key that auto-increments. Every time you insert a new row, the database automatically assigns the next available integer as the ID. You do not have to manage it yourself."

> 🎯 **Instructor Note:** Ask: "Why does every row need a unique identifier? Could we just use the user's name or email instead?" Let learners respond. Guide them to: names and emails can change; names are not guaranteed unique; an integer ID is stable, compact, and unambiguous. This sets up the foreign key concept cleanly.

**[Script:]**

"In SQLite, you create an auto-incrementing primary key by declaring the column as `INTEGER PRIMARY KEY`. SQLite treats this specially — it maps directly to the internal row ID and auto-increments without you having to do anything.

Let us write a real `CREATE TABLE` statement."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before showing the demo, ask: "If I try to insert two rows with the same `id` value into a table that has `id` as its primary key, what will the database do?" Answer: it will reject the second insert with a constraint violation error. Make sure learners understand the database enforces this — it is not just a convention.

**Demo 1 — CREATE TABLE with Constraints (whiteboard-friendly)**

```sql
CREATE TABLE users (
    id      INTEGER PRIMARY KEY,
    name    TEXT    NOT NULL,
    email   TEXT    NOT NULL UNIQUE,
    age     INTEGER CHECK(age >= 0)
);
```

**[Script:]**

"Read this top to bottom. The table is called `users`. It has four columns.

`id` is an integer primary key — unique, never null, auto-assigned.
`name` is text and cannot be null — every user must have a name.
`email` is text, cannot be null, and must be unique across all rows — no two users share an email.
`age` is an integer, and a `CHECK` constraint says it must be zero or greater — negative ages are rejected.

Notice that we have not written any Python here. This is pure SQL. The database reads this statement, creates the structure, and from this point forward enforces all of these rules automatically."

> 🎯 **Instructor Note:** Pause here. Ask: "What happens if I run this `CREATE TABLE` statement twice?" Answer: the database throws an error — the table already exists. In practice you would use `CREATE TABLE IF NOT EXISTS` to avoid this. Show that variation:

```sql
CREATE TABLE IF NOT EXISTS users (
    id    INTEGER PRIMARY KEY,
    name  TEXT    NOT NULL,
    email TEXT    NOT NULL UNIQUE
);
```

**[Script:]**

"`IF NOT EXISTS` tells the database: only create this table if it does not already exist. If it does exist, skip this statement without error. This is the safe form to use when your application runs the setup code every time it starts."

---

### 3D — Foreign Keys

**[Script:]**

"Now we have a `users` table. Suppose we want a second table to store orders — each order belongs to one user. How does the `orders` table know which user placed it?

It stores the user's ID. A column in the `orders` table holds the `id` value from the `users` table. That connecting column is called a foreign key.

A foreign key is a column in one table that references the primary key of another table. It creates a link between rows in two different tables. And crucially, the database can enforce that link — it can refuse to let you create an order for a user ID that does not exist."

> 🎯 **Instructor Note:** Draw this on the board. The visual is the fastest way to make this concrete.

```
users                          orders
+---------+--------+           +----+----------+---------+
| id (PK) | name   |           | id | item     | user_id |
+---------+--------+           +----+----------+---------+
| 1       | Alice  |  <------  | 1  | Keyboard | 1       |
| 2       | Bob    |  <------  | 2  | Monitor  | 2       |
+---------+--------+           +----+----------+---------+
                                         FK references users.id
```

**[Script:]**

"The arrow in this diagram is the foreign key relationship. `orders.user_id` references `users.id`. This means:

You cannot insert an order with `user_id = 99` if there is no user with `id = 99`. The database rejects it.
You cannot delete a user who has orders without deciding what to do with those orders first.

This is referential integrity — the database guarantees that your relationships are never broken. An order can never reference a user that does not exist."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before showing the CREATE TABLE for orders, ask: "What should happen if someone deletes a user who has three existing orders? What are the options?" Let learners think. The answer is: you have choices — reject the deletion, delete the orders automatically, or set the foreign key to NULL. We will use `ON DELETE CASCADE` which deletes the orders automatically when the user is deleted. This is one concrete option; name it and explain the choice.

**Demo 2 — Foreign Key (whiteboard-friendly)**

```sql
CREATE TABLE orders (
    id      INTEGER PRIMARY KEY,
    item    TEXT    NOT NULL,
    user_id INTEGER NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

**[Script:]**

"Read the last line. `FOREIGN KEY (user_id)` — the column in this table that is the foreign key. `REFERENCES users(id)` — it points to the `id` column of the `users` table. `ON DELETE CASCADE` — if the referenced user is deleted, all their orders are deleted automatically.

The foreign key declaration goes at the end of the column list, after all the column definitions. It is a table-level constraint, not a column-level one.

One important note for SQLite specifically: SQLite does not enforce foreign keys by default. You have to turn enforcement on with a pragma — a SQLite-specific command — at the start of your connection."

```sql
PRAGMA foreign_keys = ON;
```

**[Script:]**

"Run this once when you open a SQLite connection and foreign key enforcement is active for that connection. If you forget it, SQLite will happily let you insert orders with invalid user IDs and you will spend a long time debugging data integrity issues that the database should have caught. PostgreSQL enforces foreign keys by default — this is a SQLite-specific quirk worth knowing."

> 🎯 **Instructor Note:** This is a real confusion point. Learners sometimes test foreign key violations in SQLite, see no error, and conclude foreign keys do not work. The pragma is the cause almost every time. Name it clearly, and suggest they write it as the first line of any SQLite setup code.

**Recap of Block 3 before moving on:**

- `CREATE TABLE` defines a table's columns, data types, and constraints
- Constraints — `NOT NULL`, `UNIQUE`, `DEFAULT`, `CHECK` — are enforced automatically on every write
- A primary key uniquely identifies each row; `INTEGER PRIMARY KEY` in SQLite auto-increments
- A foreign key is a column that references the primary key of another table
- `REFERENCES` creates the link; `ON DELETE CASCADE` defines what happens when the parent row is deleted
- In SQLite, run `PRAGMA foreign_keys = ON` to activate foreign key enforcement

---

## Block 4 — SQL Fundamentals: SELECT and INSERT

### 4A — INSERT: Adding Rows to a Table

**[Script:]**

"We have a table. Now we need data in it. The `INSERT` statement adds a new row.

The basic form is: `INSERT INTO` the table name, then `VALUES` followed by the values in the same order as the columns were defined. Let me show you the full syntax first, then we will break it down."

> 🎯 **Instructor Note:** Write the two forms of INSERT on the board side by side — positional and named. Learners need to see both because they will encounter both in real code and documentation.

```sql
-- Positional form: values match column order exactly
INSERT INTO users VALUES (1, 'Alice', 'alice@example.com', 28);

-- Named form: specify columns explicitly, order does not matter
INSERT INTO users (name, email, age) VALUES ('Alice', 'alice@example.com', 28);
```

**[Script:]**

"In the positional form, you provide a value for every column in the exact order they were defined. This is fragile — if someone adds a column to the table later, your insert breaks.

In the named form, you list the columns you are providing values for, then list the corresponding values. Columns you omit get their default value, or NULL if no default is defined.

Notice that in the named form we omitted `id`. Because `id` is `INTEGER PRIMARY KEY`, SQLite assigns the next available integer automatically. We do not need to supply it.

Always prefer the named form in real code. It is explicit, it does not break when columns are added, and it is much easier to read."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I try to insert a user with no email, what will the database do?" Answer: reject the insert with a NOT NULL constraint error. Then ask: "If I insert two users with the same email?" Answer: reject the second insert with a UNIQUE constraint error. Let learners predict both before you demonstrate.

**Demo 3 — INSERT with Constraint Violations (whiteboard-friendly)**

```sql
-- Valid insert
INSERT INTO users (name, email, age) VALUES ('Alice', 'alice@example.com', 28);
INSERT INTO users (name, email, age) VALUES ('Bob',   'bob@example.com',   34);

-- This will fail: email is NOT NULL
INSERT INTO users (name, age) VALUES ('Carol', 22);

-- This will fail: email must be UNIQUE
INSERT INTO users (name, email, age) VALUES ('Dave', 'alice@example.com', 41);
```

**[Script:]**

"Run the first two inserts. Both succeed. Now run the third one — the one with no email. The database returns an error: `NOT NULL constraint failed: users.email`. The row was not inserted. The table has exactly two rows.

Now run the fourth one. `alice@example.com` already belongs to Alice. Dave cannot use the same email. The database returns: `UNIQUE constraint failed: users.email`. Again, the row was not inserted.

This is the value of constraints. You do not have to write Python code to check whether an email is already taken. The database checks it for you, every time, from any application that connects to it."

> 🎯 **Instructor Note:** Pause here and ask: "Where would you have had to write that duplicate-email check if the database did not do it for you?" Guide learners to: in every API endpoint that creates a user, in every script that imports users, in every background job that creates accounts. The database does it once, centrally. That is the point.

---

### 4B — SELECT: Querying Rows from a Table

**[Script:]**

"Now that we have data in the table, we need to get it back out. That is the `SELECT` statement — the most used statement in SQL.

The basic form is: `SELECT` the columns you want, `FROM` the table name. Let us start with the simplest possible select and build from there."

> 🎯 **Instructor Note:** Write the progression on the board as you go through it. Start with `SELECT *`, then move to named columns, then `WHERE`. Do not jump ahead — each step is a distinct concept.

**[Script:]**

"The asterisk in `SELECT *` means all columns. It returns every column for every row in the table. This is useful when you are exploring data interactively, but avoid it in application code — if the table structure changes, your application receives unexpected data."

```sql
SELECT * FROM users;
```

**[Script:]**

"To get specific columns, name them after `SELECT`, separated by commas."

```sql
SELECT name, email FROM users;
```

**[Script:]**

"This returns only the `name` and `email` columns. The `id` and `age` columns are excluded. The database only fetches and returns what you ask for.

Now, most of the time you do not want every row — you want specific rows that match a condition. That is the `WHERE` clause."

---

### 4C — SELECT with WHERE

**[Script:]**

"The `WHERE` clause filters which rows are returned. You write a condition — a comparison, a value check, a combination of checks — and the database only returns rows where that condition is true.

Think of it like Python's list comprehension filter: `[u for u in users if u['age'] > 25]`. The `WHERE` clause does the same thing, but inside the database, before the data is sent back to you."

> 🎯 **Instructor Note:** That Python comparison lands well with this audience. Write both on the board so learners can map the mental model directly.

```python
# Python equivalent
[u for u in users if u['age'] > 25]
```

```sql
-- SQL equivalent
SELECT * FROM users WHERE age > 25;
```

**[Script:]**

"The comparison operators in SQL are what you already know: `=` for equality, `>`, `<`, `>=`, `<=`, and `!=` or `<>` for not equal. String comparisons use single quotes. Do not use double quotes for string values in SQL — double quotes are for column and table names in most databases."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Assume the users table has Alice (age 28), Bob (age 34), and Carol (age 22) after you have inserted all three successfully. Before the demo, ask: "If I run `SELECT * FROM users WHERE age > 25`, how many rows come back? Which ones?" Let learners reason through it. Answer: two rows — Alice and Bob. Carol is 22, which is not greater than 25.

**Demo 4 — SELECT with WHERE (whiteboard-friendly)**

```sql
-- All rows, all columns
SELECT * FROM users;

-- Specific columns only
SELECT name, email FROM users;

-- Filtered rows
SELECT name, age FROM users WHERE age > 25;

-- Exact match
SELECT * FROM users WHERE email = 'alice@example.com';
```

**[Script:]**

"Run these one at a time. The first returns every row and column. The second returns every row but only name and email. The third returns only Alice and Bob — the two users over 25. The fourth returns exactly one row — only Alice, because only one row has that exact email.

Notice that `WHERE email = 'alice@example.com'` returns at most one row, because email has a `UNIQUE` constraint. The database knows this too — it can use that information to find the row more efficiently.

Let us look at combining conditions. You can chain multiple conditions with `AND` and `OR`."

```sql
SELECT * FROM users WHERE age > 25 AND age < 35;
```

**[Script:]**

"This returns users whose age is between 25 and 35 exclusive — in our data, that is Alice at 28. Bob is 34, which is also between those bounds, so he comes back too. Think through it carefully — `AND` means both conditions must be true simultaneously."

> 🎯 **Instructor Note:** This is a genuine pause point. Ask: "What is the difference between `WHERE age > 25 AND age < 35` and `WHERE age > 25 OR age < 35`? Walk through what each returns with our sample data." Let learners work through both. The `OR` version returns almost everyone, because nearly any age satisfies at least one of those conditions. This is a concept that trips learners up in more complex queries later.

---

### 4D — SELECT with ORDER BY and LIMIT

**[Script:]**

"Two more clauses that you will use constantly: `ORDER BY` to sort results, and `LIMIT` to cap how many rows come back.

`ORDER BY` takes a column name and optionally `ASC` for ascending or `DESC` for descending. Ascending is the default — you can omit it."

```sql
SELECT name, age FROM users ORDER BY age DESC;
SELECT name, age FROM users ORDER BY age ASC;
```

**[Script:]**

"`LIMIT` tells the database to return at most that many rows. This is essential in real applications — you never want to return every row from a large table to the client. Pagination, top-N queries, and any kind of bounded result set use `LIMIT`."

```sql
SELECT name, age FROM users ORDER BY age DESC LIMIT 1;
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With Alice at 28, Bob at 34, and Carol at 22, what does `SELECT name, age FROM users ORDER BY age DESC LIMIT 1` return?" Answer: Bob — one row, the oldest user. This is the pattern for finding the maximum without using an aggregate function. Let learners predict before you confirm.

**[Script:]**

"This is a useful pattern: `ORDER BY` a column descending, `LIMIT 1`, and you get the row with the highest value in that column. The oldest user, the most recent order, the highest-priced item — all of these follow the same pattern."

---

### 4E — Connecting INSERT and SELECT Together

**[Script:]**

"Let us put INSERT and SELECT together in one coherent sequence so you can see the full cycle. We insert data, then immediately query it to verify it was stored correctly. This is the most basic operation loop in database work — write, then read."

**Demo 5 — Full INSERT then SELECT cycle (whiteboard-friendly)**

```sql
INSERT INTO users (name, email, age) VALUES ('Carol', 'carol@example.com', 22);

SELECT * FROM users;

SELECT name FROM users WHERE age < 25;
```

**[Script:]**

"Insert Carol, then select everything — three rows now. Then select names where age is under 25 — only Carol comes back.

This cycle — insert data, immediately verify with a select — is how you test your table design and your constraints interactively. When you are building an application, you will write inserts from Python and selects from Python, but the SQL underneath is exactly what you are writing here. Understanding what the SQL does is what lets you debug it when the Python layer gives you unexpected results."

> 🎯 **Instructor Note:** End Block 4 with this grounding statement. Learners sometimes feel that SQL is a secondary concern because they will use a library on top of it. Correct this now.

**[Script:]**

"One thing to be clear about: even if you use a Python library that generates SQL for you, understanding the SQL that gets generated is essential for debugging performance problems, understanding why a query returns unexpected results, and designing your tables in a way that actually supports the queries your application needs to run. The SQL is not hidden from you — it is what the library is doing on your behalf."

**Recap of Block 4 before moving on:**

- `INSERT INTO table (columns) VALUES (values)` adds a new row; use the named form
- Omitting a primary key column lets the database auto-assign it
- Constraints are checked on every insert — violations produce an error and the row is not stored
- `SELECT columns FROM table` retrieves rows; `*` means all columns
- `WHERE condition` filters which rows are returned
- `ORDER BY column ASC/DESC` sorts results; `LIMIT n` caps the number of rows returned
- INSERT and SELECT work together as the write-read cycle for verifying stored data

---

## Block 5 — Lecture Summary

> 🎯 **Instructor Note:** Deliver these conversationally. Ask learners to supply the answers before you say them. "What does a primary key do? What constraint ensures no two rows share the same value? What clause do we use to filter rows in a SELECT?" Use this as active recall, not reading time.

**Relational Databases — Introduction and Role in Backend Development**

- A relational database stores data persistently in tables: columns define structure and type, rows hold individual records
- SQL is a declarative language — you describe what data you want; the database determines how to retrieve it
- The database is a separate system from the application; they communicate via SQL queries sent over a connection
- Relational means tables can be linked to each other through defined, enforced relationships
- Backend applications are stateless; the database holds the state that persists across requests, restarts, and scale-out

**Table Design — Creation, Constraints, Primary Keys, and Foreign Keys**

- `CREATE TABLE` defines a table's name, columns, data types, and constraints
- Common data types: `INTEGER`, `TEXT`, `REAL`, `BOOLEAN`
- Constraints enforce data rules automatically on every write: `NOT NULL`, `UNIQUE`, `DEFAULT`, `CHECK`
- A primary key uniquely identifies each row and cannot be NULL; `INTEGER PRIMARY KEY` in SQLite auto-increments
- A foreign key is a column that references the primary key of another table, enforcing referential integrity
- `REFERENCES table(column)` declares the link; `ON DELETE CASCADE` is one option for handling parent row deletion
- In SQLite, foreign key enforcement requires `PRAGMA foreign_keys = ON` at connection time
- `CREATE TABLE IF NOT EXISTS` is the safe form to use in application setup code

**SQL Fundamentals — SELECT and INSERT**

- `INSERT INTO table (columns) VALUES (values)` adds a new row; always use the named column form in application code
- Constraints are checked on every insert; a violated constraint produces an error and the row is rejected
- `SELECT columns FROM table` retrieves data; list specific column names rather than `*` in application code
- `WHERE condition` filters rows using comparison operators: `=`, `>`, `<`, `>=`, `<=`, `!=`
- Multiple conditions combine with `AND` (both must be true) and `OR` (either must be true)
- `ORDER BY column DESC` sorts results descending; `ASC` is ascending and is the default
- `LIMIT n` caps the number of rows returned — essential for any production query on a large table

**Why All of This Matters Together**

- Table design decisions — which constraints you set, how you define your primary keys, how you link tables with foreign keys — determine whether your application's data stays consistent automatically or requires constant manual checking in application code
- Writing clear INSERT and SELECT statements, and understanding what the database enforces versus what your application must check, is the foundation of every backend feature that touches stored data
- Every form submission, login check, product listing, and order confirmation in a real application is ultimately one or more of the SQL operations covered here — understanding them at this level means you can build, debug, and reason about any data layer you encounter

---

*End of script.*
