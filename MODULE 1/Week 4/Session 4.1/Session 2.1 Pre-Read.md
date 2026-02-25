# **Pre-Read Material — Conditionals & Logical Thinking in Python**

## 🎯 Purpose of This Pre-Read

In the next class, you will learn how programs **make decisions**.

Before attending the lecture, you should become familiar with:

* How computers check conditions
* What Boolean logic means
* How developers think step-by-step when solving problems

This pre-read introduces the ideas so you feel comfortable when we start coding.

---

# 1️⃣ What Are Conditionals?

In real life, we make decisions every day.

Examples:

* If it rains → take an umbrella
* If marks are above 90 → grade A
* If age is 18 or more → eligible to vote

Programs also follow the same idea.

A **conditional statement** allows a program to choose different actions based on a condition.

In Python, we mainly use:

* `if`
* `elif`
* `else`

Example idea (don’t worry about full syntax yet):

```python
age = 20

if age >= 18:
    print("Adult")
else:
    print("Minor")
```

👉 The program checks a condition and decides what to do.

---

# 2️⃣ Boolean Logic — True or False Thinking

Computers think using **True** and **False** values.

These are called **Boolean values**.

Examples of conditions:

```python
10 > 5      # True
5 == 10     # False
20 <= 20    # True
```

Programs use comparison operators like:

* `>` greater than
* `<` less than
* `==` equal to
* `!=` not equal to

Before class, try to notice:

👉 Every decision a program makes is based on something being **True or False**.

---

# 3️⃣ Combining Conditions (Logical Thinking)

Sometimes one condition is not enough.

Example:

A student passes if:

* marks > 40 **AND**
* attendance > 75

This is called **logical thinking** in programming.

Python uses logical operators:

* `and`
* `or`
* `not`

Idea example:

```python
marks = 80
attendance = 90

print(marks > 40 and attendance > 75)
```

The result will be either True or False.

---

# 4️⃣ Nested Decisions — Decisions Inside Decisions

Real life decisions are often layered.

Example:

If weather is rainy
 → If temperature is cold
  → wear a jacket

Programs can also place one condition inside another.
This is called **nesting conditionals**.

Concept idea:

```python
if condition1:
    if condition2:
        # action
```

We will explore this deeply during class.

---

# 5️⃣ Logical Thinking as a Skill

Programming is not only about typing code.
It is about thinking in a structured way.

Before writing code, developers usually:

1. Understand the problem.
2. Break it into small steps.
3. Decide conditions.
4. Write logic step-by-step.

Example thought process:

Problem: Give discount if price > 1000

Step-by-step thinking:

* Take price
* Check condition
* Decide output

This structured thinking is more important than memorizing syntax.

---

# 6️⃣ Key Words You Will Hear in Class

Try to become familiar with these terms:

* Condition
* Boolean value
* Logical operator
* Decision making
* Nested logic
* Step-by-step problem solving

You do not need to fully understand them yet — just recognize the words.

---

# 7️⃣ Think About These Before Class

Reflect on these questions:

1. When do you make decisions based on conditions in daily life?
2. Can a program make more than one decision at a time?
3. Why is “True or False” important for computers?

We will discuss these ideas during the lecture.

---

# 🚀 What You Will Learn in the Upcoming Session

During the class, you will:

✅ Write your first `if`, `elif`, and `else` programs
✅ Use Boolean logic to control program flow
✅ Build nested decision structures
✅ Learn structured logical thinking used by real developers



