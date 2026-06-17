# Lecture Script: Advanced SQL and Database Connectivity
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Joins — INNER, LEFT, RIGHT, FULL | 25 min |
| 3 | Aggregation Functions and GROUP BY | 20 min |
| 4 | Indexes and Query Optimization | 15 min |
| 5 | Connecting Python to Databases | 15 min |
| 6 | Sorting and Pagination | 12 min |
| 7 | Transactions and ACID Properties | 10 min |
| 8 | Lecture Summary and Recap | 5 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience already knows table creation, constraints, primary and foreign keys, and the four basic SQL operations. The hook should move past "databases are useful" straight into "single-table queries are not enough for real applications." Open with a concrete multi-table, multi-user scenario. Wait after the opening question.

**[Script:]**

"You can already create tables, insert data, and query a single table with WHERE. Here is what that does not cover.

A product manager asks: how many orders did each customer place last month, sorted from highest to lowest, twenty results per page? That single question touches a join across two tables, an aggregation, a sort, and pagination — four things you do not yet have.

A support engineer says the orders page used to load instantly and now takes eight seconds because the table has grown to two million rows. That is a query optimization and indexing problem.

Your Python application needs to actually talk to the database — not through a SQL shell, but from code, executing queries and getting results back as Python data. That is database connectivity.

And finally: a payment is being processed. The order needs to be marked paid and the inventory count needs to be decremented. If the server crashes between those two steps, you now have a paid order with no inventory deducted, or worse, deducted inventory with no recorded payment. That is the kind of problem transactions exist to prevent.

Every one of these is something you will hit the moment your application has more than one table and more than a handful of users. Today covers all of it: joins, aggregation, indexing, Python connectivity, sorting and pagination, and transactions. By the end you can answer the product manager's question, diagnose the slow page, connect your code to the database, and guarantee that multi-step operations do not leave your data half-finished."

---

## Block 2 — Joins: INNER, LEFT, RIGHT, FULL

### 2A — Why Joins Exist

**[Script:]**

"A foreign key links two tables. A JOIN is how you actually retrieve data across that link in one query instead of querying each table separately and matching rows yourself in application code.

A JOIN combines rows from two tables based on a matching condition — almost always the foreign key relationship. The result is a temporary combined table that exists only for the duration of the query."

> 🎯 **Instructor Note:** Write this exact data set on the board and keep it visible for the entire block. The consistent data is what makes the differences between join types visible.

```
users                          orders
+----+-------+                 +----+----------+---------+
| id | name  |                 | id | item     | user_id |
+----+-------+                 +----+----------+---------+
| 1  | Alice |                 | 1  | Keyboard | 1       |
| 2  | Bob   |                 | 2  | Monitor  | 1       |
| 3  | Carol |                 | 3  | Mouse    | 2       |
+----+-------+                 | 4  | Webcam   | 99      |
                               +----+----------+---------+
```

**[Script:]**

"Two intentional details. Carol has no orders. The webcam's `user_id` is 99 — no matching user. These edge cases exist in real databases and the join type you choose determines whether they show up in your results."

**[Script:]**

"The basic syntax: `SELECT` columns, `FROM` the first table, `JOIN` the second table `ON` the matching condition."

```sql
SELECT users.name, orders.item
FROM   users
JOIN   orders ON users.id = orders.user_id;
```

**[Script:]**

"`table.column` is a qualified reference — needed because both tables might have a column with the same name, like `id`. Without the `ON` condition, the database produces every possible combination of rows from both tables — a Cartesian product, almost always a mistake and a performance disaster on large tables."

---

### 2B — INNER JOIN

**[Script:]**

"INNER JOIN returns only rows with a match on both sides. No match on either side means the row is excluded."

> 🎯 **Instructor Note:** Draw a Venn diagram with only the overlap shaded. Keep this diagram on the board and add shading as you move through each join type.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With our data, which rows appear in an INNER JOIN on `users.id = orders.user_id`? What happens to Carol? What happens to the webcam?" Answer: Alice and Bob appear with their orders. Carol is excluded — no orders. The webcam is excluded — no matching user.

**Demo 1 — INNER JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
INNER JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Three rows: Alice-Keyboard, Alice-Monitor, Bob-Mouse. INNER JOIN is the default — plain `JOIN` without a keyword means INNER JOIN — but always write it explicitly for readability."

---

### 2C — LEFT JOIN

**[Script:]**

"LEFT JOIN returns every row from the left table — the one in `FROM` — plus matching rows from the right table. Unmatched left rows still appear, with NULL for every right-table column."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With users as the left table, what changes from the INNER JOIN result? What happens to Carol?" Answer: Carol now appears, with NULL in the item column. The webcam still does not appear — LEFT JOIN does not preserve unmatched right-side rows.

**Demo 2 — LEFT JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
LEFT JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Four rows now — the three matched plus Carol with a NULL item.

A useful pattern: LEFT JOIN combined with `WHERE orders.user_id IS NULL` finds rows in the left table with no match at all — in this case, customers who have never placed an order."

```sql
SELECT users.name
FROM   users
LEFT JOIN orders ON users.id = orders.user_id
WHERE  orders.user_id IS NULL;
```

> 🎯 **Instructor Note:** Pause here. This pattern is genuinely useful and worth a second of emphasis: "find records with no related data" is one of the most common real-world query needs, and this is how you write it.

---

### 2D — RIGHT JOIN and FULL JOIN

**[Script:]**

"RIGHT JOIN is the mirror — every row from the right table, plus matches from the left, NULL where there is no match on the left."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "In a RIGHT JOIN with orders as the right table, does the webcam appear? Does Carol?" Answer: the webcam appears with NULL for the user name. Carol does not — she has no orders, and orders is the table being fully preserved.

**Demo 3 — RIGHT JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
RIGHT JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Four rows: the three matched, plus NULL-Webcam.

In practice, RIGHT JOIN is rarely used — most developers write LEFT JOIN and swap the table order instead. If you see RIGHT JOIN in real code, you can mentally rewrite it as a LEFT JOIN with the FROM and JOIN tables reversed.

FULL JOIN returns every row from both tables — matched where possible, NULL-padded on whichever side lacks a match. It is the union of LEFT and RIGHT."

```sql
SELECT users.name, orders.item
FROM   users
FULL JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Five rows with our data: the three matched, Carol with a NULL item, and a NULL name paired with the webcam.

Important: SQLite does not support FULL JOIN natively. PostgreSQL does. If you need this in SQLite, you simulate it with a UNION of a LEFT JOIN and a RIGHT JOIN. Know the concept; know where it is supported."

**Recap of Block 2 before moving on:**

- INNER JOIN returns only matched rows from both tables
- LEFT JOIN returns all left-table rows, NULL on the right where unmatched
- RIGHT JOIN returns all right-table rows, NULL on the left where unmatched; logically equivalent to LEFT JOIN with tables swapped
- FULL JOIN returns all rows from both tables; not supported natively in SQLite
- LEFT JOIN plus `WHERE right_table.column IS NULL` finds records with no related data — a frequently used pattern

---

## Block 3 — Aggregation Functions and GROUP BY

### 3A — Aggregate Functions

**[Script:]**

"Aggregate functions compute one summary value across many rows. `COUNT` counts rows. `SUM` totals a numeric column. `AVG` computes the mean. `MIN` and `MAX` find the smallest and largest values. All five ignore NULL except `COUNT(*)`, which counts every row regardless."

> 🎯 **Instructor Note:** Establish this data set and keep it visible through Block 3.

```
orders
+----+----------+---------+-------+----------+
| id | item     | user_id | price | category |
+----+----------+---------+-------+----------+
| 1  | Keyboard | 1       | 49.99 | Hardware |
| 2  | Monitor  | 1       | 199.99| Hardware |
| 3  | Mouse    | 2       | 29.99 | Hardware |
| 4  | Python   | 2       | 39.99 | Books    |
| 5  | SQL Book | 3       | 34.99 | Books    |
| 6  | Webcam   | 3       | 89.99 | Hardware |
+----+----------+---------+-------+----------+
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What does `SELECT COUNT(*), SUM(price), AVG(price) FROM orders` return for this table?" Let learners estimate before confirming: count 6, sum 444.94, average about 74.16.

**Demo 4 — Aggregate functions (whiteboard-friendly)**

```sql
SELECT
    COUNT(*)   AS total_orders,
    SUM(price) AS total_revenue,
    AVG(price) AS avg_price,
    MIN(price) AS cheapest,
    MAX(price) AS most_expensive
FROM orders;
```

**[Script:]**

"One result row, five computed values. This is the query behind a dashboard summary card. `COUNT(*)` counts all rows including any with NULL columns; `COUNT(column)` would only count rows where that specific column is not NULL — worth knowing if your data has gaps."

---

### 3B — GROUP BY

**[Script:]**

"Aggregates so far summarize the whole table. `GROUP BY` divides rows into groups by a column's value and runs the aggregate independently per group. Think of it as a Python dictionary group-by: bucket the rows by key, then compute something per bucket."

**[Script:]**

"There is one strict rule. Every column in `SELECT` must be either in `GROUP BY` or inside an aggregate function. You cannot select a column that represents one specific row when the group itself collapses many rows into one result row — there is no single correct value to show."

> 🎯 **Instructor Note:** Write the rule on the board and keep it visible.

```
With GROUP BY: every SELECT column is either
  (a) in the GROUP BY clause, or
  (b) inside an aggregate function
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Grouping by category, what totals do you expect? Hardware has four orders, Books has two." Let learners compute: Hardware = 49.99+199.99+29.99+89.99 = 369.96. Books = 39.99+34.99 = 74.98.

**Demo 5 — GROUP BY (whiteboard-friendly)**

```sql
SELECT
    category,
    COUNT(*)   AS order_count,
    SUM(price) AS total_revenue
FROM orders
GROUP BY category
ORDER BY total_revenue DESC;
```

**[Script:]**

"Two rows: Hardware with 4 orders and 369.96 total, Books with 2 orders and 74.98 total — sorted highest revenue first. This single query structure — group by a column, aggregate, sort — produces the category breakdown behind any analytics report."

> 🎯 **Instructor Note:** Briefly mention HAVING exists for filtering grouped results by an aggregate condition (e.g., categories with more than 2 orders), but do not demo it — it is outside today's listed objectives. Naming it prevents learners from wondering why WHERE does not work on aggregate values if they try it themselves.

**Recap of Block 3 before moving on:**

- `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` compute a single summary value across rows; all ignore NULL except `COUNT(*)`
- `GROUP BY column` buckets rows by that column's value and runs aggregates independently per bucket
- Every `SELECT` column with `GROUP BY` must be grouped or aggregated — no exceptions
- `GROUP BY` combines naturally with `ORDER BY` on an aggregated alias to rank results

---

## Block 4 — Indexes and Query Optimization

### 4A — What an Index Is

**[Script:]**

"As tables grow, queries that scan every row get slow. An index is a separate data structure the database maintains alongside a table, built on one or more columns, that lets the database find matching rows without scanning the entire table.

The analogy that works well: a book has no index, and you want every page mentioning a topic — you read the whole book. A book with an index lets you jump straight to the relevant pages. A database index is the same idea, maintained automatically as data changes."

> 🎯 **Instructor Note:** Ask: "If a table has a million rows and no index, what does the database have to do to find rows where `email = 'alice@example.com'`?" Answer: scan all one million rows, checking each one — a full table scan. This is the cost an index avoids.

**[Script:]**

"Primary keys are indexed automatically — that is part of why primary key lookups are fast even on large tables. Other columns are not indexed unless you create an index explicitly."

```sql
CREATE INDEX idx_orders_user_id ON orders(user_id);
```

**[Script:]**

"This creates an index on the `user_id` column of `orders`. Any query filtering or joining on `user_id` can now use the index instead of scanning the whole table. Foreign key columns are excellent index candidates precisely because they are used constantly in JOIN conditions."

---

### 4B — When to Index, and the Cost of Indexing

**[Script:]**

"Index columns that appear frequently in `WHERE` clauses, `JOIN` conditions, or `ORDER BY` clauses. Do not index every column reflexively — indexes are not free.

Every index speeds up reads but slows down writes. Every `INSERT`, `UPDATE`, or `DELETE` must also update every index on that table. A table with ten indexes pays that cost ten times on every write. Indexes also consume disk space.

The practical guidance for a beginner: index foreign keys, index columns you filter on often, and do not index small tables — a full scan of a hundred rows is already fast, and the index overhead is not worth it."

> 🎯 **Instructor Note:** Pause here. Ask: "If you index every single column in a frequently-written table, what happens to write performance?" Guide learners to: it degrades, because every write updates every index. Indexing is a tradeoff, not a free performance upgrade.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "After creating an index on `user_id`, does the query `SELECT * FROM orders WHERE user_id = 1` need to change?" Answer: no. The SQL is identical. The database decides whether to use the index automatically. This is an important point — indexing is invisible to query syntax.

**Demo 6 — EXPLAIN to see optimization in action (whiteboard-friendly)**

```sql
EXPLAIN QUERY PLAN
SELECT * FROM orders WHERE user_id = 1;
```

**[Script:]**

"`EXPLAIN QUERY PLAN` in SQLite shows you how the database intends to execute a query — whether it scans the full table or uses an index. Without the index on `user_id`, this shows a full table scan. After creating the index, it shows the database using the index instead. This is how you verify an index is actually being used, rather than assuming it."

**Recap of Block 4 before moving on:**

- An index is a separate structure that lets the database find rows without scanning the entire table
- Primary keys are indexed automatically; other columns require an explicit `CREATE INDEX`
- Index columns used frequently in `WHERE`, `JOIN`, and `ORDER BY` — foreign keys are strong candidates
- Every index speeds up reads but slows down writes and consumes disk space — index deliberately, not reflexively
- `EXPLAIN QUERY PLAN` shows whether a query is using an index or performing a full table scan

---

## Block 5 — Connecting Python to Databases

### 5A — Why Python Needs a Database Driver

**[Script:]**

"Everything so far has been SQL run directly against the database. A real application runs that SQL from Python code, sends parameters safely, and receives results as Python data structures.

Python connects to a database through a driver — a library that implements a standard interface for executing SQL and retrieving results. For SQLite, this driver — `sqlite3` — is built into the Python standard library. No installation required."

---

### 5B — The Connect, Cursor, Execute Pattern

**[Script:]**

"The core pattern across nearly all Python database libraries is consistent: open a connection, create a cursor from that connection, use the cursor to execute SQL, then fetch results."

> 🎯 **Instructor Note:** Write this pattern on the board before the demo. It is the shape learners will recognize in every database library they use afterward.

```
connection = connect to the database
cursor     = connection.cursor()
cursor.execute(sql, parameters)
results    = cursor.fetchall() / fetchone()
connection.commit()   # for writes
connection.close()
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I run `cursor.execute('SELECT * FROM users')` and then immediately try to print the result of `execute()` directly, what do I get?" Answer: `execute()` does not return the rows — it returns the cursor object itself (or None depending on the library). You must call `fetchall()` or `fetchone()` afterward to retrieve the actual data.

**Demo 7 — Connecting with sqlite3 (whiteboard-friendly)**

```python
import sqlite3

connection = sqlite3.connect("myapp.db")
cursor = connection.cursor()

cursor.execute("SELECT name, email FROM users WHERE age > ?", (25,))
rows = cursor.fetchall()

for row in rows:
    print(row)

connection.close()
```

**[Script:]**

"`sqlite3.connect('myapp.db')` opens a connection to the file. `cursor.execute(sql, parameters)` — note the second argument: a tuple of parameters, with `?` placeholders in the SQL string. This is a parameterized query — the same protection against SQL injection that we relied on with the ORM. Never build the SQL string by inserting the value directly with an f-string.

`fetchall()` returns a list of tuples — one tuple per row, in column order. `fetchone()` returns a single tuple or `None` if there are no more rows. Always close the connection when done, or use a context manager to handle this automatically."

> 🎯 **Instructor Note:** This is the single most important safety point in this block. Ask: "Why use `?` placeholders and a parameters tuple instead of an f-string with the value inserted directly?" Answer: f-strings build the SQL by direct text substitution — vulnerable to SQL injection if the value comes from user input. Parameterized queries send the value separately from the SQL structure; the database treats it strictly as data, never as executable SQL.

**[Script:]**

"For INSERT, UPDATE, and DELETE, you call `connection.commit()` after `execute()` to actually save the change. Without `commit()`, the change exists only in the current transaction and is lost if the connection closes without committing."

```python
cursor.execute("INSERT INTO users (name, email) VALUES (?, ?)", ("Dave", "dave@example.com"))
connection.commit()
```

**Recap of Block 5 before moving on:**

- Python connects to a database through a driver; `sqlite3` is built into the standard library
- The pattern is universal: connect, create a cursor, execute, fetch, commit for writes, close
- Always use parameterized queries — `?` placeholders with a separate parameters tuple — never f-string interpolation into SQL
- `fetchall()` returns all matching rows as a list of tuples; `fetchone()` returns a single tuple or `None`
- `commit()` is required after INSERT, UPDATE, or DELETE for the change to be saved

---

## Block 6 — Sorting and Pagination

### 6A — Sorting with ORDER BY

**[Script:]**

"Sorting is `ORDER BY` — already familiar from prior SQL work. The combination worth reinforcing here is sorting alongside pagination, because together they are how every list-view endpoint in a real API works: sorted, broken into pages."

```sql
SELECT name, price FROM orders ORDER BY price DESC;
```

---

### 6B — Pagination with LIMIT and OFFSET

**[Script:]**

"`LIMIT` caps how many rows come back. `OFFSET` skips a number of rows before starting to return results. Together, they implement pagination — returning one page of results at a time instead of the entire table.

The formula: for page number `p` with `page_size` rows per page, `OFFSET = (p - 1) * page_size`, and `LIMIT = page_size`."

> 🎯 **Instructor Note:** Write the formula on the board. This is the part learners forget under pressure and get the off-by-one wrong.

```
page 1: OFFSET 0,  LIMIT 10   (rows 1–10)
page 2: OFFSET 10, LIMIT 10   (rows 11–20)
page 3: OFFSET 20, LIMIT 10   (rows 21–30)

OFFSET = (page_number - 1) * page_size
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With six orders sorted by price ascending and a page size of 2, what does page 2 contain?" Walk through the sort order first, then apply OFFSET 2, LIMIT 2 to find rows 3 and 4 in that sorted order.

**Demo 8 — Sorting and pagination together (whiteboard-friendly)**

```sql
SELECT item, price
FROM   orders
ORDER BY price ASC
LIMIT 2 OFFSET 2;
```

**[Script:]**

"Read this in execution order: sort all orders by price ascending, skip the first two, return the next two. This is page 2 of a 2-per-page listing.

Important: always pair `LIMIT`/`OFFSET` with an `ORDER BY`. Without a defined sort order, the database does not guarantee a consistent row order between queries — page 2 could return different rows on different requests, or overlap with page 1. The sort makes pagination deterministic."

> 🎯 **Instructor Note:** This is a real, common bug. Pause and ask: "What could go wrong if you paginate without an ORDER BY?" Answer: rows might be returned in a different order on each query, leading to duplicate or missing rows across pages from the user's perspective. Always sort before paginating.

**Recap of Block 6 before moving on:**

- `ORDER BY` sorts results; combine with pagination for predictable page contents
- `LIMIT n` caps the number of rows returned; `OFFSET m` skips the first `m` rows
- `OFFSET = (page_number - 1) * page_size` is the standard pagination formula
- Always pair `LIMIT`/`OFFSET` with `ORDER BY` — without a defined sort, page contents are not guaranteed to be consistent

---

## Block 7 — Transactions and ACID Properties

### 7A — What a Transaction Is

**[Script:]**

"A transaction is a group of one or more SQL operations that the database treats as a single unit. Either every operation in the transaction succeeds and is saved together, or if any operation fails, the entire group is rolled back as if none of it happened.

This matters whenever an operation requires multiple steps that must all succeed together. Transferring money between two accounts: subtract from one, add to the other. If the server crashes after the subtraction but before the addition, money has vanished — unless both steps were inside one transaction."

> 🎯 **Instructor Note:** Use the money transfer example explicitly — it is the canonical illustration and learners retain it well. Draw the two-step operation and the failure point on the board.

```
BEGIN TRANSACTION
  UPDATE accounts SET balance = balance - 100 WHERE id = 1;
  UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

If anything fails between BEGIN and COMMIT: ROLLBACK — neither update is saved.
```

---

### 7B — ACID Properties

**[Script:]**

"ACID is the set of four guarantees a transactional database provides. Each letter is a property."

> 🎯 **Instructor Note:** Write all four letters and one-line definitions on the board together. This is reference material learners will want to see as a set, not one at a time.

```
A — Atomicity:    all operations in a transaction succeed, or none do
C — Consistency:  the database moves from one valid state to another valid state
I — Isolation:    concurrent transactions do not interfere with each other
D — Durability:   once committed, the change survives a crash or restart
```

**[Script:]**

"Atomicity is the all-or-nothing guarantee — the bank transfer example. If either UPDATE fails, both are rolled back.

Consistency means the database's constraints — NOT NULL, UNIQUE, foreign keys, CHECK — are never violated, even mid-transaction. A transaction that would leave the database in an invalid state is rejected.

Isolation means that if two transactions run at the same time, neither sees the other's incomplete, in-progress changes. Each transaction behaves as though it has the database to itself until it commits.

Durability means once `COMMIT` succeeds, the change is permanent — written to disk, recoverable even if the server crashes immediately after."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "In the bank transfer example, if the second UPDATE fails due to a constraint violation, what happens to the first UPDATE that already ran?" Answer: it is rolled back too. Atomicity guarantees the whole transaction is undone, not just the failing step.

**Demo 9 — Transaction with rollback (whiteboard-friendly)**

```sql
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;
```

**[Script:]**

"In SQLite and most databases, `BEGIN TRANSACTION` starts the unit. Both UPDATEs run. `COMMIT` saves both permanently. If you instead ran `ROLLBACK` after the UPDATEs — or if an error occurred and the application issued a rollback — neither UPDATE would be saved. The balances would be exactly as they were before the transaction began.

In Python with `sqlite3`, this maps directly: operations executed through a connection are part of an implicit transaction until you call `connection.commit()`. If something goes wrong before `commit()`, you can call `connection.rollback()` to undo everything executed since the transaction began."

> 🎯 **Instructor Note:** Connect back to Block 5 here explicitly. Say: "Remember `connection.commit()` from the Python connectivity block — that was always closing a transaction. Every write you executed there was implicitly part of one." This ties the new ACID concept to code learners already saw working.

**Recap of Block 7 before moving on:**

- A transaction groups multiple operations into one unit — all succeed together or all are rolled back
- Atomicity: all-or-nothing execution of the operations in a transaction
- Consistency: the database never moves into a state that violates its constraints
- Isolation: concurrent transactions do not see each other's incomplete changes
- Durability: once committed, a change survives a crash or restart
- In Python, operations are implicitly transactional until `commit()`; `rollback()` undoes uncommitted changes

---

## Block 8 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as rapid active recall given limited time. Ask before confirming. "What does LEFT JOIN guarantee that INNER JOIN does not? What rule governs SELECT columns under GROUP BY? What is the tradeoff of adding an index? What must always accompany LIMIT/OFFSET? What does the A in ACID stand for?"

**Joins — INNER, LEFT, RIGHT, FULL**

- INNER JOIN returns only matched rows; LEFT and RIGHT preserve one side fully with NULLs on the other; FULL preserves both sides
- LEFT JOIN combined with `WHERE right_table.column IS NULL` finds records with no related data
- SQLite does not support FULL JOIN natively

**Aggregation Functions and GROUP BY**

- `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` compute summary values; `GROUP BY` runs them independently per group
- Every `SELECT` column under `GROUP BY` must be grouped or aggregated

**Indexes and Query Optimization**

- An index lets the database find rows without scanning the full table; primary keys are indexed automatically
- Index foreign keys and frequently filtered columns; every index speeds reads but slows writes
- `EXPLAIN QUERY PLAN` reveals whether a query uses an index or performs a full scan

**Connecting Python to Databases**

- Python connects through a driver; `sqlite3` is built into the standard library
- The universal pattern: connect, cursor, execute with parameters, fetch, commit for writes, close
- Always use parameterized placeholders, never f-string interpolation into SQL

**Sorting and Pagination**

- `LIMIT` and `OFFSET` implement pagination; `OFFSET = (page_number - 1) * page_size`
- Always pair pagination with `ORDER BY` for consistent, deterministic page contents

**Transactions and ACID Properties**

- A transaction groups operations into one all-or-nothing unit
- Atomicity, Consistency, Isolation, Durability are the four guarantees a transactional database provides
- `commit()` saves a transaction permanently; `rollback()` undoes it before commit

**Why All of This Matters Together**

- Joins, aggregation, indexing, Python connectivity, pagination, and transactions are the operational core of any backend that serves real traffic at real scale — they are what turn a working schema into an application that answers complex questions quickly, connects cleanly to application code, and never leaves data in a half-finished state even when something goes wrong mid-operation

---

*End of script.*
