# Introduction to Programming, Python Basics & Development Environment Setup

---

# 1️⃣ Introduction to Programming

## What is Programming?

Programming means giving **step-by-step instructions** to a computer so it can perform tasks.

A computer does not understand human language — it understands **code**.

Example:

👉 When you use a calculator app, someone has written a program that tells the computer:

* Take input
* Perform calculation
* Show result

Python is one of the easiest languages to start programming because:

* Simple syntax
* Easy to read
* Used in real industry projects

---

## Why Learn Python?

Python is widely used in:

* Artificial Intelligence
* Data Science
* Web Development
* Automation
* Software Engineering

Advantages:

* Beginner-friendly
* Huge community support
* Cross-platform (Windows, Mac, Linux)

---

# 2️⃣ Python Setup & Development Environment

## Installing Python

Python must be installed before writing programs.

Steps:

1. Download from official Python website.
2. Install with “Add Python to PATH” option enabled.
3. Verify installation using terminal.

```bash
python --version
```

If Python is installed correctly, the version number will appear.

---

## Using VS Code

VS Code is a professional code editor used by developers.

Why VS Code?

* Smart auto-completion
* Syntax highlighting
* Debugging tools
* Integrated terminal

Setup Steps:

1. Install VS Code
2. Install **Python Extension**
3. Create a folder for projects
4. Open folder in VS Code

---

## Writing Your First Python Program

Create file:

```text
hello.py
```

Write:

```python
print("Hello World")
```

Run using terminal:

```bash
python hello.py
```

Explanation:

* `print()` is a built-in function.
* It displays output on the screen.

---

# 3️⃣ Working with Terminal

The terminal is a text-based interface to interact with your computer.

Developers prefer terminal because:

* Faster workflow
* Required for professional environments
* Useful for running scripts

Common commands:

```bash
pwd    # shows current folder
ls     # list files
cd     # change directory
mkdir  # create folder
```

Example:

```bash
mkdir python_practice
cd python_practice
```

---

# 4️⃣ Variables and Data Types

## What is a Variable?

A variable is a container that stores data.

Example:

```python
name = "Ali"
age = 21
```

Here:

* `name` stores text
* `age` stores number

---

## Python Data Types

### 1. String (str)

Used for text.

```python
city = "Bangalore"
```

### 2. Integer (int)

Whole numbers.

```python
count = 10
```

### 3. Float (float)

Decimal numbers.

```python
price = 99.50
```

### 4. Boolean (bool)

True or False values.

```python
is_active = True
```

---

## Checking Data Type

```python
print(type(price))
```

Output:

```
<class 'float'>
```

---

## Dynamic Typing in Python

Python automatically decides the data type.

```python
x = 10
x = "Hello"
```

The same variable can store different types.

---

# 5️⃣ Operators

Operators perform operations on values.

---

## Arithmetic Operators

Used for calculations.

```python
a = 10
b = 3

print(a + b)   # addition
print(a - b)   # subtraction
print(a * b)   # multiplication
print(a / b)   # division
print(a % b)   # remainder
```

Real-world example:

```python
price = 500
discount = 50
final_price = price - discount
```

---

## Comparison Operators

Used to compare values.

```python
print(10 > 5)   # True
print(10 == 5)  # False
print(10 != 5)  # True
```

These return Boolean values.

---

## Logical Operators

Used to combine conditions.

```python
x = True
y = False

print(x and y)
print(x or y)
print(not x)
```

Example:

```python
age = 20
print(age > 18 and age < 30)
```

---

## Assignment Operators

Shortcut operators.

```python
count = 10
count += 5   # same as count = count + 5
count -= 2
```

---

# 6️⃣ Expressions

An expression is a combination of values and operators.

```python
result = 5 + 3 * 2
```

Python follows operator precedence:

1. Multiplication
2. Division
3. Addition
4. Subtraction

Use parentheses to control order:

```python
result = (5 + 3) * 2
```

---

# 7️⃣ Input and Output

## Output using print()

```python
print("Welcome to Python")
```

Multiple values:

```python
name = "Ali"
print("Hello", name)
```

---

## Input using input()

Input allows user interaction.

```python
name = input("Enter your name: ")
```

Important:

👉 `input()` always returns **string**.

---

## Type Conversion

To convert string into integer:

```python
age = int(input("Enter age: "))
```

Float conversion:

```python
price = float(input("Enter price: "))
```

---

## Example Program

```python
name = input("Enter your name: ")
age = int(input("Enter age: "))

print("Hello", name)
print("Next year age:", age + 1)
```

Explanation:

* User enters data.
* Program processes it.
* Output is displayed.

---

# 8️⃣ Common Beginner Mistakes

## Mistake 1 — Missing Quotes

```python
print("Hello)
```

Error occurs because string is incomplete.

---

## Mistake 2 — Adding String with Integer

```python
age = input("Enter age: ")
print(age + 1)
```

Fix:

```python
age = int(input("Enter age: "))
```

---

## Mistake 3 — Running File Incorrectly

Always run via terminal:

```bash
python file.py
```

---

# 9️⃣ Practice Exercises

## Exercise 1 — Sum of Two Numbers

```python
a = int(input("Enter first number: "))
b = int(input("Enter second number: "))
print("Sum:", a + b)
```

---

## Exercise 2 — Simple Calculator

```python
num1 = int(input("Enter number 1: "))
num2 = int(input("Enter number 2: "))

print("Addition:", num1 + num2)
print("Multiplication:", num1 * num2)
```

---

# 🔟 Key Takeaways

* Python is beginner-friendly and powerful.
* Variables store values.
* Data types include int, float, string, bool.
* Operators perform calculations and comparisons.
* input() gets user data.
* print() shows output.

---


