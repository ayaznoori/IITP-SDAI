## Session Number

2.2

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

# 1️⃣ Python Lists — A Flexible Data Structure

A **list** is an ordered collection of items.

Examples:

```python
marks = [70, 85, 90]
names = ["Ali", "Riya", "Sam"]
mixed = [10, "Python", True]
```

Key properties:

- Ordered (items have positions / indexes)  
- Mutable (you can change elements)  
- Can store different data types  

---

# 2️⃣ Creating and Accessing List Elements

### Creating Lists

```python
empty_list = []
numbers = [1, 2, 3, 4]
fruits = ["apple", "banana", "mango"]
```

### Positive Indexing

Indexes start from 0.

```python
fruits = ["apple", "banana", "mango"]

print(fruits[0])  # "apple"
print(fruits[1])  # "banana"
print(fruits[2])  # "mango"
```

### Negative Indexing

Negative indexes count from the **end**:

```python
print(fruits[-1])  # "mango"
print(fruits[-2])  # "banana"
```

---

# 3️⃣ Slicing — Getting Sub-Lists

Syntax:

```python
list[start:end]  # end is excluded
```

Example:

```python
numbers = [10, 20, 30, 40, 50]

print(numbers[1:4])   # [20, 30, 40]
print(numbers[:3])    # [10, 20, 30]
print(numbers[2:])    # [30, 40, 50]
print(numbers[-3:])   # [30, 40, 50]
```

Slicing helps when you need part of the list only.

---

# 4️⃣ Essential List Methods

Some commonly used list methods:

```python
items.append(x)   # add at end
items.insert(i,x) # insert at index i
items.remove(x)   # remove first occurrence of x
items.pop()       # remove last item
len(items)        # length of list
```

### Example: Append and Insert

```python
fruits = ["apple", "banana"]
fruits.append("mango")
fruits.insert(1, "orange")

print(fruits)  # ["apple", "orange", "banana", "mango"]
```

### Example: Remove and Pop

```python
numbers = [1, 2, 3, 4]
numbers.remove(2)   # removes value 2
last = numbers.pop()  # removes 4

print(numbers)  # [1, 3]
print(last)     # 4
```

---

# 5️⃣ Traversing Lists — Looping Through Elements

We often **traverse** a list using loops.

### Using `for` Loop

```python
marks = [70, 85, 90]

for m in marks:
    print(m)
```

### Using Index With `range`

```python
for i in range(len(marks)):
    print("Index:", i, "Value:", marks[i])
```

Use this pattern when:

- You need both index and value  
- You want to modify elements at specific positions  

---

# 6️⃣ Introduction to Functions — Writing Reusable Code

A **function** is a block of code that performs a specific task.

We define functions using `def`.

### Basic Function

```python
def greet():
    print("Hello from a function")

greet()
```

Benefits:

- Avoid repeating code  
- Improve organization  
- Make programs easier to test and debug  

---

# 7️⃣ Functions With Parameters and Return Values

### Parameters — Input To Functions

```python
def greet(name):
    print("Hello", name)

greet("Ali")
greet("Riya")
```

### Return Values — Output From Functions

```python
def add(a, b):
    result = a + b
    return result

sum_value = add(3, 5)
print(sum_value)
```

Key Ideas:

- `return` sends a value back to where the function was called  
- A function can be used inside expressions: `add(2, 3) * 10`  

---

# 8️⃣ Combining Lists and Functions — Modular Programming

We can write functions that **work on lists**.

### Example: Calculate Average of a List

```python
def average(numbers):
    total = 0
    for n in numbers:
        total = total + n

    if len(numbers) == 0:
        return 0

    return total / len(numbers)

marks = [80, 90, 70]
avg = average(marks)
print("Average:", avg)
```

This demonstrates:

- Traversal of a list  
- Iterative problem solving  
- Reusable function  

---

### Example: Find Maximum Element

```python
def find_max(numbers):
    if len(numbers) == 0:
        return None

    maximum = numbers[0]
    for n in numbers:
        if n > maximum:
            maximum = n

    return maximum

values = [3, 9, 2, 11, 7]
print("Max:", find_max(values))
```

Shows:

- Use of a loop + condition  
- Updating a variable iteratively  

---

# 9️⃣ Why Functions Help Modularity

**Modular programming** means:

> Breaking a big program into smaller, independent, reusable pieces (functions).

Advantages:

- Easier to read  
- Easier to test (you can test each function separately)  
- Easier to reuse in other programs  

Example structure:

```python
def read_marks():
    # input logic

def calculate_average(marks):
    # processing logic

def print_report(marks, avg):
    # output logic
```

Each function has **one clear responsibility**.

---

# 🔟 Common Mistakes

- Accessing invalid index (e.g., `list[10]` when list has 3 items)  
- Forgetting parentheses `()` when calling a function  
- Using `return` but not capturing the value in a variable  
- Changing a list while looping over it in a confusing way  

---

# ✅ Quick Practice Ideas

1. Create a list of 5 city names and print them using a loop.  
2. Write a function `sum_list(numbers)` that returns the sum of all numbers in a list.  
3. Write a function `count_above_threshold(numbers, threshold)` that returns how many numbers are greater than the given threshold.  


