# Lecture Script: Advanced SQL — JOINs
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | SQL JOINs — Introduction and Purpose | 20 min |
| 3 | Join Types — INNER, LEFT, RIGHT, FULL | 45 min |
| 4 | Querying Related Data — Combining Tables Using Joins | 25 min |
| 5 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** Open with a concrete, frustrating scenario. The goal is to make learners feel the limitation of querying one table at a time before they learn how to overcome it. Wait after the opening question — let the problem settle.

**[Script:]**

"You have a users table and an orders table. A customer calls support and says their order never arrived. You need to find the order and display the customer's name, email, and the item they ordered — all on the same screen.

But the customer's name and email are in the users table. The item and order details are in the orders table. If you can only query one table at a time, you have to run two separate queries, manually match up the IDs, and stitch the results together in your application code.

Now imagine doing that for a page that shows fifty orders, each with a customer name, a product name, a category name, and a shipping address. Four tables. Fifty rows. You would be making hundreds of round trips to the database and writing application code to assemble the data by hand.

This is not theoretical. Developers who do not know SQL JOINs end up writing exactly this — multiple queries, manual matching, nested loops in Python. It is slow, it is fragile, and it is entirely unnecessary. A single JOIN query fetches all of it in one round trip and hands you back exactly the shape of data you need.

JOINs are the feature that makes relational databases actually relational. Without them, you have a collection of isolated tables and a lot of glue code. With them, you have a system that answers questions about related data in one statement."

---

## Block 2 — SQL JOINs: Introduction and Purpose

### 2A — What Is a JOIN?

**[Script:]**

"A JOIN combines rows from two or more tables based on a related column between them. That related column is almost always the foreign key relationship you already defined — the column on the many side that references the primary key on the one side.

The result of a JOIN is a new temporary table — it exists only for the duration of the query. Its columns come from both source tables. Its rows are combinations of rows from each table that satisfy the join condition.

Think of it this way: a JOIN is the SQL answer to the question 'give me data from table A and table B together, matched up by this shared column'."

> 🎯 **Instructor Note:** Draw the core concept on the board before any SQL appears. Keep it simple — two tables, one shared column, one result set.

```
users                     orders
+----+-------+            +----+----------+---------+
| id | name  |            | id | item     | user_id |
+----+-------+            +----+----------+---------+
| 1  | Alice |            | 1  | Keyboard | 1       |
| 2  | Bob   |            | 2  | Monitor  | 1       |
| 3  | Carol |            | 3  | Mouse    | 2       |
+----+-------+            +----+----------+---------+

JOIN on users.id = orders.user_id

Result:
+-------+----------+
| name  | item     |
+-------+----------+
| Alice | Keyboard |
| Alice | Monitor  |
| Bob   | Mouse    |
+-------+----------+
```

**[Script:]**

"The join condition is `users.id = orders.user_id`. For each row in orders, the database finds the user row whose `id` matches the `user_id`. It then combines the columns from both rows into one result row.

Notice the syntax for referencing a column: `table_name.column_name`. When you query more than one table, columns might share the same name — `id` exists in both tables. The dot notation tells the database which table you mean. This is called a qualified column reference, and you will use it in every JOIN you write."

---

### 2B — The Basic JOIN Syntax

**[Script:]**

"The structure of a JOIN query builds on the `SELECT` statement you already know. You add a `JOIN` clause after the `FROM` clause, naming the second table and the condition to match on."

> 🎯 **Instructor Note:** Write the anatomy on the board and label each part before showing a runnable demo. Learners need to read the structure before they read working code.

```sql
SELECT   columns
FROM     first_table
JOIN     second_table ON first_table.column = second_table.column
WHERE    condition (optional)
ORDER BY column (optional);
```

**[Script:]**

"Read this top to bottom. `SELECT` names the columns you want in the result — from either table. `FROM` names the first table. `JOIN` names the second table. `ON` specifies the condition — which columns to match between the two tables. `WHERE` and `ORDER BY` work exactly as before, now filtering or sorting the combined result.

The `ON` clause is not optional. Without it, the database does not know how to match rows between the tables — it would produce every possible combination of rows, which is almost never what you want."

> 🎯 **Instructor Note:** Ask: "What do you think happens if you join two tables with no ON condition and each table has 100 rows?" Answer: you get 10,000 rows — every row from table A paired with every row from table B. This is called a Cartesian product. It is almost always a mistake and can bring a database to its knees on large tables. Name it, so learners understand why ON is mandatory.

---

## Block 3 — Join Types: INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN

### 3A — The Core Question: What Happens to Non-Matching Rows?

**[Script:]**

"There are four join types. They all use the same basic syntax. The only difference between them is what they do with rows that have no match on the other side.

Before we look at each type, let me set up the exact data we will use throughout this block. Having the same data in front of you for all four types is what makes the differences visible."

> 🎯 **Instructor Note:** Write this exact data set on the board and leave it up for the entire block. Do not change it between examples — the consistent data is what lets learners see what each join type includes or excludes.

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

"Intentional choices in this data set. Carol has no orders — she is in users but not referenced in orders. The webcam order has `user_id = 99` — a user ID that does not exist in the users table. This is possible if foreign key enforcement was off. Both of these edge cases exist in real databases, and the join type you choose determines what shows up in your results."

---

### 3B — INNER JOIN

**[Script:]**

"INNER JOIN returns only rows where there is a match on both sides. If a user has no orders, they do not appear. If an order references a non-existent user, it does not appear. The result contains only the intersection — rows that have a corresponding row in both tables."

> 🎯 **Instructor Note:** Draw a Venn diagram on the board. Shade only the overlapping center for INNER JOIN. Keep this diagram for all four types — add shading progressively as you go through them.

```
users ●──────●──────● orders
            ↑
       INNER JOIN
      (only matches)
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** With the data set on the board, ask: "If I run an INNER JOIN between users and orders on `users.id = orders.user_id`, which rows appear in the result? Think about Carol and think about the webcam order." Wait for answers. Correct: Alice and Bob appear because they have matching orders. Carol does not appear — no orders. The webcam does not appear — no matching user. Only three result rows.

**Demo 1 — INNER JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
INNER JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Run this. Three rows: Alice-Keyboard, Alice-Monitor, Bob-Mouse. Carol is gone — she has no orders. The webcam is gone — user 99 does not exist.

`INNER JOIN` is the default join type. If you write just `JOIN` without a type keyword, SQL treats it as an INNER JOIN. But always write the full word — explicit code is easier to read and debug.

When should you use INNER JOIN? When you only want rows that have a complete match on both sides. Most common use case: show me all orders with their customer names, but only for orders where the customer still exists."

---

### 3C — LEFT JOIN

**[Script:]**

"LEFT JOIN returns all rows from the left table — the table named in `FROM` — plus any matching rows from the right table. If a left-side row has no match in the right table, it still appears in the result, with NULL values for every column from the right table.

The left table is fully preserved. The right table contributes where it can, and NULL where it cannot."

> 🎯 **Instructor Note:** Update the Venn diagram — shade the full left circle plus the overlap. Say out loud: "The left circle is entirely in the result. The right circle contributes only where it overlaps."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With users as the left table and orders as the right table, which rows appear now? What happens to Carol? What happens to the webcam order?" Correct: all three users appear. Carol appears with NULL in the item column. The webcam still does not appear — it has no match in the left table, and LEFT JOIN does not preserve unmatched right-side rows.

**Demo 2 — LEFT JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
LEFT JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Run this. Four rows now. Alice-Keyboard, Alice-Monitor, Bob-Mouse — the same three as before. Plus Carol-NULL. Carol is in the result but her `item` is NULL because she has no orders.

The webcam is still missing. The webcam's `user_id` is 99, which does not match any row in the left table — users. LEFT JOIN only guarantees the left table is preserved, not the right.

Left JOIN is the most commonly used join after INNER JOIN. The classic use case: show me all users, and if they have orders, show those too. You want every user regardless of whether they have placed an order. Null in the order column means no orders — that is meaningful information, not a missing row."

> 🎯 **Instructor Note:** Pause here. Ask: "How would you find users who have never placed an order using a LEFT JOIN?" Let learners think. The answer: filter `WHERE orders.item IS NULL` — or more precisely `WHERE orders.user_id IS NULL`. When a LEFT JOIN produces NULLs on the right side, those rows represent unmatched left-side rows. This is a very common and useful pattern — left join, then filter for NULLs to find rows with no related records.

```sql
SELECT users.name
FROM   users
LEFT JOIN orders ON users.id = orders.user_id
WHERE  orders.user_id IS NULL;
```

**[Script:]**

"This returns Carol — the only user with no orders. This pattern — LEFT JOIN followed by WHERE right_table.column IS NULL — is how you find records that have no related data. Remember it."

---

### 3D — RIGHT JOIN

**[Script:]**

"RIGHT JOIN is the mirror of LEFT JOIN. It returns all rows from the right table — the table named in `JOIN` — plus any matching rows from the left table. If a right-side row has no match in the left table, it appears with NULLs for all left-table columns.

The right table is fully preserved. The left table contributes where it can."

> 🎯 **Instructor Note:** Update the Venn diagram — shade the full right circle plus the overlap.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With users as the left table and orders as the right table in a RIGHT JOIN, which rows appear? What happens to Carol? What happens to the webcam?" Correct: all four orders appear. The webcam appears with NULL for the user name. Carol does not appear — she has no orders and the right table is orders, not users.

**Demo 3 — RIGHT JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
RIGHT JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Run this. Four rows. Alice-Keyboard, Alice-Monitor, Bob-Mouse — same as INNER JOIN. Plus NULL-Webcam. The webcam appears with a NULL name because user 99 does not exist in the users table. But every order is in the result.

Carol is gone — she has no orders, and orders is the right table that is fully preserved.

Practical note: RIGHT JOIN is less commonly used than LEFT JOIN. Most developers write LEFT JOINs and swap the table order rather than writing RIGHT JOINs — either produces the same result with different table order. If you see a RIGHT JOIN in real code, you can always mentally rewrite it as a LEFT JOIN with the tables swapped. Use whichever makes the query easier to read — typically that means keeping the primary or more important table on the left and using LEFT JOIN."

---

### 3E — FULL JOIN

**[Script:]**

"FULL JOIN — also called FULL OUTER JOIN — returns all rows from both tables. Where there is a match, the columns from both tables are populated. Where there is no match on either side, the missing columns are NULL.

It is the union of LEFT JOIN and RIGHT JOIN. Every row from both tables appears in the result, matched where possible, NULL-padded where not."

> 🎯 **Instructor Note:** Update the Venn diagram — shade both full circles entirely.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "How many rows does the FULL JOIN produce with our data set? Which rows have NULLs and on which side?" Correct: five rows. Alice-Keyboard, Alice-Monitor, Bob-Mouse are fully matched. Carol-NULL has a NULL on the right — no orders. NULL-Webcam has a NULL on the left — no user. Five rows total.

**Demo 4 — FULL JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
FULL JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Five rows. The three matched rows, Carol with a NULL item, and a NULL name row for the webcam.

Important caveat: SQLite does not support FULL JOIN natively. PostgreSQL does. MySQL does not — it requires a workaround using UNION. If you are working in SQLite and want to see this, you would simulate it with a UNION of a LEFT JOIN and a RIGHT JOIN. For now, understand the concept and know where it is supported.

When do you use FULL JOIN? It is the least common in practice. The most typical use case is data reconciliation — you have two lists and you want to see everything in both, clearly marked with which side each row came from."

---

### 3F — Join Type Summary and How to Choose

> 🎯 **Instructor Note:** Draw all four Venn diagrams side by side on the board now and label them. This comparison is the clearest summary of the differences.

```
INNER JOIN      LEFT JOIN       RIGHT JOIN      FULL JOIN
  ●─●─●           ●═●─●          ●─●═●           ●═●═●
  (overlap        (left +        (right +        (both +
   only)           overlap)       overlap)        overlap)
```

**[Script:]**

"Ask yourself: do I need rows from both sides even when there is no match?

No — use INNER JOIN. Only matched rows matter.
Yes, and I need all left-side rows — use LEFT JOIN.
Yes, and I need all right-side rows — use RIGHT JOIN. Or flip the tables and use LEFT JOIN.
Yes, and I need every row from both sides — use FULL JOIN.

In practice: INNER JOIN and LEFT JOIN cover ninety percent of real query needs. Learn those two deeply. RIGHT JOIN is interchangeable with LEFT JOIN given table reordering. FULL JOIN is specialized."

**Recap of Block 3 before moving on:**

- INNER JOIN returns only rows with a match on both sides — unmatched rows from either table are excluded
- LEFT JOIN returns all rows from the left table; right-side columns are NULL where there is no match
- RIGHT JOIN returns all rows from the right table; left-side columns are NULL where there is no match
- FULL JOIN returns all rows from both tables; NULLs fill whichever side has no match
- SQLite does not support FULL JOIN natively; PostgreSQL does
- LEFT JOIN followed by `WHERE right_table.column IS NULL` finds records with no related data on the right side
- In practice, INNER JOIN and LEFT JOIN cover the vast majority of use cases

---

## Block 4 — Querying Related Data: Combining Tables Using Joins

### 4A — Using Table Aliases

**[Script:]**

"Before we write more complex queries, let us clean up the syntax. When you reference `users.name` and `orders.item` repeatedly, the qualified names get verbose. SQL lets you assign a short alias to each table using `AS` — or just a space followed by the alias. From that point in the query, you use the alias instead of the full table name."

```sql
SELECT u.name, o.item
FROM   users  AS u
JOIN   orders AS o ON u.id = o.user_id;
```

**[Script:]**

"`u` is now an alias for `users`. `o` is an alias for `orders`. The query reads faster and is easier to write. In longer queries with more tables, aliases become essential. Convention is to use the first letter or first two letters of the table name. Be consistent — if you use `u` for users in the `FROM` clause, use `u` everywhere in that query."

> 🎯 **Instructor Note:** Ask: "Can I use any alias I want, or are there rules?" Answer: almost any word works, with the exception of reserved SQL keywords. One letter or two-letter abbreviations are strongly conventional and what you will see in real codebases.

---

### 4B — Selecting Specific Columns Across Tables

**[Script:]**

"When you join tables, you choose which columns to include in the result from any of the joined tables. You are not forced to take all columns from both. Be selective — only ask for what you actually need."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If users has columns `id`, `name`, `email`, `age` and orders has columns `id`, `item`, `user_id` — what do I write in the SELECT to get just the user's name, email, and the order item?" Let learners write it before you show it.

**Demo 5 — Selective columns with aliases (whiteboard-friendly)**

```sql
SELECT u.name, u.email, o.item
FROM   users  AS u
JOIN   orders AS o ON u.id = o.user_id
ORDER BY u.name;
```

**[Script:]**

"Three columns in the result: name and email from users, item from orders. Sorted by name alphabetically. Alice appears twice — one row per order. This is correct and expected: the join produces one result row per matching pair, not one row per user."

> 🎯 **Instructor Note:** The "one row per matching pair" behavior surprises many beginners. They expect one row per user and are confused when Alice appears twice. Clarify: a JOIN does not aggregate or deduplicate. If Alice has two orders, she appears in two rows. If a user has ten orders, they appear ten times. The shape of the result reflects the number of matches, not the number of rows in either source table.

---

### 4C — Filtering Joined Results with WHERE

**[Script:]**

"You can add a `WHERE` clause to a JOIN query exactly as you would to any `SELECT`. The `WHERE` clause filters the combined result after the join has been performed. Use the aliased column references the same way."

**Demo 6 — JOIN with WHERE filter (whiteboard-friendly)**

```sql
SELECT u.name, o.item
FROM   users  AS u
JOIN   orders AS o ON u.id = o.user_id
WHERE  u.name = 'Alice';
```

**[Script:]**

"This returns only Alice's orders — Keyboard and Monitor. The join combines all matched rows first, then WHERE filters down to only the rows where the user's name is Alice.

You can also filter on columns from the right table."

```sql
SELECT u.name, o.item
FROM   users  AS u
LEFT JOIN orders AS o ON u.id = o.user_id
WHERE  o.item IS NULL;
```

**[Script:]**

"This is the LEFT JOIN NULL-filter pattern from Block 3. Users with no orders. Carol comes back. This combination — LEFT JOIN plus WHERE IS NULL on the right side — is one of the most practically useful query patterns you will write."

---

### 4D — Joining Three Tables

**[Script:]**

"Real applications often need to join more than two tables. You add additional JOIN clauses, one per table. Each JOIN has its own ON condition. The query builds up a combined result set that spans all joined tables.

Let us add a products table to make this concrete."

> 🎯 **Instructor Note:** Add this table to the board:

```
products
+----+-----------+-------+
| id | name      | price |
+----+-----------+-------+
| 1  | Keyboard  | 49.99 |
| 2  | Monitor   | 199.99|
| 3  | Mouse     | 29.99 |
+----+-----------+-------+
```

**[Script:]**

"Now orders has a `product_id` column that references the products table. Each order is linked to a product, and each product has a price. To show order items with both the user's name and the product price, we join all three."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "How many JOIN clauses do I need to combine users, orders, and products?" Answer: two — one to join orders to users, one to join orders to products. The orders table is the bridge between the other two.

**Demo 7 — Three-table JOIN (whiteboard-friendly)**

```sql
SELECT u.name, p.name AS product, p.price
FROM   orders   AS o
JOIN   users    AS u ON u.id    = o.user_id
JOIN   products AS p ON p.id   = o.product_id;
```

**[Script:]**

"Two JOIN clauses. Orders is the starting table — it has the foreign keys connecting to both users and products. We join users on the user foreign key, then join products on the product foreign key. The `AS product` in the SELECT renames the `p.name` column in the output — without it, you would see two columns both named `name`, which is confusing.

Read any multi-table JOIN by starting from the FROM table and following each JOIN clause. The FROM table is usually the one that holds the foreign keys — the many side of the relationships."

> 🎯 **Instructor Note:** Pause here. Ask: "Does the order of JOIN clauses matter?" Answer: for INNER JOINs, the logical result is the same regardless of order. But clarity matters — start from the most central table and join outward. For LEFT JOINs, order matters more because the left side is fully preserved — the first table in FROM is always preserved.

**Recap of Block 4 before moving on:**

- Table aliases — `FROM users AS u` — shorten qualified column references and are essential in multi-table queries
- SELECT only the columns you need from whichever tables you have joined
- A JOIN produces one result row per matching pair — a user with five orders appears five times
- `WHERE` filters the combined result after the join; use aliased references consistently
- Multi-table joins chain additional `JOIN ... ON ...` clauses; the FROM table is typically the one holding the foreign keys
- In multi-table queries, rename ambiguous columns in SELECT using `AS` to avoid duplicate column names in the result

---

## Block 5 — Lecture Summary

> 🎯 **Instructor Note:** Deliver this section as active recall. Ask each question out loud before confirming the answer. "What does INNER JOIN exclude? What does LEFT JOIN guarantee? What does a NULL on the right side of a LEFT JOIN tell you? How many result rows does a JOIN produce for a user with three matching orders?" Keep the energy up — this is consolidation, not wind-down.

**SQL JOINs — Introduction and Purpose**

- A JOIN combines rows from two or more tables based on a matching condition, typically a foreign key relationship
- The result is a temporary table containing columns from all joined tables, with one row per matching pair
- The `ON` clause specifies which columns to match between tables; omitting it produces a Cartesian product — every possible row combination — which is almost always a mistake
- Qualified column references — `table_name.column_name` or `alias.column_name` — are required when the same column name exists in multiple tables
- Table aliases assigned with `AS` shorten syntax and are conventional in any query involving more than one table

**Join Types — INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN**

- INNER JOIN returns only rows that have a match on both sides; unmatched rows from either table are excluded entirely
- LEFT JOIN returns all rows from the left table; columns from the right table are NULL where no match exists
- RIGHT JOIN returns all rows from the right table; columns from the left table are NULL where no match exists; it is logically equivalent to LEFT JOIN with table order swapped
- FULL JOIN returns all rows from both tables, NULLs on whichever side lacks a match; not supported in SQLite
- LEFT JOIN followed by `WHERE right_table.column IS NULL` is the standard pattern for finding records with no related data
- INNER JOIN and LEFT JOIN cover the overwhelming majority of practical query needs

**Querying Related Data — Combining Tables Using Joins**

- A JOIN result contains one row per matching pair, not one row per source row; a user with three orders appears three times
- `WHERE` filters the combined result after the join is performed; it applies to columns from any joined table
- Multi-table queries chain multiple `JOIN ... ON ...` clauses; the `FROM` table is typically the one holding the foreign keys
- Rename ambiguous or duplicate column names in `SELECT` using `column AS alias`
- Read a multi-table query by starting from the `FROM` table and following each `JOIN` outward

**Why All of This Matters Together**

- JOINs are what make the relational model useful in practice — they allow a single query to answer questions that span multiple tables, replacing multiple round trips and manual application-level assembly with one precise SQL statement
- The choice of join type is a data question, not just a syntax question: INNER JOIN when only complete matches are meaningful, LEFT JOIN when unmatched records carry information, FULL JOIN when both sides must be fully accounted for
- Every page, report, and API response in a real backend application that displays data from more than one entity — user orders, product categories, post authors, enrollment records — is answered by a JOIN query; understanding join mechanics at this level means you can write, read, and debug any data retrieval requirement you encounter

---

*End of script.*
