# Implement String Data Types

## Understanding Strings
Strings are immutable sequences of characters.

---

<div align="center">

![Python String Character Indexing](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.5.png)

</div>

### Why "String"?
From "string of characters" — like beads on a string: "Hello" = ['H', 'e', 'l', 'l', 'o'].

### Character Encoding
Computers store characters as numbers: 'A'=65.
- **ASCII** (1960s): 128 characters (English, digits).
- **Unicode** (1991): 1 million+ characters (all languages, emoji).
Python 3 uses Unicode by default: `message = "Hello 世界 🌍"`.

### Strings Are Immutable
Once created, strings cannot be changed. Modifications create a **new** string.
```python
name = "Alice"
name = name.upper()  # Creates new string "ALICE"
```
**Why immutable?** Safety, efficiency, hashable (for dictionary keys).

### Real-World Analogy
Like a **text message**: individual characters, specific order, once sent you can't edit it.

## Creating Strings

### Using Quotes
```python
name = 'Alice'        # Single quotes
message = "Hello!"    # Double quotes
```

### Quotes Inside Strings
```python
text1 = "He said, 'Hello!'"  # Use opposite quote type
text2 = 'She said, "Hi!"'
text3 = "He said, \"Hello!\"" # Or escape with backslash
```

### Multi-line Strings
```python
long_text = """This is a
multi-line string."""
```

## String Operations

### Concatenation (+)
```python
first = "Hello"
second = "World"
result = first + " " + second # "Hello World"
```
**Cannot mix strings and numbers directly**: Use `str()` for numbers.
```python
age = 25
message = "Age: " + str(age) # "Age: 25"
```

### Repetition (*)
```python
laugh = "ha" * 3  # "hahaha"
```

### Length
```python
name = "Alice"
length = len(name) # 5
```

## String Methods
Built-in functions to manipulate strings.

### Case Conversion
```python
name = "Alice"
print(name.upper()) # "ALICE"
print(name.lower()) # "alice"
```

### Strip Whitespace
```python
text = "  hello  "
print(text.strip()) # "hello"
```

### Replace
```python
text = "Hello World"
new_text = text.replace("World", "Python") # "Hello Python"
```

### Check Contents
```python
email = "user@example.com"
print(email.startswith("user")) # True
print("@" in email)             # True
```

## Practical Examples

### Full Name
```python
first_name = "Alice"
last_name = "Johnson"
full_name = first_name + " " + last_name # "Alice Johnson"
```

### Email Creation
```python
username = "alice"
domain = "example.com"
email = username + "@" + domain # "alice@example.com"
```

## Key Takeaways
1. Strings are text in quotes.
2. Use `+` to concatenate, `*` to repeat.
3. Use `str()` to mix strings and numbers.
4. Many useful string methods available.
5. Use `len()` to get length.
6. Strings are immutable (modifications create new strings).

---

## Format Output with F-strings

## Introduction
F-strings provide a clean, readable way to embed expressions in strings.

---

<div align="center">

![Python f-string Formatting](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.10.png)

*F-strings embed expressions inside {curly braces} within strings — Python evaluates them at runtime for clean, readable output*

</div>

---

### The Evolution of String Formatting

Python's string formatting methods:

**1991 - Python 1.x**: String concatenation
```python
"Hello " + name + ", you are " + str(age)
```
- Clunky, manual type conversion, hard to read.

**1997 - Python 1.5**: % formatting (from C)
```python
"Hello %s, you are %d" % (name, age)
```
- Powerful but cryptic format codes.

**2006 - Python 2.6/3.0**: .format() method
```python
"Hello {}, you are {}".format(name, age)
```
- More flexible and readable, but verbose.

**2015 - Python 3.6**: F-strings (PEP 498)
```python
f"Hello {name}, you are {age}"
```
- Concise, readable, fast. Standard method.

### Why F-strings Matter

**The problem**: Building strings with variables was awkward:
```python
name = "Alice"
score = 95
level = 10
message = "Player " + name + " scored " + str(score) + " points at level " + str(level) + "!"
# Hard to read, easy to make mistakes!
```

**The solution**: F-strings use **template syntax** (string interpolation):
```python
message = f"Player {name} scored {score} points at level {level}!"
# Clean, readable, obvious!
```

### Real-World Analogy

F-strings are like **Mad Libs**: a template with blanks to fill.

### Why Called "F-strings"?

The `f` stands for **"formatted string"**. It tells Python to evaluate expressions inside `{}`.
```python
name = "Alice"
print("Hello {name}")  # Prints: Hello {name}
print(f"Hello {name}") # Prints: Hello Alice
```

---

## Basic F-string Syntax

### Simple Variable Embedding
```python
name = "Alice"
age = 25
message = f"Hello, {name}! You are {age} years old."
print(message)
# Output: Hello, Alice! You are 25 years old.
```

### Multiple Variables
```python
first = "John"
last = "Doe"
age = 30
print(f"{first} {last} is {age}")
# Output: John Doe is 30
```

---

## Expressions in F-strings

### Arithmetic
```python
price = 10
quantity = 3
print(f"Total: ${price * quantity}")
# Output: Total: $30
```

### Method Calls
```python
name = "alice"
print(f"Welcome, {name.upper()}!")
# Output: Welcome, ALICE!
```

---

## Number Formatting

### Decimal Places
```python
pi = 3.14159265
print(f"Pi: {pi:.2f}")   # Pi: 3.14
```

### Width and Alignment
```python
num = 42
print(f"{num:5}")    # "   42" (width 5, right-aligned)
```

### Thousands Separator
```python
big_num = 1000000
print(f"{big_num:,}")  # 1,000,000
```

---

## Practical Examples

### Example 1: Receipt
```python
item = "Coffee"
price = 4.50
quantity = 2
total = price * quantity

print(f"Item: {item}")
print(f"Price: ${price:.2f}")
print(f"Quantity: {quantity}")
print(f"Total: ${total:.2f}")
```
Output:
Item: Coffee
Price: $4.50
Quantity: 2
Total: $9.00

---

## Comparing Formatting Methods

### Concatenation (Old)
```python
name = "Bob"
age = 30
msg = "Hi " + name + ", you are " + str(age)
```

### .format() (Older)
```python
msg = "Hi {}, you are {}".format(name, age)
```

### F-strings (Modern) ✅
```python
msg = f"Hi {name}, you are {age}"
```

---

## The Philosophy Behind F-strings

**Python's Zen**: "Readability counts"

Compare:
```python
# Concatenation
msg = "Total: $" + str(10 * 3) + " for " + str(3) + " items"

# F-string
msg = f"Total: ${10 * 3} for {3} items"
```
F-strings are readable, concise, powerful, and fast.

---

## Key Takeaways
1.  **F-strings are Python 3.6+'s modern solution**.
2.  **Start with `f`** before quotes: `f"..."`.
3.  **Use `{}` for interpolation** – embed variables and expressions.
4.  **Can evaluate expressions**: `{price * quantity}`, `{name.upper()}`.
5.  **Format specifiers**: `:.2f` (decimals), `:,` (thousands).
6.  **More readable** than alternatives.
7.  **String interpolation** fills template placeholders.
8.  **Python philosophy**: Readability counts.

---

## Creating and Initializing Lists in Python

---

<div align="center">

![Python List Create Initialize Elements](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.1.svg)

*A visual representation of array-based data structures showing how lists store elements in indexed positions*

</div>

---

## Introduction
Lists are **ordered collections** – fundamental data structures for managing **groups of related data** efficiently.
They solve the problem of needing separate variables for each item by using one name for many items, accessible by position.

### The Power of Collections
Lists allow working with data at scale:
```python
# Without lists - managing 1000 items is a nightmare
# student1 = "Alice"
# student2 = "Bob"
# ...

# With lists - elegant for any size!
students = ["Alice", "Bob", "Charlie"] # Can hold millions!
```

### Creating Empty Lists
- **Method 1: Square brackets (preferred)**
    ```python
    empty = []
    ```
- **Method 2: list() constructor**
    ```python
    empty = list()
    ```
- **Check if empty**
    ```python
    if not empty:
        print("List is empty")
    ```

---

### Lists with Initial Values
- **Integers**: `numbers = [1, 2, 3]`
- **Strings**: `fruits = ['apple', 'banana']`
- **Mixed types**: `student = ['Alice', 21, 3.8, True]`
- **Duplicates allowed**: `numbers = [1, 2, 2]`

---

### From Other Types
- **From range:**
    ```python
    nums = list(range(5))       # [0, 1, 2, 3, 4]
    evens = list(range(2, 7, 2)) # [2, 4, 6]
    ```
- **From strings:**
    ```python
    chars = list("hello")      # ['h', 'e', 'l', 'l', 'o']
    words = "a b c".split()     # ['a', 'b', 'c']
    items = "x,y,z".split(',') # ['x', 'y', 'z']
    ```

---

### Nested Lists
Lists can contain other lists for multi-dimensional data.
```python
# 2D list (matrix)
matrix = [
    [1, 2, 3],
    [4, 5, 6]
]

# Access: matrix[row_index][column_index]
row = matrix[0]     # [1, 2, 3]
element = matrix[0][1] # 2

# Example: Student data
students = [
    ['Alice', 85, 90],
    ['Bob', 78, 82]
]
```

---

### Special Patterns
- **Repeated elements:**
    ```python
    zeros = [0] * 5  # [0, 0, 0, 0, 0]
    # Caution: [[0]*3]*3 creates references, not independent sublists.
    ```
- **Concatenation:**
    ```python
    list1 = [1, 2]
    list2 = [3, 4]
    combined = list1 + list2  # [1, 2, 3, 4]
    ```

---

### Quick Reference
```python
# Empty
empty = []

# With values
nums = [1, 2, 3]

# From range
nums = list(range(5))

# From string
chars = list("abc")
words = "a b c".split()

# Nested
matrix = [[1, 2], [3, 4]]

# Repeated
zeros = [0] * 3

# Combined
result = [1] + [2, 3]
```

---

### Key Points
- Use `[]` for empty lists.
- Lists are mutable and ordered.
- Allow duplicates and mixed types.
- `list(range(n))` for sequences.
- `string.split()` for words.
- Nested lists for tables/matrices.
- `+` concatenates lists, `*` repeats elements.

---

## Accessing List Elements Using Indexing

---

<div align="center">

![Python List Indexing Access Element](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.2.png)

*An array data structure with metadata fields, illustrating how indexing enables direct element access by position*

</div>

---

Indexing enables **random access** to list elements by their position, offering **constant-time** (O(1)) retrieval. This means instant access regardless of list size, unlike sequential scanning. It's like apartment numbers or library call numbers – you go straight to the address. Python uses **zero-based indexing** (first element is `[0]`), a convention from languages like C, which simplifies memory offset calculations.

### Positive Indexing
- Indices start at 0.
- First element: `list[0]`
- Last element: `list[len(list) - 1]`
- Accessing an index out of bounds raises an `IndexError`.
```python
fruits = ['apple', 'banana', 'orange']
first = fruits[0]      # 'apple'
last = fruits[len(fruits) - 1] # 'orange'
```

### Negative Indexing
- Counts elements from the end of the list.
- `-1` refers to the last element, `-2` to the second to last, and so on.
- Eliminates the need to know the list's length for end-based access.
```python
items = [10, 20, 30, 40, 50]
#        -5  -4  -3  -2  -1
last = items[-1]         # 50
second_last = items[-2]  # 40
```

### Nested Lists
- Lists can contain other lists, forming multi-dimensional structures.
- Use multiple sets of square brackets (`list[row][col]`) to access elements.
```python
matrix = [
    [1, 2, 3],
    [4, 5, 6]
]
row = matrix[0]      # [1, 2, 3]
element = matrix[0][1]  # 2
last = matrix[-1][-1]   # 6
```

### Error Handling
- Attempting to access an index outside `[-len(list), len(list)-1]` will raise an `IndexError`.
- Use `try-except` blocks or explicit bounds checking for safe access.
```python
numbers = [1, 2, 3]
try:
    value = numbers[10] # Out of bounds
except IndexError:
    value = None # Handle error gracefully
```

### Finding Elements
- Use the `in` operator to check if an element exists (`value in list`).
- Use `list.index(value)` to find the *first* index of a specific element.
- `list.index()` raises a `ValueError` if the element is not found.
```python
items = ['a', 'b', 'c']
if 'b' in items:
    print("Found") # Output: Found
index = items.index('b') # 1
```

### Quick Reference
```python
lst = ['a', 'b', 'c', 'd', 'e']

# Positive
lst[0]    # First ('a')
lst[4]    # Last ('e')

# Negative
lst[-1]   # Last ('e')
lst[-2]   # Second to last ('d')

# Nested
m = [[1,2], [3,4]]
m[0][1]   # 2

# Find
'b' in lst          # True
lst.index('c')      # 2
len(lst)            # 5
```
**Remember:** 0-based indexing, negative from end, bounds checking, `index()` raises error if not found, nested lists use multiple brackets.

---

## Extracting List Portions Using Slicing

---

<div align="center">

![Python List Slicing start stop step](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.3.png)

*Python slicing index diagram showing how positive and negative indices map to list elements*

</div>

---

## Introduction

Slicing enables efficient range extraction from lists using simple notation, a core Python philosophy. It's like cutting a loaf of bread – you get a portion, but the original loaf remains intact. This concise `[start:stop:step]` pattern is powerful. The exclusive `stop` index prevents off-by-one errors and the use of negative indices simplifies working from the end of a list.

### The [start:stop:step] Pattern

-   **start**: Where to begin (default: 0)
-   **stop**: Where to end (exclusive, default: end)
-   **step**: Jump size (default: 1)

### Basic Slice Syntax

```python
# Syntax: list[start:stop:step]
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
numbers[2:7]    # [2, 3, 4, 5, 6]
# Key: Start is inclusive, stop is exclusive.
```

---

### Omitting Start and Stop

```python
data = [10, 20, 30, 40, 50, 60, 70]
data[:4]     # [10, 20, 30, 40]  (From beginning)
data[3:]     # [40, 50, 60, 70]  (To end)
data[:]      # [10, 20, 30, 40, 50, 60, 70] (Shallow copy)
```

---

### Negative Indices

```python
items = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
items[-3:]      # [8, 9, 10] (Last 3 elements)
items[:-2]      # [1, 2, 3, 4, 5, 6, 7, 8] (All except last 2)
items[2:-2]     # [3, 4, 5, 6, 7, 8] (Mix positive and negative)
```

---

### Step Parameter

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
numbers[::2]    # [0, 2, 4, 6, 8] (Every 2nd element)
numbers[1::2]   # [1, 3, 5, 7, 9] (Every 2nd from index 1)
numbers[::-1]   # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0] (Negative Step - Reverse)
```

---

### Slice Assignment

```python
nums = [1, 2, 3, 4, 5]
nums[1:3] = [20, 30]             # Replace slice: [1, 20, 30, 4, 5]
nums = [1, 2, 3, 4, 5]
nums[1:3] = [20, 30, 40, 50]     # Different length OK: [1, 20, 30, 40, 50, 4, 5]
nums = [1, 2, 3, 4, 5]
nums[1:3] = []                   # Delete with empty list: [1, 4, 5]
nums = [1, 2, 5]
nums[2:2] = [3, 4]               # Insert (empty slice): [1, 2, 3, 4, 5]

nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
nums[::2] = [10, 20, 30, 40, 50] # Replace with Step: [10, 1, 20, 3, 30, 5, 40, 7, 50, 9]
# Must match length when using step for replacement!
```

---

### Common Patterns

**Split List:**
```python
data = [1, 2, 3, 4, 5, 6, 7, 8]
mid = len(data) // 2
first_half = data[:mid]    # [1, 2, 3, 4]
second_half = data[mid:]   # [5, 6, 7, 8]
```

**Remove First/Last:**
```python
items = ['start', 'a', 'b', 'c', 'end']
core = items[1:-1]  # ['a', 'b', 'c']
```

**Reverse:**
```python
original = [1, 2, 3, 4, 5]
reversed = original[::-1]  # [5, 4, 3, 2, 1]
```

**Copy:**
```python
original = [1, 2, 3]
copy = original[:] # Shallow copy
```

**Skip Elements:**
```python
every_third = data[::3] # Every nth element
odds = data[1::2] # Alternating
evens = data[::2]
```

---

### Processing Chunks

```python
data = list(range(20))
chunk_size = 5
for i in range(0, len(data), chunk_size):
    chunk = data[i:i+chunk_size]
    print(chunk)
```

**Sliding Window:**
```python
nums = [1, 2, 3, 4, 5, 6]
window_size = 3
for i in range(len(nums) - window_size + 1):
    window = nums[i:i+window_size]
    print(window)
```

---

### Quick Reference

```python
lst = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Basic
lst[2:7]     # [2, 3, 4, 5, 6]
lst[:5]      # [0, 1, 2, 3, 4]
lst[5:]      # [5, 6, 7, 8, 9]
lst[:]       # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Negative
lst[-3:]     # [7, 8, 9]
lst[:-3]     # [0, 1, 2, 3, 4, 5, 6]
lst[2:-2]    # [2, 3, 4, 5, 6, 7]

# Step
lst[::2]     # [0, 2, 4, 6, 8]
lst[1::2]    # [1, 3, 5, 7, 9]
lst[::-1]    # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

# Assignment (example results not shown here)
lst[2:5] = [20, 30, 40]  # Replace
lst[2:2] = [10]          # Insert
lst[2:5] = []            # Delete
```

**Remember:**
-   Creates new list (except assignment)
-   Stop is exclusive
-   Negative step reverses
-   Empty slice for insertion

---

## Modifying Lists Using Built-in Methods

---

<div align="center">

![Python List Built-in Methods](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.4.png)

*Illustration of a list modification operation: inserting a new element into a linked data structure*

</div>

---

## Introduction

List methods apply **Object-Oriented Programming (OOP)**, giving lists built-in behaviors for self-manipulation.

### In-Place Modification: Critical Concept

Most list methods modify **in-place** (change the original) and return **None**:
```python
nums = [3, 1, 2]
result = nums.sort()  # nums is now [1, 2, 3]
print(result)  # None!
```

**Why?** Memory efficiency - don't create copies unnecessarily. This is **destructive** operations - original is lost!

**Common beginner mistake**:
```python
nums = nums.sort()  # nums becomes None! Data lost!
```

### Adding Elements

```python
# append() - Add single element to end
fruits = ['apple']
fruits.append('banana')  # ['apple', 'banana']

# extend() - Add multiple elements from iterable
fruits = ['apple']
fruits.extend(['orange', 'mango']) # ['apple', 'orange', 'mango']

# insert() - Add at specific position
fruits = ['apple', 'orange']
fruits.insert(1, 'grape') # ['apple', 'grape', 'orange']
```

---

### Removing Elements

```python
numbers = [1, 2, 3, 2, 4]

# remove(value) - Remove first occurrence of value (raises ValueError if not found)
numbers.remove(2)  # [1, 3, 2, 4]

# pop([index]) - Remove and return element by index (last by default)
last = numbers.pop()     # Returns 4, list is [1, 3, 2]
second = numbers.pop(1)  # Returns 3, list is [1, 2]

# clear() - Remove all elements
numbers.clear()  # []
```

---

### Sorting and Reversing

```python
numbers = [3, 1, 4]

# sort() - Sort in-place (ascending by default, use reverse=True for descending)
numbers.sort()              # [1, 3, 4]
numbers.sort(reverse=True)  # [4, 3, 1]

# reverse() - Reverse in-place
numbers.reverse()  # [1, 3, 4]
```

**Custom Sorting:**
```python
# Use 'key' argument (e.g., key=len for length)
words = ['apple', 'pie', 'banana']
words.sort(key=len)  # ['pie', 'apple', 'banana']
```

---

### Searching and Counting

```python
items = ['a', 'b', 'c', 'b', 'd']

# index(value) - Find position of first occurrence (raises ValueError if not found)
pos = items.index('b')  # 1

# count(value) - Count occurrences
count = items.count('b')  # 2
```

---

### Copying Lists

```python
original = [1, 2, 3]

# copy() - Create shallow copy (independent top-level list)
copy = original.copy()
copy.append(4)
# original: [1, 2, 3]
# copy: [1, 2, 3, 4]
```

**Shallow vs Deep Copy:**
```python
# Shallow copy - nested objects still referenced
original = [[1, 2], [3, 4]]
shallow = original.copy()
shallow[0][0] = 999
# original: [[999, 2], [3, 4]] - affected!

# Deep copy - fully independent (requires 'import copy')
import copy
deep = copy.deepcopy(original)
deep[0][0] = 999
# original: [[1, 2], [3, 4]] - unchanged
```

---

### Return Values

**Important: Most methods return None**

```python
# These modify in-place, return None
numbers = [3, 1, 2]
result = numbers.sort()
# result is None
# numbers is [1, 2, 3]

# DON'T DO THIS
numbers = numbers.sort()  # numbers becomes None!

# pop() is exception - returns removed value
numbers = [1, 2, 3]
last = numbers.pop()  # last is 3, numbers is [1, 2]
```

**Built-in Functions Return New Lists (preserve original):**
```python
# sorted() - returns new sorted list
original = [3, 1, 2]
sorted_list = sorted(original)
# original: [3, 1, 2] - unchanged
# sorted_list: [1, 2, 3]

# reversed() - returns iterator (convert to list if needed)
original = [1, 2, 3]
rev_list = list(reversed(original))  # [3, 2, 1]
```

---

### Quick Reference

```python
lst = [1, 2, 3]

# Add
lst.append(4)        # Add to end
lst.extend([5, 6])   # Add multiple
lst.insert(0, 0)     # Insert at position

# Remove
lst.remove(2)        # Remove by value
val = lst.pop()      # Remove and return last
val = lst.pop(0)     # Remove and return at index
lst.clear()          # Remove all

# Organize
lst.sort()           # Sort ascending
lst.sort(reverse=True)  # Sort descending
lst.reverse()        # Reverse order

# Search
idx = lst.index(3)   # Find position
cnt = lst.count(2)   # Count occurrences

# Copy
cp = lst.copy()      # Shallow copy
```

**Remember:**
- Methods modify in-place
- Most return None (except pop)
- Check existence before remove/index
- Use copy() to preserve original

---

## Iterating Through Lists Using Loops

---

<div align="center">

![Python Iterating List with for Loop](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.5.png)

*Flowchart illustrating how a loop iterates through elements, checking conditions and processing each item sequentially*

</div>

---

## Introduction

List iteration represents the marriage of **data structures and control flow** - using loops to systematically process collections. This is where lists truly become powerful, enabling **batch processing** of data.

### Why Iteration Matters

**Single-item processing** (limited): Can only handle one piece of data at a time manually.
**Iteration** (powerful): Process thousands, millions, billions of items with the same code.

**Real-world impact**: Netflix processes millions of viewing records using iteration. Google searches billions of web pages. Your programs can work at scale using the same concept!

### Real-World Analogy

List iteration is like **an assembly line worker**:
- **Conveyor belt**: The list (items moving by)
- **Worker**: The loop (processes each item)
- **Same action**: Apply same operation to every item
- **Automatic**: Next item comes automatically

Or like **scanning groceries at checkout**:
- **Cart**: Your list
- **Scanner**: The loop
- **Each item**: Gets scanned (processed) one by one
- **Total**: Accumulated result (like sum/count)

### Python's Elegant Iteration

**C/Java way** (verbose):
```c
for (int i = 0; i < length; i++) {
    process(array[i]);  // Manual indexing
}
```

**Python way** (elegant):
```python
for item in list:
    process(item)  # Direct access!
```

This **direct iteration** makes Python code readable and less error-prone - no index mistakes!

### Basic For Loop

```python
# Iterate over elements
fruits = ['apple', 'banana', 'orange']
for fruit in fruits:
    print(fruit)
# apple
# banana
# orange

# With conditional
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        print(f"{num} is even")

# Accumulate result
total = 0
for num in numbers:
    total += num
print(total)  # 15
```

**Pattern:**
- `for item in list:` - most common
- Direct access to element values
- Clean and readable

---

### Using enumerate()

```python
# Get index and value
fruits = ['apple', 'banana', 'orange']

for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")
# apple
# banana
# orange

# Start from 1
for num, fruit in enumerate(fruits, start=1):
    print(f"{num}. {fruit}")
# apple
# banana
# orange
```

**When to Use:**
- Need both index and value
- Modifying elements by index
- Finding positions of matches

**Modify by Index:**
```python
grades = [75, 82, 68]
for i, grade in enumerate(grades):
    if grade < 80:
        grades[i] = 80
# grades: [80, 82, 80]
```

---

### While Loop

```python
# Index-based iteration
fruits = ['apple', 'banana', 'orange']
i = 0

while i < len(fruits):
    print(fruits[i])
    i += 1

# Conditional termination
numbers = [10, 20, 30, 40, 50]
i = 0

while i < len(numbers) and numbers[i] < 40:
    print(numbers[i])
    i += 1
# , 20, 30
```

**When to Use:**
- Need manual index control
- Conditional loop termination
- Variable step sizes

---

### Using zip()

```python
# Parallel iteration
names = ['Alice', 'Bob', 'Charlie']
scores = [85, 92, 78]

for name, score in zip(names, scores):
    print(f"{name}: {score}")
# Alice: 85
# Bob: 92
# Charlie: 78

# Three or more lists
first = ['Alice', 'Bob']
last = ['Smith', 'Jones']
ages = [25, 30]

for f, l, a in zip(first, last, ages):
    print(f"{f} {l}, {a}")
# Alice Smith, 25
# Bob Jones, 30
```

**Key Points:**
- Stops at shortest list
- Perfect for related data
- Can zip 2+ lists

**Create Dictionary:**
```python
keys = ['name', 'age', 'city']
values = ['Alice', 25, 'NYC']

person = dict(zip(keys, values))
# {'name': 'Alice', 'age': 25, 'city': 'NYC'}
```

---

### Safe Modification

**DON'T - Modify While Iterating:**
```python
# BAD - can skip elements
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # WRONG
```

**DO - Iterate Over Copy:**
```python
# GOOD - iterate over copy
numbers = [1, 2, 3, 4, 5]
for num in numbers[:]:
    if num % 2 == 0:
        numbers.remove(num)
# numbers: [1, 3, 5]
```

**DO - Use List Comprehension:**
```python
# BEST - create new list
numbers = [1, 2, 3, 4, 5]
numbers = [n for n in numbers if n % 2 != 0]
# numbers: [1, 3, 5]
```

**SAFE - Modify by Index:**
```python
# Safe to modify by index
numbers = [1, 2, 3, 4, 5]
for i in range(len(numbers)):
    numbers[i] *= 2
# numbers: [2, 4, 6, 8, 10]
```

---

### Nested Lists

```python
# D iteration
matrix = [
    [1, 2, 3],
    [4, 5, 6]
]

# Iterate rows
for row in matrix:
    print(row)
# [1, 2, 3]
# [4, 5, 6]

# Iterate all elements
for row in matrix:
    for element in row:
        print(element, end=' ')
    print()
# 2 3
# 5 6

# With indices
for i, row in enumerate(matrix):
    for j, element in enumerate(row):
        print(f"[{i}][{j}] = {element}")
```

---

### Loop Control

```python
# break - exit early
for num in [1, 2, 3, 4, 5]:
    if num > 3:
        break
    print(num)
# 2 3

# continue - skip iteration
for num in [1, 2, 3, 4, 5]:
    if num % 2 == 0:
        continue
    print(num)
# 3 5

# else clause
for num in [1, 2, 3]:
    if num > 10:
        break
else:
    print("No large numbers")
# No large numbers
```

---

### Quick Reference

```python
lst = ['a', 'b', 'c']

# Basic iteration
for item in lst:
    print(item)

# With index
for i, item in enumerate(lst):
    print(i, item)

# Start from 1
for i, item in enumerate(lst, start=1):
    print(i, item)

# Parallel lists
lst2 = [1, 2, 3]
for item1, item2 in zip(lst, lst2):
    print(item1, item2)

# By index
for i in range(len(lst)):
    print(lst[i])

# While loop
i = 0
while i < len(lst):
    print(lst[i])
    i += 1
```

**Remember:**
- Use `for item in list:` for simple iteration
- Use `enumerate()` when you need indices
- Use `zip()` for parallel lists
- Don't modify list while iterating over it
- Modify by index is safe

---

## Creating and Using Tuples

---

<div align="center">

![Python Tuple Creation and Usage](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.7.jpg)

*Python data processing architecture: tuples are fundamental data structures used throughout the Python interpreter for passing grouped values*

</div>

---

## Introduction

Tuples introduce **immutability** to sequences. They embody the philosophy that "data that shouldn't change, can't change."

### Why Tuples Exist

**The mutability problem**: Lists can be accidentally modified:
```python
config = ['prod', 5432]
some_function(config) # Config might change!
```

**Tuple solution**: Immutable = guaranteed safety:
```python
config = ('prod', 5432)
some_function(config) # Config can't change!
```

**Analogy**: Tuples are like **engraved stone tablets** — permanent, read-only, original message preserved. Lists are like paper lists — erasable, modifiable.

### Performance Benefits

Tuples are **faster and smaller** than lists due to their fixed size. This allows Python to optimize aggressively. They are also **hashable**, meaning they can be dictionary keys (lists cannot).

### Creating Tuples

```python
# Empty tuple
empty = ()

# With values
coords = (3, 4)

# Without parentheses (tuple packing)
point = 3, 4

# Single element - MUST have comma!
single = (5,)    # Tuple
not_tuple = (5)  # Just int 5

# Using tuple() constructor
from_list = tuple([1, 2, 3])
from_string = tuple('hi')
```

---

### Accessing Elements

```python
fruits = ('apple', 'banana', 'orange')

# Indexing (same as lists)
fruits[0]    # 'apple'
fruits[-1]   # 'orange'

# Slicing (same as lists)
fruits[1:3]  # ('banana', 'orange')
fruits[::2]  # ('apple', 'orange')

# Nested
matrix = ((1, 2), (3, 4))
matrix[0][1]  # 2
```

---

### Immutability

```python
colors = ('red', 'green', 'blue')

# CANNOT modify
# colors[0] = 'yellow'  # TypeError

# CANNOT add/remove
# colors.append('yellow') # AttributeError

# CAN create new tuples
combined = colors + ('yellow',) # ('red', 'green', 'blue', 'yellow')
```

**Note:** A tuple is immutable, but it can contain mutable objects (e.g., a list). The mutable object's contents can still be changed.

---

### Tuple Methods

**Only 2 methods:**

```python
numbers = (1, 2, 3, 2, 4)

numbers.count(2)  # 2
numbers.index(3)  # 2
```

---

### Common Operations

```python
t = (1, 2, 3, 4, 5)

len(t)  # 5
3 in t  # True
min(t)  # 1
max(t)  # 5
sum(t)  # 15

for item in t:
    print(item)
```

---

### Multiple Return Values

```python
def get_user():
    return 'Alice', 25, 'NYC'

user = get_user()         # ('Alice', 25, 'NYC')
name, age, city = get_user() # Unpack directly

def divide(a, b):
    return a // b, a % b

q, r = divide(17, 5)
```

---

### When to Use Tuples

**Use Tuples:**
```python
# Fixed data (constants)
DAYS = ('Mon', 'Tue', 'Wed')
RGB_RED = (255, 0, 0)

# Dictionary keys
locations = {}
locations[(0, 0)] = "Origin"

# Return multiple values
def get_dimensions():
    return 1920, 1080

# Fixed structure records
person = ('Alice', 25, 'Engineer')
```

**Use Lists:**
```python
# Data that changes (dynamic)
cart = ['milk', 'bread']
cart.append('eggs')
```

---

### Basic Unpacking

```python
# Simple unpacking
point = (3, 4)
x, y = point  # x=3, y=4

# Swapping
a, b = 10, 5
a, b = b, a   # Swap: a=5, b=10

# In loops
students = [('Alice', 85), ('Bob', 92)]
for name, score in students:
    print(f"{name}: {score}")
```

---

### Tuples vs Lists

| Feature | Tuple | List |
|---------|-------|------|
| Mutable | No | Yes |
| Syntax | `(1, 2, 3)` | `[1, 2, 3]` |
| Methods | 2 | 11 |
| Speed | Faster | Slower |
| Dict key | Yes | No |
| Use case | Fixed data | Changing data |

---

### Quick Reference

```python
# Create
t = (1, 2, 3)
single = (5,)

# Access
t[0]
t[1:3]

# Methods
t.count(2)
t.index(3)

# Operations
len(t)
x in t
min(t)

# Unpack
x, y, z = (1, 2, 3)
```

**Remember:**
- Immutable = safe and fast
- Use for fixed data/structures
- Perfect for coordinates, records

---

## Understanding Tuple Immutability

<div align="center">

![Python Tuple Immutable Fixed Elements](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.8.jpg)

*Hash table data structure: immutability enables tuples to be used as hash keys, providing constant-time lookups*

</div>

### Introduction
Immutability (write-once, read-forever) is a core functional programming principle for data protection. It prevents bugs from unexpected modifications. Analogy: a signed contract or blockchain records guarantee integrity.

Immutability enables **hashing**, converting data to a fixed number. This allows O(1) dictionary lookups. Only immutable objects can be hashed because their content never changes.

### What Immutability Means
```python
# Lists: Mutable
lst = [1, 2, 3]; lst[0] = 100; lst.append(4)

# Tuples: Immutable
tup = (1, 2, 3); # tup[0] = 100 # TypeError; # tup.append(4) # AttributeError
```
**Cannot:** Modify, add, remove elements, or sort/reverse in place.
**Can:** Access elements, slice (creates new tuple), concatenate (creates new tuple), iterate.

### Why Immutability Matters
1.  **Data Integrity:** Tuples protect critical data from accidental modification.
    `CONFIG = ('localhost', 5432, 'mydb') # Cannot accidentally modify.`
2.  **Dictionary Keys:** Tuples are hashable (if all elements are immutable).
    `locations = {(0, 0): 'origin', (10, 20): 'point'}`
3.  **Performance:** Faster than lists and use less memory.
4.  **Thread Safety:** Cannot be modified, thus safe across threads, preventing race conditions.

### Shallow vs Deep Immutability
**Shallow Immutability (Python's Default):** The tuple's *structure* is immutable, but if it contains *mutable objects* (like lists), those nested objects can still be changed.
```python
data = (1, 2, [3, 4])
# data[2] = [5, 6]  # TypeError (cannot reassign tuple element)
data[2].append(5) # Works! (modifies internal list)
```
**True Immutability:** Achieved by ensuring all elements within a tuple (and any nested structures) are also immutable.
```python
truly_immutable = (
    'string',           # immutable
    42,                 # immutable
    (1, 2, 3),          # immutable tuple
    frozenset([1, 2])   # immutable set
)
```

### Hashability
**Hashable = Can Be Dict Key:** Tuples of immutable types are hashable.
```python
point = (10, 20); hash(point) # Works
locations = {point: 'home'}   # Works
bad = (1, [2, 3]); # hash(bad) # TypeError (tuple with mutable is not hashable)
```
**Applications:** Coordinate keys (`grid = {(0, 0): 'start'}`), caching (`cache = {(arg1, arg2): result}`), graph edges (`edges = {('A', 'B'): 5}`).

### Common Patterns
*   **Constants:** `DAYS = ('Mon', ...)`
*   **Configuration:** `DB_CONFIG = ('postgresql', 'localhost', 5432, 'mydb')`
*   **Records:** `student = (101, 'Alice', 3.8)`
*   **Multiple Returns:** `def get_stats(nums): return min(nums), max(nums), sum(nums)/len(nums)`

### Working Around Immutability
**Creating Modified Copies:** To "change" a tuple, create a new one.
```python
original = (1, 2, 3, 4)
modified = original[:2] + (999,) + original[3:] # Replace 3rd element with 999
```
**When to Use Lists Instead:** For frequent modifications, start with a list, then convert to a tuple when done.
```python
data = [1, 2, 3]; data.append(4); data[0] = 100
final = tuple(data)
```

### Common Pitfalls
1.  **Assuming Deep Immutability:** `config = (1, [2, 3]); config[1].append(4)` *works* because the nested list is mutable.
2.  **Single-Element Tuple:** `(5)` is an `int`, `(5,)` is a `tuple` (note the comma).
3.  **Unhashable Tuples:** A tuple containing mutable elements (e.g., `(1, [2, 3])`) cannot be used as a dictionary key or set element.

### Quick Comparison
| Feature | Tuple | List |
|---------|-------|------|
| Mutable | No | Yes |
| Hashable | If all elements are | No |
| Dict key | Yes (if hashable) | No |
| Speed | Faster | Slower |
| Memory | Less | More |
| Use for | Fixed data | Changing data |

### Key Takeaways
**Immutability Benefits:** ✓ Data integrity, ✓ Dict keys, ✓ Better performance, ✓ Thread safe, ✓ Clear intent.

**Remember:** Tuple structure is immutable, but contents may contain mutable objects. Only immutable tuples are hashable. Use for safety and performance; create new tuples instead of modifying.

**When to Use:** Constants, configuration, dictionary keys, function return values, data that shouldn't change, and when hashability is required.