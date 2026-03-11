## Title

DSA: Python Lists and Functions for Modular Programming

## Objective

Understand how to work with Python lists and use functions to improve code organization and reusability.

## Topics & Subtopics Covered

- **Lists**
  - Defining and manipulating Python lists
  - Using essential list methods
  - Performing traversals and indexing (positive indexing, negative indexing, slicing)
- **Functions**
  - Defining functions using `def`
  - Understanding the need for functions
  - Using parameters and return values to write modular and reusable programs

---

# 📝 MCQs — Lists & Functions (with Explanations)

---

## Q1. Which of the following correctly defines a list in Python?

A) `marks = (10, 20, 30)`  
B) `marks = {10, 20, 30}`  
C) `marks = [10, 20, 30]`  
D) `marks = <10, 20, 30>`  

✅ **Correct Answer:** C  

**Explanation:**  
Lists use square brackets `[]`. Option A is a tuple, B is a set.

---

## Q2. What will be the output?

```python
fruits = ["apple", "banana", "mango"]
print(fruits[1])
```

A) `apple`  
B) `banana`  
C) `mango`  
D) Error  

✅ **Correct Answer:** B  

**Explanation:**  
Indexing starts from 0, so index 1 is `"banana"`.

---

## Q3. What does the following code print?

```python
numbers = [10, 20, 30, 40, 50]
print(numbers[1:4])
```

A) `[10, 20, 30]`  
B) `[20, 30, 40]`  
C) `[30, 40, 50]`  
D) `[10, 20, 30, 40]`  

✅ **Correct Answer:** B  

**Explanation:**  
Slice `[1:4]` includes indexes 1, 2, 3 → values 20, 30, 40.

---

## Q4. Which method adds an element to the **end** of a list?

A) `add()`  
B) `push()`  
C) `append()`  
D) `insert()`  

✅ **Correct Answer:** C  

**Explanation:**  
`append(x)` adds `x` at the end. `insert(i, x)` inserts at index `i`.

---

## Q5. Identify the correct function definition and call.

A)

```python
def add(a, b):
    print(a + b)

add(2, 3)
```

B)

```python
def add(a, b)
    return a + b

add(2, 3)
```

C)

```python
def add(a, b):
    return a + b

add 2, 3
```

D)

```python
add(a, b):
    return a + b
```

✅ **Correct Answer:** A  

**Explanation:**  
Option A has correct syntax and call. B is missing colon after parameters, C is missing parentheses in function call, D has no `def`.

---

## Q6. What is the output of this code?

```python
def square(x):
    return x * x

print(square(3))
```

A) `3`  
B) `6`  
C) `9`  
D) Error  

✅ **Correct Answer:** C  

**Explanation:**  
`square(3)` returns `3 * 3` which is `9`.

---

## Q7. What will be printed?

```python
numbers = [1, 2, 3]
numbers.append(4)
print(len(numbers))
```

A) 3  
B) 4  
C) 5  
D) Error  

✅ **Correct Answer:** B  

**Explanation:**  
List starts with 3 elements, `append(4)` adds one more → length becomes 4.

---

## Q8. Which statement about functions is TRUE?

A) A function can only return integers.  
B) A function cannot take parameters.  
C) A function can be called multiple times with different arguments.  
D) A function must print its result instead of returning it.  

✅ **Correct Answer:** C  

**Explanation:**  
Functions are reusable; they can be called multiple times with different values.

---

# ✍️ Subjective / Coding Question (Medium–Hard)

## Q9. Product Inventory Using Lists and Functions

### 🔹 Problem Statement

Write a Python program that manages a small inventory of products using **lists and functions**.

Your program should:

1. Use **two lists**:
   - `product_names` — list of product names (strings)  
   - `product_prices` — list of corresponding prices (floats)  
2. Provide the following functions:
   - `add_product(name, price)` → adds a product to both lists  
   - `total_value()` → returns the sum of all product prices  
   - `find_most_expensive()` → returns the name and price of the costliest product  
3. In the main program:
   - Ask user how many products they want to add.  
   - Take name and price for each product.  
   - Call the functions above.  
   - Print:

```text
Total products: <count>
Total inventory value: <total_value>
Most expensive: <name> (<price>)
```

---

## ✅ Reference Solution (For Instructor / Review)

```python
product_names = []
product_prices = []


def add_product(name, price):
    product_names.append(name)
    product_prices.append(price)


def total_value():
    total = 0
    for p in product_prices:
        total = total + p
    return total


def find_most_expensive():
    if len(product_prices) == 0:
        return None, 0

    max_price = product_prices[0]
    max_index = 0

    for i in range(1, len(product_prices)):
        if product_prices[i] > max_price:
            max_price = product_prices[i]
            max_index = i

    return product_names[max_index], max_price


n = int(input("How many products? "))

for i in range(1, n + 1):
    name = input(f"Enter name of product {i}: ")
    price = float(input(f"Enter price of product {i}: "))
    add_product(name, price)

total = total_value()
name, price = find_most_expensive()

print("Total products:", len(product_names))
print("Total inventory value:", total)
print("Most expensive:", name, "(", price, ")")
```

---

