# 📝 **MCQ Questions (8) — With Explanation**

---

## **Q1. What will be the output of the following code?**

```python
age = 20

if age >= 18:
    print("Adult")
else:
    print("Minor")
```

A) Minor
B) Adult
C) Error
D) Nothing

✅ **Correct Answer:** B

**Explanation:**
The condition `age >= 18` is True, so the `if` block executes and prints **Adult**.

---

## **Q2. Which operator is used to check equality in Python?**

A) `=`
B) `==`
C) `!=`
D) `<>`

✅ **Correct Answer:** B

**Explanation:**
`==` compares values.
`=` assigns a value to a variable.

---

## **Q3. What will be the output?**

```python
marks = 85

if marks > 90:
    print("A")
elif marks > 70:
    print("B")
else:
    print("C")
```

A) A
B) B
C) C
D) Error

✅ **Correct Answer:** B

**Explanation:**
First condition is False.
Second condition is True, so it prints **B**.

---

## **Q4. Which statement about Boolean logic is TRUE?**

A) `and` returns True if one condition is True
B) `or` requires both conditions True
C) `and` requires both conditions True
D) `not` always returns True

✅ **Correct Answer:** C

**Explanation:**
`and` → both must be True.
`or` → at least one True.

---

## **Q5. What will be the output?**

```python
print(not (10 > 5))
```

A) True
B) False
C) Error
D) 0

✅ **Correct Answer:** B

**Explanation:**

```text
10 > 5 → True
not True → False
```

---

## **Q6. Identify the nested conditional structure.**

A)

```python
if age > 18:
    print("Adult")
```

B)

```python
if age > 18:
    if citizen:
        print("Eligible")
```

C)

```python
print(age > 18)
```

D)

```python
age = 18
```

✅ **Correct Answer:** B

**Explanation:**
Nested means an `if` inside another `if`.

---

## **Q7. What will happen when this code runs?**

```python
num = 5

if num > 0:
print("Positive")
```

A) Prints Positive
B) SyntaxError
C) IndentationError
D) Logical Error

✅ **Correct Answer:** C

**Explanation:**
Python requires indentation after `if`. Missing indentation causes an error.

---

## **Q8. What will be the output?**

```python
x = 10

if x > 5:
    print("A")
elif x > 8:
    print("B")
else:
    print("C")
```

A) A
B) B
C) C
D) A and B

✅ **Correct Answer:** A

**Explanation:**
Python stops after the first True condition.
Even though `x > 8` is also True, it will not execute.

---

# ✍️ **Subjective Question (Medium–Hard Level)**

## **Q9. Grade Evaluation & Eligibility Program**

### 🔹 Problem Statement

Write a Python program that performs the following tasks:

1. Take student name as input.
2. Take student marks (integer) as input.
3. Take attendance percentage as input.
4. Assign grade using conditionals:

* Marks ≥ 90 → Grade A
* Marks ≥ 75 → Grade B
* Marks ≥ 50 → Grade C
* Otherwise → Fail

5. Determine scholarship eligibility using nested conditionals:

* Student must NOT fail.
* Attendance must be greater than 80.

6. Display output in this format:

```
Hello <name>
Grade: <grade>
Scholarship Eligible: <True/False>
```

---

## ✅ **Expected Solution (Reference Answer)**

```python
name = input("Enter student name: ")
marks = int(input("Enter marks: "))
attendance = int(input("Enter attendance: "))

# Grade calculation
if marks >= 90:
    grade = "A"
elif marks >= 75:
    grade = "B"
elif marks >= 50:
    grade = "C"
else:
    grade = "Fail"

# Nested conditional for scholarship
eligible = False
if grade != "Fail":
    if attendance > 80:
        eligible = True

print("Hello", name)
print("Grade:", grade)
print("Scholarship Eligible:", eligible)
```

---

