# Writing While Loops

## What You'll Learn
This lesson teaches how to use while loops to repeat code as long as a condition remains True, a fundamental tool for repetitive tasks.

## Why This Matters
Loops automate repetitive tasks. Imagine building:
- A password checker that keeps asking until correct.
- A game that runs until the player loses.
- A menu system that repeats until the user chooses to quit.

## What is a While Loop?
A **while loop** repeats a block of code **as long as** a condition is True. It checks the condition **before** each repetition.

```python
while condition:
    # Code to repeat
```

---

## Simple Example: Counting

```python
count = 1

while count <= 5:
    print(count)
    count = count + 1

print("Done!")
```
**How it works:** The loop prints `count` and increments it until `count` is no longer `<= 5`.

---

## Real-World Example: Password Validation

```python
password = ""

while len(password) < 8:
    password = input("Enter password (min 8 characters): ")
    if len(password) < 8:
        print("Too short! Try again.")

print("Password accepted!")
```
**How this works:** The loop keeps asking for a password **while** its length is less than 8 characters.

---

## The Critical Part: Updating the Condition

**WARNING:** The condition must eventually become False, or your loop will run forever!

### Infinite Loop (BAD)
```python
count = 1

while count <= 5:
    print(count)
    # Forgot to change count! Loop never stops!
```

### Correct Loop (GOOD)
```python
count = 1

while count <= 5:
    print(count)
    count = count + 1  # This makes condition eventually False!
```

---

## Another Example: Game Menu

```python
choice = ""

while choice != "quit":
    print("\n--- Game Menu ---")
    print("1. Start Game")
    print("2. View Score")
    print("Type 'quit' to exit")

    choice = input("Your choice: ")

    if choice == "1":
        print("Starting game...")
    elif choice == "2":
        print("Your score: 100")
    elif choice == "quit":
        print("Thanks for playing!")
    else:
        print("Invalid choice")
```
This keeps showing the menu until the user types "quit".

---

## While Loops vs If Statements

### If Statement (Runs Once)
```python
score = 50

if score < 100:
    print("Keep playing")  # Prints once
```

### While Loop (Repeats)
```python
score = 50

while score < 100:
    print("Keep playing")  # Prints repeatedly
    score = score + 10     # Must update score!
```
**Key difference:** `if` checks once; `while` checks repeatedly and keeps running.

---

## Try It Yourself (Before Class)

Run this code and observe the output:

```python
number = 1

while number <= 10:
    if number % 2 == 0:
        print(f"{number} is even")
    else:
        print(f"{number} is odd")
    number = number + 1

print("Finished checking numbers!")
```

**Try modifying:**
1.  Change `number <= 10` to `number <= 3` - what happens?
2.  Change `number = number + 1` to `number = number + 2` - what happens?
3.  Comment out the `number = number + 1` line - what happens? (Stop it with Ctrl+C!)

---

## Common While Loop Patterns

### Pattern 1: Counting
```python
count = 0
while count < 5:
    print("Hello")
    count += 1
```

### Pattern 2: User Input Until Valid
```python
age = -1
while age < 0 or age > 120:
    age = int(input("Enter your age (0-120): "))
```

### Pattern 3: Keep Running Until Command
```python
command = ""
while command != "stop":
    command = input("Enter command (type 'stop' to quit): ")
    print(f"You entered: {command}")
```

---

## Warning: Infinite Loops
If you accidentally create an infinite loop, your program will hang. Press `Ctrl + C` (or `Cmd + C` on Mac) to stop it.

---

## Key Takeaways
Before class, remember:
1.  **While loops repeat** - as long as condition is True
2.  **Condition checked first** - before each iteration
3.  **Must update** - something in the loop must make condition False eventually
4.  **Infinite loops are bad** - always make sure your condition can become False
5.  **Great for user input** - when you don't know how many times to repeat

## Questions to Think About
- When would you use a while loop instead of just writing code multiple times?
- What happens if the condition is False from the start?
- How do you decide what condition to use for a while loop?

---

## Controlling While Loops Using Break Statement

## What is Break?

`break` lets you **exit a loop immediately**, even if the loop condition is still True. It's like an emergency stop button!

### Simple Analogy
Think of `break` like **"found what I'm looking for, stop searching!"**: Looking through a playlist for a song. **Found it? BREAK - stop scrolling!**

## Why This Matters
Sometimes you need to stop a loop immediately when something specific happens:
- A user types "quit"
- You find what you're searching for
- An error occurs
- A maximum number of attempts is reached

The `break` statement lets you exit the loop instantly, without waiting for the normal loop condition to become False.

---

## What is the Break Statement?

The **break statement** immediately exits the current loop, no matter what the loop condition says.

```python
while condition:
    # Some code
    if some_special_condition:
        break  # Exit loop right now!
    # More code
```

When Python hits `break`, it:
1.  **Stops the loop immediately**
2.  **Jumps to the first line after the loop**
3.  **Doesn't check the loop condition again**

---

## Simple Example: Exit on Command

```python
while True:  # This would normally run forever!
    command = input("Enter command (or 'quit' to exit): ")

    if command == "quit":
        break  # Exit the loop

    print(f"You entered: {command}")

print("Program ended")
```

**Output example:**
Enter command (or 'quit' to exit): hello
You entered: hello
Enter command (or 'quit' to exit): test
You entered: test
Enter command (or 'quit' to exit): quit
Program ended

**Key point:** We used `while True` (infinite loop) but `break` stops it when needed.

---

## When to Use Break

### Use Case 1: Search and Stop

When you're looking for something and want to stop once you find it:

```python
numbers = [3, 7, 2, 9, 5, 11, 8]
target = 9
index = 0

while index < len(numbers):
    if numbers[index] == target:
        print(f"Found {target} at position {index}!")
        break  # No need to keep searching
    index += 1
```

### Use Case 2: Password Attempts

Limit the number of tries:

```python
attempts = 0
max_attempts = 3

while attempts < max_attempts:
    password = input("Enter password: ")

    if password == "secret123":
        print("Access granted!")
        break  # Correct password, exit loop

    attempts += 1
    remaining = max_attempts - attempts
    print(f"Wrong! {remaining} attempts remaining")

if password != "secret123":
    print("Account locked")
```

---

## Try It Yourself (Before Class)

Run this code:

```python
total = 0

while True:
    number = input("Enter a number (or 'done' to finish): ")

    if number == "done":
        break

    total += int(number)
    print(f"Running total: {total}")

print(f"Final total: {total}")
```

**Try:**
1.  Enter a few numbers, then type "done"
2.  What happens if you type "done" immediately?

---

## Key Takeaways

Before class, remember:
1.  **`break` exits immediately** - stops the loop right away
2.  **No condition check** - doesn't wait for normal loop condition
3.  **Great with `while True`** - creates controlled infinite loops
4.  **Common use cases** - search-and-stop, user input validation, menu systems

---

# Controlling While Loops Using Continue Statement

## What is Continue?
`continue` skips the **rest of the current loop iteration** and jumps to the next one. It allows you to skip specific cases while keeping the loop running, unlike `break` which exits the loop entirely.

### Analogy
Think of `continue` like **skipping a song** on a playlist: you don't like the current song, so you skip to the next without stopping the entire playback.

## The Continue Statement

```python
while condition:
    # Code before continue
    if skip_this_iteration:
        continue  # Skips remaining code in this iteration, goes to next
    # Code after continue (skipped if continue runs)
```

**Key difference from break:**
- `break` → **Exit the entire loop**
- `continue` → **Skip to the next iteration**

## Simple Example: Skip Even Numbers
```python
number = 0

while number < 5:
    number += 1
    if number % 2 == 0:
        continue  # Skip even numbers
    print(number)
```
**Output:**
1
3
5

## When to Use Continue
### Use Case: Filtering Input
Use `continue` to skip invalid entries and only process valid ones.

```python
total = 0
count = 0

while count < 5:
    value_str = input("Enter a positive number: ")
    value = int(value_str)

    if value <= 0:
        print("Invalid! Must be positive.")
        continue  # Skip to next iteration, don't count this

    total += value
    count += 1  # Only increment for valid input

print(f"Average: {total / count}")
```

## Break vs Continue

### Break: Stop Entire Loop
```python
for i in range(5):
    if i == 2:
        break  # Exit loop completely
    print(i)
# Output: 0, 1
```

### Continue: Skip One Iteration
```python
for i in range(5):
    if i == 2:
        continue  # Skip only i=2
    print(i)
# Output: 0, 1, 3, 4
```

## Real-World Example: Menu with Validation
```python
while True:
    print("\n1. Add item")
    print("2. View items")
    print("3. Quit")

    choice = input("Choose: ")

    if choice not in ["1", "2", "3"]:
        print("Invalid choice!")
        continue  # Skip rest, show menu again

    if choice == "1":
        print("Adding item...")
    elif choice == "2":
        print("Viewing items...")
    elif choice == "3":
        print("Goodbye!")
        break
```

## Try It Yourself (Before Class)
Run this code:
```python
count = 0

while count < 10:
    count += 1

    if count % 3 == 0:
        continue

    print(count)
```
**Questions:**
1.  What numbers get printed?
2.  What numbers are skipped?
3.  Try changing `count % 3 == 0` to `count == 5` - what happens?

## Key Takeaways
1.  `continue` skips to the next iteration; doesn't exit loop.
2.  Code after `continue` in the current iteration is skipped.
3.  Loop keeps running (unlike `break`).
4.  Great for filtering unwanted cases.
5.  Loop condition still checked after each iteration.

---

For loops automatically process each item in a collection.

### Simple Analogy

Think of for loops like **going through a checklist**:
- "For each task on my to-do list, mark it complete"
- "For each friend, send birthday message"
- "For each photo, apply filter"

### Why This Matters

For loops are one of the most commonly used features in Python for tasks like:
- Processing every student in a class list
- Checking every character in a password
- Calculating the total of all prices in a shopping cart
- Displaying every item in a menu

---

A **for loop** automatically iterates over each item in a sequence (list, string, range, etc.) and runs code for each item.

```python
for item in sequence:
    # Do something with item
```

**Key differences from while loops:**
- **While loop**: "Keep going while condition is True" (you control when it stops)
- **For loop**: "Go through each item in this sequence" (automatic iteration)

---

## Simple Examples

### Example 1: Iterate Over a List

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(fruit)
```

**Output:**
apple
banana
cherry

**What happens:**
1. Take first item ("apple") and run the code
2. Take second item ("banana") and run the code
3. Take third item ("cherry") and run the code
4. No more items → stop

### Example 2: Iterate Over a String

```python
word = "Python"

for letter in word:
    print(letter)
```

**Output:**
P
y
t
h
o
n

---

## For Loops vs While Loops

### Using While (More Code)

```python
fruits = ["apple", "banana", "cherry"]
index = 0

while index < len(fruits):
    print(fruits[index])
    index += 1
```

### Using For (Cleaner)

```python
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(fruit)
```

**For loops are better when:**
- You need to process every item in a sequence
- You don't need the index
- You want cleaner, more readable code

---

## Real-World Examples

### Example 1: Calculate Total Price

```python
prices = [19.99, 5.50, 12.00, 8.75]
total = 0

for price in prices:
    total += price

print(f"Total: ${total}")
```

---

## Try It Yourself (Before Class)

Run this code:

```python
numbers = [10, 20, 30, 40, 50]
doubled = []

for num in numbers:
    doubled.append(num * 2)

print(doubled)
```

**Questions:**
1. What does this code do?
2. What will be in the `doubled` list?
3. Try changing `num * 2` to `num + 5` - what happens?

---

## Key Takeaways

Before class, remember:
1. **For loops iterate sequences** - automatically go through each item
2. **Cleaner than while** - for sequences you know the length of
3. **Works with many types** - lists, strings, ranges, tuples, etc.
4. **Loop variable** - gets each item automatically
5. **No index needed** - unless you specifically want it

---

## Using Range() Function for Numeric Iteration

`range()` is Python's **smart number generator**. It uses **lazy generation**, creating numbers one-by-one as needed, not all at once. This makes it memory-efficient, even for billions of numbers.

**Analogy**: Think of `range()` like a **number counter at a deli** – it dispenses numbers on-demand, saving space by not printing all tickets upfront.

## Three Ways to Use range()

`range()` generates a sequence of numbers:

**1. range(stop)** - Count from 0 to `stop-1`

```python
for i in range(5):
    print(i)
# Output: 0, 1, 2, 3, 4
```

**2. range(start, stop)** - Count from `start` to `stop-1`

```python
for i in range(2, 6):
    print(i)
# Output: 2, 3, 4, 5
```

**3. range(start, stop, step)** - Count with custom increment

```python
for i in range(0, 10, 2):
    print(i)
# Output: 0, 2, 4, 6, 8
```

## Important: range() Stops BEFORE the End

The `stop` value is **exclusive** (not included):

```python
range(5)      # 0, 1, 2, 3, 4
range(1, 4)   # 1, 2, 3
range(0, 10, 2) # 0, 2, 4, 6, 8
```
This is convenient for working with 0-indexed lists.

## Common Patterns

### Pattern 1: Repeat N Times

```python
# Print "Hello" 5 times
for i in range(5):
    print("Hello")
```

### Pattern 2: Count from 1 to N

```python
# Count 1 to 10
for i in range(1, 11):
    print(i)
```

### Pattern 3: Even Numbers

```python
# Print even numbers from 0 to 10
for i in range(0, 11, 2):
    print(i)
# Output: 0, 2, 4, 6, 8, 10
```

### Pattern 4: Countdown

```python
# Countdown from 10 to 1
for i in range(10, 0, -1):
    print(i)
print("Blast off!")
```

## Real-World Examples

### Example 1: Multiplication Table

```python
number = 5

for i in range(1, 11):
    print(f"{number} x {i} = {number * i}")
```

### Example 2: Access List by Index

```python
fruits = ["apple", "banana", "cherry"]

for i in range(len(fruits)):
    print(f"Item {i}: {fruits[i]}")
# Output:
# Item 0: apple
# Item 1: banana
# Item 2: cherry
```

## Try It Yourself (Before Class)

Run this code:

```python
for i in range(1, 6):
    print("*" * i)
```

**Questions:**
1. What pattern does this create?
2. Try changing `range(1, 6)` to `range(5, 0, -1)` - what happens?
3. Change `"*" * i` to `str(i) * i` - what do you get?

## Key Takeaways

Before class, remember:
1.  **range(n)** - 0 to n-1
2.  **range(start, stop)** - start to stop-1
3.  **range(start, stop, step)** - custom increment
4.  **Stop is exclusive** - never includes the stop value
5.  **Efficient** - doesn't create a list in memory

---

# Controlling For Loops with Break and Continue

## Why Control Flow in For Loops?
For loops need **intelligent iteration** for tasks like searching (stop when found), filtering (skip unwanted items), and validation (exit on error). `break` and `continue` provide this control.

### Simple Analogy
`break` and `continue` are like **instructions to a mail carrier**:
- **Break**: "Stop delivering mail on this street"
- **Continue**: "Skip this house but keep delivering to others"
The mail carrier adapts based on conditions!

## What You'll Learn
You'll learn how to use `break` and `continue` with for loops to control execution, similar to while loops.

## Why This Matters
You often need to exit loops early, skip items, or handle special cases, and `break`/`continue` provide this fine control.

---

## Quick Recap
**break** → Exit the loop completely
**continue** → Skip to the next iteration

These work identically in both while and for loops!

---

## Using Break in For Loops

### Example 1: Search and Stop
```python
numbers = [3, 7, 12, 9, 15, 2]

for num in numbers:
    if num > 10:
        print(f"Found number greater than 10: {num}")
        break  # Stop searching
    print(f"Checking {num}...")

# Output:
# Checking 3...
# Checking 7...
# Found number greater than 10: 12
```

---

## Using Continue in For Loops

### Example 1: Skip Specific Items
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

for num in numbers:
    if num % 2 == 0:
        continue  # Skip even numbers
    print(num)

# Output: 1, 3, 5, 7, 9 (only odd numbers)
```

---

## Combining Break and Continue
```python
numbers = [5, -2, 10, -7, 15, 20, -3]

for num in numbers:
    if num < 0:
        continue  # Skip negative numbers

    if num > 15:
        print(f"Found large number: {num}")
        break  # Stop at first number > 15

    print(num)

# Output:
# 5
# 10
# 15
# Found large number: 20
```

---

## Key Takeaways
1. **break**: exits the loop completely
2. **continue**: skips to next item
3. **Same behavior**: identical in for and while loops
4. **Common patterns**: search-and-stop, filtering, validation
5. **Can combine**: use both for complex logic