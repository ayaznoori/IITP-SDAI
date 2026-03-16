## Session Number

3.1

## Title

DSA: Strings, Dictionaries & HashMaps

## Objective

Store and manage data using dictionaries, handle missing keys safely, and manipulate and format strings for data processing.

## Topics & Subtopics Covered

- **Dictionaries**
  - Defining and using Python dictionaries
  - Accessing and modifying key-value pairs
  - Using dictionary methods (`get()`, `keys()`, `values()`, `items()`, `update()`)
  - Handling missing keys using `get()` and `setdefault()`
  - Introduction to `defaultdict` from `collections`
- **Strings**
  - String indexing and slicing
  - String concatenation
  - String formatting using f-strings and `.format()`

---

# 🌱 Pre-Read — Strings, Dictionaries & HashMaps

This pre-read will help you build intuition for:

- How strings behave as sequences of characters  
- Why dictionaries (hash maps) are powerful for storing data using **keys** instead of positions  
- How safe key access differs from direct indexing  

You do **not** need to memorize code; just understand the ideas and vocabulary.

---

## 1️⃣ Strings as Sequences

You already know strings like:

```python
name = "Python"
```

Important ideas:

- A string is a **sequence** of characters.  
- You can think of each character having a **position (index)**.  

Conceptually:

```text
P   y   t   h   o   n
0   1   2   3   4   5
```

In the session, you will:

- Pick out parts of a string (slicing)  
- Join strings together (concatenation)  
- Format strings nicely for output  

---

## 2️⃣ Why We Need Dictionaries / HashMaps

Lists store data by **position** (index):

```python
marks = [80, 90, 75]
```

But many real problems are naturally **key-based**, not position-based:

- Student roll number → name  
- Country code → country name  
- Product ID → price  

For this, Python gives us **dictionaries** (also called hash maps).

Concept idea:

```python
student = {
    "name": "Ali",
    "age": 21
}
```

Here:

- `"name"` and `"age"` are **keys**  
- `"Ali"` and `21` are the corresponding **values**  

We will learn how to:

- Look up values by key  
- Add and update key-value pairs  
- Handle missing keys safely  

---

## 3️⃣ Safe vs Unsafe Access

With lists, accessing an invalid index causes an **error**.  
With dictionaries, accessing a missing key directly also errors:

```python
person = {"name": "Riya"}
# person["age"]  # KeyError if "age" not present
```

Python provides **safer** methods like:

- `get(key, default)` → returns default if key is missing  
- `setdefault(key, default)` → sets a default if key is missing  

You will see how this helps avoid crashes when data is incomplete.

---

## 4️⃣ HashMap Intuition (High Level)

You do **not** need to know the full internals yet, but:

- A dictionary/hash map uses a **hash function** internally to quickly find values by key.  
- This is why lookups like `student["name"]` are usually very fast.  

For now, remember:

> “Dictionaries give you **fast lookup by key**.”

---

## 5️⃣ String Formatting — Cleaning Output

When printing data from dictionaries or other variables, you’ll often want clean messages like:

```text
Student Ali is 21 years old.
```

Python lets us do this with:

- **f-strings**: `f"Student {name} is {age} years old."`  
- `.format()`: `"Student {} is {} years old.".format(name, age)`  

You will practice:

- Building readable, properly formatted strings from data  
- Combining strings and variables without errors  

---

## 6️⃣ What to Be Familiar With Before Class

Just recognize these terms and ideas:

- String indexing and slicing  
- String concatenation  
- Dictionary / hash map  
- Key-value pair  
- `get()`, `keys()`, `values()`  
- “Missing key” problem  

We will turn these into working Python programs step-by-step in the lecture.

