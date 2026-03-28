## LO–9.1.19: Writing while loops for repeated execution (25 minutes)

### Write While Loops

### CS Theory Bite
> **Analogy**: A `while` loop is like a **treadmill** — you keep running as long as the condition (your energy) holds True.
> **Why it matters**: While loops handle situations where you don't know how many iterations are needed — user input validation, searching, etc.

### Hook
**Say**: "You're trying to guess a password. You keep trying WHILE you haven't guessed it. That's a while loop - repeating code until a condition becomes False!"

```python
attempts = 0
max_attempts = 3

while attempts < max_attempts:
    print(f"Attempt {attempts + 1}")
    attempts += 1

print("Done!")
```

### While Loop Basics
**Say**: "While loops repeat code AS LONG AS a condition is True."

**Structure**:
```python
while condition:
    # Code to repeat
    # Must update variables!
```

**Simple countdown**:
```python
count = 5

while count > 0:
    print(count)
    count -= 1  # MUST update or infinite loop!

print("Blastoff!")
```

**Key Points**:
- Condition checked BEFORE each iteration
- If condition starts False, code never runs
- MUST update variables or loop runs forever
- Use when you don't know how many times to loop

### Input Validation Pattern
**Say**: "While loops are perfect for getting valid input from users!"

```python
age = int(input("Enter your age (0-120): "))

while age < 0 or age > 120:
    print("Invalid! Must be 0-120")
    age = int(input("Try again: "))

print(f"Valid age: {age}")
```

### Advanced Examples
**Say**: "Let's build a number guessing game!"

```python
secret = 42
guess = int(input("Guess my number (1-100): "))

while guess != secret:
    if guess < secret:
        print("Too low!")
    else:
        print("Too high!")
    guess = int(input("Guess again: "))

print("Correct! You win!")
```

### Common Patterns
**Accumulator pattern**:
```python
total = 0
num = int(input("Enter number (0 to stop): "))

while num != 0:
    total += num
    num = int(input("Enter number (0 to stop): "))

print(f"Total: {total}")
```

**Counter pattern**:
```python
count = 0
num = 1

while num <= 100:
    count += 1
    num += 1

print(f"Counted to 100: {count} times")
```

### Practice
New Year countdown:
```python
count = 10

while count > 0:
    print(count)
    count -= 1

print("Happy New Year!")
```

---

## LO–9.1.20: Controlling while loops using break statement (25 minutes)

### Control Loops with Break

### CS Theory Bite
> **Origin**: `break` implements **early termination** — exiting a loop before its natural end using **sentinel values**.
> **Analogy**: `break` is like pulling the **emergency brake** — the loop stops immediately.
> **Why it matters**: `break` keeps code clean by avoiding complex boolean flags for loop exit.

### Hook (3 minutes)
**Say**: "You're searching for your keys. You check rooms one by one. BREAK means you found them - stop searching!"

```python
rooms = ["kitchen", "bedroom", "bathroom", "garage"]

for room in rooms:
    print(f"Checking {room}...")
    if room == "bathroom":
        print("Found keys!")
        break  # Stop searching!

print("Search complete!")
```

### Break Basics (6 minutes)
**Say**: "`break` exits a loop immediately, even if the condition is still True."

```python
count = 0

while count < 10:
    print(count)
    if count == 5:
        print("Breaking out!")
        break  # Exit loop NOW
    count += 1

print("After loop")  # This runs
```

**Key Points**:
- `break` exits the loop immediately
- Skips remaining code in loop
- Continues execution after the loop
- Works in both while and for loops

```python
while True:  # Infinite loop!
    password = input("Enter password: ")
    if password == "secret":
        print("Access granted!")
        break  # Exit the infinite loop
    print("Wrong password")
```

### Break with While Loops (6 minutes)
**Say**: "Perfect for input validation with an escape!"

```python
while True:
    age = int(input("Enter age: "))

    if age < 0:
        print("Age can't be negative!")
        continue  # Skip to next iteration

    if age > 120:
        print("Invalid age!")
        continue

    # Valid age!
    break

print(f"Age accepted: {age}")
```

### Loop-Else Clause (6 minutes)
**Say**: "`else` after a loop runs ONLY if `break` was never called!"

```python
for item in sequence:
    if condition:
        break
else:
    # Runs if NO break occurred
    print("Loop completed normally")
```

**Example**: Finding a prime number
```python
number = 17

for i in range(2, number):
    if number % i == 0:
        print(f"{number} is not prime")
        break
else:
    print(f"{number} is prime!")
```

### Nested Loops with Break (4 minutes)
**Say**: "`break` only exits the INNER loop, not the outer one!"

```python
for i in range(3):
    print(f"Outer loop: {i}")
    for j in range(3):
        print(f"  Inner loop: {j}")
        if j == 1:
            break  # Only breaks INNER loop
    print("Outer continues...")
```

### Practice (3 minutes)
Find first negative number:
```python
numbers = [5, 10, -3, 8, 12]

for num in numbers:
    if num < 0:
        print(f"First negative: {num}")
        break
else:
    print("No negative numbers found")
```

---

## LO–9.1.21: Controlling while loops using continue statement (25 minutes)

### Control Loops with Continue

### CS Theory Bite
> **Origin**: `continue` implements the **filter pattern** — skip items that don't meet criteria and process only those do. It's the loop equivalent of "next, please!"
> **Analogy**: `continue` is like a **bouncer checking IDs** — "You don't qualify? Skip. Next person." The line keeps moving.
> **Why it matters**: `continue` avoids deep nesting; instead of wrapping code in `if`, just skip unwanted cases.

### Hook
"Imagine looking through a list of students. You want to print everyone EXCEPT those who are absent. That's `continue` - skip some, keep going!"

```python
# WITH continue - skip evens, keep going
for num in range(1, 11):
    if num % 2 == 0:
        continue  # Skip evens
    print(num)  # Only odds print

print("Loop finished!")
# Output: 1 3 5 7 9
```

### Continue Basics
"`continue` skips the rest of the current iteration and jumps to the next one."

```python
while condition:
    # code
    if skip_condition:
        continue  # Jump to next iteration NOW
    # This code is skipped if continue executes
    # more code
```

**Key Points**:
- `continue` skips remaining code in current iteration.
- Loop continues with next iteration.
- Different from `break` which exits the loop entirely.
- Works in both `while` and `for` loops.

```python
count = 0

while count < 5:
    count += 1
    if count == 3:
        continue  # Skip when count is 3
    print(count)

# Output: 1 2 4 5 (3 is skipped!)
```

### Continue vs Break
"`break` EXITS the loop. `continue` SKIPS to the next iteration."

```python
print("=== CONTINUE Demo ===")
for i in range(1, 6):
    if i == 3:
        continue  # Skip 3, keep going
    print(i)
# Output: 1 2 4 5

print("\n=== BREAK Demo ===")
for i in range(1, 6):
    if i == 3:
        break  # Stop at 3
    print(i)
# Output: 1 2
```

**Real-World**: Processing data with errors.

```python
scores = [85, 92, -1, 78, -1, 95]  # -1 means error

total = 0
count = 0

for score in scores:
    if score < 0:
        continue  # Skip error values
    total += score
    count += 1

average = total / count
print(f"Average: {average}")  # 87.5
```

### Common Patterns
"Use `continue` to filter out unwanted items!"

**Skip invalid data**:
```python
temperatures = [22.5, -999, 23.1, -999, 21.8, 24.2]
valid_temps = []

for temp in temperatures:
    if temp == -999:  # Error reading
        continue
    valid_temps.append(temp)

avg = sum(valid_temps) / len(valid_temps)
print(f"Average temp: {avg:.1f}°C")
```

**Process only certain items**:
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Print only multiples of 3
for num in numbers:
    if num % 3 != 0:
        continue  # Skip non-multiples
    print(num)  # 3, 6, 9
```

### Input Validation with Continue
```python
while True:
    age = int(input("Enter age: "))

    if age < 0:
        print("Age can't be negative!")
        continue  # Ask again

    if age > 120:
        print("Invalid age!")
        continue  # Ask again

    # Valid age!
    break

print(f"Age accepted: {age}")
```

### Practice
Print words longer than 3 characters:

```python
words = ["cat", "elephant", "dog", "giraffe", "ant", "bear"]

for word in words:
    if len(word) <= 3:
        continue  # Skip short words
    print(word)

# Output: elephant, giraffe, bear
```

---

## LO–9.1.22: Writing for loops to iterate over sequences (20 minutes)

### CS Theory Bite

> **Origin**: Python's `for` loop is a 'for-each' loop, implementing the **iterator pattern**. It iterates over any iterable object.
>
> **Analogy**: A `for` loop is like a **conveyor belt** — items come one at a time, you process each, and it automatically stops at the end.
>
> **Why it matters**: For loops are the most common loop — 90% of iteration tasks are "do something for each item."


### Hook

**Say**: "Want to print every student's name? You could copy-paste print() 30 times... or use a for loop and do it in 2 lines!"

```python
# With for loop (clean!)
fruits = ["apple", "banana", "orange"]
for fruit in fruits:
    print(fruit)
```

### For Loop Basics

**Say**: "for loops automatically iterate through each item in a sequence."

```python
for item in sequence:
    # Do something with item
    # item is a variable holding current element
```

**Examples**: 
```python
# List
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    print(num)
```

**Key Points**:
- No manual indexing needed
- Works with lists, strings, tuples, sets, dictionaries
- Loop variable gets each item automatically
- Can't modify the original list during iteration

### Dictionary Iteration

**Say**: "Dictionaries are different - you can iterate keys, values, or both!"

```python
student = {
    "name": "Alice",
    "age": 20,
    "grade": "A"
}

# Keys only (default)
for key in student:
    print(key)

# Values only
for value in student.values():
    print(value)

# Both keys and values
for key, value in student.items():
    print(f"{key}: {value}")
```

### Using Enumerate

**Say**: "Need the index too? Use enumerate()!"

```python
colors = ["red", "green", "blue"]

for index, color in enumerate(colors):
    print(f"Position {index}: {color}")

# Output:
# Position 0: red
# Position 1: green
# Position 2: blue
```

### Practice

Sum all numbers in a list:
```python
numbers = [5, 10, 15, 20, 25]

total = 0
for num in numbers:
    total += num

print(f"Sum: {total}")
```

---

## LO–9.1.23: Using range function for numeric iteration (15 minutes)

### Use Range() Function

### CS Theory Bite
`range()` uses **lazy evaluation** — it doesn't create a list, but generates numbers on demand. `range(1000000)` uses almost no memory! It's like a **number dispenser at a deli counter** — giving you the next number when asked. `range()` is key for efficient numeric iteration.

### Hook
Need to count 1 to 10? You could type them out, or use `range()` to do it automatically!

```python
# Manual way (tedious!)
for num in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    print(num)

# Range way (automatic!)
for num in range(1, 11):
    print(num)
```

### Range Basics
`range()` generates numbers on the fly. Three ways to use it!

```python
# range(stop) - starts at 0
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# range(start, stop) - custom start
for i in range(1, 6):
    print(i)  # 1, 2, 3, 4, 5

# range(start, stop, step) - custom increment
for i in range(0, 10, 2):
    print(i)  # 0, 2, 4, 6, 8
```

**Key Points**:
- Stop value is NOT included
- Default start is 0
- Default step is 1
- Can count backwards with negative step

```python
# Counting backwards
for i in range(5, 0, -1):
    print(i)  # 5, 4, 3, 2, 1
```

### Common Patterns
Here are the patterns you'll use most!

```python
# Count exactly n times
for i in range(10):
    print("Hello!")  # Prints 10 times

# to n inclusive
n = 5
for i in range(1, n+1):
    print(i)  # 1, 2, 3, 4, 5

# Iterate through indices
fruits = ["apple", "banana", "orange"]
for i in range(len(fruits)):
    print(f"{i}: {fruits[i]}")

# Odd numbers
for num in range(1, 21, 2):
    print(num)  # 1, 3, 5, ..., 19

# Even numbers
for num in range(0, 21, 2):
    print(num)  # 0, 2, 4, ..., 20
```

### Real-World Application
**Multiplication table**:
```python
n = 7

print(f"Multiplication table for {n}:")
for i in range(1, 11):
    print(f"{n} × {i} = {n * i}")
```

### Practice
Sum numbers 1 to 50:

```python
total = 0
for num in range(1, 51):
    total += num

print(f"Sum: {total}")  # 1275
```

---

## LO–9.1.24: Controlling for loops with break and continue (20 minutes)

### Control For Loops with Break and Continue

### CS Theory Bite
> **Origin**: `break` and `continue` in for loops enable **early termination optimization** — stop searching once found. This turns O(n) worst-case into O(1) best-case.
>
> **Analogy**: Like **searching a phone book** — once you find the name (break), you stop flipping pages. Or skip entries that aren't relevant (continue).
>
> **Why it matters**: Efficient loops don't do unnecessary work — `break` and `continue` are optimization tools.

### Hook (3 minutes)
Imagine you're looking for your keys. When you find them, you BREAK - stop searching. If a drawer is locked? You CONTINUE - skip it and check the next one!

```python
drawers = ["socks", "locked", "books", "keys", "clothes"]

for drawer in drawers:
    if drawer == "locked":
        print(f"Skipping {drawer}")
        continue  # Skip this one
    if drawer == "keys":
        print("Found keys!")
        break  # Stop searching!
    print(f"Checking {drawer}...")

# Output:
# Checking socks...
# Skipping locked
# Checking books...
# Found keys!
```

### The Break Statement (5 minutes)
`break` exits the loop immediately.

```python
for item in collection:
    if condition:
        break  # Exit loop NOW
    # This code runs only if break doesn't execute
```

**Example - Find first even number**:
```python
numbers = [1, 3, 5, 8, 9, 11, 13]

for num in numbers:
    if num % 2 == 0:
        print(f"Found first even: {num}")
        break
    print(f"Checking {num}...")

# Output:
# Checking 1...
# Checking 3...
# Checking 5...
# Found first even: 8
```

### The Continue Statement (5 minutes)
`continue` skips the rest of this iteration and moves to the next.

```python
for item in collection:
    if condition:
        continue  # Skip rest, go to next
    # This code is skipped when continue executes
```

**Example - Print only positive numbers**:
```python
numbers = [5, -2, 10, -7, 3, 8]

for num in numbers:
    if num < 0:
        continue  # Skip negatives
    print(num)

# Output: 5 10 3 8
```

### Break vs Continue (3 minutes)
```python
# BREAK - Exits the entire loop
for i in range(5):
    if i == 3:
        break
    print(i)
# Output: 0 1 2

# CONTINUE - Skips to next iteration
for i in range(5):
    if i == 3:
        continue
    print(i)
# Output: 0 1 2 4 (3 is skipped!)
```

### The For-Else Clause (4 minutes)
`else` after a loop runs ONLY if `break` was never called!

```python
for item in collection:
    if condition:
        break
else:
    # Runs if NO break occurred
    print("Loop completed normally")
```

**Example - Search with feedback**:
```python
numbers = [1, 3, 5, 7, 9]
target = 4

for num in numbers:
    if num == target:
        print(f"Found {target}!")
        break
else:
    print(f"{target} not in list")

# Output: 4 not in list
```