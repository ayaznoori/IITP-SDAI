# LECTURE SCRIPT

# Problem Solving – Searching, Sorting, and Recursion for Efficient Solutions

**Total Duration: 2 Hours 30 Minutes**

---

##  Hook (10 minutes)

“Two students solve the same coding problem.

One writes 25 lines and it passes.
Another writes 10 lines and it passes faster.

Who solved it better?”

- In DSA, correct is important.
- Efficient is more important.

Today’s class is about **thinking like a problem solver**, not just writing code.

By the end of this session, students will be able to:

✅ Identify whether a problem needs searching or sorting  
✅ Use sorting to simplify problem solving  
✅ Combine recursion with searching/sorting  
✅ Write brute-force and optimized solutions  
✅ Analyze time complexity of different approaches

---

## Section 1 — Problem Solving Framework (20 minutes)

Start with this board flow:

### Step 1: Understand the problem
Ask students:

* What is the input?
* What is the output?
* Are there constraints?
* Is the data already sorted?

### Step 2: Solve small examples manually

Example:

```text
Array = [4, 2, 7, 1]
Target = 7
```

Ask:
“How would you solve this without coding?”

Students will usually say:
“Check one by one.”

Good — that is the brute-force solution.

### Step 3: Think optimization
Ask:
“Can sorting help?”
“Can searching help?”
“Can recursion help?”

This section is important because students often jump to coding too early.

---

## Section 2 — Searching as a Problem Solving Tool (20 minutes)

### Problem 1: Check if target exists

```python
arr = [4, 9, 1, 7, 3]
target = 7
```

### Brute Force — Linear Search

```python
def exists(arr, target):
    for num in arr:
        if num == target:
            return True
    return False
```

### Teaching points

* Works on any array
* Easy to write
* Time Complexity = **O(n)**

Ask students:

👉 “Can this be faster?”

Expected answer:
Only if data is sorted.

---

## Section 3 — Sorting Before Solving (20 minutes)

Explain:

Many problems are hard in unsorted form, but easy after sorting.

### Problem 2: Find if a pair sums to target

```text
arr = [8, 4, 1, 6, 2]
target = 10
```

### Brute Force

Try all pairs.

```python
def pair_sum_bruteforce(arr, target):
    n = len(arr)
    for i in range(n):
        for j in range(i + 1, n):
            if arr[i] + arr[j] == target:
                return True
    return False
```

### Complexity

* Time = **O(n²)**

Now ask:
“Can sorting help?”

### Optimized Idea

Sort array first:

```text
[1, 2, 4, 6, 8]
```

Use two pointers.

```python
def pair_sum_sorted(arr, target):
    arr.sort()
    left = 0
    right = len(arr) - 1

    while left < right:
        s = arr[left] + arr[right]
        if s == target:
            return True
        elif s < target:
            left += 1
        else:
            right -= 1
    return False
```

### Complexity

* Sorting = **O(n log n)**
* Two pointers = **O(n)**

Total = **O(n log n)**

This is a key problem-solving moment.

---

## Section 4 — Binary Search as Efficient Problem Solving (25 minutes)

### Problem 3: Search target in sorted array

```text
arr = [1, 3, 5, 7, 9, 11]
target = 7
```

Ask students:
“If the array is sorted, why should we still check every element?”

Introduce Binary Search.

### Iterative Binary Search

```python
def binary_search(arr, target):
    left = 0
    right = len(arr) - 1

    while left <= right:
        mid = (left + right) // 2

        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1
```

### Teaching points

* Only works on sorted arrays
* Eliminates half the search space each step
* Time Complexity = **O(log n)**

Use whiteboard trace:

```text
[1, 3, 5, 7, 9, 11]
mid = 5
target = 7
```

Walk step by step.

---

## Section 5 — Recursion + Searching (20 minutes)

Explain:

Binary Search is also a classic recursion problem.

### Recursive Binary Search

```python
def binary_search_recursive(arr, target, left, right):
    if left > right:
        return -1

    mid = (left + right) // 2

    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, right)
    else:
        return binary_search_recursive(arr, target, left, mid - 1)
```

Call:

```python
arr = [1, 3, 5, 7, 9, 11]
print(binary_search_recursive(arr, 7, 0, len(arr)-1))
```

### Teaching points

* Recursion solves smaller subproblems
* Base case is very important
* Same logic as iterative version

Ask:
“What gets smaller in every recursive call?”

Answer:
Search space.

---

## Section 6 — Sorting + Searching Together in Problems (25 minutes)

### Problem 4: Find first occurrence after sorting

```text
arr = [4, 2, 2, 7, 2, 9]
target = 2
```

Approach:

1. Sort array
2. Use Binary Search
3. Move left to find first occurrence

```python
def first_occurrence(arr, target):
    arr.sort()
    left = 0
    right = len(arr) - 1
    ans = -1

    while left <= right:
        mid = (left + right) // 2

        if arr[mid] == target:
            ans = mid
            right = mid - 1
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return ans
```

### Teaching points

* One algorithm is often not enough
* Real problem solving combines techniques

This is a very important mindset shift.

---

## Section 7 — Recursion + Sorting (Merge Sort Idea) (20 minutes)

Introduce idea without going too deep if students are early-stage.

Explain:

Merge Sort uses:

* Recursion to split array
* Sorting logic to merge results

### Concept

```text
[5, 2, 8, 1]
→ split
[5, 2] and [8, 1]
→ split more
→ merge in sorted order
```

### Mini Merge Sort Example

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

### Teaching points

* Divide and conquer
* Recursion breaks big problem into smaller problems
* Merge step builds final answer

---

## Section 8 — Comparing Approaches (10 minutes)

Use a comparison table on board:

| Problem | Brute Force | Better Approach |
|---|---|---|
| Search target | Linear Search O(n) | Binary Search O(log n) |
| Pair Sum | Nested loops O(n²) | Sort + Two Pointers O(n log n) |
| Sorting large array | Manual comparisons | Merge Sort O(n log n) |

Ask students:
“What changes an average coder into a good problem solver?”

Expected answer:
Choosing the right approach.

---

## Section 9 — Practice Problems (20 minutes)

### Practice 1
Check whether a number exists in array.

### Practice 2
Find index of target in sorted array.

### Practice 3
Find if any pair sums to target.

### Practice 4
Sort array and search element.

Encourage students to first write:

1. Input
2. Output
3. Brute force
4. Better approach

---

## Wrap-Up (10 minutes)

Key takeaways:

1. Problem solving starts with understanding
2. Searching helps locate data
3. Sorting often simplifies hard problems
4. Recursion helps solve smaller subproblems
5. Efficient solutions often combine multiple techniques

Final line to students:

- “In interviews, many people know syntax.
The selected ones know how to think.”
