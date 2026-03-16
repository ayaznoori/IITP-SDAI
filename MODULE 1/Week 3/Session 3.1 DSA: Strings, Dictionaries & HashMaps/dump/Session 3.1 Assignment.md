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

# 📝 MCQs — Strings, Dictionaries & HashMaps (with Explanations)

---

## Q1. What will be the output of the following code?

```python
text = "Python"
print(text[1:4])
```

A) `Pyt`  
B) `yth`  
C) `tho`  
D) `ytho`  

✅ **Correct Answer:** B  

**Explanation:**  
`text[1:4]` includes indexes 1, 2, 3 → `y`, `t`, `h` → `"yth"`.

---

## Q2. Which of the following correctly defines a dictionary in Python?

A)

```python
student = ["name": "Ali", "age": 21]
```

B)

```python
student = ("name": "Ali", "age": 21)
```

C)

```python
student = {"name": "Ali", "age": 21}
```

D)

```python
student = {"name", "Ali", "age", 21}
```

✅ **Correct Answer:** C  

**Explanation:**  
Dictionaries use curly braces with `key: value` pairs.

---

## Q3. What will this code print?

```python
student = {"name": "Riya", "age": 20}
print(student.get("city", "Unknown"))
```

A) `None`  
B) `city`  
C) `Unknown`  
D) Error  

✅ **Correct Answer:** C  

**Explanation:**  
`city` key is missing, so `get("city", "Unknown")` returns the default `"Unknown"`.

---

## Q4. Which method would you use to get both keys and values together from a dictionary?

A) `keys()`  
B) `values()`  
C) `items()`  
D) `update()`  

✅ **Correct Answer:** C  

**Explanation:**  
`items()` returns key–value pairs which can be unpacked in loops.

---

## Q5. What is the output of the following code?

```python
from collections import defaultdict

counts = defaultdict(int)

counts["a"] += 1
counts["b"] += 2

print(counts["c"])
```

A) 0  
B) 1  
C) 2  
D) Error  

✅ **Correct Answer:** A  

**Explanation:**  
`defaultdict(int)` gives default value `0` for missing keys.

---

## Q6. What will be printed?

```python
student = {"name": "Ali", "age": 21}
student.setdefault("city", "Delhi")
student.setdefault("city", "Mumbai")
print(student["city"])
```

A) `Delhi`  
B) `Mumbai`  
C) `None`  
D) Error  

✅ **Correct Answer:** A  

**Explanation:**  
`setdefault` sets `"city"` to `"Delhi"` the first time. The second call does not overwrite it.

---

## Q7. Which of the following best describes a **hash map**?

A) A list of numbers sorted in order.  
B) A data structure that maps keys to values using hashing for fast lookup.  
C) A function that returns the length of a list.  
D) A table used only for string concatenation.  

✅ **Correct Answer:** B  

**Explanation:**  
Hash maps (like Python dictionaries) use hashing to quickly find values by key.

---

## Q8. Which is the most Pythonic way to format this output?

Goal:  

```text
Student Ali is 21 years old.
```

Given:

```python
name = "Ali"
age = 21
```

A)

```python
print("Student " + name + " is " + str(age) + " years old.")
```

B)

```python
print("Student {} is {} years old.".format(name, age))
```

C)

```python
print(f"Student {name} is {age} years old.")
```

D) Both B and C  

✅ **Correct Answer:** D  

**Explanation:**  
Both `.format()` and f-strings are clean and recommended; f-strings are the more modern approach.

---

# ✍️ Subjective / Coding Question (Medium–Hard)

## Q9. Word Frequency Counter With Safe Dictionary Access

### 🔹 Problem Statement

Write a Python program that:

1. Takes a sentence as input from the user.  
2. Converts it to lowercase.  
3. Splits it into words (assume words are separated by spaces).  
4. Uses a dictionary (or `defaultdict`) to count how many times each word appears.  
5. Prints the word counts in the following format:

```text
<word>: <count>
```

Additional requirements:

- Use **safe access patterns**: `get()` or `defaultdict(int)` to avoid KeyError.  
- Use an f-string to format the output line for each word.  

---

## ✅ Reference Solution (For Instructor / Review)

```python
from collections import defaultdict

sentence = input("Enter a sentence: ")

sentence = sentence.lower()
words = sentence.split()

freq = defaultdict(int)

for w in words:
    freq[w] += 1

for word, count in freq.items():
    print(f"{word}: {count}")
```

Alternative solution without `defaultdict`:

```python
sentence = input("Enter a sentence: ")

sentence = sentence.lower()
words = sentence.split()

freq = {}

for w in words:
    freq[w] = freq.get(w, 0) + 1

for word, count in freq.items():
    print(f"{word}: {count}")
```

---

