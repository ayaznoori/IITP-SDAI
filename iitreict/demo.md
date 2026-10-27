# Filtering, Sorting, and Deriving Columns in Pandas

### Easy Questions (Q1–Q4) — Recall and direct application

**Question 1 (Easy MCQ - 1 correct option):**
A junior data analyst at an e-commerce company is filtering a Pandas DataFrame for mid-tier products. The code is:

```python
mid_tier = df[df["unit_price"].between(1000, 10000)]
```

No `inclusive` argument is passed. The table has one product priced at exactly 1000 and another priced at exactly 10000.

**Which boundary prices does this call keep?**

- A) Only the price of exactly 1000, the 10000 price is dropped
- B) Both the price of exactly 1000 and the price of exactly 10000
- C) Neither boundary price, only prices strictly between the two
- D) Only the price of exactly 10000, the 1000 price is dropped

**Correct Answer:** B

**Answer Explanation:**
- **A)** Wrong. Keeping only the lower bound is what `inclusive="left"` does. The default is not `"left"`.
- **B)** Correct. By default `.between()` uses `inclusive="both"`, so a value equal to either bound passes the filter.
- **C)** Wrong. Dropping both boundary values is the behaviour of `inclusive="neither"`, which must be passed explicitly.
- **D)** Wrong. Keeping only the upper bound is what `inclusive="right"` does. The default keeps both ends.

**Question 2 (Easy MCQ - 1 correct option):**
A backend engineer wants a column of product names in capital letters. The engineer runs `df["product"].upper()` and gets an `AttributeError`.

**Which change makes the code work?**

- A) Convert the column with pd.to_datetime() before calling .upper()
- B) Select it with double brackets: df[["product"]].upper()
- C) Add inplace=True so .upper() edits the column directly
- D) Use the .str accessor: df["product"].str.upper()

**Correct Answer:** D

**Answer Explanation:**
- **A)** Wrong. `pd.to_datetime()` turns text into date-time values. It has nothing to do with string methods, and a product name is not a date.
- **B)** Wrong. Double square brackets select columns and return a DataFrame. A DataFrame is still not a Python string, so `.upper()` remains unavailable.
- **C)** Wrong. `inplace=True` is an argument of methods such as `drop()`. It cannot make a string method exist on a Series.
- **D)** Correct. A column is a Pandas Series, not a Python string. String methods are reached through the `.str` accessor, so `df["product"].str.upper()` capitalises every value.

**Question 3 (Easy MCQ - 1 correct option):**
A category manager asks a data analyst for an order list grouped by category in alphabetical order. Inside each category, the most expensive products must come first.

**Which call produces this order?**

- A) df.sort_values(by=["category", "unit_price"], ascending=[True, False])
- B) df.sort_values(by=["unit_price", "category"], ascending=[False, True])
- C) df.sort_values(by=["category", "unit_price"], ascending=[False, True])
- D) df.sort_values(by=["category", "unit_price"], ascending=False)

**Correct Answer:** A

**Answer Explanation:**
- **A)** Correct. The first column in `by` is the main sort key. `True` sorts category A to Z, and `False` sorts unit price from highest to lowest within each category.
- **B)** Wrong. Putting `unit_price` first makes price the main key, so rows are no longer grouped by category.
- **C)** Wrong. The flags are flipped. Categories run Z to A and prices run lowest first inside each category.
- **D)** Wrong. A single `False` makes both columns descending, so categories run Z to A instead of alphabetically.

**Question 4 (Easy MCQ - 1 correct option):**
An inventory analyst has a small DataFrame of quantities and runs this code:

```python
import pandas as pd

stock = pd.DataFrame({"quantity": [30, 70, 60, 60, 25]})
top = stock.nlargest(2, "quantity", keep="all")
print(top)
```

**How many rows does `top` hold, and which values?**

- A) 2 rows: 70 and the first 60
- B) 2 rows: 70 and the last 60
- C) 3 rows: 70 and both 60s
- D) 4 rows: 70, both 60s and 30

**Correct Answer:** C

**Answer Explanation:**
- **A)** Wrong. Returning 70 and only the first 60 is the result of `keep="first"`.
- **B)** Wrong. Returning 70 and only the last 60 is the result of `keep="last"`.
- **C)** Correct. With `keep="all"`, every row tied at the cut-off survives, even when the result has more than n rows. The value 60 appears twice, so 70 plus both 60s gives 3 rows.
- **D)** Wrong. `keep="all"` only keeps extra rows that tie at the cut-off value. 30 is below 60, so it is not part of the top two values.

---

### Moderate Questions (Q5–Q6 MCQ, Q7–Q8 MSQ)

**Question 5 (Moderate MCQ - 1 correct option):**
A data engineer runs a nightly job on a 10-million-row orders table in the cloud. The job only needs the 5 most expensive products. Cloud processing time is billed, so the engineer wants the cheapest correct method.

**Which approach should the engineer choose?**

- A) df.sort_values("unit_price", ascending=False).head(5), since sorting is O(n)
- B) df.nlargest(5, "unit_price"), since it avoids a full sort and runs near O(n)
- C) df.sort_index().head(5), since index order already matches price order
- D) df.nsmallest(5, "unit_price"), since it skips the full sort and runs near O(n)

**Correct Answer:** B

**Answer Explanation:**
- **A)** Wrong. The result would be correct, but a full sort costs about O(n log n), not O(n). On millions of rows that wasted effort is exactly what should be avoided.
- **B)** Correct. `nlargest()` returns the top n rows without sorting the whole column, so it runs in roughly linear time, O(n). That saves real processing time at this scale.
- **C)** Wrong. `sort_index()` orders rows by their index labels, not by price. Nothing guarantees that label order matches price order.
- **D)** Wrong. `nsmallest()` is equally fast, but it returns the cheapest rows, not the most expensive ones.

**Question 6 (Moderate MCQ - 1 correct option):**
A reporting analyst wants to remove a constant `shipping_fee` column. The analyst runs these two lines:

```python
df.drop(columns=["shipping_fee"])
print(df.columns)
```

The printed list still contains `shipping_fee`, and no error appears.

**What explains this, and what is the fix?**

- A) drop() needs the column name in an index argument, not columns
- B) shipping_fee holds one constant value, and constant columns cannot be dropped
- C) drop() failed silently because the column was already gone
- D) drop() returned a new frame, so reassign it or pass inplace=True

**Correct Answer:** D

**Answer Explanation:**
- **A)** Wrong. `columns=[...]` is the correct way to name columns for `drop()`. The call itself worked.
- **B)** Wrong. A constant column is an ordinary column. It can be dropped like any other.
- **C)** Wrong. Dropping a column that is already gone raises an error rather than failing silently. Here the column still exists.
- **D)** Correct. Without reassignment or `inplace=True`, `drop()` returns a result and leaves the original DataFrame unchanged. Writing `df = df.drop(columns=["shipping_fee"])` or `df.drop(columns=["shipping_fee"], inplace=True)` fixes it.

**Question 7 (Moderate MSQ - Multiple correct options):**
A data analyst loads an orders CSV file with Pandas. The `order_date` column holds text such as `2024-01-08`. The analyst plans to convert it with `pd.to_datetime()` and then work out the weekday and the age of each order.

**Which statements about this date work are correct?**

- A) Before conversion, df["order_date"].dtype reports object
- B) .dt.day_name() returns numbers 1 to 7 instead of names
- C) (pd.Timestamp("2024-03-31") - df["order_date"]).dt.days gives days
- D) Calling .dt.month on the raw text column returns the month

**Correct Answers:** A, C

**Answer Explanation:**
- **A)** Correct. A date column loaded from a CSV file arrives as strings, and Pandas reports strings with the `object` dtype.
- **B)** Wrong. `day_name()` returns weekday names such as Monday or Sunday. It does not return numbers.
- **C)** Correct. After conversion, subtracting the dates from a fixed timestamp gives a time difference, and `.dt.days` turns it into a whole number of days.
- **D)** Wrong. The `.dt` accessor works on datetime columns. The text must first be converted with `pd.to_datetime()` before any date part can be extracted.

**Question 8 (Moderate MSQ - Multiple correct options):**
A pricing analyst is deriving a label column for orders with this line:

```python
df["order_tier"] = np.where(df["total_sales"] > 50000, "High Value", "Standard")
```

**Which two statements about this labelling approach are true?**

- A) A row with total_sales of exactly 50000 is labelled "High Value"
- B) A row with total_sales of 50001 is labelled "High Value"
- C) A new column set by one .loc condition shows "Standard" in other rows
- D) A third tier needs another np.where() nested in the false branch

**Correct Answers:** B, D

**Answer Explanation:**
- **A)** Wrong. The condition is strictly greater than 50,000. A value of exactly 50,000 fails it and gets "Standard".
- **B)** Correct. 50,001 is greater than 50,000, so the condition is true and the row gets "High Value".
- **C)** Wrong. When `.loc` creates a new column for only the rows that meet a condition, the other rows become `NaN`. They get no fallback label unless a second `.loc` assignment sets one.
- **D)** Correct. `np.where()` has one true value and one false value. Nesting another `np.where()` in the false branch adds a further tier, such as "Mid Tier".

---

### Hard Questions (Q9–Q10 MSQ)

**Question 9 (Hard MSQ - Multiple correct options):**
A revenue analyst at an online store is deriving billing and tier columns from this orders table:

| order_id | product | quantity | unit_price |
|---|---|---|---|
| 2001 | Monitor | 3 | 12000 |
| 2002 | Mouse | 20 | 400 |
| 2003 | Desk Chair | 6 | 5000 |
| 2004 | Laptop | 1 | 52000 |
| 2005 | Headphones | 10 | 1200 |

The analyst runs:

```python
df["total_sales"] = df["quantity"] * df["unit_price"]
df["final_billing_amount"] = np.where(
    df["quantity"] > 5, df["total_sales"] * 0.9, df["total_sales"]
)
df["detailed_order_tier"] = np.where(
    df["total_sales"] > 50000, "High Value",
    np.where(df["total_sales"] > 25000, "Mid Tier", "Standard")
)
```

**Which statements about the resulting columns are correct?**

- A) The desk chair order bills 27000 yet stays in the "Mid Tier" tier
- B) Exactly two of the five orders end up labelled "Mid Tier"
- C) The headphones order bills 12000, as quantity 10 is not above 5
- D) The laptop order is "High Value" and its bill stays at 52000

**Correct Answers:** A, B, D

**Answer Explanation:**
Total sales per order: Monitor 3 × 12,000 = 36,000. Mouse 20 × 400 = 8,000. Desk Chair 6 × 5,000 = 30,000. Laptop 1 × 52,000 = 52,000. Headphones 10 × 1,200 = 12,000.
- **A)** Correct. Quantity 6 is above 5, so the bill is 30,000 × 0.9 = 27,000. The tier uses `total_sales` (30,000), which is above 25,000 but not above 50,000, so it stays "Mid Tier".
- **B)** Correct. Monitor (36,000) and Desk Chair (30,000) fall above 25,000 and up to 50,000. Laptop is "High Value", and Mouse and Headphones are "Standard".
- **C)** Wrong. Quantity 10 is greater than 5, so the discount applies. The bill is 12,000 × 0.9 = 10,800, not 12,000.
- **D)** Correct. 52,000 is above 50,000, so the tier is "High Value". Quantity 1 is not above 5, so the bill equals total sales, 52,000.

**Question 10 (Hard MSQ - Multiple correct options):**
A data engineer starts from this DataFrame, whose index labels are 0 to 4:

| index | product | unit_price |
|---|---|---|
| 0 | Laptop | 48000 |
| 1 | Mouse | 600 |
| 2 | Monitor | 11000 |
| 3 | Bookshelf | 3200 |
| 4 | Headphones | 1800 |

The engineer then runs:

```python
scrambled = df.sort_values("unit_price")
```

**Which conclusions about `scrambled` and the follow-up calls hold?**

- A) scrambled.sort_index() returns Laptop as its first row
- B) scrambled.reset_index(drop=True) restores the original row order
- C) After scrambled.reset_index(drop=True), the row labelled 0 is Mouse
- D) scrambled.iloc[0] returns Laptop, because Laptop carries label 0

**Correct Answers:** A, C

**Answer Explanation:**
Sorting ascending by price gives Mouse (label 1), Headphones (4), Bookshelf (3), Monitor (2), Laptop (0), so the labels read 1, 4, 3, 2, 0.
- **A)** Correct. `sort_index()` reorders rows by their existing labels, so label 0 (Laptop) returns to the top and the original order is restored.
- **B)** Wrong. `reset_index(drop=True)` keeps the current price order and only renumbers the labels 0 to 4. It does not restore the original order.
- **C)** Correct. After the reset, Mouse is still the first row and now carries the new label 0.
- **D)** Wrong. `.iloc` means integer location, so `.iloc[0]` is the first row by position. In `scrambled` that row is Mouse, whatever its label.

---

### Hard Subjective (Q11)

**Question 11 (Hard Subjective - Practical Task):**

**1. Problem Statement:**
**Your task:** Write a Pandas function that turns raw order rows into a ranked sales report with derived columns.
You are a data analyst at an e-commerce company. The sales head wants one clean report of mid-range orders, ranked by revenue. Each row must show a short product code, the weekday and a value tier.

**2. Approach:**
1. Convert `order_date` from text to real dates with `pd.to_datetime()`.
2. Add `total_sales` as quantity times unit price, and `product_code` as the first three letters of the product in capitals.
3. Add `day_of_week` with the weekday name of each order date.
4. Add `order_tier`: above 50,000 is "High Value", above 25,000 is "Mid Tier", and anything else is "Standard".
5. Keep orders with a unit price from 1,000 to 45,000, both ends included. Sort by `total_sales` from highest to lowest and renumber the rows from 0.
6. Drop `product`, `category`, `order_date`, `quantity` and `unit_price` so only the report columns remain.

**3. Expected Output:**

Sample input (the `orders` DataFrame, already built in the Starter Code):

```text
order_id,product,category,order_date,quantity,unit_price
3001,Laptop,Electronics,2024-01-08,2,42000
3002,Mouse,Electronics,2024-01-09,12,450
3003,Desk Chair,Furniture,2024-01-10,5,4800
3004,Monitor,Electronics,2024-01-12,3,11000
3005,Bookshelf,Furniture,2024-01-13,4,3200
3006,Headphones,Electronics,2024-01-14,18,1400
3007,Coffee Table,Furniture,2024-01-15,1,7000
3008,Laptop,Electronics,2024-01-16,1,46000
```

Expected output for that input:

```text
   order_id  total_sales product_code day_of_week  order_tier
0      3001        84000          LAP      Monday  High Value
1      3004        33000          MON      Friday    Mid Tier
2      3006        25200          HEA      Sunday    Mid Tier
3      3003        24000          DES   Wednesday    Standard
4      3005        12800          BOO    Saturday    Standard
5      3007         7000          COF      Monday    Standard
```

**4. Starter Code:**

```python
import pandas as pd
import numpy as np

# Sample input - already filled in for you.
orders = pd.DataFrame({
    "order_id": [3001, 3002, 3003, 3004, 3005, 3006, 3007, 3008],
    "product": ["Laptop", "Mouse", "Desk Chair", "Monitor", "Bookshelf",
                "Headphones", "Coffee Table", "Laptop"],
    "category": ["Electronics", "Electronics", "Furniture", "Electronics",
                 "Furniture", "Electronics", "Furniture", "Electronics"],
    "order_date": ["2024-01-08", "2024-01-09", "2024-01-10", "2024-01-12",
                   "2024-01-13", "2024-01-14", "2024-01-15", "2024-01-16"],
    "quantity": [2, 12, 5, 3, 4, 18, 1, 1],
    "unit_price": [42000, 450, 4800, 11000, 3200, 1400, 7000, 46000],
})


def build_report(df):
    df = df.copy()
    # TODO: convert order_date to datetime
    # TODO: add total_sales and product_code
    # TODO: add day_of_week and order_tier
    # TODO: filter by unit_price, sort by total_sales, renumber rows
    # TODO: drop the columns the report does not need
    return df


print(build_report(orders).to_string())
```

**Bonus (Optional):**
Add a `final_billing_amount` column that applies a 10% discount when quantity is greater than 5. Then print the weekday with the highest `total_sales` by reading the top row with `.iloc[0]`. Not graded.

**Submission Format:**
Paste your complete answer (including code, if applicable) into the text box on the platform, following the structure shown in the Starter Code above.

**Answer Explanation (Model Answer):**

- **Ideal Approach Walkthrough:**
  1. Copy the input so the original `orders` DataFrame stays unchanged while deriving new columns.
  2. Convert `order_date` with `pd.to_datetime()`. Before this step the column is `object`, and the `.dt` accessor would not work.
  3. Multiply `quantity` by `unit_price`. Column arithmetic is vectorised, so every row is computed in one line.
  4. Build `product_code` with `.str[:3].str.upper()`. A column is a Series, so string methods need the `.str` accessor.
  5. Use `.dt.day_name()` for the weekday and a nested `np.where()` for the three tiers. The inner `np.where()` sits in the false branch of the outer one.
  6. Filter with `.between(1000, 45000)`. The default `inclusive="both"` keeps both bounds. This removes the Mouse (450) and the 46,000 Laptop.
  7. Sort descending by `total_sales`, then `reset_index(drop=True)` renumbers rows 0 to 5 without adding the old index as a column.
  8. Reassign the result of `drop()`. Without reassignment the columns would stay.
- **Expected Code / Solution Files:**
  ```python
  import pandas as pd
  import numpy as np

  orders = pd.DataFrame({
      "order_id": [3001, 3002, 3003, 3004, 3005, 3006, 3007, 3008],
      "product": ["Laptop", "Mouse", "Desk Chair", "Monitor", "Bookshelf",
                  "Headphones", "Coffee Table", "Laptop"],
      "category": ["Electronics", "Electronics", "Furniture", "Electronics",
                   "Furniture", "Electronics", "Furniture", "Electronics"],
      "order_date": ["2024-01-08", "2024-01-09", "2024-01-10", "2024-01-12",
                     "2024-01-13", "2024-01-14", "2024-01-15", "2024-01-16"],
      "quantity": [2, 12, 5, 3, 4, 18, 1, 1],
      "unit_price": [42000, 450, 4800, 11000, 3200, 1400, 7000, 46000],
  })


  def build_report(df):
      df = df.copy()
      df["order_date"] = pd.to_datetime(df["order_date"])
      df["total_sales"] = df["quantity"] * df["unit_price"]
      df["product_code"] = df["product"].str[:3].str.upper()
      df["day_of_week"] = df["order_date"].dt.day_name()
      df["order_tier"] = np.where(
          df["total_sales"] > 50000, "High Value",
          np.where(df["total_sales"] > 25000, "Mid Tier", "Standard")
      )
      df = df[df["unit_price"].between(1000, 45000)]
      df = df.sort_values("total_sales", ascending=False)
      df = df.reset_index(drop=True)
      df = df.drop(columns=["product", "category", "order_date", "quantity", "unit_price"])
      return df


  print(build_report(orders).to_string())
  ```
- **Alternative Approaches:** The two-sided filter can also be written as `(df["unit_price"] >= 1000) & (df["unit_price"] <= 45000)`, though `.between()` is more readable. The drop can use `df.drop(columns=[...], inplace=True)` instead of reassignment. Filtering before deriving the columns gives the same report and computes fewer rows.
- **Bonus (if present):** Add `df["final_billing_amount"] = np.where(df["quantity"] > 5, df["total_sales"] * 0.9, df["total_sales"])` before the drop. After sorting, `report["day_of_week"].iloc[0]` gives "Monday", since order 3001 (84,000) is the top row.