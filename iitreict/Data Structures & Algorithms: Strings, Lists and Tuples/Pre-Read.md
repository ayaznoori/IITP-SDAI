# Implement String Data Types

## What are Strings?
Strings are text data - any characters in quotes.
```python
name = "Alice"
message = 'Hello, World!'
```
Single `''` or double `""` quotes both work.

### Why Strings Are Different
Computers only understand numbers. Each character ('A') is stored as a number (65). Python translates automatically.
You can't do `"5" + 5` because `"5"` is text, `5` is a number – they're stored differently.

### Simple Analogy
A string is like a **necklace**: each character is a bead, strung together in order. "Hello" is 5 beads: H-e-l-l-o. Order matters.

## String Operations

### Concatenation (Combining)
```python
first = "Hello"
second = "World"
combined = first + " " + second # "Hello World"
```

### Repetition
```python
laugh = "ha" * 3 # "hahaha"
```

### Length
```python
name = "Alice"
print(len(name)) # 5
```

## Special Characters
Use opposite quotes inside strings, or escape with backslash:
```python
message = "He said, 'Hello!'"
message_escaped = "He said, \"Hello!\""
```

## When to Use Strings
Names, messages, emails, URLs, addresses.

## Try It
```python
first_name = "Alice"
last_name = "Johnson"
full_name = first_name + " " + last_name
print(full_name) # Alice Johnson
```

---

## Format Output with F-strings

## What are F-strings?

**F-strings** make it easy to embed variables in strings!

### Old Way (Concatenation)
```python
name = "Alice"
age = 25
message = "My name is " + name + " and I am " + str(age)
```

### New Way (F-strings) ✨
```python
name = "Alice"
age = 25
message = f"My name is {name} and I am {age}"
```
Much cleaner!

### The Problem F-strings Solve

Combining text and variables is painful without f-strings:
```python
name = "Bob"
score = 95
level = 10

# Concatenation - lots of + and str()
msg = "Player " + name + " scored " + str(score) + " at level " + str(level)
# 😩 Hard to read!
```
With f-strings, it's natural:
```python
msg = f"Player {name} scored {score} at level {level}"
# 😊 Clean and obvious!
```

### Simple Analogy

Think of f-strings like **filling in blanks**: "My name is _____ and I'm _____ years old" becomes "My name is Alice and I'm 25 years old".

### Why "F" String?

The `f` stands for **"formatted"** — it tells Python: "This string has special formatting with `{}` placeholders."

Without `f`:
```python
name = "Alice"
print("Hello {name}")  # Prints: Hello {name} (literal)
```
With `f`:
```python
name = "Alice"
print(f"Hello {name}")  # Prints: Hello Alice (filled in!)
```

## F-string Syntax

Put `f` before the string, use `{}` for variables:

```python
score = 95
print(f"You scored {score} points!")
```

## Expressions in F-strings

Can do calculations inside `{}`:

```python
price = 50
quantity = 3
print(f"Total: ${price * quantity}")  # Total: $150
```

## Formatting Numbers

```python
pi = 3.14159265
print(f"Pi is approximately {pi:.2f}")  # Pi is approximately 3.14
```
`.2f` means: 2 decimal places, float.

## Try It
```python
name = "Bob"
age = 30
city = "Boston"
print(f"{name} is {age} years old and lives in {city}")
```

---

## Creating and Initializing Lists in Python

## What Are Lists?
Lists are Python's containers for storing multiple items in order. They can hold anything and grow/shrink as needed.

### Simple Analogy
Think of lists like a **shopping list**:
- **Ordered items**: Milk, eggs, bread.
- **Update anytime**: Add/remove items.
- **One paper**, many items!

### Why Lists Changed Everything
Before lists, programmers needed separate variables for each item:
```python
# score1 = 85, score2 = 92... (unscalable)
```
With lists, it's elegant and scalable:
```python
scores = [85, 92, 78] # Can hold millions!
average = sum(scores) / len(scores)  # Easy!
```

### Key Concepts
- Ordered collection of items.
- Created with square brackets `[]`.
- Items separated by commas.
- Can hold any data type.

**Simple Example:**
```python
fruits = ['apple', 'banana', 'orange']
print(fruits[0]) # apple (access by position)
print(len(fruits)) # 3 (get length)
```

### Quick Examples
**Empty List:**
```python
cart = []
```
**With Values:**
```python
numbers = [1, 2, 3]
names = ['Alice', 'Bob']
mixed = ['text', 42, True]
```
**From Range:**
```python
nums = list(range(3)) # [0, 1, 2]
```
**From String:**
```python
letters = list("Py") # ['P', 'y']
words = "hello world".split() # ['hello', 'world']
```
**Nested:**
```python
matrix = [
    [1, 2],
    [3, 4]
]
print(matrix[0][1]) # 2
```

### Try to Predict
```python
# What will these create?
a = []
b = [1, 2]
c = list(range(2))
d = list("hi")
e = "a,b".split(',')
```
**Answers:**
- a: `[]`
- b: `[1, 2]`
- c: `[0, 1]`
- d: `['h', 'i']`
- e: `['a', 'b']`

---

## Accessing List Elements Using Indexing

### What is Indexing?
Indexing is your **direct access pass** to any item in a list by its position. Imagine a parking garage: spaces are numbered (0, 1, 2...), and you go straight to space #42 for your car. This gives **instant access** to any element.

### Why Zero-Based?
Python, like most modern languages, uses **0-based indexing**. This means the first item is at `index 0`. Think of it as '0 steps from the start' rather than 'position 1'. It's efficient and mathematically consistent.

```python
fruits = ['apple', 'banana', 'orange']
# apple is 0 steps from start!
```

### Why It Matters
Without indexing, you can't get or modify specific elements. With it, you gain instant, precise control over list data, essential for complex algorithms.

### Key Concepts
- Use square brackets: `list[index]`
- Indices start at 0
- Access by position

**Example:**
```python
fruits = ['apple', 'banana', 'orange']
print(fruits[0])  # apple (first)
print(fruits[2])  # orange (last)
```

### Positive vs Negative Indexing
**Positive (from start):**
```python
items = ['a', 'b', 'c', 'd']
items[0]   # 'a' (first)
items[3]   # 'd' (last)
```
**Negative (from end):**
```python
items[-1]  # 'd' (last)
items[-2]  # 'c' (second to last)
```

### Common Patterns
**First and Last:**
```python
data = [10, 20, 30]
first = data[0]      # 10
last = data[-1]      # 30
```
**Nested Lists:**
```python
matrix = [[1, 2], [3, 4]]
print(matrix[0][1])  # 2
```
**Finding:**
```python
names = ['Alice', 'Bob']
if 'Bob' in names:
    idx = names.index('Bob')
    print(f"Found at {idx}")  # 1
```

### Try to Predict
```python
lst = ['x', 'y', 'z']
a = lst[0]    # ?
b = lst[-1]   # ?
c = lst[1]    # ?
d = lst[-2]   # ?
```
Answers: a: 'x', b: 'z', c: 'y', d: 'y'.

Indexing is the foundation - master it first!

---

## Extracting List Portions Using Slicing

## What is Slicing?

Slicing is Python's **superpower notation** for grabbing chunks of lists. Think of it like DVR recording: you have a full show (your list), and you want just minutes 10-20 (a slice) – `show[10:20]` extracts that segment simply, without affecting the original.

### The Magic Syntax

```python
list[start:stop:step]
```

-   **start**: "Begin here" (included)
-   **stop**: "Stop here" (NOT included - important!)
-   **step**: "Take every Nth item"

**Why stop is exclusive?** Makes ranges work perfectly with loops! `range(5)` gives 0-4, `list[:5]` gives first 5 items. 

### Why It Matters

Slicing is essential for data manipulation, providing a clean, one-line way to extract, copy, and manipulate list portions, replacing verbose loops.

### The Slice Syntax

**Basic Format:**
```python
list[start:stop:step]
```

-   `start`: Where to begin (inclusive)
-   `stop`: Where to end (exclusive)
-   `step`: Interval between elements

**Example:**
```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
# Get elements 2 through 6
portion = numbers[2:7]
print(portion)  # [2, 3, 4, 5, 6]
# Notice: stop at 7 means up to (not including) 7
```

### Common Patterns

**First/Last n Elements:**
```python
data = [10, 20, 30, 40, 50, 60]
first = data[:3]     # First 3: [10, 20, 30]
last = data[-2:]     # Last 2: [50, 60]
```

**Copy a List:**
```python
original = [1, 2, 3, 4, 5]
copy = original[:]
# Modify copy without affecting original
copy.append(6)
```

**Reverse a List:**
```python
nums = [1, 2, 3, 4, 5]
reversed = nums[::-1]  # [5, 4, 3, 2, 1]
```

**Skip Elements:**
```python
items = [0, 1, 2, 3, 4, 5, 6, 7, 8]
events = items[::2]   # Every 2nd element: [0, 2, 4, 6, 8]
```

### Key Rules

1.  **Stop is Exclusive:**
    ```python
    lst = [1, 2, 3, 4, 5]
    lst[1:3]  # [2, 3] - stops before index 3
    ```

2.  **Omit Start or Stop:**
    ```python
    lst[:3]   # From beginning to index 3
    lst[3:]   # From index 3 to end
    lst[:]    # Entire list (creates copy)
    ```

3.  **Negative Indices:**
    ```python
    lst[-3:]  # Last 3 elements
    lst[:-2]  # All except last 2
    ```

### Try to Predict

```python
data = [10, 20, 30, 40, 50, 60, 70, 80]

a = data[2:5]    # ?
b = data[:4]     # ?
c = data[5:]     # ?
d = data[-2:]    # ?
e = data[::2]    # ?
f = data[::-1]   # ?
```

Answers:
-   a: `[30, 40, 50]`
-   b: `[10, 20, 30, 40]`
-   c: `[60, 70, 80]`
-   d: `[70, 80]`
-   e: `[10, 30, 50, 70]`
-   f: `[80, 70, 60, 50, 40, 30, 20, 10]`

Slicing is powerful - master it for cleaner code!

---

## Modifying Lists Using Built-in Methods

## What Are List Methods?

List methods are **built-in functionalities for every list**, allowing direct manipulation rather than complex loops. They are like integrated controls for your data.

### Important: In-Place vs Return

**Biggest gotcha** - most methods change the list but return **None**:
```python
numbers = [3, 1, 2]
result = numbers.sort()  # numbers is [1, 2, 3], but...
print(result)  # None!

# DON'T DO THIS:
numbers = numbers.sort()  # Now numbers is None! Lost your data!
```

**Why?** Efficiency - don't wastefully create copies when you want to modify the original!

### Why It Matters

List methods provide efficient, readable ways to add, remove, sort, and search list elements, making your code cleaner and more powerful.

### Adding Elements

**append() - Add to End:**
```python
cart = []
cart.append("apple")
print(cart)  # ['apple']
```

**extend() - Add Multiple:**
```python
cart = ['apple']
cart.extend(['banana', 'orange'])
print(cart)  # ['apple', 'banana', 'orange']
```

**insert() - Add at Position:**
```python
fruits = ['apple', 'orange']
fruits.insert(1, 'banana')
print(fruits)  # ['apple', 'banana', 'orange']
```

### Removing Elements

**remove() - Remove by Value:**
```python
items = [1, 2, 3, 2]
items.remove(2)  # Removes first 2
print(items)     # [1, 3, 2]
```

**pop() - Remove and Return:**
```python
numbers = [1, 2, 3]
last = numbers.pop()     # Returns 3
print(numbers)           # [1, 2]
```

### Organizing Lists

**sort() - Sort in Place:**
```python
numbers = [3, 1, 4]
numbers.sort()
print(numbers)  # [1, 3, 4]
```

**reverse() - Reverse Order:**
```python
items = [1, 2, 3]
items.reverse()
print(items)  # [3, 2, 1]
```

### Finding Elements

**index() - Find Position:**
```python
fruits = ['apple', 'banana', 'orange']
pos = fruits.index('banana')
print(pos)  # 1
```

**count() - Count Occurrences:**
```python
numbers = [1, 2, 2, 3, 2]
count = numbers.count(2)
print(count)  # 3
```

### Try to Predict

```python
lst = []
lst.append(1)
lst.append(2)
lst.extend([3, 4])
lst.insert(0, 0)
lst.remove(2)
x = lst.pop()

# What is lst?
# What is x?
```

Answers:
- lst: `[0, 1, 3]`
- x: `4`

List methods are your essential toolkit for list manipulation!

---

## Iterating Through Lists Using Loops

## What is Iteration?

Iteration is **automatically going through every item** in your list - like a machine that processes each piece on an assembly line!

### Simple Analogy

Think of iteration like **handing out flyers**:
- **Stack of flyers**: Your list
- **You**: The loop
- **Action**: Give one flyer to each person
- **Automatic**: Move to next person, repeat until done

Or like **feeding animals at a zoo**:
- **List of enclosures**: Your list
- **Zookeeper**: The loop
- **Each cage**: Gets fed (processed)
- **Systematic**: Don't skip any, don't feed twice

### Why Iteration is Magic

**Without iteration** - nightmare:
```python
print(list[0])
print(list[1])
print(list[2])
# ...what if there are 1000 items?!
```

**With iteration** - elegant:
```python
for item in list:
    print(item)
# Works for any size list!
```

**Scales infinitely** - same code handles 10 items or 10 million!

### What You'll Learn

Iteration means processing each element in a list one by one. Essential for working with list data.

### Why It Matters

Without iteration:
- Can't process multiple elements
- Can't apply logic to each item
- Limited to manual access

With iteration:
- Process all elements automatically
- Apply transformations
- Calculate aggregates

### Basic For Loop

**Most Common Pattern:**
```python
fruits = ['apple', 'banana', 'orange']

for fruit in fruits:
    print(fruit)
# apple
# banana
# orange
```

**With Conditions:**
```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

for num in numbers:
    if num % 2 == 0:
        print(f"{num} is even")
# is even
# is even
# is even
# is even
# is even
```

### Getting Index with enumerate()

Sometimes you need both the position and the value:

```python
fruits = ['apple', 'banana', 'orange']

for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
# apple
# banana
# orange

# Start counting from 1
for position, fruit in enumerate(fruits, start=1):
    print(f"{position}. {fruit}")
# apple
# banana
# orange
```

### Parallel Lists with zip()

Process multiple related lists together:

```python
names = ['Alice', 'Bob', 'Charlie']
scores = [85, 92, 78]

for name, score in zip(names, scores):
    print(f"{name}: {score}")
# Alice: 85
# Bob: 92
# Charlie: 78
```

### Common Patterns

**Calculate Sum:**
```python
numbers = [10, 20, 30, 40, 50]
total = 0

for num in numbers:
    total += num

print(total)  # 150
```

**Build New List:**
```python
numbers = [1, 2, 3, 4, 5]
squares = []

for num in numbers:
    squares.append(num ** 2)

print(squares)  # [1, 4, 9, 16, 25]
```

**Count Matches:**
```python
grades = [85, 92, 78, 90, 88]
passing = 0

for grade in grades:
    if grade >= 80:
        passing += 1

print(f"Passing: {passing}")  # 4
```

### Important: Don't Modify While Iterating

```python
# WRONG - unpredictable behavior
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # BAD!

# RIGHT - iterate over copy
numbers = [1, 2, 3, 4, 5]
for num in numbers[:]:
    if num % 2 == 0:
        numbers.remove(num)
```

### Try to Predict

```python
scores = [70, 85, 90, 75]
total = 0

for score in scores:
    total += score

average = total / len(scores)

# What is total?
# What is average?
```

Answers:
- total: `320`
- average: `80.0`

Iteration unlocks the power of lists!

---

## Creating and Using Tuples

## What Are Tuples?

Tuples are **read-only lists** – Python's way of saying "this data is final, don't touch it!" They are immutable sequences.

### Simple Analogy

Think of tuples like a **sealed envelope**: contents are locked in and cannot be changed after creation. Lists are like open boxes, allowing items to be added or removed.

### Why "Permanent" Data?

Some data should **never** change. For example:
- GPS coordinates: `(40.7128, -74.0060)` (New York's location is fixed!)
- RGB colors: `(255, 0, 0)` (pure red doesn't change)
- Birthday: `(1990, 5, 15)` (your birth date is fixed)

**Tuple = "I promise this won't change"**

### Creating Tuples

**Basic syntax:**
```python
# With parentheses
point = (3, 4)

# Without parentheses (tuple packing)
coords = 3, 4 # Still creates tuple!

# Empty tuple
empty = ()
```

**Important - Single element:**
```python
# WRONG - not a tuple!
not_tuple = (5)  # This is just int 5

# RIGHT - need comma
single = (5,)  # This is a tuple
```

### Accessing Elements

Same as lists:
```python
fruits = ('apple', 'banana', 'orange')

# Indexing
fruits[0]    # 'apple'
fruits[-1]   # 'orange'

# Slicing
fruits[1:3]  # ('banana', 'orange')
```

### Immutability

Cannot modify after creation:
```python
colors = ('red', 'green', 'blue')

# This will ERROR
# colors[0] = 'yellow'  # TypeError!
# colors.append('purple') # AttributeError!
```
But can create new tuples through operations like concatenation:
```python
colors1 = ('red', 'green')
colors2 = ('blue',)
all_colors = colors1 + colors2 # ('red', 'green', 'blue')
```

### Tuple Methods

Tuples have only 2 methods (vs 11 for lists):
```python
numbers = (1, 2, 3, 2, 4, 2)

# count() - how many times
numbers.count(2)  # 3

# index() - find position of first occurrence
numbers.index(3)  # 2
```

### Multiple Return Values

Functions often return tuples:
```python
def get_name_age():
    return 'Alice', 25 # Returns tuple

# Unpack the result
name, age = get_name_age()
print(f"{name} is {age}") # Alice is 25
```

### When to Use Tuples

**Use Tuple:**
- Data shouldn't change (e.g., fixed coordinates, RGB colors, database records)
- When used as dictionary keys (tuples are hashable, lists are not)
- When returning multiple values from a function

**Use List:**
- Data will change (e.g., adding/removing items in a shopping cart, a to-do list)

### Lists vs Tuples

| Aspect | Tuple | List |
|--------|-------|------|
| Change? | No | Yes |
| Speed | Faster | Slower |
| Memory | Less | More |
| Syntax | `(1,2,3)` | `[1,2,3]` |

### Try to Predict

```python
t = (10, 20, 30)
a, b, c = t

# What are a, b, c?
```
Answer:
- a = 10
- b = 20
- c = 30

This is called "unpacking"!

Tuples = immutable, safe, fast!

---

## Understanding Tuple Immutability

## What Is Immutability?
Immutability means "frozen forever" - once created, it cannot be changed. It's like a permanent tattoo: choose wisely for data that shouldn't change.

### The Safety Guarantee
Immutability is Python's way of saying "I promise this won't change unexpectedly." Like a sealed time capsule, its contents are protected from tampering.
```python
config = ('production_server', 5432, 'maindb')
# Pass to 100 functions - stays safe!
```

### Why It Matters
Mutable data can lead to accidental changes (`config = ['localhost']`, then `config[0] = 'wrong_server'`). Immutable data prevents this (`config = ('localhost')`, then `config[0] = 'wrong_server'` results in a TypeError).

### What Immutability Means
**Cannot Change:** Tuples cannot have elements modified, added, or removed after creation.
```python
point = (10, 20)
# point[0] = 15      # TypeError
# point.append(30)   # AttributeError
```
**Can Create New:** You can create new tuples based on existing ones.
```python
point = (10, 20)
new_point = point + (30,)  # Creates new tuple (10, 20, 30); original (10, 20) unchanged
```

### Key Benefits
1.  **Safety:** Prevents accidental modifications, ensuring data integrity.
2.  **Dictionary Keys:** Tuples (if all elements are immutable) can be used as dictionary keys.
    `locations = {(0, 0): 'home', (10, 20): 'work'}`
3.  **Performance:** Generally faster and use less memory than lists.

### Shallow Immutability
**Important caveat:** A tuple's structure is fixed, but if it contains mutable objects (like lists), those nested objects can still be changed.
```python
data = (1, 2, [3, 4])
# data[0] = 999  # Error
data[2].append(5)  # Works! Now: (1, 2, [3, 4, 5])
```

### Hashable = Dict Keys
Tuples with all immutable elements are hashable and work as dictionary keys.
```python
point = (10, 20); locations = {point: 'home'}  # Works
bad = (1, [2, 3]); # locations = {bad: 'test'}  # Error! (contains mutable list)
```

### When to Use Tuples
**Use Tuple for:**
*   Data that shouldn't change (e.g., coordinates, configuration, constants).
*   As dictionary keys (when hashable).

**Use List for:**
*   Data that will change (e.g., a shopping cart, to-do list, requiring frequent adds/removes).

Immutability = safety + performance!