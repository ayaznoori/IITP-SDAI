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

# 🌱 Pre-Read — Lists & Functions

This pre-read prepares you for:

- Working with **lists** (a core data structure in Python)  
- Understanding why we use **functions** to organize code  

You will only see **concepts** here — we will write and run full programs in class.

---

## 1️⃣ What Is a List in Real Life?

In real life, we often use lists:

- List of students in a class  
- List of items in a shopping cart  
- List of marks in different subjects  

In Python, a **list** represents exactly this idea — a collection of values.

Example idea (not focusing on syntax details yet):

```python
marks = [80, 90, 75]
```

You can:

- Add new marks  
- Remove marks  
- Go through each mark with a loop  

We will learn these actions properly in the session.

---

## 2️⃣ Why Lists Are Important in DSA

Lists are:

- The simplest way to store **multiple values** in a single variable  
- The base for many Data Structures & Algorithms concepts  

They allow us to:

- Store data in order  
- Access by position (index)  
- Process the entire list using loops  

You will see:

- Positive indexing: starting from 0  
- Negative indexing: from the end  
- Slicing: taking parts of the list  

---

## 3️⃣ What Is a Function? (Intuition)

Think of a function as:

> “A small machine that takes input, does some work, and gives output.”

Real life example:

- Mixer:
  - Inputs: fruits + milk  
  - Work: blending  
  - Output: milkshake  

In Python:

```python
def add(a, b):
    return a + b
```

Here:

- Inputs → `a`, `b` (parameters)  
- Work → `a + b`  
- Output → `return a + b`  

---

## 4️⃣ Why Do We Need Functions?

Without functions, code becomes:

- Long  
- Repetitive  
- Hard to test  

With functions:

- We can **reuse** logic  
- We can give each function **one clear job**  
- We can test each part independently  

Example idea:

Instead of writing average calculation code many times, we can write:

```python
def average(numbers):
    # logic
```

And call:

```python
average(marks_list)
average(prices_list)
```

---

## 5️⃣ Lists + Functions = Modular Programming

**Modular programming** means:

> Breaking a big program into smaller, reusable building blocks.

Example structure:

```text
read data  → process data → print result
```

We can write:

- `read_data()` function  
- `process_data()` function  
- `print_result()` function  

Lists often hold the **data**, and functions implement the **logic**.

---

## 6️⃣ What You Should Be Familiar With Before Class

Just recognize these terms:

- List  
- Index  
- Slicing  
- Method (like `append`, `remove`)  
- Function  
- Parameter  
- Return value  
- Modular program  

You don’t need to master them yet — the goal is to feel comfortable when you first see the full code in the lecture.

