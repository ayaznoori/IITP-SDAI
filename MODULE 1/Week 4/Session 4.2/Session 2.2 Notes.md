 # **Lecture Notes — Conditionals & Logical Thinking in Python**

---

# 1️⃣ Introduction to Decision Making in Programming

So far, programs execute line by line from top to bottom.
But real-world applications need **decision-making ability**.

Example:

* If password is correct → login
* If marks ≥ 90 → Grade A
* If balance < 0 → show warning

Decision-making in Python is done using **conditional statements**.

---

# 2️⃣ Boolean Logic — Foundation of Conditionals

A condition always evaluates to:

```text
True  or  False
```

These are Boolean values.

Example:

```python
print(10 > 5)   # True
print(5 == 2)   # False
```

---

## Comparison Operators

| Operator | Meaning          | Example    |
| -------- | ---------------- | ---------- |
| `>`      | Greater than     | `10 > 5`   |
| `<`      | Less than        | `3 < 2`    |
| `==`     | Equal to         | `5 == 5`   |
| `!=`     | Not equal        | `5 != 3`   |
| `>=`     | Greater or equal | `10 >= 10` |
| `<=`     | Less or equal    | `4 <= 6`   |

Example:

```python
age = 20
print(age >= 18)
```

Output:

```text
True
```

---

## Logical Operators

Logical operators combine multiple conditions.

### AND

Both conditions must be True.

```python
marks = 80
attendance = 90

print(marks > 40 and attendance > 75)
```

---

### OR

At least one condition must be True.

```python
print(10 > 5 or 2 > 10)
```

---

### NOT

Reverses the result.

```python
print(not True)   # False
```

---

# 3️⃣ The `if` Statement

The `if` statement checks a condition.

Syntax:

```python
if condition:
    statement
```

Example:

```python
age = 18

if age >= 18:
    print("You are eligible to vote")
```

Explanation:

* Python checks condition.
* If True → block runs.
* If False → skipped.

---

## Indentation in Python

Python uses **indentation** instead of braces `{}`.

Correct:

```python
if True:
    print("Hello")
```

Wrong:

```python
if True:
print("Hello")
```

Indentation defines code blocks.

---

# 4️⃣ The `else` Statement

Used when the condition is False.

```python
age = 15

if age >= 18:
    print("Adult")
else:
    print("Minor")
```

Flow:

* If condition True → first block runs.
* Otherwise → else block runs.

---

# 5️⃣ The `elif` Statement

Used when multiple conditions exist.

Example:

```python
marks = 75

if marks >= 90:
    print("Grade A")
elif marks >= 70:
    print("Grade B")
else:
    print("Grade C")
```

Execution:

* Python checks from top to bottom.
* First matching condition executes.

---

# 6️⃣ Nested Conditionals

Nested means placing a conditional inside another conditional.

Example:

```python
age = 22
citizen = True

if age >= 18:
    if citizen:
        print("Eligible to vote")
```

Explanation:

Step 1: Check outer condition
Step 2: If True → check inner condition

---

## Why Use Nesting?

Used when decisions depend on multiple levels.

Real example:

* If user logged in
   → If account active
    → Show dashboard

---

# 7️⃣ Logical Thinking in Programming

Programming is about structured thinking.

Before writing code, think:

1. What is the problem?
2. What inputs are needed?
3. What conditions exist?
4. What should happen if condition is True or False?

Example Problem:

Give discount if price > 1000.

Thinking steps:

```text
Step 1: Take price
Step 2: Check condition
Step 3: Apply logic
```

Code:

```python
price = 1200

if price > 1000:
    print("Discount applied")
```

---

# 8️⃣ Step-by-Step Problem Solving Habit

Developers break problems into smaller parts.

Example:

Check if a number is positive, negative, or zero.

Steps:

1. Take number
2. Compare with zero
3. Print result

Program:

```python
num = int(input("Enter number: "))

if num > 0:
    print("Positive")
elif num < 0:
    print("Negative")
else:
    print("Zero")
```

---

# 9️⃣ Common Beginner Mistakes

## Mistake 1 — Using = instead of ==

Wrong:

```python
if age = 18:
```

Correct:

```python
if age == 18:
```

---

## Mistake 2 — Wrong Indentation

Python will raise an error if indentation is incorrect.

---

## Mistake 3 — Incorrect Logical Thinking

Students sometimes write:

```python
if marks > 90:
elif marks > 70:
```

Without understanding order matters.

Always check **higher conditions first**.

---

# 🔟 Practice Examples

## Example 1 — Even or Odd

```python
num = int(input("Enter number: "))

if num % 2 == 0:
    print("Even")
else:
    print("Odd")
```

---

## Example 2 — Scholarship Eligibility

```python
marks = int(input("Enter marks: "))
age = int(input("Enter age: "))

if marks > 80 and age < 25:
    print("Eligible")
else:
    print("Not Eligible")
```

---

# 📌 Key Takeaways

* Conditionals allow programs to make decisions.
* Boolean logic produces True/False results.
* `if`, `elif`, `else` control program flow.
* Nested conditionals handle complex decisions.
* Logical thinking is more important than memorizing syntax.

 