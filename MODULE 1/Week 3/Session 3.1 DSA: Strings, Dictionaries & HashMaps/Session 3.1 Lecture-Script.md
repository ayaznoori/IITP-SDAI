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

# 🎤 Lecture Script — Strings, Dictionaries & HashMaps

⏱️ **Total Duration: ~2 hours 30 minutes**

---

## 🎯 Hook (5 minutes)

Start with a question:

> “If I give you a student’s roll number, how quickly can your program find their details?”

Explain:

- Lists are good when we care about **positions** (index 0, 1, 2…).  
- In real systems, we often care about **keys** (roll number, email, ID).  

Key message:

> “Today we learn how to map **keys → values** using dictionaries (hash maps), and how to format text cleanly using strings.”

---

## Section 1 — Quick Recap of Strings (15 minutes)

Remind students that strings are sequences:

```python
text = "Python"
```

### Live Demo — Indexing

```python
print(text[0])
print(text[1])
print(text[-1])
```

Ask:

- “What index gives you `h`?”  

### Live Demo — Slicing

```python
print(text[1:4])
print(text[:3])
print(text[3:])
```

Explain:

- `start` included, `end` excluded.  
- Negative slices like `text[-3:]`.  

Transition:

> “We will soon use strings together with dictionaries to build data-processing programs.”

---

## Section 2 — String Concatenation & Formatting (20 minutes)

### Concatenation Problems

Show:

```python
name = "Ali"
age = 21

print("Student " + name + " is " + str(age) + " years old.")
```

Explain:

- Need `str(age)` → can get messy.  

### Introduce f-Strings

```python
msg = f"Student {name} is {age} years old."
print(msg)
```

Highlight:

- Cleaner, easier to read.  
- Can embed expressions:

```python
print(f"Next year {name} will be {age + 1}")
```

### `.format()` Alternative

```python
msg = "Student {} is {} years old.".format(name, age)
print(msg)
```

Ask students to rewrite one concatenation example using f-strings.

---

## Section 3 — Introducing Dictionaries / HashMaps (25 minutes)

Analogy:

> “Think of a real dictionary: you look up a **word** (key) to find its **meaning** (value).”

### Basic Definition

```python
student = {
    "name": "Riya",
    "age": 20,
    "course": "Python"
}
```

Explain:

- Keys are usually strings.  
- Values can be any type.  

### Accessing Values by Key

```python
print(student["name"])
print(student["age"])
```

Ask:

- “What happens if we do `student['city']`?”  

Run and show **KeyError**.

Teaching point:

> “Direct key access is powerful but unsafe if the key might be missing.”

---

## Section 4 — Updating and Modifying Dictionaries (15 minutes)

### Modify Values

```python
student["age"] = 21
```

### Add New Key–Value Pairs

```python
student["city"] = "Bangalore"
```

### Delete Keys

```python
del student["course"]
```

Explain:

- Dictionaries are **mutable**.  

---

## Section 5 — Dictionary Methods (25 minutes)

Use existing `student` dict and demo:

```python
print(student.keys())
print(student.values())
print(student.items())
```

### Loop Over Keys and Values

```python
for key in student.keys():
    print(key)

for value in student.values():
    print(value)

for key, value in student.items():
    print(key, "=", value)
```

Explain practical uses:

- Building reports  
- Debugging data  

### `update()` Method

```python
student.update({"age": 22, "country": "India"})
```

Explain:

- Useful when merging multiple pieces of information.

---

## Section 6 — Safe Access: `get()` and `setdefault()` (25 minutes)

### Problem: Missing Keys

```python
student = {"name": "Ali"}

# print(student["age"])  # KeyError
```

Explain:

- Data from real systems can be incomplete.  
- We do not want our program to crash.

### `get()` with Default

```python
age = student.get("age", 0)
city = student.get("city", "Unknown")

print(age)
print(city)
```

Teaching point:

> “Use `get()` when you’re **not sure** if the key exists.”

---

### `setdefault()`

Explain:

> “`setdefault` both reads and ensures a key has a default value.”

Demo:

```python
preferences = {}

theme = preferences.setdefault("theme", "light")
print(theme)                 # 'light'
print(preferences["theme"])  # 'light'
```

Then:

```python
theme_again = preferences.setdefault("theme", "dark")
print(theme_again)           # still 'light'
```

Explain:

- It only sets the value if the key was missing.  

---

## Section 7 — `defaultdict` from `collections` (20 minutes)

Introduce:

```python
from collections import defaultdict
```

Explain use case:

> “We often want to count or group things without checking if keys exist first.”

### Counting Example

```python
from collections import defaultdict

freq = defaultdict(int)
text = "banana"

for ch in text:
    freq[ch] += 1

print(freq)
```

Explain:

- `defaultdict(int)` creates missing keys with default `0`.  
- No explicit `if key not in freq` needed.

### Grouping Example

```python
group = defaultdict(list)

group["A"].append("Ali")
group["A"].append("Asha")
group["B"].append("Balu")

print(group)
```

Explain:

- Missing keys automatically start as empty lists.

---

## Section 8 — Combining Strings + Dictionaries (20 minutes)

Build a small profile:

```python
student = {
    "name": "Riya",
    "age": 21,
    "city": "Bangalore"
}

msg = f"{student.get('name', 'Unknown')} from {student.get('city', 'Unknown')} is {student.get('age', 0)} years old."
print(msg)
```

Explain:

- Using `get()` for safety.  
- Using f-strings for readability.  

Add a mini exercise:

> “Format and print: `Name: <name> | Age: <age> | City: <city>` using f-string and `.format()`.”

---

## Section 9 — Guided Practice (20 minutes)

Give students 2–3 tasks:

1. Create a dictionary `book` with keys: `title`, `author`, `price`; then print a formatted description.  
2. Given a string input, build a frequency dictionary for characters using `defaultdict(int)`.  
3. Use `setdefault()` to ensure a student dictionary always has `"language"` set (default `"Python"`).  

Walk around and help them:

- Choose between direct indexing vs `get()`  
- Use f-strings properly  
- Understand error messages (`KeyError`)  

---

## Section 10 — Wrap-Up (5 minutes)

Summarize key ideas:

1. Strings can be indexed and sliced; we can format them cleanly with f-strings and `.format()`.  
2. Dictionaries map **keys → values**, perfect for structured data like student profiles.  
3. Methods like `get()`, `setdefault()`, and `update()` make dictionaries safer and more powerful.  
4. `defaultdict` simplifies counting and grouping patterns.  
5. Combining strings + dictionaries lets us build readable reports and process data effectively.

End with:

> “Whenever you think ‘I want to look up something by a name or ID’, you should immediately think: **dictionary / hash map**.”

