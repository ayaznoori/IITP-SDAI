## Session Number

2.2

## Title

DSA: Python Lists and Functions for Modular Programming

## Objective

Understand how to work with Python lists and use functions to improve code organization and reusability.

## Topics & Subtopics Covered

- **Lists**
  - Defining and manipulating Python lists
  - Using essential list methods
  - Performing traversals and indexing (positive indexing, negative indexing, slicing)
- **Functions**
  - Defining functions using `def`
  - Understanding the need for functions
  - Using parameters and return values to write modular and reusable programs

---

# 🎤 Lecture Script — Lists & Functions for Modular Programming

⏱️ **Total Duration: ~2 hours 30 minutes**

---

## 🎯 Hook (5 minutes)

Ask:

> “If a student has 5 subject marks, will you create 5 separate variables, or 1 collection?”

Show naive approach:

```python
mark1 = 80
mark2 = 75
mark3 = 90
```

Then show list:

```python
marks = [80, 75, 90]
```

Explain:

- Lists help us store and process multiple values.  
- Functions help us **reuse logic** on those values.  

Key line:

> “Today we connect Data Structures (Lists) with modular code (Functions).”

---

## Section 1 — Creating and Accessing Lists (20 minutes)

Create examples live:

```python
marks = [80, 90, 70]
names = ["Ali", "Riya", "Sam"]
mixed = [10, "Python", True]
```

Ask students:

- “Can a list store different types?” (Yes)

### Positive Indexing

```python
fruits = ["apple", "banana", "mango"]

print(fruits[0])
print(fruits[1])
print(fruits[2])
```

Explain:

- Index starts at 0  
- Accessing invalid index → error  

### Negative Indexing

```python
print(fruits[-1])  # last item
print(fruits[-2])
```

Relate to:

- “Last bench, last-but-one bench” analogy.

---

## Section 2 — Slicing (20 minutes)

Explain:

> “Slicing allows us to take sub-lists from a list.”

Syntax:

```python
list[start:end]   # end not included
```

Demo:

```python
numbers = [10, 20, 30, 40, 50]

print(numbers[1:4])   # [20, 30, 40]
print(numbers[:3])    # [10, 20, 30]
print(numbers[2:])    # [30, 40, 50]
print(numbers[-3:])   # [30, 40, 50]
```

Ask students to predict each output before running.

Teaching point:

- Slicing does **not** change original list (unless reassigned).

---

## Section 3 — Essential List Methods (25 minutes)

Introduce common operations:

```python
append
insert
remove
pop
len
```

### Live Demo

```python
fruits = ["apple", "banana"]

fruits.append("mango")
print(fruits)

fruits.insert(1, "orange")
print(fruits)

fruits.remove("banana")
print(fruits)

last = fruits.pop()
print("Popped:", last)
print(fruits)

print("Length:", len(fruits))
```

Ask:

- “What happens if we call `remove` with a value that’s not in the list?”  
- Show the error and explain.

---

## Section 4 — Traversing Lists With Loops (20 minutes)

Reconnect with previous session (loops).

### `for` Loop Over List

```python
marks = [80, 90, 70]

for m in marks:
    print("Mark:", m)
```

Explain:

- Easy when we just need values.

### Using Index

```python
for i in range(len(marks)):
    print("Index:", i, "Value:", marks[i])
```

Explain when this is useful:

- When needing positions  
- When updating items by index  

---

## Section 5 — Introduction to Functions (20 minutes)

Explain:

> “A function is a reusable block of code with a name.”

Basic syntax:

```python
def greet():
    print("Hello from function")

greet()
```

Emphasize:

- `def` keyword  
- Function name  
- Parentheses `()`  
- Indentation  

---

### Functions With Parameters

```python
def greet(name):
    print("Hello", name)

greet("Ali")
greet("Riya")
```

Explain:

- `name` is a **parameter** inside the function.  
- `"Ali"` and `"Riya"` are **arguments** passed when calling.

---

### Functions With Return Values

```python
def add(a, b):
    result = a + b
    return result

sum_value = add(3, 5)
print(sum_value)
```

Teaching point:

- After `return`, function ends.  
- If no `return`, function returns `None`.

---

## Section 6 — Using Functions for Modular Programming (25 minutes)

Split a task into multiple functions.

### Example — Student Marks Report

Goal:

> Read 3 marks, calculate average, print report.

Design:

```python
def read_marks():
    m1 = int(input("Enter mark 1: "))
    m2 = int(input("Enter mark 2: "))
    m3 = int(input("Enter mark 3: "))
    return [m1, m2, m3]

def average(marks):
    total = 0
    for m in marks:
        total = total + m
    return total / len(marks)

def print_report(marks, avg):
    print("Marks:", marks)
    print("Average:", avg)

marks = read_marks()
avg = average(marks)
print_report(marks, avg)
```

Explain:

- Each function does **one job**.  
- The main flow uses these building blocks.  

---

## Section 7 — Lists + Functions Exercises (20 minutes)

Give students 2–3 tasks:

1. Write a function `sum_list(numbers)` that returns sum.  
2. Write a function `find_max(numbers)` that returns maximum.  
3. Write a function `count_above_threshold(numbers, threshold)` that counts numbers greater than `threshold`.  

Walk through one full solution on screen and relate it to:

- Traversal  
- Parameters  
- Return values  

---

## Section 8 — Common Mistakes (10 minutes)

Discuss:

- Accessing invalid index → IndexError  
- Forgetting to call function with `()` (just writing `greet` instead of `greet()`)  
- Writing `return` but not capturing the result in a variable  
- Modifying lists unexpectedly in functions (explain by example if time permits)  

Show at least one broken code snippet and fix it with students.

---

## Section 9 — Mini Project (15–20 minutes)

**Mini Project Idea: Simple Analytics on Marks List**

Requirements:

1. Read `n` marks from user into a list.  
2. Use functions to:
   - Compute average  
   - Find highest mark  
   - Count how many marks are ≥ passing marks (e.g., 40)  
3. Print a small report.

Skeleton to guide students:

```python
def read_marks(n):
    # TODO

def average(marks):
    # TODO

def highest(marks):
    # TODO

def count_pass(marks, pass_mark):
    # TODO

n = int(input("How many marks? "))
marks = read_marks(n)

avg = average(marks)
high = highest(marks)
passed = count_pass(marks, 40)

print("Marks:", marks)
print("Average:", avg)
print("Highest:", high)
print("Passed:", passed)
```

Guide them to fill functions using loops and list operations learned.

---

## Section 10 — Wrap-Up (5 minutes)

Summarize:

1. Lists store ordered collections and support indexing, slicing, and methods.  
2. Loops help traverse lists for processing.  
3. Functions package logic into reusable blocks.  
4. Combining lists + functions leads to **modular**, maintainable programs.  

End with:

> “From now on, when you see repeated data, think **lists**;  
> when you see repeated logic, think **functions**.”


