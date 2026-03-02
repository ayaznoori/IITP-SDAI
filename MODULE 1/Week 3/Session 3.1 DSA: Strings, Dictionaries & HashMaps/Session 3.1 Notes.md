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

# 1️⃣ Strings — Indexing and Slicing

### Strings as Sequences

```python
text = "Python"
```

Indexes:

```text
P   y   t   h   o   n
0   1   2   3   4   5
```

### Indexing

```python
print(text[0])   # 'P'
print(text[3])   # 'h'
print(text[-1])  # 'n'
print(text[-2])  # 'o'
```

### Slicing

Syntax: `text[start:end]` (end excluded)

```python
print(text[1:4])   # 'yth'
print(text[:3])    # 'Pyt'
print(text[3:])    # 'hon'
print(text[-3:])   # 'hon'
```

---

# 2️⃣ String Concatenation and Formatting

### Concatenation

```python
first = "Hello"
second = "World"

message = first + " " + second
print(message)  # 'Hello World'
```

### f-Strings (Recommended)

```python
name = "Ali"
age = 21

msg = f"Student {name} is {age} years old."
print(msg)
```

### `.format()` Method

```python
msg = "Student {} is {} years old.".format(name, age)
print(msg)
```

These help build clean strings from variables and data structures (like dictionaries).

---

# 3️⃣ Dictionaries — Basics (Key–Value Pairs)

### Defining Dictionaries

```python
student = {
    "name": "Riya",
    "age": 20,
    "course": "Python"
}
```

### Accessing Values

```python
print(student["name"])  # 'Riya'
print(student["age"])   # 20
```

If the key does not exist and you use `student["xyz"]`, Python raises a **KeyError**.

---

### Modifying and Adding Key–Value Pairs

```python
student["age"] = 21            # modify existing
student["city"] = "Bangalore"  # add new key

del student["course"]          # delete key
```

---

# 4️⃣ Useful Dictionary Methods

```python
student = {
    "name": "Riya",
    "age": 21,
    "city": "Bangalore"
}
```

### `keys()`, `values()`, `items()`

```python
print(student.keys())    # dict_keys([...])
print(student.values())  # dict_values([...])
print(student.items())   # dict_items([...])
```

These are iterable, so you can loop over them:

```python
for key in student.keys():
    print(key)

for value in student.values():
    print(value)

for key, value in student.items():
    print(key, "=", value)
```

### `update()`

```python
student.update({"age": 22, "country": "India"})
```

This merges changes into the dictionary (adds new keys or overwrites existing ones).

---

# 5️⃣ Handling Missing Keys Safely

Direct access:

```python
age = student["age"]  # KeyError if 'age' not present
```

### `get()` with Default

```python
age = student.get("age", 0)
city = student.get("city", "Unknown")
```

- If key exists → returns its value  
- If key missing → returns default (second argument)  

### `setdefault()`

```python
language = student.setdefault("language", "Python")
```

- If `language` exists → returns its value, dictionary unchanged  
- If missing → sets `"language": "Python"` and returns `"Python"`  

Useful when you want to ensure a key always exists.

---

# 6️⃣ Introduction to `defaultdict`

From `collections` module:

```python
from collections import defaultdict

counts = defaultdict(int)

counts["a"] += 1  # auto-creates with default 0
counts["b"] += 1
counts["a"] += 1

print(counts["a"])  # 2
print(counts["c"])  # 0 (default int() is 0)
```

Why useful:

- Avoids manual checks for missing keys when counting or aggregating.  
- `defaultdict(list)` is common for grouping values.

Example:

```python
group = defaultdict(list)
group["A"].append("Ali")
group["A"].append("Asha")
group["B"].append("Balu")
```

---

# 7️⃣ Strings + Dictionaries for Data Processing

Combining both:

```python
student = {"name": "Ali", "age": 21, "city": "Delhi"}

msg = f"{student.get('name', 'Unknown')} from {student.get('city', 'Unknown')} is {student.get('age', 0)} years old."
print(msg)
```

Key ideas:

- Use `get()` to handle missing keys safely.  
- Use f-strings to build readable sentences from dictionary data.

---

# 8️⃣ Small Patterns to Remember

- **Counting frequencies**:

```python
from collections import defaultdict

freq = defaultdict(int)

text = "banana"

for ch in text:
    freq[ch] += 1

print(freq)  # counts of each character
```

- **Building records from input**:

```python
student = {}
student["name"] = input("Enter name: ")
student["age"] = int(input("Enter age: "))
```

- **Formatting reports**:

```python
report = f"Name: {student['name']}, Age: {student['age']}"
print(report)
```

---

# 9️⃣ Common Mistakes

- Using `student["missing_key"]` instead of `student.get("missing_key")` when not sure the key exists.  
- Forgetting quotes around string keys:

```python
student[name]  # wrong, tries to use variable name
student["name"]  # correct
```

- Mixing up list indexing and dictionary key access syntax.  

---

# 🔟 Practice Ideas

1. Create a dictionary for a book: title, author, price; print a formatted description using an f-string.  
2. Given a string, build a dictionary of character counts (frequency map).  
3. Use `defaultdict(list)` to group student names by grade (`"A"`, `"B"`, etc.).  

