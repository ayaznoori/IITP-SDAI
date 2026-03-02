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

# 1️⃣ Insertion Sort — Intuition

Insertion sort works like sorting **playing cards** in your hand:

1. Assume the first card is in the correct place.  
2. Pick the next card.  
3. Move it left until it reaches its correct position among the already-sorted cards.  
4. Repeat for each card.  

We maintain:

- A **sorted portion** on the left  
- An **unsorted portion** on the right  

At each step, we **insert** one element from the unsorted part into the correct position in the sorted part.

---

# 2️⃣ Step-by-Step Example

Sort the list:

```python
arr = [5, 2, 4, 6, 1]
```

We process from index 1 onward.

### Pass 1 (i = 1, key = 2)

Sorted part: `[5] | 2 4 6 1`

- Compare 2 with 5  
- 2 < 5 → shift 5 to the right  

Result:

```text
[2, 5, 4, 6, 1]
```

### Pass 2 (i = 2, key = 4)

Sorted part: `[2, 5] | 4 6 1`

- Compare 4 with 5 → 4 < 5 → shift 5 right  
- Compare 4 with 2 → 4 ≥ 2 → stop  

Result:

```text
[2, 4, 5, 6, 1]
```

### Pass 3 (i = 3, key = 6)

Sorted part: `[2, 4, 5] | 6 1`

- 6 ≥ 5 → already in correct place  

Result:

```text
[2, 4, 5, 6, 1]
```

### Pass 4 (i = 4, key = 1)

Sorted part: `[2, 4, 5, 6] | 1`

- 1 < 6 → shift 6  
- 1 < 5 → shift 5  
- 1 < 4 → shift 4  
- 1 < 2 → shift 2  
- Now insert 1 at start  

Final:

```text
[1, 2, 4, 5, 6]
```

---

# 3️⃣ Insertion Sort — Python Implementation

```python
def insertion_sort(arr):
    # Start from index 1 because arr[0] is considered sorted
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1

        # Move elements of arr[0..i-1], that are greater than key,
        # one position ahead of their current position
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        # Place key at its correct position
        arr[j + 1] = key
```

Key points:

- `key` is the element we’re trying to insert.  
- `j` moves left as long as elements are greater than `key`.  
- Elements are **shifted right** to make space.  

---

# 4️⃣ Tracing and Visualizing the Algorithm

To understand insertion sort deeply, trace variables:

```python
arr = [5, 2, 4, 6, 1]

for i in range(1, len(arr)):
    key = arr[i]
    j = i - 1
    print("Pass", i, "- key:", key)

    while j >= 0 and arr[j] > key:
        print("  comparing", key, "with", arr[j])
        arr[j + 1] = arr[j]
        j -= 1

    arr[j + 1] = key
    print("  array now:", arr)
```

This helps students see:

- Which elements are compared  
- How the array changes after each pass  

---

# 5️⃣ Flowchart-Level Logic

High-level flow:

1. For `i` from 1 to `n-1`  
2. Set `key = arr[i]`  
3. Set `j = i - 1`  
4. While `j >= 0` and `arr[j] > key`:  
   - Shift `arr[j]` right to `arr[j + 1]`  
   - Decrease `j`  
5. Place `key` at `arr[j + 1]`  

This can be turned into a flowchart showing:

- Outer loop  
- Inner while loop  
- Decision diamonds for comparisons  

---

# 6️⃣ Time Complexity of Insertion Sort

Let \( n \) be the number of elements.

### Best Case (Already Sorted)

- Each new key is greater than previous elements  
- Inner `while` almost never runs  
- Complexity: **O(n)**  

### Worst Case (Reverse Sorted)

- Every new key is smaller than all previous elements  
- Inner `while` runs many times for each `i`  
- Number of comparisons is roughly:

\[
1 + 2 + 3 + \dots + (n - 1) = \frac{n(n - 1)}{2}
\]

- Complexity: **O(n²)**  

### Average Case

- On average we assume about half of the elements are shifted  
- Complexity: **O(n²)**  

### Space Complexity

- We sort **in-place** (no extra large data structures)  
- Space complexity: **O(1)** extra space  

---

# 7️⃣ When Is Insertion Sort Useful?

Strengths:

- Simple to implement and understand  
- Works well for **small arrays**  
- Efficient if data is **almost sorted**  
- In-place and stable (does not reorder equal elements unnecessarily)  

Weaknesses:

- Slow for **large, random** datasets due to O(n²) time  

---

# 8️⃣ Python’s Built-in Sorting vs Insertion Sort

Python offers:

```python
nums = [5, 2, 4, 6, 1]

sorted_nums = sorted(nums)  # returns new list
print(sorted_nums)

nums.sort()                 # sorts in place
print(nums)
```

Notes:

- Built-in sorting is **highly optimized** and uses an algorithm called **Timsort** (hybrid of merge sort + insertion sort ideas).  
- Time complexity: usually **O(n log n)**.  

Comparison:

- **Insertion sort**: O(n²) in general, O(n) in best case.  
- **Built-ins**: O(n log n) in general, designed for real-world performance.  

In practice:

- Use insertion sort for **learning** and very small cases.  
- Use built-in `sorted()` / `.sort()` in real applications.  

---

# 9️⃣ Common Mistakes

- Forgetting to place `key` back at `arr[j + 1]`.  
- Using wrong comparison condition (e.g., `arr[j] < key` instead of `arr[j] > key` when sorting ascending).  
- Off-by-one errors in loop ranges.  
- Not understanding that the inner loop **shifts** elements, it doesn’t swap only once.  

---

# 🔟 Practice Ideas

1. Manually trace insertion sort on a small list like `[3, 1, 4, 2]`.  
2. Modify the algorithm to sort in **descending** order.  
3. Compare the time of your insertion sort with `sorted()` on lists of size 10, 100, 1000 (conceptually or using `time` module).  

