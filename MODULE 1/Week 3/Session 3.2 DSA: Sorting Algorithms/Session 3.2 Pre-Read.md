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

# 🌱 Pre-Read — Insertion Sort & Sorting Basics

This pre-read prepares your **intuition** for insertion sort and complexity, before we code.

---

## 1️⃣ Why Sorting Matters

In real life:

- Sorting marks from lowest to highest  
- Ordering names alphabetically  
- Sorting products by price  

In programming, sorting helps:

- Search faster  
- Present clean reports  
- Prepare data for further algorithms  

We will focus on one simple algorithm: **insertion sort**.

---

## 2️⃣ Intuition for Insertion Sort

Think about how you sort playing cards in your hand:

1. Start with the first card (already “sorted”).  
2. Pick the next card.  
3. Insert it into the correct position among the sorted cards in your hand.  
4. Repeat for all cards.  

This is **insertion sort**.

Key idea:

> “We grow a sorted portion of the list, inserting each new element in the correct place.”

---

## 3️⃣ High-Level Steps (No Code Yet)

For a list of numbers:

```text
[5, 2, 4, 6, 1]
```

Insertion sort conceptually:

1. First element alone is “sorted”: `[5] | 2 4 6 1`  
2. Insert `2` in correct position: `[2, 5] | 4 6 1`  
3. Insert `4`: `[2, 4, 5] | 6 1`  
4. Insert `6`: `[2, 4, 5, 6] | 1`  
5. Insert `1`: `[1, 2, 4, 5, 6]`  

We will translate this into code and step-by-step diagrams in class.

---

## 4️⃣ Time Complexity — Basic Idea

Complexity tells us **how an algorithm’s running time grows** as the input size grows.

- If we sort 10 numbers vs 10,000 numbers, will it still be fast?  

You will hear terms like:

- “Big O” notation (`O(n)`, `O(n²)`, etc.)  
- “Time complexity” and “space complexity”  

For now, remember:

- Insertion sort is fast for **small** or **almost sorted** lists  
- But can be slow for **very large** unsorted lists  

---

## 5️⃣ Python’s Built-in Sorting

Python already gives:

- `sorted(iterable)` → returns new sorted list  
- `list.sort()` → sorts list in place  

Example:

```python
nums = [5, 2, 4, 6, 1]

print(sorted(nums))  # new list
nums.sort()          # modifies nums
print(nums)
```

In this session, you will:

- Implement insertion sort manually  
- Compare it with `sorted()` / `.sort()`  
- Understand **why** built-ins are preferred in production.

---

## 6️⃣ What to Keep in Mind Before Class

Be familiar with these words:

- Sorting  
- Insertion sort (card analogy)  
- Time complexity  
- Space complexity  
- `sorted()` and `list.sort()`  

We will build full code, dry runs, and even simple flowcharts during the lecture.

