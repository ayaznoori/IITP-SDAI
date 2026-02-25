# **Lecture Script — Conditionals & Logical Thinking in Python**

---

# 🎯 Hook (5 minutes)

Start by asking students:

“Can a program make decisions like humans?”

Give real-life examples:

* If rain → carry umbrella
* If marks ≥ 90 → Grade A
* If password correct → login

Explain:

👉 Until now, our programs ran line by line.
👉 Today we teach Python **how to think and decide**.

Key statement to students:

“Conditionals are the brain of your program.”

---

# Section 1 — Boolean Thinking (True / False) (15 minutes)

Explain:

Computers do not think in emotions or language — they think in:

```text
True or False
```

Live Demo:

```python
print(10 > 5)
print(5 == 2)
print(20 <= 20)
```

Ask students:

“What do you notice?”

Guide them:

Every comparison produces a Boolean result.

---

## Comparison Operators

Write on board:

* `>`
* `<`
* `==`
* `!=`
* `>=`
* `<=`

Demo:

```python
age = 20
print(age >= 18)
```

Explain:

👉 This result becomes the foundation of decision making.

---

# Section 2 — Logical Operators (15 minutes)

Explain:

Sometimes one condition is not enough.

Example story:

“To enter a competition: age < 25 AND marks > 80”

Code:

```python
marks = 85
age = 22

print(marks > 80 and age < 25)
```

Explain operators:

* `and` → both must be True
* `or` → at least one True
* `not` → reverse result

Demo:

```python
print(True and False)
print(True or False)
print(not True)
```

Ask class:

“What happens if one condition fails?”

---

# Section 3 — The `if` Statement (20 minutes)

Explain structure:

```python
if condition:
    action
```

Example:

```python
age = 19

if age >= 18:
    print("Eligible to vote")
```

Teaching Point:

👉 Indentation matters in Python.

Show wrong example:

```python
if age >= 18:
print("Hello")
```

Explain:

Python uses indentation instead of brackets.

---

# Section 4 — The `else` Statement (15 minutes)

Explain:

Sometimes we want an alternative action.

Example:

```python
age = 15

if age >= 18:
    print("Adult")
else:
    print("Minor")
```

Teaching Tip:

Ask students:

“What happens if condition is False?”

Make them predict output before running.

---

# Section 5 — The `elif` Statement (20 minutes)

Explain:

Used when we have multiple conditions.

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

Explain execution order:

Python checks from top to bottom.

Important Teaching Line:

👉 “Order of conditions matters.”

Show incorrect ordering example and ask students to debug.

---

# Section 6 — Nested Conditionals (20 minutes)

Explain:

A conditional inside another conditional.

Real-life analogy:

“If user logged in
 → If account active
  → Show dashboard”

Code:

```python
age = 22
citizen = True

if age >= 18:
    if citizen:
        print("Eligible to vote")
```

Ask students:

“Why not combine conditions?”

Then show alternative:

```python
if age >= 18 and citizen:
```

Explain difference between nesting vs logical operators.

---

# Section 7 — Logical Thinking & Problem Solving (20 minutes)

Pause coding.

Explain mental process developers use:

1. Understand problem
2. Identify conditions
3. Decide outcomes
4. Write logic

Example Problem:

Check if number is positive, negative, or zero.

Ask students to THINK before coding.

Then write:

```python
num = int(input("Enter number: "))

if num > 0:
    print("Positive")
elif num < 0:
    print("Negative")
else:
    print("Zero")
```

Highlight:

👉 Coding starts with thinking, not typing.

---

# Section 8 — Common Mistakes (10 minutes)

Mistake 1 — Using `=` instead of `==`

```python
if age = 18:
```

Explain difference between assignment and comparison.

---

Mistake 2 — Incorrect indentation.

---

Mistake 3 — Logical order mistakes.

Example:

```python
if marks >= 50:
elif marks >= 90:
```

Explain why it fails logically.

---

# Section 9 — Guided Practice (20 minutes)

## Practice 1 — Even or Odd

```python
num = int(input("Enter number: "))

if num % 2 == 0:
    print("Even")
else:
    print("Odd")
```

Ask students to explain logic verbally.

---

## Practice 2 — Scholarship Checker

```python
marks = int(input("Enter marks: "))
age = int(input("Enter age: "))

if marks > 80 and age < 25:
    print("Eligible")
else:
    print("Not Eligible")
```

Encourage prediction before running.

---

# Section 10 — Mini Challenge (10 minutes)

Ask students to design logic first:

“Create a program that checks login status and subscription type.”

Let them write pseudocode before coding.

---

# 🧠 Wrap-Up (5 minutes)

Key Takeaways:

1. Conditionals allow programs to make decisions.
2. Boolean logic drives True/False thinking.
3. `if`, `elif`, `else` control program flow.
4. Nested logic handles complex decisions.
5. Good programmers think step-by-step before coding.

End with:

👉 “Programming is not about syntax — it is about logical thinking.”

---