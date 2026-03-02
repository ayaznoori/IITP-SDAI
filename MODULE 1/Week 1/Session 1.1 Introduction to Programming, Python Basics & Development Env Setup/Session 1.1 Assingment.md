# 📝 **MCQs (With Explanation)**

---

## **Q1. What will be the output of the following program?**

```python
age = input("Enter age: ")
print(type(age))
```

A) `<class 'int'>`
B) `<class 'float'>`
C) `<class 'str'>`
D) Error

✅ **Correct Answer:** C

**Explanation:**
`input()` always returns a string value, even if the user enters numbers. Python does not automatically convert it to integer.

---

## **Q2. Which of the following is the correct way to update a variable using an assignment operator?**

A)

```python
count =+ 5
```

B)

```python
count += 5
```

C)

```python
count == 5
```

D)

```python
count := 5
```

✅ **Correct Answer:** B

**Explanation:**
`+=` is a shorthand assignment operator meaning:

```python
count = count + 5
```

Option C is comparison, not assignment.

---

## **Q3. What will be the output of the expression below?**

```python
result = (10 + 5) * 2
print(result)
```

A) 20
B) 25
C) 30
D) 40

✅ **Correct Answer:** C

**Explanation:**
Parentheses change operator precedence:

```
10 + 5 = 15
15 * 2 = 30
```

---

## **Q4. Which data type is used to store decimal numbers in Python?**

A) int
B) float
C) str
D) bool

✅ **Correct Answer:** B

**Explanation:**
`float` represents numbers with decimal values like `10.5`, `99.99`.

---

## **Q5. What is the purpose of the terminal when working with Python?**

A) To design UI
B) To store variables
C) To execute Python programs and navigate files
D) To install hardware drivers

✅ **Correct Answer:** C

**Explanation:**
Developers use terminal to:

* Run scripts
* Navigate folders
* Install packages
* Manage environment

---

## **Q6. Identify the correct logical operator example.**

A)

```python
print(10 and 5)
```

B)

```python
print(True and False)
```

C)

```python
print(10 ++ 5)
```

D)

```python
print(10 && 5)
```

✅ **Correct Answer:** B

**Explanation:**
Logical operators (`and`, `or`, `not`) work with Boolean values.

Python does NOT use `&&`.

---

## **Q7. What will happen when running this code?**

```python
num = input("Enter number: ")
print(num + 5)
```

A) Prints number + 5
B) Prints string result
C) TypeError occurs
D) SyntaxError occurs

✅ **Correct Answer:** C

**Explanation:**
`num` is a string. Python cannot add string + integer.

Correct solution:

```python
num = int(input("Enter number: "))
```

---

## **Q8. Which of the following statements about Python variables is TRUE?**

A) Variables must declare data type before assignment
B) Python uses dynamic typing
C) Variables cannot change type
D) Variables require semicolons

✅ **Correct Answer:** B

**Explanation:**
Python is dynamically typed:

```python
x = 10
x = "Hello"
```

Same variable holds different data types.

---

# ✍️ **SUBJECTIVE QUESTION (Medium–Hard Level)**

## **Q9. Student Registration Program (Input/Output + Operators + Variables)**

### 🔹 Problem Statement

Write a Python program that simulates a simple **Student Registration System**.

Your program should:

1. Take student name as input.
2. Take student age as input.
4. Take mark1 as input
5. Take mark2 as input
4. Calculate total marks and average marks.
5. Display the following output:

```
Hello <name>
Age next year: <age+1>
Total Marks: <total>
Average Marks: <average>
Eligible for Scholarship: <True/False>
```

### 🔹 Scholarship Rule

A student is eligible if:

* average marks greater than 75 AND
* age less than 25

You must use:

✅ Variables
✅ Data types
✅ Arithmetic operators
✅ Logical operators
✅ Expressions
✅ input() and print()

---

## ✅ **Expected Answer (Reference Solution)**

```python
name = input("Enter student name: ")
age = int(input("Enter age: "))

mark1 = float(input("Enter subject 1 marks: "))
mark2 = float(input("Enter subject 2 marks: "))

total = mark1 + mark2
average = total / 2

eligible = average > 75 and age < 25

print("Hello", name)
print("Age next year:", age + 1)
print("Total Marks:", total)
print("Average Marks:", average)
print("Eligible for Scholarship:", eligible)
```

---