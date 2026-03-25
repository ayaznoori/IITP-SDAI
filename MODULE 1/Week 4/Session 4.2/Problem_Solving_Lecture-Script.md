#  LECTURE SCRIPT

# Problem Solving – Applying Sorting, Searching, and Recursion for Efficient Solutions

**Total Duration: 2 Hours 30 Minutes**

---

##  Hook (10 minutes)

Ask students:

> “If you already know Insertion Sort and Binary Search, does that mean you can solve DSA problems efficiently?”

Pause and let them think.

Explain:

Knowing an algorithm is only the **first step**.  
The real skill is:

- **choosing when to use it**

Today’s class is a **problem-solving session**.

You already know the concepts.  
Today, you will learn how to **apply them together**.

By the end of this session, students will be able to:

✅ Identify whether sorting is needed before searching  
✅ Decide whether Binary Search is applicable  
✅ Use Insertion Sort or built-in sort appropriately  
✅ Apply iterative and recursive Binary Search in problems  
✅ Compare brute-force and efficient approaches

---

## Section 1 — Problem Solving Mindset (15 minutes)

Write this on board:

# Before coding, ask:

1. What is the input?
2. What is the output?
3. Is the data sorted?
4. Can Binary Search be used?
5. Do I need sorting first?
6. What is the most efficient option?

Explain:

Students often fail not because they don’t know code —
they fail because they choose the wrong approach.

---

## Section 2 — Problem 1: Can Binary Search Be Used? (20 minutes)

### Given:

```text
arr = [8, 3, 6, 1, 9, 4]
target = 6
```

Ask students:

> “Can we apply Binary Search directly?”

Expected answer:
❌ No, because the array is not sorted.

This is the first problem-solving checkpoint:

### Rule:
Binary Search is possible **only after sorting**.

---

### Discussion

Now ask:

> “Should we sort first, or just scan the array?”

Discuss:

### Option 1 — Direct Search
Check each element one by one.

### Option 2 — Sort + Binary Search
Sort first, then search efficiently.

This is the key learning:
The best solution depends on the situation.

---

## Section 3 — Problem 2: Sort First, Then Search (25 minutes)

### Problem Statement

Given an unsorted array and a target, sort the array using **Insertion Sort** and then search for the target using **Iterative Binary Search**.

---

### Step 1 — Insertion Sort

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

---

### Step 2 — Iterative Binary Search

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

---

### Full Problem Solution

```python
arr = [8, 3, 6, 1, 9, 4]
target = 6

sorted_arr = insertion_sort(arr)
index = binary_search(sorted_arr, target)

print("Sorted Array:", sorted_arr)
print("Target Index:", index)
```

---

### Teaching points

* Students are combining two known ideas
* This is a real DSA workflow
* First organize data, then search efficiently

---

## Section 4 — Problem 3: Visualizing Binary Search (20 minutes)

### Problem

```text
arr = [1, 3, 4, 6, 8, 9]
target = 6
```

Trace on board:

| left | right | mid | arr[mid] |
|---|---|---|---|
| 0 | 5 | 2 | 4 |
| 3 | 5 | 4 | 8 |
| 3 | 3 | 3 | 6 |

Explain carefully:

* If target is bigger → move left
* If target is smaller → move right
* Search space keeps shrinking

This builds the student’s **problem-solving confidence**.

---

## Section 5 — Problem 4: Recursive Binary Search (20 minutes)

Explain:

Students already know iterative Binary Search.  
Now we ask:

> “Can we solve the same problem recursively?”

### Code

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

Call it:

```python
arr = [1, 3, 4, 6, 8, 9]
print(recursive_binary_search(arr, 6, 0, len(arr)-1))
```

---

### Teaching points

Compare with iterative version:

| Type | Time | Space |
|---|---|---|
| Iterative Binary Search | O(log n) | O(1) |
| Recursive Binary Search | O(log n) | O(log n) |

Explain:
Recursive version uses extra stack space.

---

## Section 6 — Problem 5: Choosing Between Insertion Sort and Built-in Sort (20 minutes)

Ask students:

> “Should we always use Insertion Sort in real projects?”

Expected answer:
❌ No

Explain:

### Insertion Sort is useful for:
* learning logic
* manual tracing
* understanding sorting process

### Built-in sort is useful for:
* real applications
* faster implementation
* practical coding

---

### Example

```python
arr = [8, 3, 6, 1, 9, 4]

print(sorted(arr))   # returns new sorted list

arr.sort()
print(arr)           # sorts original list
```

---

### Discussion

When solving DSA problems:

* If asked to implement logic → write Insertion Sort
* If asked to solve efficiently in practical code → built-in sort may be better

This is an important problem-solving decision.

---

## Section 7 — Problem 6: When is Sorting Worth It? (15 minutes)

Ask:

> “If I need to search only once in an unsorted array, should I sort first?”

Discuss:

### Case 1 — One search only
Direct scan may be enough.

### Case 2 — Many searches
Sorting once may be worth it.

This is one of the best real-world DSA discussions in this session.

Students learn:

- efficiency depends on usage

---

## Section 8 — Guided Practice Problems (20 minutes)

### Practice 1
Given an unsorted array and target:
* Decide whether Binary Search can be used
* If not, explain what must be done first

### Practice 2
Sort array using Insertion Sort and search target

### Practice 3
Solve same problem using built-in sort and Binary Search

### Practice 4
Implement both iterative and recursive Binary Search and compare them

Encourage students to answer before coding:

1. Which technique is needed?
2. Why?
3. What is the time complexity?

---

## Wrap-Up (5 minutes)

Key takeaways:

1. Problem solving is about choosing the right technique
2. Binary Search requires sorted data
3. Sorting can make searching efficient
4. Recursion can solve the same problem differently
5. Good programmers don’t just know algorithms — they know when to use them

Final line:

- “The best coder is not the one who writes first —
it is the one who thinks first.”
