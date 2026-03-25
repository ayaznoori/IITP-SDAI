# Problem Solving – Applying Sorting, Searching, and Recursion

---

# 1 Purpose of This Session

This session focuses on **applying previously learned concepts** to solve DSA problems efficiently.

Students already know:

* Insertion Sort
* Binary Search
* Recursive Binary Search
* Why Binary Search needs sorted data
* Time complexity basics
* `sorted()` vs `list.sort()`

The goal now is to learn:

- **how to choose the right approach**

---

# 2 Problem Solving Checklist

Before writing code, ask:

1. Is the data sorted?
2. Can Binary Search be applied?
3. Do I need sorting first?
4. Should I use Insertion Sort or built-in sort?
5. Should I use iterative or recursive Binary Search?
6. What is the most efficient option?

---

# 3 Sorting Before Searching

Binary Search only works on sorted data.

### Example

```text
arr = [8, 3, 6, 1, 9, 4]
target = 6
```

Binary Search cannot be applied directly.

We must first sort the array:

```text
[1, 3, 4, 6, 8, 9]
```

Then Binary Search becomes possible.

---

# 4 Insertion Sort for Problem Solving

Insertion Sort is useful when:

* The problem explicitly asks for manual implementation
* We want to trace sorting step by step
* We are learning how sorting works internally

---

## Insertion Sort Code

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1

        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        arr[j + 1] = key

    return arr
```

### Time Complexity

* Best Case: **O(n)**
* Worst Case: **O(n²)**

### Space Complexity

* **O(1)**

---

# 5 Binary Search for Problem Solving

Binary Search is useful when:

* The array is already sorted
* We need faster searching
* Search space can be reduced repeatedly

---

## Iterative Binary Search

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

* **O(log n)**

### Space Complexity

* **O(1)**

---

# 6 Recursive Binary Search

Recursive Binary Search solves the same problem using recursive calls.

```python
def recursive_binary_search(arr, target, left, right):
    if left > right:
        return -1

    mid = (left + right) // 2

    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return recursive_binary_search(arr, target, mid + 1, right)
    else:
        return recursive_binary_search(arr, target, left, mid - 1)
```

### Time Complexity

* **O(log n)**

### Space Complexity

* **O(log n)** because of recursion stack

---

# 7 Iterative vs Recursive Binary Search

| Feature | Iterative | Recursive |
|---|---|---|
| Logic | Loop-based | Function call-based |
| Time Complexity | O(log n) | O(log n) |
| Space Complexity | O(1) | O(log n) |
| Easy to trace | Yes | Yes |
| Uses stack space | No | Yes |

---

# 8 Using Built-in Sorting

Python provides built-in sorting methods.

---

## `sorted()`

Returns a new sorted list.

```python
arr = [8, 3, 6, 1]
new_arr = sorted(arr)
print(new_arr)
print(arr)
```

---

## `list.sort()`

Sorts the original list directly.

```python
arr = [8, 3, 6, 1]
arr.sort()
print(arr)
```

---

# 9 Choosing the Right Approach

## Case 1 — One search only in unsorted array

Use direct scanning if needed.

## Case 2 — Multiple searches in same array

Sort once, then use Binary Search repeatedly.

This is an important problem-solving decision.

---

# 10 Combined Problem Solving Example

### Problem:
Given an unsorted array and a target:

1. Sort the array
2. Search for the target efficiently

---

## Solution

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1

        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        arr[j + 1] = key

    return arr


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


arr = [8, 3, 6, 1, 9, 4]
target = 6

sorted_arr = insertion_sort(arr)
index = binary_search(sorted_arr, target)

print("Sorted Array:", sorted_arr)
print("Target Index:", index)
```

---

# 11 Key Takeaways

* Problem solving is about choosing the correct technique
* Binary Search cannot be used on unsorted arrays
* Sorting often enables faster searching
* Insertion Sort helps understand sorting logic
* Built-in sorting is useful in practical coding
* Iterative and recursive Binary Search solve the same problem differently
