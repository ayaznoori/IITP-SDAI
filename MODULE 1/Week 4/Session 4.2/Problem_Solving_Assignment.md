# Assignment — Problem Solving using Sorting, Searching, and Recursion

---

## **Q1. Why can Binary Search not be applied directly to an unsorted array?**

A) Because it only works for strings  
B) Because it requires comparing with the middle element in ordered data  
C) Because recursion is required  
D) Because Python does not support it  

✅ **Correct Answer:** B

**Explanation:**  
Binary Search depends on the array being sorted so that half of the search space can be eliminated correctly after each comparison.

---

## **Q2. Which sorting algorithm have you implemented manually in this module?**

A) Merge Sort  
B) Quick Sort  
C) Insertion Sort  
D) Selection Sort  

✅ **Correct Answer:** C

**Explanation:**  
Students have learned Insertion Sort and used it to understand how sorting works step by step.

---

## **Q3. What is the worst-case time complexity of Insertion Sort?**

A) O(1)  
B) O(log n)  
C) O(n)  
D) O(n²)  

✅ **Correct Answer:** D

**Explanation:**  
In the worst case, Insertion Sort may need to shift many elements for each position, resulting in O(n²) time complexity.

---

## **Q4. What is the worst-case time complexity of Binary Search?**

A) O(n²)  
B) O(n)  
C) O(log n)  
D) O(1)  

✅ **Correct Answer:** C

**Explanation:**  
Binary Search halves the search space in every step, making it much faster than linear scanning for sorted arrays.

---

## **Q5. Which of the following is TRUE about iterative and recursive Binary Search?**

A) Both have O(log n) time complexity  
B) Recursive Binary Search has O(1) space complexity  
C) Iterative Binary Search always runs faster because of syntax  
D) Recursive Binary Search does not need a base case  

✅ **Correct Answer:** A

**Explanation:**  
Both iterative and recursive Binary Search take O(log n) time, but recursive Binary Search uses extra stack space.

---

## **Q6. Which Python method sorts the original list directly?**

A) `sorted()`  
B) `sort()`  
C) `append()`  
D) `reverse()`  

✅ **Correct Answer:** B

**Explanation:**  
`list.sort()` modifies the original list, while `sorted()` returns a new sorted list.

---

## **Q7. If you need to search many times in the same unsorted array, what is usually a better strategy?**

A) Always scan from left to right  
B) Sort once and use Binary Search repeatedly  
C) Use recursion without sorting  
D) Print the array first  

✅ **Correct Answer:** B

**Explanation:**  
If multiple searches are required, sorting once and then applying Binary Search can be more efficient overall.

---

## **Q8. What is the main purpose of this session?**

A) Learn brand-new sorting algorithms  
B) Memorize only code syntax  
C) Apply learned searching and sorting concepts to solve problems  
D) Avoid time complexity completely  

✅ **Correct Answer:** C

**Explanation:**  
This session focuses on problem solving using concepts students have already learned, rather than introducing many new algorithms.

---

#  SUBJECTIVE QUESTION

## **Q9. Sort and Search Efficiently**

### 🔹 Problem Statement

You are given an unsorted array of integers and a target value.

Write a Python program to do the following:

1. Sort the array using **Insertion Sort**
2. Print the sorted array
3. Search for the target using **Iterative Binary Search**
4. Search for the same target using **Recursive Binary Search**
5. Print whether the target is found and its index

---

### 🔹 Requirements

You must:

✅ Use functions  
✅ Use Insertion Sort  
✅ Use Iterative Binary Search  
✅ Use Recursive Binary Search  
✅ Print meaningful output  
✅ Explain time complexity

---

## ✅ Expected Answer (Reference Solution)

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


arr = [8, 3, 6, 1, 9, 4]
target = 6

sorted_arr = insertion_sort(arr)

print("Sorted Array:", sorted_arr)
print("Iterative Binary Search Result:", binary_search(sorted_arr, target))
print("Recursive Binary Search Result:", recursive_binary_search(sorted_arr, target, 0, len(sorted_arr)-1))
```

---

## ✅ Sample Output

```text
Sorted Array: [1, 3, 4, 6, 8, 9]
Iterative Binary Search Result: 3
Recursive Binary Search Result: 3
```
