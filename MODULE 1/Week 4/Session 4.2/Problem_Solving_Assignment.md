#  Assignment — Problem Solving using Searching, Sorting, and Recursion

---

## **Q1. Which searching algorithm works correctly only on a sorted array?**

A) Linear Search  
B) Binary Search  
C) Bubble Search  
D) Sequential Check  

✅ **Correct Answer:** B

**Explanation:**  
Binary Search requires the array to be sorted because it repeatedly checks the middle element and discards half of the remaining search space.

---

## **Q2. What is the worst-case time complexity of Linear Search?**

A) O(1)  
B) O(log n)  
C) O(n)  
D) O(n log n)  

✅ **Correct Answer:** C

**Explanation:**  
In the worst case, Linear Search checks every element before finding the target or confirming it is absent.

---

## **Q3. Why is sorting often used before solving a problem?**

A) Because sorting always reduces memory usage  
B) Because sorting can make searching and comparisons easier  
C) Because sorting changes the size of the array  
D) Because recursion requires sorted arrays  

✅ **Correct Answer:** B

**Explanation:**  
Many problems become easier after sorting because data becomes structured, allowing efficient techniques like Binary Search or Two Pointers.

---

## **Q4. What is the time complexity of Binary Search in the worst case?**

A) O(1)  
B) O(n)  
C) O(log n)  
D) O(n²)  

✅ **Correct Answer:** C

**Explanation:**  
Binary Search cuts the search space in half on each step, making it much faster than Linear Search for sorted arrays.

---

## **Q5. Which concept is most important to stop recursion correctly?**

A) Loop variable  
B) Base case  
C) Sorting order  
D) Input size  

✅ **Correct Answer:** B

**Explanation:**  
A recursive function must include a base case, otherwise it will continue calling itself indefinitely and may cause a stack overflow.

---

## **Q6. Which of the following is the best approach to check whether any pair in a sorted array adds up to a target?**

A) Use nested loops only  
B) Use stack and queue  
C) Use two pointers  
D) Use recursion only  

✅ **Correct Answer:** C

**Explanation:**  
For a sorted array, the two-pointer technique is efficient because it allows checking sums from both ends in linear time.

---

## **Q7. Merge Sort mainly uses which problem-solving strategy?**

A) Greedy  
B) Divide and Conquer  
C) Dynamic Programming  
D) Backtracking  

✅ **Correct Answer:** B

**Explanation:**  
Merge Sort divides the array into smaller parts, recursively sorts them, and then merges them back in sorted order.

---

## **Q8. Which statement is TRUE about problem solving in DSA?**

A) Brute-force solutions should always be avoided  
B) Efficient solutions are not important if output is correct  
C) A brute-force solution is often the first step before optimization  
D) Sorting is useful only for display purposes  

✅ **Correct Answer:** C

**Explanation:**  
A brute-force solution helps in understanding the problem clearly before improving it with optimized approaches.

---

#  SUBJECTIVE QUESTION

## **Q9. Efficient Search and Pair Sum Problem**

### 🔹 Problem Statement

You are given an integer array and a target value.

Write Python functions to solve the following:

### Part A
Check whether a target number exists in the array using **Linear Search**.

### Part B
Sort the array and check whether any **pair of numbers** sums to the target using an efficient approach.

### Part C
Write a **recursive Binary Search** function to search for a target in a sorted array.

---

### 🔹 Requirements

You must:

✅ Use functions  
✅ Use searching techniques  
✅ Use sorting  
✅ Use recursion  
✅ Compare brute-force and optimized approaches  

---

## ✅ Expected Answer (Reference Solution)

```python
# Part A — Linear Search
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1


# Part B — Pair Sum using Sorting + Two Pointers
def pair_sum_sorted(arr, target):
    arr.sort()
    left = 0
    right = len(arr) - 1

    while left < right:
        current_sum = arr[left] + arr[right]

        if current_sum == target:
            return True
        elif current_sum < target:
            left += 1
        else:
            right -= 1

    return False


# Part C — Recursive Binary Search
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


# Driver code
arr = [8, 4, 1, 6, 2, 7]
target = 10

print("Linear Search for 6:", linear_search(arr, 6))
print("Pair Sum Exists:", pair_sum_sorted(arr[:], target))

sorted_arr = sorted(arr)
print("Sorted Array:", sorted_arr)
print("Binary Search for 7:", binary_search_recursive(sorted_arr, 7, 0, len(sorted_arr) - 1))
```

---

## ✅ Sample Output

```text
Linear Search for 6: 3
Pair Sum Exists: True
Sorted Array: [1, 2, 4, 6, 7, 8]
Binary Search for 7: 4
```
