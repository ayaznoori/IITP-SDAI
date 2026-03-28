## Write While Loops

While loops introduce **iteration** - the ability to repeat code automatically. This is fundamental for dynamic program behavior.

---

<div align="center">

<div style="overflow: hidden; display: inline-block;">
  <img src="https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.19.png" 
       style="margin-top: -40px; display: block;" 
       alt="Python while Loop Flowchart">
</div>

*A while loop checks its condition before each iteration — if True, the body executes; if False, the loop exits*

</div>
---

### Real-World Analogy
While loops are like "while" instructions:
- "While the laundry is wet, keep drying"
- "While there are dirty dishes, keep washing"

The action repeats until the condition changes.

## While Loops
A while loop repeats code **as long as** a condition is True.

### Basic Syntax
```python
while condition:
    # Code to repeat
    # Must eventually make condition False!
```

## Simple Examples

### Example 1: Count to 5
```python
count = 1

while count <= 5:
    print(count)
    count += 1
```

### Example 2: User Input Validation
```python
password = ""

while len(password) < 8:
    password = input("Enter password (min 8 chars): ")

print("Password accepted!")
```

## Infinite Loops
**Warning**: If condition never becomes False, loop runs forever!

```python
# Infinite loop - BAD!
count = 1
while count <= 5:
    print(count)
    # Forgot to increment count!

# Correct
count = 1
while count <= 5:
    print(count)
    count += 1  # Makes condition eventually False
```

## While with Break
```python
while True:  # Infinite loop
    password = input("Enter password: ")
    if password == "secret":
        break  # Exit loop
    print("Wrong password!")
```

## Real-World Applications

### Number Guessing Game
```python
secret = 42
guess = 0

while guess != secret:
    guess = int(input("Guess the number: "))
    if guess < secret:
        print("Too low!")
    elif guess > secret:
        print("Too high!")

print("Correct!")
```

## Key Takeaways
1.  **Repeats while True**: Loop continues as long as condition is True
2.  **Must update**: Condition must eventually become False
3.  **Check before run**: Condition checked before each iteration
4.  **Infinite loops**: Dangerous if condition never becomes False
5.  **Use for validation**: Great for user input validation

---

## Control While Loops with Break

The `break` statement provides an **emergency exit** from loops — stopping immediately when a special condition is met.

---

<div align="center">

<div style="overflow: hidden; display: inline-block;">
  <img src="https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.20.png" 
       style="margin-top: -50px; display: block;" 
       alt="Python break Statement Flowchart">
</div>

*The break statement exits the loop immediately — jumping straight past the loop body to the next statement*

</div>

---

### Why Break Exists
Sometimes you need to exit a loop for reasons other than the main condition:
- **Search**: Stop when item is found.
- **Validation**: Stop on first error.
- **User quit**: Stop immediately when user says "quit".

`break` simplifies logic that would otherwise require complex condition flags.

### Real-World Analogy
`break` is like **"stop the search!"**: Looking for keys: **Found them? BREAK - stop searching!**

## The Break Statement
`break` immediately exits the current loop, regardless of the loop condition.

### Basic Syntax
```python
while condition:
    # Code
    if some_condition:
        break  # Exit loop immediately
    # More code
```

## Examples

### Example 1: Search and Stop
```python
numbers = [3, 7, 2, 9, 5]
target = 9
index = 0

while index < len(numbers):
    if numbers[index] == target:
        print(f"Found {target} at index {index}")
        break  # Stop searching
    index += 1
```

### Example 2: Exit on Command
```python
while True:
    command = input("Enter command (or 'quit'): ")
    if command == "quit":
        break
    print(f"Executing: {command}")

print("Program ended")
```

## Break vs Condition
```python
# Using break
while True:
    x = int(input("Enter number (0 to stop): "))
    if x == 0:
        break
    print(f"You entered: {x}")

# Using condition
x = -1
while x != 0:
    x = int(input("Enter number (0 to stop): "))
    if x != 0:
        print(f"You entered: {x}")
```

## Key Takeaways
1.  **Immediate exit**: `break` stops loop instantly.
2.  **Skips condition**: Doesn't check loop condition.
3.  **Use for early exit**: When you find what you need.
4.  **Works in `while` and `for`**: `break` works in both loop types.
5.  **One loop only**: Only exits innermost loop (in nested loops).

---

# Control While Loops with Continue

## Introduction
The `continue` statement **skips the rest of the current iteration** and jumps to the next one. Unlike `break` which exits entirely, `continue` moves to the next loop cycle.

---

<div align="center">

![Python continue Statement Flow Diagram](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.21.png)

*The continue statement skips the remaining loop body and jumps back to the condition check for the next iteration*

</div>

---

### Why Use Continue
-   **Filter while processing**: Skip invalid entries but continue with others.
-   **Conditional skipping**: Ignore specific cases without stopping entirely.
-   **Cleaner logic**: Avoid deeply nested `if` statements for filtering.

### Real-World Analogy
`continue` is like **"skip this, next!"** on an assembly line: a defective item is detected, so it's skipped, and the line continues with the next item (the entire line doesn't stop).

## The Continue Statement
`continue` skips the rest of the current iteration and proceeds to the next.

### Basic Syntax
```python
while condition:
    # Code before continue
    if some_condition:
        continue  # Skips remaining code in this iteration
    # Code after continue (skipped if continue runs)
```

## Examples

### Skip Even Numbers
```python
count = 0

while count < 10:
    count += 1
    if count % 2 == 0:
        continue  # Skip even numbers
    print(count)  # Only prints odd numbers

# Output: 1 3 5 7 9
```

### Continue vs Break
-   **`continue`**: Skips the current iteration, loop keeps running.
-   **`break`**: Exits the entire loop immediately.

```python
# continue - skips iteration
count_c = 0 # Using different variable for clarity
while count_c < 5:
    count_c += 1
    if count_c == 3:
        continue  # Skips printing 3, loop continues
    print(count_c)
# Output: 1 2 4 5

# break - exits loop
count_b = 0 # Using different variable for clarity
while count_b < 5:
    count_b += 1
    if count_b == 3:
        break  # Stops loop at 3
    print(count_b)
# Output: 1 2
```

## Key Takeaways
1.  **Skips to next iteration**; does not exit loop.
2.  **Code after `continue` in current iteration is skipped**.
3.  Ideal for **filtering** unwanted items.
4.  Ensure loop control variables (e.g., `count`) are **updated before `continue`** to avoid infinite loops.
5.  Commonly used for **input validation**.

---

For loops enable automatic iteration over collections, processing each item in a sequence.

### Why For Loops Are Revolutionary

While loops require manual counter and index tracking; for loops offer a natural "for each item" approach, making code more readable and Pythonic.

### Real-World Analogy

For loops are like **"for each" instructions**:
- "For each student in the class, mark attendance"
- "For each email in inbox, check if spam"
- "For each item in cart, calculate price"

--- 
<div align="center">

<div style="overflow: hidden; display: inline-block;">
  <img src="https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.22.png" 
       style="margin-bottom: -25px; display: block;" 
       alt="Python for Loop Flowchart Diagram">
</div>

*Flowchart of a for loop: the loop checks the sequence for remaining items, executes the body for each item, and exits when the sequence is exhausted*

</div>

---

A for loop iterates over a sequence (list, string, range, etc.).

### Basic Syntax

```python
for item in sequence:
    # Code using item
```

## Examples

### Example: Iterate List

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(fruit)

# Output:
# apple
# banana
# cherry
```

## For Loop vs While Loop

```python
# For loop - when you know iterations
for i in range(5):
    print(i)

# While loop - when condition-based
count = 0
while count < 5:
    print(count)
    count += 1
```

## Key Takeaways

1. **Iterates sequences**: Works with lists, strings, ranges
2. **Automatic**: No manual counter needed
3. **Cleaner than while**: For known iterations
4. **item variable**: Automatically gets next value
5. **Use when**: You know what to iterate over

---

## Use Range Function

`range()` is a crucial innovation for **numeric iteration**, introducing **lazy generation**. It produces values on-demand, not all at once in memory, making it highly memory-efficient. Python 3's `range()` is always lazy.

**Analogy**: `range()` is like a **ticket dispenser machine**; it prints the next ticket only when you ask, handling billions of numbers without storing them all.

**Lazy Evaluation Example**: This does not create 1 billion numbers in memory:
```python
# This doesn't create 1 billion numbers in memory!
for i in range(1000000000):
    if i == 5:
        break  # Exit early - only 6 numbers ever created
```

<div align="center">

![Python range() Function with for Loop](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.23.jpg)

*The range() function generates numbers on demand, enabling efficient numeric iteration without storing the entire sequence in memory*

</div>

## The Range Function
`range()` generates a sequence of numbers, commonly used with `for` loops.

### Three Forms

```python
range(stop)              # 0 to stop-1
range(start, stop)       # start to stop-1
range(start, stop, step) # start to stop-1, by step
```

## Examples

### Form 1: range(stop)

```python
for i in range(5):
    print(i)
# Output: 0 1 2 3 4
```

### Form 2: range(start, stop)

```python
for i in range(2, 7):
    print(i)
# Output: 2 3 4 5 6
```

### Form 3: range(start, stop, step)

```python
for i in range(0, 10, 2):
    print(i)
# Output: 0 2 4 6 8
```

### Counting Down

```python
for i in range(5, 0, -1):
    print(i)
# Output: 5 4 3 2 1
```

## Real-World Application

### Multiplication Table

```python
num = 5

for i in range(1, 11):
    print(f"{num} x {i} = {num * i}")
```

## Range with Lists

```python
fruits = ["apple", "banana", "cherry"]

# Access by index
for i in range(len(fruits)):
    print(f"{i}: {fruits[i]}")
# Output:
# 0: apple
# 1: banana
# 2: cherry
```

## Key Takeaways

1.  **Generates numbers**: Not a list, but a range object
2.  **Stops before end**: `range(5)` goes 0 to 4, not 5
3.  **Default start**: 0 if not specified
4.  **Default step**: 1 if not specified
5.  **Can count down**: Use negative step

---

# Control For Loops with Break and Continue

## Introduction
`Break` and `continue` enable **flow interruption** in for loops, allowing early exit and selective processing. This addresses **efficiency problems** by preventing loops from running to completion unnecessarily, saving time and making logic cleaner.

### Break vs Continue: Core Concepts
**Break**: "I found what I needed, stop the entire loop now"
- **Analogy**: Searching for keys – once found, stop searching.
- **Use case**: First match, error detection, early termination.

**Continue**: "Skip this one item, but keep going through the rest"
- **Analogy**: Assembly line inspector – defective item? Skip it, inspect next one.
- **Use case**: Filtering, ignoring special cases, validation.

This is **algorithmic efficiency** – doing less work for the same goal.

---

<div align="center">

![Python break vs continue Statement Flowchart](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.24.png)

*The break statement immediately exits the loop when a condition is met, while continue skips the current iteration and proceeds to the next one*

</div>

---

## Break and Continue in For Loops

### Break in For Loop
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

for num in numbers:
    if num == 6:
        break  # Stop at 6
    print(num)

# Output: 1 2 3 4 5
```

### Continue in For Loop
```python
for num in range(1, 11):
    if num % 2 == 0:
        continue  # Skip even numbers
    print(num)

# Output: 1 3 5 7 9
```

## Examples

### Find First Match
```python
names = ["Alice", "Bob", "Charlie", "David"]
target = "Charlie"

for i in range(len(names)):
    if names[i] == target:
        print(f"Found {target} at index {i}")
        break
```

### Filter While Iterating
```python
scores = [95, 45, 87, 32, 91, 68]

for score in scores:
    if score < 60:
        continue  # Skip failing scores
    print(f"Passing score: {score}")

# Output: Passing score: 95, 87, 91, 68
```

### Early Exit on Error
```python
data = [10, 20, 0, 40, 50]

for num in data:
    if num == 0:
        print("Error: Zero found!")
        break
    result = 100 / num
    print(f"100 / {num} = {result}")
```

## Nested Loops with Break
```python
# Break only exits innermost loop
for i in range(3):
    for j in range(3):
        if j == 2:
            break  # Exits inner loop only
        print(f"i={i}, j={j}")
```

## Key Takeaways
1. **break**: Exits for loop immediately
2. **continue**: Skips to next iteration
3. **Same as while**: Works identically
4. **Use break for**: Search, early exit
5. **Use continue for**: Filtering, skipping