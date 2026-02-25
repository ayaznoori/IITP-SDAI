# 🎤 **LECTURE SCRIPT**

# Introduction to Programming, Python Basics & Development Environment Setup

⏱️ **Total Duration: 2 Hours 30 Minutes**

---

## 🎯 Hook (5 minutes)

“Today you are not just learning Python… you are learning how software engineers actually start building real programs.”

Ask students:

👉 *How do apps like Instagram or Amazon start?*
They start with **simple programs**, variables, logic, and input/output.

Python is one of the most beginner-friendly and industry-ready languages used in:

* AI & Machine Learning
* Automation
* Web Development
* Data Science

By the end of this session, you will:

✅ Install Python
✅ Run programs using VS Code & Terminal
✅ Understand variables, operators, expressions
✅ Take user input and print output

---

## Section 1 — Python Setup & Development Environment (25 minutes)

### Explain Concept

Python is an **interpreted language**, meaning code runs line by line.

### Step 1: Install Python

Show students:

```bash
python --version
```

If not installed → download from python.org.

Explain:

👉 Python automatically installs **pip** (package manager).

---

### Step 2: VS Code Setup

Explain why developers use VS Code:

* Lightweight
* Extensions
* Debugging
* Terminal integration

Steps:

1. Install VS Code
2. Install Python Extension
3. Create folder → `python-basics`

---

### Step 3: Running First Program

Create file:

```
hello.py
```

```python
print("Hello World")
```

Run using terminal:

```bash
python hello.py
```

Explain difference:

❌ Not double-clicking file
✅ Running through terminal like developers

---

## Section 2 — Navigating Terminal (15 minutes)

Explain terminal = developer control panel.

Basic commands:

```bash
pwd     # current directory
ls      # list files
cd      # change folder
```

Live Demo:

```bash
mkdir python-class
cd python-class
code .
```

Explain:

👉 Terminal improves productivity
👉 Needed for real projects

---

## Section 3 — Variables and Data Types (25 minutes)

Start with question:

“What is a variable?”

👉 A container to store data.

```python
name = "Ayaz"
age = 25
price = 99.99
is_student = True
```

Explain data types:

* str
* int
* float
* bool

Show:

```python
print(type(name))
```

Key teaching point:

Python is **dynamically typed**.

---

## Section 4 — Operators (30 minutes)

Break into categories.

### Arithmetic Operators

```python
a = 10
b = 3

print(a + b)
print(a * b)
print(a / b)
print(a % b)
```

Explain real-world examples:

* price calculations
* score averages

---

### Comparison Operators

```python
print(a > b)
print(a == b)
print(a != b)
```

Explain:

👉 Used in decision making later (if statements).

---

### Logical Operators

```python
x = True
y = False

print(x and y)
print(x or y)
print(not x)
```

---

### Assignment Operators

```python
count = 10
count += 5
count -= 2
```

Explain shorthand coding used in industry.

---

## Section 5 — Expressions (10 minutes)

Explain:

Expression = combination of values + operators.

```python
result = 5 + 3 * 2
```

Explain order of operations.

Ask students:

👉 What will be output?

---

## Section 6 — Input and Output (25 minutes)

### print()

```python
print("Welcome to Python")
```

Formatting:

```python
name = "Ali"
print("Hello", name)
```

---

### input()

```python
name = input("Enter your name: ")
print("Hello", name)
```

Explain:

⚠️ input always returns string.

Convert types:

```python
age = int(input("Enter age: "))
```

---

### Mini Project Example

```python
name = input("Enter name: ")
age = int(input("Enter age: "))

print("Hello", name)
print("Next year you will be", age + 1)
```

Students love this — keep interactive.

---

## Section 7 — Common Beginner Mistakes (10 minutes)

Mistake 1:

```python
print("Hello)
```

Missing quotes.

Mistake 2:

```python
age = input("Enter age")
print(age + 1)
```

TypeError → must convert.

---

## Section 8 — Practice Problems (15 minutes)

### Practice 1

Take two numbers from user and print sum.

```python
a = int(input("Enter first number: "))
b = int(input("Enter second number: "))
print(a + b)
```

---

### Practice 2

Create a calculator:

```python
num1 = int(input("Enter number 1: "))
num2 = int(input("Enter number 2: "))

print("Addition:", num1 + num2)
print("Multiplication:", num1 * num2)
```

---

## Wrap-Up (10 minutes)

Key points:

1. Python installed and configured
2. VS Code + Terminal workflow
3. Variables & Data Types
4. Operators & Expressions
5. input() and print()

