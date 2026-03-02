## Session Number

2.1

## Title

Loops & Iterations in Python

## Objective

Automating repetitive tasks with loops; Understanding when to use For vs While; Implementing loop control statements

## Topics & Subtopics Covered

- **Loops**
  - Using `for` loops and `while` loops
  - Controlling execution with `break` and `continue`
  - Automating repetitive sequences
- **Problem Solving**
  - Iterative problem solving
  - Breaking problems into small testable parts

---

# 📝 MCQs — Loops & Iterations (with Explanations)

---

## Q1. What is the output of this code?

```python
for i in range(1, 4):
    print(i)
```

A) 1 2  
B) 1 2 3  
C) 0 1 2  
D) 0 1 2 3  

✅ **Correct Answer:** B  

**Explanation:**  
`range(1, 4)` generates 1, 2, 3. The `stop` value (4) is not included.

---

## Q2. Which loop is more appropriate when you **do not know** how many times to repeat?

A) `for` loop  
B) `while` loop  
C) Both are the same  
D) None of the above  

✅ **Correct Answer:** B  

**Explanation:**  
`while` loops continue as long as a condition is True and are suitable when the number of iterations is not fixed.

---

## Q3. What will this code print?

```python
count = 0

for num in range(1, 6):
    if num % 2 == 0:
        count = count + 1

print(count)
```

A) 0  
B) 2  
C) 3  
D) 5  

✅ **Correct Answer:** B  

**Explanation:**  
Even numbers between 1 and 5 are 2 and 4 → total 2.

---

## Q4. What does the `break` statement do inside a loop?

A) Skips current iteration  
B) Exits the loop immediately  
C) Restarts the loop from beginning  
D) Does nothing  

✅ **Correct Answer:** B  

**Explanation:**  
`break` terminates the loop and control moves to the first statement after the loop.

---

## Q5. What will be the output?

```python
for i in range(1, 6):
    if i == 3:
        continue
    print(i)
```

A) 1 2 3 4 5  
B) 1 2 3 4  
C) 1 2 4 5  
D) 2 3 4 5  

✅ **Correct Answer:** C  

**Explanation:**  
When `i == 3`, `continue` skips the `print(i)` for that iteration.

---

## Q6. Which of the following may cause an infinite loop?

A)

```python
for i in range(10):
    print(i)
```

B)

```python
num = 1
while num <= 5:
    print(num)
    num = num + 1
```

C)

```python
num = 1
while num <= 5:
    print(num)
```

D)

```python
for ch in "abc":
    print(ch)
```

✅ **Correct Answer:** C  

**Explanation:**  
In C, `num` is never updated, so the condition `num <= 5` stays True forever.

---

## Q7. What will be printed?

```python
i = 1

while i < 5:
    print(i)
    i = i + 2
```

A) 1 2 3 4  
B) 1 3  
C) 1 3 5  
D) 2 4  

✅ **Correct Answer:** B  

**Explanation:**  
Values of `i` are 1, 3; then `i` becomes 5 and the condition `i < 5` fails.

---

## Q8. Which of the following best describes **iterative problem solving**?

A) Writing one long line of code  
B) Solving a problem in repeated, small steps, updating values each time  
C) Solving everything without using loops  
D) Using only `if-else` statements  

✅ **Correct Answer:** B  

**Explanation:**  
Iterative problem solving uses loops to move step-by-step towards the solution.

---

# ✍️ Subjective / Coding Question (Medium–Hard)

## Q9. Customer Billing With Loops

### 🔹 Problem Statement

Write a Python program that:

1. Asks the user how many items they bought (`n`).  
2. Uses a **loop** to take the price of each item.  
3. Calculates the **total bill**.  
4. Uses another loop to print the line:

```text
Item <number>: <price>
```

5. Finally prints:

```text
Total items: <n>
Total bill: <total>
Average price: <average>
```

### 🔹 Requirements

- Use **at least one `for` loop**.  
- Use **iterative updates** (e.g., `total = total + price`).  
- Handle the case where `n` could be 0 (no items).  

---

## ✅ Reference Solution (For Instructor / Review)

```python
n = int(input("Enter number of items: "))

prices = []
total = 0

for i in range(1, n + 1):
    price = float(input(f"Enter price of item {i}: "))
    prices.append(price)
    total = total + price

print("------ Bill Details ------")

for i in range(1, n + 1):
    print(f"Item {i}: {prices[i - 1]}")

if n > 0:
    average = total / n
else:
    average = 0

print("Total items:", n)
print("Total bill:", total)
print("Average price:", average)
```

---

