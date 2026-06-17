# Lecture Script: Advanced SQL and Database Connectivity
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | Joins — INNER, LEFT, RIGHT, FULL | 40 min |
| 3 | Aggregation Functions and GROUP BY | 30 min |
| 4 | WHERE — Precise Filtering Across Joins and Groups | 20 min |
| 5 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience already knows table creation, constraints, primary and foreign keys, and the four basic SQL operations including a basic WHERE clause. The hook should move past "databases are useful" straight into "single-table queries are not enough for real questions." Open with a concrete multi-table, multi-row scenario. Wait after the opening question.

**[Script:]**

"You can already create tables, insert data, and filter a single table with a simple WHERE condition. Here is what that does not cover.

A product manager asks: show me every customer alongside their orders, including customers who have never ordered anything — they want to see who has gone quiet. That requires combining two tables in a single query, with a method that does not silently drop the customers who have no matching orders.

A second question: what is the total revenue per product category? That requires summarizing many rows down into one result per category — not filtering individual rows, but computing something across groups of them.

Both of these are everyday business questions. Neither is answerable with the single-table WHERE filtering you already know. Today covers exactly the tools that close this gap: joins that combine tables in four distinct ways depending on what you need preserved, aggregation functions and GROUP BY that summarize data per group, and a deeper, more precise use of WHERE that applies correctly across joined and grouped data. By the end of today you can answer both of those questions directly in SQL."

---

## Block 2 — Joins: INNER, LEFT, RIGHT, FULL

### 2A — Why Joins Exist

**[Script:]**

"A foreign key links two tables. A JOIN is how you retrieve data across that link in one query, instead of querying each table separately and matching rows by hand in application code.

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

"Two intentional details. Carol has no orders. The webcam's `user_id` is 99 — no matching user. These edge cases exist in real databases and the join type you choose determines whether they show up in your results.

The basic syntax: `SELECT` columns, `FROM` the first table, `JOIN` the second table `ON` the matching condition."

```sql
SELECT users.name, orders.item
FROM   users
JOIN   orders ON users.id = orders.user_id;
```

**[Script:]**

"`table.column` is a qualified reference — needed because both tables might have a column with the same name, like `id`. Without the `ON` condition, the database produces every possible combination of rows from both tables — a Cartesian product, almost always a mistake and a performance disaster on large tables."

> 🎯 **Instructor Note:** Ask: "If users has 3 rows and orders has 4 rows, and I join them with no ON condition at all, how many result rows do I get?" Answer: 12 — every possible pairing. Name this as a Cartesian product so learners recognize the term if they see it in an error or in documentation.

---

### 2B — INNER JOIN

**[Script:]**

"INNER JOIN returns only rows with a match on both sides. No match on either side means the row is excluded."

> 🎯 **Instructor Note:** Draw a Venn diagram with only the overlap shaded. Keep this diagram on the board and add shading as you move through each join type — by the end of Block 2 you will have all four side by side.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With our data, which rows appear in an INNER JOIN on `users.id = orders.user_id`? What happens to Carol? What happens to the webcam?" Answer: Alice and Bob appear with their orders. Carol is excluded — no orders. The webcam is excluded — no matching user.

**Demo 1 — INNER JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
INNER JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Three rows: Alice-Keyboard, Alice-Monitor, Bob-Mouse. INNER JOIN is the default — plain `JOIN` without a keyword means INNER JOIN — but always write it explicitly for readability.

When do you reach for INNER JOIN? When unmatched rows on either side carry no useful information for your question — you only want complete pairs."

---

### 2C — LEFT JOIN

**[Script:]**

"LEFT JOIN returns every row from the left table — the one named in `FROM` — plus matching rows from the right table. Unmatched left rows still appear, with NULL for every right-table column."

> 🎯 **Instructor Note:** Update the Venn diagram — shade the full left circle plus the overlap.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With users as the left table, what changes from the INNER JOIN result? What happens to Carol?" Answer: Carol now appears, with NULL in the item column. The webcam still does not appear — LEFT JOIN does not preserve unmatched right-side rows.

**Demo 2 — LEFT JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
LEFT JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Four rows now — the three matched plus Carol with a NULL item. This is the exact answer to the product manager's question from the hook: every customer, including the ones who have gone quiet with no orders at all.

A useful pattern: LEFT JOIN combined with `WHERE orders.user_id IS NULL` finds rows in the left table with no match at all — customers who have never placed an order, isolated on their own."

```sql
SELECT users.name
FROM   users
LEFT JOIN orders ON users.id = orders.user_id
WHERE  orders.user_id IS NULL;
```

> 🎯 **Instructor Note:** Pause here. This pattern is genuinely useful and worth a second of emphasis: "find records with no related data" is one of the most common real-world query needs, and this is how you write it. We will return to WHERE in more depth in Block 4 — for now, notice that it works identically after a JOIN as it did on a single table.

---

### 2D — RIGHT JOIN

**[Script:]**

"RIGHT JOIN is the mirror of LEFT JOIN — every row from the right table, plus matches from the left, NULL where there is no match on the left."

> 🎯 **Instructor Note:** Update the Venn diagram — shade the full right circle plus the overlap.

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

In practice, RIGHT JOIN is rarely used — most developers write LEFT JOIN and swap the table order instead. If you see RIGHT JOIN in real code, you can mentally rewrite it as a LEFT JOIN with the FROM and JOIN tables reversed."

---

### 2E — FULL JOIN

**[Script:]**

"FULL JOIN returns every row from both tables — matched where possible, NULL-padded on whichever side lacks a match. It is the union of LEFT and RIGHT."

> 🎯 **Instructor Note:** Update the Venn diagram one final time — shade both full circles entirely. You now have all four diagrams visible side by side, which is the clearest summary of the block.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "How many total rows does a FULL JOIN produce with our data set?" Answer: five — the three matched rows, Carol with a NULL item, and a NULL name paired with the webcam.

**Demo 4 — FULL JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
FULL JOIN orders ON users.id = orders.user_id;
```

**[Script:]**

"Five rows with our data: the three matched, Carol with a NULL item, and a NULL name paired with the webcam.

Important: SQLite does not support FULL JOIN natively. PostgreSQL does. If you need this behavior in SQLite, you simulate it with a UNION of a LEFT JOIN and a RIGHT JOIN. Know the concept and know where it is supported — do not assume every database engine implements all four."

**Recap of Block 2 before moving on:**

- A JOIN combines rows from two tables based on a matching condition, typically a foreign key; omitting `ON` produces a Cartesian product
- INNER JOIN returns only matched rows from both tables
- LEFT JOIN returns all left-table rows; right-side columns are NULL where unmatched
- RIGHT JOIN returns all right-table rows; left-side columns are NULL where unmatched; logically equivalent to LEFT JOIN with tables swapped
- FULL JOIN returns all rows from both tables; not supported natively in SQLite
- LEFT JOIN plus `WHERE right_table.column IS NULL` finds records with no related data — a frequently used pattern

---

## Block 3 — Aggregation Functions and GROUP BY

### 3A — Aggregate Functions

**[Script:]**

"Aggregate functions compute one summary value across many rows. `COUNT` counts rows. `SUM` totals a numeric column. `AVG` computes the mean. `MIN` and `MAX` find the smallest and largest values. All five ignore NULL values in the column being aggregated, except `COUNT(*)`, which counts every row regardless of NULLs."

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

**Demo 5 — Aggregate functions (whiteboard-friendly)**

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

"One result row, five computed values. This is the query behind a dashboard summary card. `COUNT(*)` counts all rows including any with NULL columns; `COUNT(column)` would only count rows where that specific column is not NULL — worth knowing if your data has gaps.

You can also combine a `WHERE` filter before any aggregate runs — the aggregate only operates on rows that pass the filter."

```sql
SELECT COUNT(*), SUM(price) FROM orders WHERE category = 'Hardware';
```

**[Script:]**

"4 rows, summing to 369.96. The filter applies before the aggregate computes — we will cover exactly this ordering in detail in Block 4."

---

### 3B — GROUP BY

**[Script:]**

"Aggregates so far summarize the whole table, or the whole filtered set. But what if you want the total revenue per category, broken out separately for each one? That is `GROUP BY`. It divides rows into groups by a column's value and runs the aggregate independently per group.

Think of it as a Python dictionary group-by: bucket the rows by key, then compute something per bucket."

> 🎯 **Instructor Note:** Write the Python mental model on the board.

```python
from collections import defaultdict

totals = defaultdict(float)
for row in orders:
    totals[row['category']] += row['price']

# Result: {'Hardware': 369.96, 'Books': 74.98}
```

```sql
SELECT category, SUM(price)
FROM orders
GROUP BY category;
```

**[Script:]**

"Same logic. Group by the category column, sum the prices within each group. One result row per unique category value.

There is one strict rule. Every column in `SELECT` must be either in the `GROUP BY` clause, or inside an aggregate function. You cannot select a column that represents one specific row when the group itself collapses many rows into one result row — there is no single correct value to show for an ungrouped, unaggregated column."

> 🎯 **Instructor Note:** Write the rule on the board and keep it visible for the rest of the block.

```
With GROUP BY: every SELECT column is either
  (a) in the GROUP BY clause, or
  (b) inside an aggregate function
```

```sql
-- INVALID: item is not in GROUP BY and not aggregated
SELECT category, item, SUM(price) FROM orders GROUP BY category;

-- VALID
SELECT category, SUM(price) FROM orders GROUP BY category;
```

**[Script:]**

"SQLite may run the invalid query and pick an arbitrary value for `item` — do not rely on this. PostgreSQL rejects it outright. Write GROUP BY queries that follow the rule consistently."

> 🎯 **Instructor Note:** Ask: "Why does the database not allow selecting an ungrouped, unaggregated column?" Let learners reason it through in their own words — hearing themselves explain it solidifies the model. Guide toward: each result row now represents many original rows collapsed together; there is no single correct `item` value to display for an entire group.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** With the data on the board, ask: "What totals do you expect per category? Hardware has four orders, Books has two." Let learners compute: Hardware = 49.99+199.99+29.99+89.99 = 369.96. Books = 39.99+34.99 = 74.98.

**Demo 6 — GROUP BY with multiple aggregates (whiteboard-friendly)**

```sql
SELECT
    category,
    COUNT(*)   AS order_count,
    SUM(price) AS total_revenue,
    AVG(price) AS avg_price
FROM orders
GROUP BY category
ORDER BY total_revenue DESC;
```

**[Script:]**

"Two rows: Hardware with 4 orders, 369.96 total, 92.49 average. Books with 2 orders, 74.98 total, 37.49 average — sorted highest revenue first.

This single query structure — group by a column, aggregate, sort — produces the category breakdown behind any analytics report. Replace `category` with `user_id` and you have per-customer revenue instead."

**Recap of Block 3 before moving on:**

- `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` compute a single summary value across rows; all ignore NULL except `COUNT(*)`
- `WHERE` can filter rows before an aggregate runs, narrowing what the aggregate computes over
- `GROUP BY column` buckets rows by that column's value and runs aggregates independently per bucket
- Every `SELECT` column with `GROUP BY` must be grouped or aggregated — no exceptions
- `GROUP BY` combines naturally with `ORDER BY` on an aggregated alias to rank results

---

## Block 4 — WHERE: Precise Filtering Across Joins and Groups

### 4A — WHERE Operators in Depth

**[Script:]**

"You already know the basic comparison operators from earlier SQL work: `=`, `>`, `<`, `>=`, `<=`, `!=`. Let us push further into the operators and combinations that make WHERE precise enough for real questions, and then look carefully at exactly when WHERE applies in a query that also has a JOIN and a GROUP BY."

> 🎯 **Instructor Note:** Write the operator set on the board as a quick refresher before moving to combinations.

```
=   !=   >   <   >=   <=
AND   OR   NOT
LIKE   IN   BETWEEN   IS NULL / IS NOT NULL
```

**[Script:]**

"One trap worth repeating: equality in SQL is a single `=`, never `==`. And `AND` binds more tightly than `OR` — when you mix them in one condition, use parentheses to make your intent explicit, or you risk a result that looks plausible but is subtly wrong."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** With the users table (Alice 28, Bob 34, Carol 22) on the board, ask: "What does `WHERE (age > 20 AND age < 30) OR name = 'Bob'` return? Walk through it row by row before confirming." Answer: Alice (28, satisfies the age range), Carol (22, satisfies the age range), and Bob (matches by name). All three.

**Demo 7 — Combined WHERE conditions (whiteboard-friendly)**

```sql
SELECT name, age FROM users WHERE (age > 20 AND age < 30) OR name = 'Bob';

SELECT name FROM users WHERE age BETWEEN 22 AND 30;

SELECT name FROM users WHERE email LIKE '%@example.com';
```

**[Script:]**

"First query returns all three — confirmed by working through the parenthesized logic. Second returns Alice and Carol — both within 22 to 30 inclusive. Third returns everyone, since all emails in our data end that way.

These are the same WHERE tools from earlier sessions. What is new now is using them correctly once a JOIN and a GROUP BY are also part of the query — and that is where a real ordering question appears."

---

### 4B — WHERE on a Joined Result

**[Script:]**

"When a query includes a JOIN, WHERE filters the already-combined result. You can filter on a column from either table, using the same qualified `table.column` syntax."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "In a LEFT JOIN between users and orders, if I add `WHERE users.name = 'Alice'`, how many rows come back?" Answer: two — Alice's two orders, Keyboard and Monitor. The join happens first, producing all matched and unmatched combinations, and WHERE then narrows that combined set down to rows where the name matches.

**Demo 8 — WHERE after a JOIN (whiteboard-friendly)**

```sql
SELECT users.name, orders.item
FROM   users
JOIN   orders ON users.id = orders.user_id
WHERE  users.name = 'Alice';
```

**[Script:]**

"Two rows. The JOIN combines all matching pairs first; WHERE then filters that combined result down to rows where the user's name is Alice. Order of execution matters here, and it matters even more once GROUP BY enters the picture."

---

### 4C — Why WHERE Cannot Filter Aggregated Results

**[Script:]**

"Here is the critical distinction for this block. Suppose you want only categories with more than three orders. Your first instinct might be `WHERE COUNT(*) > 3`. That does not work, and understanding why ties together everything in this session.

WHERE filters individual rows before any grouping happens. At the point WHERE runs, groups do not exist yet — there is nothing to count, sum, or average. The aggregate values that GROUP BY produces are not yet computed when WHERE is evaluated."

> 🎯 **Instructor Note:** This is the most important conceptual moment in the entire lecture. Write the execution order on the board and keep it visible for the rest of Block 4.

```
Execution order in a query with JOIN, WHERE, and GROUP BY:
1. FROM / JOIN  — combine the tables
2. WHERE        — filter individual rows from that combined result
3. GROUP BY     — form groups from the rows that passed WHERE
4. Aggregate    — compute per group (COUNT, SUM, etc.)
5. ORDER BY     — sort the final result
```

**[Script:]**

"WHERE runs at step 2 — before grouping and before aggregation. It can only see individual row values, never an aggregate result. If you need to filter based on an aggregate value — total per category greater than some threshold — that is a different clause, used after GROUP BY, applied to the aggregated result. That clause is outside today's scope, but knowing why WHERE cannot do this job is exactly the point: WHERE is fundamentally a row-level filter, always evaluated before any summarization takes place."

> 🎯 **Instructor Note:** If a learner asks "then what clause does filter aggregates?" — acknowledge directly that there is a dedicated clause for that, but it is not part of today's material, and redirect: "What matters today is recognizing why WHERE specifically cannot do it — that diagnosis is the transferable skill." Do not demo or name unrelated clauses beyond acknowledging one exists.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I run `SELECT category, SUM(price) FROM orders WHERE category = 'Hardware' GROUP BY category`, does this work? What does WHERE filter here versus what GROUP BY does?" Answer: this works correctly, because `WHERE category = 'Hardware'` filters individual rows by a literal column value — not by an aggregate. Only Hardware rows proceed to grouping, and the single resulting group is summed.

**Demo 9 — WHERE correctly combined with GROUP BY (whiteboard-friendly)**

```sql
SELECT category, SUM(price) AS total
FROM   orders
WHERE  category = 'Hardware'
GROUP BY category;
```

**[Script:]**

"Read this in execution order. WHERE filters to Hardware rows only — rows 1, 2, 3, and 6 from our data. GROUP BY then forms groups from that filtered set — in this case just one group, since only Hardware rows remain. SUM computes the total across that group: 369.96.

This is the correct, fully general pattern: WHERE narrows which rows are considered at all, using only row-level conditions; GROUP BY and the aggregate then operate on whatever WHERE allowed through."

> 🎯 **Instructor Note:** Close with a direct recall check. Ask the room to state, in one sentence each: "What can WHERE filter on, and what can it never filter on?" Listen for: WHERE filters individual row values; it can never filter on the result of an aggregate function, because aggregates are not computed yet when WHERE runs.

**Recap of Block 4 before moving on:**

- WHERE supports the same operators as before — comparisons, AND/OR/NOT, LIKE, IN, BETWEEN, IS NULL — now applied with full precision
- Parentheses are required whenever AND and OR are mixed in one condition, since AND binds more tightly
- WHERE filters the combined result of a JOIN exactly as it filters a single table — qualify columns with `table.column`
- Execution order: combine tables, then WHERE filters individual rows, then GROUP BY forms groups, then aggregates compute
- WHERE can only filter on row-level column values — never on the result of an aggregate function, because aggregates do not exist yet at the point WHERE is evaluated

---

## Block 5 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What does LEFT JOIN preserve that INNER JOIN does not? What rule governs SELECT columns under GROUP BY? At what point in query execution does WHERE run, relative to GROUP BY?" Keep the energy up for this close.

**Joins — INNER, LEFT, RIGHT, FULL**

- A JOIN combines rows from two tables on a matching condition; omitting `ON` produces a Cartesian product
- INNER JOIN returns only matched rows from both tables
- LEFT JOIN preserves every row from the left table, NULL-padding unmatched right-side columns
- RIGHT JOIN preserves every row from the right table, NULL-padding unmatched left-side columns; logically equivalent to a LEFT JOIN with table order reversed
- FULL JOIN preserves every row from both tables; not supported natively in SQLite
- LEFT JOIN combined with `WHERE related_table.column IS NULL` finds rows with no related match

**Aggregation Functions and GROUP BY**

- `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` compute a single summary value across a set of rows
- `GROUP BY column` divides rows into groups by that column's value and computes aggregates independently per group
- Every `SELECT` column under `GROUP BY` must be either grouped or wrapped in an aggregate function
- `GROUP BY` combines naturally with `ORDER BY` on an aggregated alias to rank grouped results

**WHERE — Precise Filtering Across Joins and Groups**

- WHERE supports comparisons, AND/OR/NOT, LIKE, IN, BETWEEN, and IS NULL — the same operators across single tables, joined tables, and pre-grouped data
- Parentheses are required whenever AND and OR are combined in one condition
- WHERE filters a joined result exactly as it filters a single table, using qualified column references
- WHERE always runs before GROUP BY and before any aggregate is computed — it can filter only row-level values, never an aggregate result

**Why All of This Matters Together**

- Joins let a single query answer questions that span multiple tables instead of requiring multiple round trips and manual assembly in application code; aggregation and GROUP BY turn many rows into the summarized answers that real business questions actually ask for; and a precise understanding of exactly when WHERE applies — before grouping, on row-level values only — is what separates a query that looks correct from one that is actually correct, which is the difference that matters the moment these tools are used against real, large, multi-table data

---

*End of script.*