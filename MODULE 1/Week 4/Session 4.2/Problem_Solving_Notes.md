# Problem Solving – Searching, Sorting, and Recursion for Efficient Solutions

---

# 1️ Problem Solving in DSA

Problem solving in DSA means using the right algorithmic technique to solve a problem correctly and efficiently.

A good solution should focus on:

* Correctness
* Simplicity
* Efficiency

General process:

1. Understand the problem
2. Identify input and output
3. Think of brute-force solution
4. Improve using better techniques

---

# 2️ Searching Techniques

Searching means finding an element or condition inside data.

Examples:

* Find target number in array
* Check if student ID exists
* Search for a word in a list

---

## Linear Search

Linear Search checks elements one by one.

### Example

```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1
```

### Time Complexity

* Best Case: **O(1)**
* Worst Case: **O(n)**

### Use When

* Array is unsorted
* Simplicity is enough

---

## Binary Search

Binary Search works only on **sorted arrays**.

It repeatedly checks the middle element and reduces the search space by half.

### Example

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

### Time Complexity

* Best Case: **O(1)**
* Worst Case: **O(log n)**

### Use When

* Data is sorted
* Fast searching is needed

---

# 3️ Sorting as a Problem Solving Tool

Sorting arranges data in order.

Examples:

* Ascending marks
* Alphabetical names
* Sorted IDs

Sorting is useful because many problems become easier after sorting.

---

## Example – Sort + Solve

### Problem

Check if any pair sums to target.

```text
arr = [8, 4, 1, 6, 2]
target = 10
```

### Brute Force

Check all pairs.

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

* **O(n²)**

---

## Optimized Using Sorting

Sort array first, then use two pointers.

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

---

# 4️ Recursion in Problem Solving

Recursion is a technique where a function calls itself to solve a smaller version of the same problem.

A recursive solution needs:

1. **Base case** → stops recursion
2. **Recursive case** → reduces problem size

---

## Recursive Binary Search

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

### Why it works

At each step:

* We compare target with middle element
* We discard half of the array
* We recursively solve the remaining half

---

# 5️ Combining Sorting and Searching

Real DSA problems often use multiple techniques together.

Example:

* Sort first
* Then search efficiently

---

## Example – First Occurrence After Sorting

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

This combines:

* Sorting
* Binary Search
* Search optimization

---

# 6️ Recursion + Sorting (Merge Sort Idea)

Merge Sort is a divide-and-conquer algorithm.

It works in 3 steps:

1. Divide array into halves
2. Recursively sort each half
3. Merge sorted halves

---

## Merge Sort Example

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

### Time Complexity

* **O(n log n)**

---

# 7️ Problem Solving Strategy

When solving any DSA problem, ask:

1. Can I solve it directly?
2. Is there a brute-force method?
3. Can sorting simplify it?
4. Can searching make it faster?
5. Can recursion reduce repeated work?

---

# 8️ Time Complexity Comparison

| Technique | Time Complexity |
|---|---|
| Linear Search | O(n) |
| Binary Search | O(log n) |
| Pair Sum Brute Force | O(n²) |
| Pair Sum using Sort + Two Pointers | O(n log n) |
| Merge Sort | O(n log n) |

---

# 9️ Key Takeaways

* Searching helps find data efficiently
* Sorting often makes problems easier
* Recursion helps solve smaller subproblems
* Many interview problems combine multiple techniques
* Good problem solving is about choosing the right method
