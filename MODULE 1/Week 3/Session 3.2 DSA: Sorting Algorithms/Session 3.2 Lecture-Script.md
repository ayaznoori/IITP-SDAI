## Session Number

3.2

## Title

DSA: Sorting Algorithms

## Objective

Sort data using insertion sort, analyze its complexity and performance, and compare custom implementations with Python’s built-in sorting methods.

## Topics & Subtopics Covered

- **Insertion Sort**
  - Intuition and working of insertion sort
  - Implementing insertion sort in Python with step-by-step visualization
  - Tracing algorithm execution manually
  - Creating flowcharts for sorting logic
- **Complexity Analysis**
  - Analyzing time and space complexity
  - Evaluating efficiency and performance of insertion sort
- **Comparison**
  - Comparing Python’s built-in `sorted()` function and `list.sort()` method with custom implementations

---

# 🎤 Lecture Script — Insertion Sort & Sorting Algorithms

⏱️ **Total Duration: ~2 hours 30 minutes**

---

## 🎯 Hook (5 minutes)

Ask:

> “If I give you 10,000 numbers, how quickly can your program sort them?”

Discuss:

- Sorting is a **core DSA problem**.  
- Bank statements, leaderboards, search results — all use sorting.  

Explain:

> “Today we’ll deeply understand **one** sorting algorithm — insertion sort — and then compare it to Python’s built-in sorting.”

---

## Section 1 — Intuition for Insertion Sort (15 minutes)

Use playing cards analogy:

1. Start with one card (already ‘sorted’).  
2. Take the next card from the deck.  
3. Insert it into the correct place in your hand.  
4. Repeat until no cards remain.  

Show a quick visual on the board:

```text
Step 0: [5 | 2 4 6 1]
Step 1: [2 5 | 4 6 1]
Step 2: [2 4 5 | 6 1]
Step 3: [2 4 5 6 | 1]
Step 4: [1 2 4 5 6]
```

Highlight:

- Left side = sorted portion  
- Right side = unsorted portion  
- Each step inserts one element into sorted part.  

---

## Section 2 — Manual Tracing Example (25 minutes)

Take array:

```python
arr = [5, 2, 4, 6, 1]
```

Walk through passes:

- Pass 1: insert `2`  
- Pass 2: insert `4`  
- Pass 3: insert `6`  
- Pass 4: insert `1`  

Write on board:

### Pass 1 (i = 1, key = 2)

```text
[5, 2, 4, 6, 1]
 key = 2, j = 0
 compare 2 and 5 → shift 5
[5, 5, 4, 6, 1]
 insert 2 at position 0
[2, 5, 4, 6, 1]
```

Do similar for other passes, involving students in predicting moves.

Emphasize:

- Elements **shift** right, not randomly swapped.  

---

## Section 3 — Coding Insertion Sort Step-by-Step (30 minutes)

Start from skeleton:

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1

        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        arr[j + 1] = key
```

Walk through line-by-line:

- Why `range(1, len(arr))`?  
- Why `j = i - 1`?  
- Condition `j >= 0 and arr[j] > key` meaning.  

Add print-based tracing:

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        print("Pass", i, "- key:", key)

        while j >= 0 and arr[j] > key:
            print("  shifting", arr[j], "to position", j + 1)
            arr[j + 1] = arr[j]
            j -= 1

        arr[j + 1] = key
        print("  array now:", arr)
```

Run with `[5, 2, 4, 6, 1]` and discuss each output line.

---

## Section 4 — Flowchart / Pseudocode (15 minutes)

On board, draw:

1. Start  
2. For each index `i` from 1 to n-1  
3. Save `key = arr[i]`  
4. Initialize `j = i - 1`  
5. While `j >= 0` and `arr[j] > key`, shift `arr[j]` to `arr[j + 1]` and decrement `j`  
6. Place `key` at `arr[j + 1]`  
7. Repeat  

Optional: ask one student to narrate algorithm verbally while you point to steps in flowchart.

---

## Section 5 — Complexity Analysis (25 minutes)

Start with intuitive questions:

- “What happens to the number of comparisons as the list grows?”  

### Best Case (Already Sorted)

In array like `[1, 2, 3, 4, 5]`:

- For each `i`, `arr[j] > key` is False on first check.  
- Inner `while` rarely runs.  
- Total work ≈ proportional to `n`.  
- Time complexity: **O(n)**.  

### Worst Case (Reverse Sorted)

In array like `[5, 4, 3, 2, 1]`:

- For each `i`, `key` is smaller than all previous elements.  
- Inner `while` runs many times:

```text
i = 1 → up to 1 comparison
i = 2 → up to 2 comparisons
i = 3 → up to 3 comparisons
...
i = n-1 → up to n-1 comparisons
```

Total ≈ \( 1 + 2 + \dots + (n-1) = \frac{n(n-1)}{2} \) → **O(n²)**.

### Average Case

- Also **O(n²)** for random data.  

### Space Complexity

- In-place algorithm, only a few extra variables: `key`, `i`, `j`.  
- Space complexity: **O(1)** extra space.  

Summarize in a table on board:

| Case        | Time Complexity |
|------------|-----------------|
| Best       | O(n)            |
| Average    | O(n²)           |
| Worst      | O(n²)           |

---

## Section 6 — When Is Insertion Sort Reasonable? (10 minutes)

Discuss:

- For **small arrays**, constant factors matter more than complexity; insertion sort can be fine.  
- For **almost sorted** arrays, insertion sort can be quite efficient (few shifts).  
- Many advanced algorithms (like Timsort) use insertion sort as a building block on small segments.  

Key message:

> “Insertion sort is not for huge datasets, but it’s very useful to understand and good for small or nearly sorted data.”

---

## Section 7 — Built-in Sorting in Python (20 minutes)

Introduce:

```python
nums = [5, 2, 4, 6, 1]

print(sorted(nums))  # new sorted list
nums.sort()          # sort in place
print(nums)
```

Explain:

- `sorted(iterable)`:
  - Works on any iterable (list, tuple, etc.)  
  - Returns a **new** sorted list  
- `list.sort()`:
  - Method of list  
  - Sorts the list **in-place**  

### Compare With Our Insertion Sort

Write a simple driver:

```python
data = [5, 2, 4, 6, 1]

copy1 = data.copy()
copy2 = data.copy()

insertion_sort(copy1)
print("Insertion sort:", copy1)

print("sorted():", sorted(copy2))
```

Show that results are the same, but:

- Built-ins are usually **far faster** on large data.  
- Python’s sort uses **Timsort** (O(n log n)).  

---

## Section 8 — Conceptual Performance Comparison (15 minutes)

Without deep benchmarking, discuss:

- For n = 10:
  - Insertion sort is okay.  
- For n = 10,000:
  - O(n²) ~ 100 million steps  
  - O(n log n) ~ 10,000 × log₂(10,000) ≈ 10,000 × 14 = 140,000  

Show big difference in step counts.

Key message:

> “Always prefer built-in sort in production; use custom sorts mainly for learning and special cases.”

---

## Section 9 — Guided Practice (20 minutes)

Give short tasks:

1. Modify insertion sort to sort in **descending** order.  
2. Write `insertion_sort_strings` that sorts a list of strings alphabetically.  
3. Dry run insertion sort by hand on `[3, 1, 4, 2]` and compare your steps with program output.  

Help students:

- Adjust comparison operators correctly (`<` vs `>`).  
- Understand how string comparison works in Python.  

---

## Section 10 — Wrap-Up (5 minutes)

Summarize:

1. Insertion sort builds a sorted portion by inserting elements one by one.  
2. It has O(n²) time in general, O(n) in best case, O(1) extra space.  
3. It’s good for small or nearly sorted data, but not large random datasets.  
4. Python’s built-in `sorted()` / `.sort()` use more advanced algorithms (Timsort) and are preferred in practice.  

End with:

> “Understanding insertion sort gives you a strong base for analyzing and comparing other sorting algorithms.”

