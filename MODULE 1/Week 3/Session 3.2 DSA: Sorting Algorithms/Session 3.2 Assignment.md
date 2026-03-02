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

# 📝 MCQs — Insertion Sort & Sorting (with Explanations)

---

## Q1. What is the main idea behind insertion sort?

A) Repeatedly finding the minimum element and placing it at the beginning.  
B) Dividing the list into two halves and merging them.  
C) Building a sorted portion by inserting each new element into its correct place.  
D) Randomly shuffling until the list is sorted.  

✅ **Correct Answer:** C  

**Explanation:**  
Insertion sort maintains a sorted part on the left and inserts each new element into the correct position.

---

## Q2. In the list `[5, 2, 4, 6, 1]`, after the first pass of insertion sort (processing element `2`), what will the list look like?

A) `[2, 5, 4, 6, 1]`  
B) `[5, 2, 4, 6, 1]`  
C) `[2, 4, 5, 6, 1]`  
D) `[1, 2, 4, 6, 5]`  

✅ **Correct Answer:** A  

**Explanation:**  
2 is inserted before 5, resulting in `[2, 5, 4, 6, 1]`.

---

## Q3. What is the worst-case time complexity of insertion sort for an array of size `n`?

A) O(1)  
B) O(log n)  
C) O(n)  
D) O(n²)  

✅ **Correct Answer:** D  

**Explanation:**  
In the worst case (reverse sorted), insertion sort performs roughly \( n(n-1)/2 \) comparisons → O(n²).

---

## Q4. Which of the following is TRUE about insertion sort?

A) It always requires extra arrays, so space complexity is O(n).  
B) It sorts the list in-place, using O(1) extra space.  
C) It cannot work with negative numbers.  
D) It only works if the list is already sorted.  

✅ **Correct Answer:** B  

**Explanation:**  
Insertion sort modifies the list in-place using only a few extra variables.

---

## Q5. What will be the list after running one full insertion sort pass on `[3, 1, 4, 2]`?

A) `[1, 2, 3, 4]`  
B) `[2, 1, 3, 4]`  
C) `[1, 3, 4, 2]`  
D) `[1, 3, 2, 4]`  

✅ **Correct Answer:** A  

**Explanation:**  
After all passes, insertion sort produces a fully sorted list `[1, 2, 3, 4]`.

---

## Q6. Which built-in in Python sorts a list **in-place**?

A) `sorted()`  
B) `list.sort()`  
C) `sort(list)`  
D) `order()`  

✅ **Correct Answer:** B  

**Explanation:**  
`list.sort()` modifies the original list; `sorted()` returns a new sorted list.

---

## Q7. For a nearly sorted list, insertion sort’s performance is closest to:

A) O(1)  
B) O(log n)  
C) O(n)  
D) O(n²)  

✅ **Correct Answer:** C  

**Explanation:**  
When the list is nearly sorted, only a few shifts are needed → close to linear time.

---

## Q8. Which of the following best describes the time complexity of Python’s built-in sorting (`sorted()`, `list.sort()`) for large random data?

A) O(n)  
B) O(n log n)  
C) O(n²)  
D) O(log n)  

✅ **Correct Answer:** B  

**Explanation:**  
Python’s sort uses Timsort, which has O(n log n) time complexity in the general case.

---

# ✍️ Subjective / Coding Question (Medium–Hard)

## Q9. Implement and Compare Insertion Sort With Built-In Sort

### 🔹 Problem Statement

Write a Python program that:

1. Defines a function `insertion_sort(arr)` that sorts a list **in-place** using insertion sort.  
2. Generates a list of integers from user input (you can ask the user for `n` and then for `n` numbers).  
3. Makes a **copy** of that list and sorts:
   - One copy using your `insertion_sort()`  
   - The other copy using Python’s built-in `sorted()`  
4. Prints both sorted lists and verifies if they are the same.  

Optional (if students are comfortable):

- Use the `time` module to roughly compare how long each sort takes for larger lists.  

---

## ✅ Reference Solution (For Instructor / Review)

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1

        # Move elements greater than key to one position ahead
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        arr[j + 1] = key


n = int(input("Enter number of elements: "))
data = []

for i in range(n):
    value = int(input(f"Enter element {i + 1}: "))
    data.append(value)

copy1 = data.copy()
copy2 = data.copy()

insertion_sort(copy1)
builtin_sorted = sorted(copy2)

print("Original data:", data)
print("After insertion sort:", copy1)
print("After built-in sorted():", builtin_sorted)

print("Are both results same?", copy1 == builtin_sorted)
```

You can also extend this with simple timing using `time.time()` for demonstration.

---

