# Lecture Script: Advanced SQL — Aggregate Functions and GROUP BY
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | Aggregation — Introduction and Purpose | 15 min |
| 3 | Aggregate Functions — COUNT, SUM, AVG, MIN, MAX | 25 min |
| 4 | GROUP BY — Grouping Records and Aggregating | 30 min |
| 5 | HAVING — Filtering Grouped Data | 20 min |
| 6 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** Open with a business scenario that makes the limitation of row-by-row thinking visible. The goal is to show that the questions applications actually need to answer are not about individual rows — they are about counts, totals, and summaries. Let the question sit for a moment before continuing.

**[Script:]**

"A product manager sends you a message: how many orders did we receive last month? What is our average order value? Which customer has spent the most? Which product category has the highest total revenue?

None of these questions are answered by a single row. They are answered by looking at many rows and computing something across them — a count, a sum, an average.

If you do not know aggregate functions, you fetch every row from the database into Python, loop over them, and compute the answer yourself. For ten rows that is fine. For ten thousand rows, you have transferred ten thousand rows over the network, loaded them into memory, and done arithmetic in Python that the database could have done in a fraction of the time, returning a single number.

That is the problem aggregation solves. It pushes the computation into the database, close to the data, and sends back exactly the answer you asked for — one number, one row, one summary — instead of the entire dataset.

And `GROUP BY` extends this further. Not just the total for all orders, but the total per customer. Not just the average across all products, but the average per category. `GROUP BY` breaks data into groups and applies the aggregation to each group independently.

The combination — aggregate functions and `GROUP BY` — is how every analytics feature, every dashboard metric, and every summary report in a real application is built."

---

## Block 2 — Aggregation: Introduction and Purpose

### 2A — What Is Aggregation?

**[Script:]**

"Aggregation means taking a set of values and computing a single summary value from them. Count how many there are. Add them up. Find the largest. Find the smallest. Find the average.

In Python, you do this with functions like `len()`, `sum()`, `max()`, `min()`. In SQL, you do it with aggregate functions — `COUNT`, `SUM`, `MAX`, `MIN`, `AVG`. The concept is identical. The difference is that SQL aggregate functions run inside the database on the full dataset without you having to retrieve it first."

> 🎯 **Instructor Note:** Write the Python-to-SQL mapping on the board. The comparison lands immediately with this audience.

```
Python          SQL equivalent
len(values)  →  COUNT(column)
sum(values)  →  SUM(column)
max(values)  →  MAX(column)
min(values)  →  MIN(column)
mean = sum/len  AVG(column)
```

**[Script:]**

"Same idea, different syntax, and — crucially — running on the server rather than in your application."

---

### 2B — The Data Set for This Session

**[Script:]**

"We will use a consistent data set for all examples. Take a moment to read it so the numbers are in your head before we start aggregating."

> 🎯 **Instructor Note:** Write this on the board and leave it visible for the entire session.

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

users
+----+-------+
| id | name  |
+----+-------+
| 1  | Alice |
| 2  | Bob   |
| 3  | Carol |
+----+-------+
```

**[Script:]**

"Six orders across three users. Two categories — Hardware and Books. Prices ranging from 29.99 to 199.99. We will be computing counts, sums, averages, and per-group breakdowns from this data throughout the session."

---

## Block 3 — Aggregate Functions: COUNT, SUM, AVG, MIN, MAX

### 3A — COUNT

**[Script:]**

"`COUNT` returns the number of rows that match the query. It has two forms that behave differently and the difference matters.

`COUNT(*)` counts every row, including rows where some columns are NULL.
`COUNT(column)` counts only the rows where that specific column is not NULL.

Most of the time you want `COUNT(*)` when you are counting records. Use `COUNT(column)` when you specifically want to count how many rows have a value in a particular column."

> 🎯 **Instructor Note:** Write both forms on the board with their definitions before any demo. The NULL distinction trips up learners consistently.

```sql
COUNT(*)        -- every row, including NULLs
COUNT(column)   -- only rows where column is NOT NULL
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** With the orders table visible, ask: "What does `SELECT COUNT(*) FROM orders` return?" Answer: 6. Then ask: "If the `price` column had two NULL values, what would `SELECT COUNT(price) FROM orders` return?" Answer: 4. Make the distinction concrete before the demo.

**Demo 1 — COUNT (whiteboard-friendly)**

```sql
-- Count all rows
SELECT COUNT(*) FROM orders;

-- Count rows where price is not null
SELECT COUNT(price) FROM orders;

-- Count with a filter
SELECT COUNT(*) FROM orders WHERE category = 'Books';
```

**[Script:]**

"First query: 6 — all six orders.
Second query: also 6 in our data — all prices are populated. If two were NULL, it would return 4.
Third query: 2 — only the Python book and the SQL Book are in the Books category.

`COUNT` with a `WHERE` clause counts only the rows that pass the filter. The count is always computed after filtering."

---

### 3B — SUM

**[Script:]**

"`SUM` adds up the values in a numeric column. It ignores NULL values — if a row has NULL in the column being summed, that row does not contribute to the total. This is usually the behavior you want, but it is worth knowing."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** With the data on the board, ask: "What does `SELECT SUM(price) FROM orders` return?" Let learners add up the prices: 49.99 + 199.99 + 29.99 + 39.99 + 34.99 + 89.99 = 444.94. Wait for answers before running.

**Demo 2 — SUM (whiteboard-friendly)**

```sql
-- Total of all order prices
SELECT SUM(price) FROM orders;

-- Total for hardware orders only
SELECT SUM(price) FROM orders WHERE category = 'Hardware';
```

**[Script:]**

"444.94 for the first query. For hardware only: 49.99 + 199.99 + 29.99 + 89.99 = 369.96.

`SUM` on a non-numeric column returns NULL or an error depending on the database. Always sum numeric columns. Always check your column type before aggregating."

---

### 3C — AVG

**[Script:]**

"`AVG` computes the arithmetic mean — the sum divided by the count of non-NULL values. Like `SUM`, it ignores NULLs. This matters because if some rows have no price, they do not count toward the average — not zero, not missing — they are simply excluded from both the sum and the count."

> 🎯 **Instructor Note:** The NULL exclusion from AVG is a real source of bugs. Developers expect NULLs to count as zero and pull the average down — they do not. If you need NULLs to count as zero, use `COALESCE(column, 0)` to replace NULLs before averaging. Do not go deep on COALESCE here — name it and move on.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What is the average order price across all six orders?" Let learners calculate: 444.94 / 6 = 74.16 approximately. Accept estimates — the exact value matters less than the operation.

**Demo 3 — AVG (whiteboard-friendly)**

```sql
-- Average price across all orders
SELECT AVG(price) FROM orders;

-- Average price for books
SELECT AVG(price) FROM orders WHERE category = 'Books';
```

**[Script:]**

"About 74.16 for all orders. For books: (39.99 + 34.99) / 2 = 37.49.

Notice that `AVG` returns a decimal result even when the inputs are integers. In SQLite, this returns a float. In PostgreSQL, dividing integers returns an integer — you would cast to a decimal for a precise average. For now, the behavior in SQLite is what you expect."

---

### 3D — MIN and MAX

**[Script:]**

"`MIN` and `MAX` return the smallest and largest values in a column. They work on numbers, text, and dates. For text, they return lexicographically first and last — alphabetical order. Both ignore NULL values."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What does `SELECT MIN(price), MAX(price) FROM orders` return?" With the data on the board, learners can read off 29.99 and 199.99. Ask a follow-up: "What does `SELECT MIN(item) FROM orders` return?" Answer: alphabetically first item name — 'Keyboard' would come before 'Monitor', 'Mouse', 'Python', 'SQL Book', 'Webcam'. So 'Keyboard'. This shows MIN/MAX work on text.

**Demo 4 — MIN and MAX (whiteboard-friendly)**

```sql
-- Cheapest and most expensive order
SELECT MIN(price), MAX(price) FROM orders;

-- Alphabetically first and last item name
SELECT MIN(item), MAX(item) FROM orders;
```

**[Script:]**

"29.99 and 199.99 for prices — Mouse and Monitor respectively. For item names: Keyboard comes first alphabetically, Webcam comes last. SQL applies the same comparison rules to text columns that it uses in ORDER BY.

A useful pattern: `MAX(id)` on a table with an auto-incrementing primary key returns the most recently inserted row's ID. Not the most elegant approach, but you will see it in real code."

---

### 3E — Combining Multiple Aggregates in One Query

**[Script:]**

"You can put multiple aggregate functions in a single SELECT. They all run over the same set of rows simultaneously and return a single result row with one value per aggregate."

**Demo 5 — Multiple aggregates (whiteboard-friendly)**

```sql
SELECT
    COUNT(*)    AS total_orders,
    SUM(price)  AS total_revenue,
    AVG(price)  AS avg_order_value,
    MIN(price)  AS cheapest,
    MAX(price)  AS most_expensive
FROM orders;
```

**[Script:]**

"One query, one result row, five computed values. This is the query behind a typical dashboard summary card — total orders, total revenue, average order value, cheapest and most expensive item.

Notice the column aliases — `AS total_orders`, `AS total_revenue`. Without them, the column headers in the result would be the function expressions themselves: `COUNT(*)`, `SUM(price)`. Aliases make the output readable and are essential when the result is consumed by application code."

**Recap of Block 3 before moving on:**

- `COUNT(*)` counts all rows; `COUNT(column)` counts only non-NULL values in that column
- `SUM(column)` totals a numeric column, ignoring NULLs
- `AVG(column)` computes the mean, ignoring NULLs — rows with NULL do not count as zero
- `MIN(column)` and `MAX(column)` work on numbers, text, and dates; both ignore NULLs
- Multiple aggregate functions can be combined in one SELECT, each with an alias for readable output
- Aggregate functions run after `WHERE` filtering — the count or sum reflects only the filtered rows

---

## Block 4 — GROUP BY: Grouping Records and Aggregating

### 4A — What GROUP BY Does

**[Script:]**

"The aggregate functions we have used so far produce one result row for the entire table — or for whatever rows pass the `WHERE` filter. But what if you want the total revenue per category? The number of orders per user? The average price per category?

That is `GROUP BY`. It divides the rows into groups based on the values in one or more columns, and then runs the aggregate function independently on each group.

Think of it in Python terms: it is like running a dictionary group-by. You sort your rows into buckets by a key, then compute something for each bucket."

> 🎯 **Instructor Note:** Write the Python mental model on the board. This maps cleanly to what learners already know.

```python
# Python equivalent
from collections import defaultdict

totals = defaultdict(float)
for row in orders:
    totals[row['category']] += row['price']

# Result: {'Hardware': 369.96, 'Books': 74.98}
```

```sql
-- SQL equivalent
SELECT category, SUM(price)
FROM orders
GROUP BY category;
```

**[Script:]**

"Same logic. Group by the category column, sum the prices within each group. One result row per unique category value. The database does the bucketing and the arithmetic."

---

### 4B — The SELECT Rule for GROUP BY

**[Script:]**

"There is one strict rule in `GROUP BY` that confuses almost everyone at first.

When you use `GROUP BY`, every column in your `SELECT` must be either in the `GROUP BY` clause, or inside an aggregate function. You cannot select a column that is not grouped and not aggregated.

This is logical if you think it through. If you group by category, each result row represents an entire group — potentially many rows collapsed into one. Which individual row's `item` name would you show? There is no meaningful answer. So the database does not allow it."

> 🎯 **Instructor Note:** Write the rule on the board and leave it up. Then show both the invalid and valid forms side by side.

```sql
-- INVALID: item is not in GROUP BY and not aggregated
SELECT category, item, SUM(price)
FROM orders
GROUP BY category;

-- VALID: only category (grouped) and SUM(price) (aggregated)
SELECT category, SUM(price)
FROM orders
GROUP BY category;
```

**[Script:]**

"Some databases — SQLite among them — will actually run the invalid query and pick an arbitrary value for `item`. Do not rely on this behavior. It is non-standard, produces unpredictable results, and PostgreSQL rejects it with an error. Write GROUP BY queries that follow the rule: grouped column or aggregate function in every SELECT position."

> 🎯 **Instructor Note:** This is the most important pause point in Block 4. Ask: "Why does the database not allow you to select a column that is not grouped and not aggregated?" Let learners reason through it again in their own words. Hearing themselves explain it reinforces the mental model.

---

### 4C — GROUP BY with COUNT

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** With the data on the board, ask: "If I group orders by user_id and count the rows in each group, what result do I get?" Let learners count: user 1 has 2 orders (Keyboard, Monitor), user 2 has 2 orders (Mouse, Python book), user 3 has 2 orders (SQL Book, Webcam). Answer: three rows, each with a count of 2.

**Demo 6 — GROUP BY with COUNT (whiteboard-friendly)**

```sql
SELECT user_id, COUNT(*) AS order_count
FROM   orders
GROUP BY user_id;
```

**[Script:]**

"Three rows. user_id 1: count 2. user_id 2: count 2. user_id 3: count 2. Each group contains two rows, so each count is 2.

Let us make this more readable by joining users to get names instead of IDs."

```sql
SELECT u.name, COUNT(*) AS order_count
FROM   orders  AS o
JOIN   users   AS u ON u.id = o.user_id
GROUP BY u.name;
```

**[Script:]**

"Now the result shows Alice: 2, Bob: 2, Carol: 2. We joined first, then grouped. The `GROUP BY` applies to the combined result after the join.

Notice that `GROUP BY u.name` uses the alias reference. In some databases you can also write `GROUP BY 2` to reference the second column in the SELECT — but always prefer the column name for readability."

---

### 4D — GROUP BY with SUM and AVG

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What does total revenue look like broken down by category?" With the data on the board, learners can compute: Hardware = 49.99 + 199.99 + 29.99 + 89.99 = 369.96. Books = 39.99 + 34.99 = 74.98. Two result rows.

**Demo 7 — GROUP BY with SUM and AVG (whiteboard-friendly)**

```sql
SELECT
    category,
    COUNT(*)       AS order_count,
    SUM(price)     AS total_revenue,
    AVG(price)     AS avg_price
FROM orders
GROUP BY category;
```

**[Script:]**

"Two rows — one per category. Hardware: 4 orders, 369.96 total, 92.49 average. Books: 2 orders, 74.98 total, 37.49 average.

This single query produces the data behind a category breakdown report. Replace `category` with `user_id` and you have per-customer revenue. Replace with `DATE(order_date)` on a table with dates and you have daily revenue. The structure is always the same: what to group by, what to aggregate."

---

### 4E — GROUP BY with ORDER BY

**[Script:]**

"You can combine `GROUP BY` with `ORDER BY` to sort the grouped results. The `ORDER BY` applies to the result of the grouping — you can sort by the grouped column or by an aggregated value."

**Demo 8 — GROUP BY with ORDER BY (whiteboard-friendly)**

```sql
SELECT
    category,
    SUM(price) AS total_revenue
FROM   orders
GROUP BY category
ORDER BY total_revenue DESC;
```

**[Script:]**

"Hardware comes first — higher total revenue. Books second. Sorting by an aggregated value in `ORDER BY` works naturally because the aggregate is computed during the `GROUP BY` phase, and by the time `ORDER BY` runs, each result row already has its `total_revenue` value."

> 🎯 **Instructor Note:** Ask: "In what order does SQL execute the clauses in a GROUP BY query? FROM first, then WHERE, then GROUP BY, then ORDER BY?" Let learners guess. The conceptual order is: FROM (get the rows), WHERE (filter individual rows), GROUP BY (form groups), aggregate (compute per group), ORDER BY (sort the results), LIMIT (cap the output). This order matters — it explains why WHERE cannot reference aggregate values, which is exactly the problem HAVING solves.

**Recap of Block 4 before moving on:**

- `GROUP BY column` divides rows into groups based on unique values in that column, then runs aggregate functions on each group independently
- Every column in `SELECT` when using `GROUP BY` must be either in the `GROUP BY` clause or inside an aggregate function
- `GROUP BY` can be combined with `JOIN` — join first, then group the combined result
- Multiple aggregate functions can appear in the same `GROUP BY` query, each computed per group
- `ORDER BY` on an aggregate alias sorts the grouped results

---

## Block 5 — HAVING: Filtering Grouped Data

### 5A — Why WHERE Cannot Filter Groups

**[Script:]**

"You now know how to group rows and compute aggregates per group. The next natural question is: how do I filter the groups? I do not want all categories — I only want categories with more than three orders. I do not want all users — I only want users who have spent more than one hundred dollars.

Your first instinct will be to use `WHERE`. Try it mentally: `WHERE COUNT(*) > 3`. That does not work. `WHERE` filters individual rows before grouping happens. By the time `WHERE` runs, the groups do not exist yet — there is nothing to count."

> 🎯 **Instructor Note:** This is the most important conceptual distinction in Block 5. Write the execution order on the board and keep it visible.

```
Execution order in a GROUP BY query:
1. FROM      — get the rows
2. WHERE     — filter individual rows
3. GROUP BY  — form groups
4. Aggregate — compute per group (COUNT, SUM, etc.)
5. HAVING    — filter groups by aggregate result
6. ORDER BY  — sort
7. LIMIT     — cap output
```

**[Script:]**

"HAVING runs at step 5 — after the groups are formed and after the aggregates are computed. That is the only point where aggregate values exist as filterable things. `WHERE` runs at step 2, before any of that happens.

The rule is simple: `WHERE` filters rows, `HAVING` filters groups."

> 🎯 **Instructor Note:** Write this on the board as a one-liner. Return to it after every demo.

```
WHERE  → filters rows    (before grouping)
HAVING → filters groups  (after aggregation)
```

---

### 5B — HAVING Syntax

**[Script:]**

"The `HAVING` clause goes after `GROUP BY` and before `ORDER BY`. Its syntax looks like `WHERE` — a condition — but it can reference aggregate functions because it runs after they are computed."

```sql
SELECT   column, AGGREGATE(column)
FROM     table
GROUP BY column
HAVING   AGGREGATE(column) condition
ORDER BY column;
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I group orders by category and filter with `HAVING COUNT(*) > 2`, which categories appear? Books has 2 orders, Hardware has 4." Answer: only Hardware — it has 4 orders, which is greater than 2. Books has exactly 2, which does not satisfy the `>` condition.

**Demo 9 — HAVING with COUNT (whiteboard-friendly)**

```sql
SELECT   category, COUNT(*) AS order_count
FROM     orders
GROUP BY category
HAVING   COUNT(*) > 2;
```

**[Script:]**

"One row: Hardware with 4. Books with 2 is excluded — 2 is not greater than 2. Change `> 2` to `>= 2` and both categories appear.

Notice that in the `HAVING` clause we wrote `COUNT(*)` again — not `order_count`. The alias defined in `SELECT` is generally not available in `HAVING` because `HAVING` is evaluated before the final column naming step. Some databases allow alias references in `HAVING` — SQLite is permissive here — but for portability, repeat the aggregate expression in `HAVING`."

> 🎯 **Instructor Note:** This alias-in-HAVING behavior varies by database. Mention it briefly, recommend repeating the aggregate expression as the safe habit, and move on. Do not go deep on database-specific behavior.

---

### 5C — HAVING with SUM and AVG

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I group by user_id and filter with `HAVING SUM(price) > 100`, which users appear?" With the data: Alice spent 49.99 + 199.99 = 249.98. Bob spent 29.99 + 39.99 = 69.98. Carol spent 34.99 + 89.99 = 124.98. Answer: Alice (249.98) and Carol (124.98). Bob at 69.98 is under 100.

**Demo 10 — HAVING with SUM (whiteboard-friendly)**

```sql
SELECT   user_id, SUM(price) AS total_spent
FROM     orders
GROUP BY user_id
HAVING   SUM(price) > 100;
```

**[Script:]**

"Two rows: user 1 at 249.98 and user 3 at 124.98. Bob — user 2 at 69.98 — is filtered out.

Let us add a JOIN to show names and an ORDER BY to sort by highest spenders."

```sql
SELECT   u.name, SUM(o.price) AS total_spent
FROM     orders AS o
JOIN     users  AS u ON u.id = o.user_id
GROUP BY u.name
HAVING   SUM(o.price) > 100
ORDER BY total_spent DESC;
```

**[Script:]**

"Alice at 249.98, Carol at 124.98. This is the 'top spenders' query — the backbone of any customer value report. JOIN to get names, GROUP BY to aggregate per customer, HAVING to filter to customers above a threshold, ORDER BY to rank them."

---

### 5D — WHERE and HAVING Together

**[Script:]**

"WHERE and HAVING can be used in the same query. They serve different purposes and both apply — they do not conflict. WHERE filters rows before grouping. HAVING filters groups after aggregation."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I add `WHERE category = 'Hardware'` to the previous grouped query, what changes?" Answer: only Hardware orders are considered before grouping. The SUM and GROUP BY then operate only on those four rows. Any HAVING condition is applied to groups formed only from Hardware rows.

**Demo 11 — WHERE and HAVING together (whiteboard-friendly)**

```sql
SELECT   user_id, SUM(price) AS hardware_spent
FROM     orders
WHERE    category = 'Hardware'
GROUP BY user_id
HAVING   SUM(price) > 50;
```

**[Script:]**

"Read this in execution order. First, WHERE filters to Hardware rows only — rows 1, 2, 3, and 6 from our data. Then GROUP BY groups those four rows by user_id. Then SUM computes the total per user within that filtered set. Then HAVING removes users who spent 50 or less on Hardware.

User 1 — Alice — spent 49.99 + 199.99 = 249.98 on Hardware. Passes.
User 2 — Bob — spent 29.99 on Hardware. Does not pass the `> 50` threshold.
User 3 — Carol — spent 89.99 on Hardware. Passes.

Two rows: user 1 and user 3.

This pattern — WHERE to narrow the rows before grouping, HAVING to filter the resulting groups — is the full picture of how filtering works in aggregation queries."

> 🎯 **Instructor Note:** Final pause. Ask the room to explain out loud the difference between WHERE and HAVING in one sentence each. Call on two or three people. The answers you are listening for: "WHERE filters individual rows before grouping" and "HAVING filters groups after the aggregate is computed." If learners can say this clearly, the concept is solid.

**Recap of Block 5 before moving on:**

- `WHERE` filters individual rows before groups are formed; it cannot reference aggregate functions
- `HAVING` filters groups after aggregation is complete; it can reference aggregate functions
- `HAVING` goes after `GROUP BY` and before `ORDER BY` in the query
- Repeat the aggregate expression in `HAVING` rather than referencing the SELECT alias, for portability
- `WHERE` and `HAVING` can coexist in the same query: WHERE narrows the input rows, HAVING narrows the output groups

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask learners to supply each answer before you confirm. "What is the difference between COUNT(*) and COUNT(column)? What does GROUP BY do to the rows? Why can't WHERE filter grouped results? What clause do you use instead?" Brisk pace — this is consolidation, not review for its own sake.

**Aggregation — Introduction and Purpose**

- Aggregation computes a single summary value from a set of rows: a count, a sum, an average, a minimum, or a maximum
- Aggregate functions run inside the database against the full dataset, avoiding the need to fetch all rows into application memory
- In SQL, `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` correspond to `len`, `sum`, `mean`, `min`, `max` in Python — same concept, executed server-side

**Aggregate Functions — COUNT, SUM, AVG, MIN, MAX**

- `COUNT(*)` counts all rows including NULLs; `COUNT(column)` counts only non-NULL values in that column
- `SUM(column)` totals a numeric column, ignoring NULL values — NULLs do not contribute to the total
- `AVG(column)` computes the mean of non-NULL values — rows with NULL are excluded from both sum and count, not treated as zero
- `MIN(column)` and `MAX(column)` return the smallest and largest values; both work on numbers, text, and dates; both ignore NULLs
- Multiple aggregate functions can be combined in one `SELECT`, each with an alias for readable output
- All aggregate functions run after `WHERE` filtering — the result reflects only the rows that pass the filter

**GROUP BY — Grouping Records and Performing Aggregations**

- `GROUP BY column` divides rows into groups based on unique values in that column; aggregate functions then run independently per group
- Every column in `SELECT` with a `GROUP BY` must be either in the `GROUP BY` clause or inside an aggregate function — ungrouped, unaggregated columns are not permitted
- `GROUP BY` can follow a `JOIN` — the grouping applies to the combined result after the join
- `ORDER BY` on an aggregated alias sorts the grouped result in the final output

**HAVING — Filtering Grouped Data**

- `WHERE` filters individual rows before grouping; it cannot reference aggregate functions because groups do not yet exist when WHERE runs
- `HAVING` filters groups after aggregation is complete; it can reference aggregate functions
- Execution order: FROM → WHERE → GROUP BY → aggregate → HAVING → ORDER BY → LIMIT
- `WHERE` and `HAVING` can coexist: WHERE narrows the input rows, HAVING narrows the output groups
- In `HAVING`, repeat the aggregate expression rather than referencing the SELECT alias, for cross-database portability

**Why All of This Matters Together**

- Every analytics feature in a real application — dashboard metrics, revenue breakdowns, customer rankings, category summaries — is answered by an aggregate query, often with GROUP BY and HAVING
- Aggregate functions eliminate the need to transfer entire datasets to the application layer for computation; the database returns one summary row instead of thousands of raw rows
- The WHERE-versus-HAVING distinction is what separates correct analytical queries from subtly wrong ones that appear to work but filter at the wrong stage; understanding execution order is the foundation of writing and debugging any aggregate query you encounter

---

*End of script.*
