# Implement String Data Types

## Introduction
Strings represent text data in Python. They're one of the most commonly used data types.

---

<div align="center">

![Python String Character Indexing](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.5.png)

*Strings are immutable sequences of characters — Python provides rich methods for slicing, searching, and transforming text*

</div>

---

## Understanding Strings

### Why "String"?

The term "string" comes from "string of characters" - imagine beads on a string, where each bead is a character. A string is a **sequence** of characters strung together:

"Hello" = ['H', 'e', 'l', 'l', 'o']

### From Numbers to Text

Remember: computers only understand numbers (binary). So how do they store text?

Each character is assigned a number:
- 'A' = 65
- 'B' = 66
- 'a' = 97
- '!' = 33

This mapping is called **character encoding**:
- **ASCII** (1960s): 128 characters - English letters, digits, symbols
- **Unicode** (1991): 1 million+ characters - emoji, Chinese, Arabic, math symbols 😊

Python 3 uses Unicode by default - that's why you can write: `message = "Hello 世界 🌍"`

### Strings Are Immutable

**Important concept**: Once created, strings cannot be changed. Every "modification" creates a **new** string:

```python
name = "Alice"
name = name.upper()  # Creates new string "ALICE"
# The original "Alice" is discarded
```

Think of strings like **printed words on paper** - you can't erase and change them, you must print a new paper.

**Why immutable?**
- **Safety**: No accidental changes
- **Efficiency**: Same string can be reused in memory
- **Hashable**: Can be used as dictionary keys (you'll learn later)

### Real-World Analogy

A string is like a **text message**:
- Made up of individual characters (letters, numbers, spaces, emojis)
- Has a specific order (sequence matters!)
- Once sent, you can't edit it - only send a new message

## Creating Strings

### Using Quotes
```python
# Single quotes
name = 'Alice'

# Double quotes
message = "Hello, World!"

# Both work the same!
```

### Quotes Inside Strings
```python
# Use opposite quote type
text1 = "He said, 'Hello!'"
text2 = 'She said, "Hi!"'

# Or escape with backslash
text3 = "He said, \"Hello!\""
```

### Multi-line Strings
```python
long_text = """This is a
multi-line string that
spans several lines."""
```

---

## String Operations

### Concatenation (+)
```python
first = "Hello"
second = "World"
result = first + " " + second
print(result)  # "Hello World"
```

**Can't mix strings and numbers**:
```python
age = 25
# message = "Age: " + age  # ❌ Error!
message = "Age: " + str(age)  # ✅ Works!
```

### Repetition (*)
```python
laugh = "ha" * 3
print(laugh)  # "hahaha"

line = "=" * 20
print(line)  # "===================="
```

### Length
```python
name = "Alice"
length = len(name)
print(length)  # 5
```

---

## String Methods

### Upper and Lower Case
```python
name = "Alice"
print(name.upper())  # "ALICE"
print(name.lower())  # "alice"
```

### Strip Whitespace
```python
text = "  hello  "
print(text.strip())  # "hello"
```

### Replace
```python
text = "Hello World"
new_text = text.replace("World", "Python")
print(new_text)  # "Hello Python"
```

### Check Contents
```python
email = "user@example.com"
print(email.startswith("user"))  # True
print(email.endswith(".com"))    # True
print("@" in email)              # True
```

---

## Practical Examples

### Example 1: Full Name
```python
first_name = "Alice"
last_name = "Johnson"
full_name = first_name + " " + last_name
print(full_name)  # "Alice Johnson"
```

### Example 2: Email Creation
```python
username = "alice"
domain = "example.com"
email = username + "@" + domain
print(email)  # "alice@example.com"
```

---

## Key Takeaways
1. Strings are text in quotes
2. Use `+` to concatenate
3. Use `*` to repeat
4. Can't directly mix strings and numbers
5. Many useful string methods available
6. Use `len()` to get length


---

# Format Output with F-strings

## Introduction
F-strings (formatted string literals) provide a clean, readable way to embed expressions in strings.

---

<div align="center">

![Python f-string Formatting](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.10.png)

*F-strings embed expressions inside {curly braces} within strings — Python evaluates them at runtime for clean, readable output*

</div>

---

### The Evolution of String Formatting

Python has had multiple ways to format strings over its history:

**1991 - Python 1.x**: String concatenation
```python
"Hello " + name + ", you are " + str(age)
```
- Simple but clunky
- Must convert types manually
- Hard to read with many variables

**1997 - Python 1.5**: % formatting (borrowed from C)
```python
"Hello %s, you are %d" % (name, age)
```
- More powerful but cryptic
- Hard to remember format codes (%s, %d, %f)

**2006 - Python 2.6/3.0**: .format() method
```python
"Hello {}, you are {}".format(name, age)
```
- More flexible and readable
- But still verbose

**2015 - Python 3.6**: F-strings (PEP 498)
```python
f"Hello {name}, you are {age}"
```
- Concise, readable, and fast
- Expressions evaluated at runtime
- Now the standard way

### Why F-strings Matter

**The problem**: Building strings with variables was always awkward in Python. Consider:
```python
name = "Alice"
score = 95
level = 10
message = "Player " + name + " scored " + str(score) + " points at level " + str(level) + "!"
# Hard to read, easy to make mistakes!
```

**The solution**: F-strings use **template syntax** - placeholders that get filled in:
```python
message = f"Player {name} scored {score} points at level {level}!"
# Clean, readable, obvious!
```

This is called **string interpolation** - inserting variable values into a string template.

### Real-World Analogy

Think of f-strings like **mail merge** in word processing:
- You write a letter template: "Dear {name}, you won {prize}!"
- The program fills in the blanks for each person
- Same template, different data

Or think of them like **Mad Libs**:
- Template: "The {adjective} {noun} {verb} the {noun}!"
- Fill in: "The big dog ate the sandwich!"

F-strings are Mad Libs for programmers - templates with blanks to fill.

### Why Called "F-strings"?

The `f` stands for **"formatted string"**. The `f` prefix tells Python:
- This string has placeholders: `{}`
- Evaluate the expressions inside: `{x + 1}`
- Format them into the final string

---

## Basic F-string Syntax

### Simple Variable Embedding
```python
name = "Alice"
age = 25
message = f"Hello, {name}! You are {age} years old."
print(message)
# Output: Hello, Alice! You are {25 years old.
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

### Comparisons
```python
age = 20
print(f"Adult: {age >= 18}")
# Output: Adult: True
```

---

## Number Formatting

### Decimal Places
```python
pi = 3.14159265
print(f"Pi: {pi:.2f}")   # Pi: 3.14
print(f"Pi: {pi:.4f}")   # Pi: 3.1416
```

### Width and Alignment
```python
num = 42
print(f"{num:5}")    # "   42" (width 5, right-aligned)
print(f"{num:<5}")   # "42   " (left-aligned)
print(f"{num:^5}")   # " 42  " (centered)
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

### Example 2: Student Report
```python
name = "Alice Johnson"
math_score = 95
english_score = 87
average = (math_score + english_score) / 2

print(f"Student: {name}")
print(f"Math: {math_score}")
print(f"English: {english_score}")
print(f"Average: {average:.1f}")
```

# Output: 25°C = 77.0°F

---

## Comparing Formatting Methods

### Concatenation (Old)
```python
name = "Bob"
age = 30
# Hard to read, error-prone
msg = "Hi " + name + ", you are " + str(age)
```

### .format() (Older)
```python
msg = "Hi {}, you are {}".format(name, age)
# Better, but verbose
```

### F-strings (Modern) ✅
```python
msg = f"Hi {name}, you are {age}"
# Clean and readable!
```

---

## The Philosophy Behind F-strings

**Python's Zen**: "Readability counts"

Compare these approaches:
```python
# Concatenation - error-prone
msg = "Total: $" + str(price * qty) + " for " + str(qty) + " items"

# F-string - clear intent
msg = f"Total: ${price * qty} for {qty} items"
```

F-strings embody Python's philosophy:
- **Readable**: Looks like what it does
- **Concise**: No boilerplate
- **Powerful**: Can embed any expression
- **Fast**: Evaluated at compile-time (faster than .format())

### Why This Matters for You

As a programmer, you'll spend more time reading code than writing it. F-strings make your intent obvious:
- What's the template?
- Where do values get inserted?
- What calculations happen?

All visible at a glance.

**Best practice**: Use f-strings as your default string formatting method in Python 3.6+. Only use older methods when working with legacy code.

---

## Key Takeaways
1. **F-strings are Python 3.6+'s modern solution** to string formatting
2. **Start with `f`** before quotes: `f"..."`
3. **Use `{}` for interpolation** - embed variables and expressions
4. **Can evaluate expressions**: `{price * quantity}`, `{name.upper()}`
5. **Format specifiers**: `:.2f` (decimals), `:,` (thousands), `:^5` (alignment)
6. **More readable than alternatives** - concatenation, %, .format()
7. **String interpolation** - fill template placeholders with values
8. **Python philosophy**: Readability counts, explicit is better than implicit


---

## Creating and Initializing Dictionaries for Key-Value Pairs

---

## Introduction

Dictionaries represent **associative arrays** - one of computer science's most powerful abstractions. They solve the fundamental problem: "How do I organize data by **meaning** instead of position?" This is the **hash map** - arguably the **most important data structure** after arrays!

---

<div align="center">

![Python Dictionary Key-Value Pairs](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.18.png)

*Dictionaries are hash tables — keys are hashed to array indices for O(1) access to their associated values*

</div>

---

### Why Dictionaries Are Revolutionary

**Before dictionaries** (positional thinking): Data organized by **index** - you must remember positions:
```python
# Student data as list - fragile!
student = ['Alice', 22, 'A', 'alice@email.com']
# What's index 2? Grade or email? Have to remember!
print(student[2])  # Is this grade or something else?
# Change order → entire code breaks!
```

**With dictionaries** (semantic thinking): Data organized by **meaning** - self-documenting:
```python
# Student data as dict - robust!
student = {'name': 'Alice', 'age': 22, 'grade': 'A', 'email': 'alice@email.com'}
print(student['grade'])  # Crystal clear!
# Add fields, reorder → nothing breaks!
```

**Real-world impact**: **Modern programming is impossible without dictionaries**! JSON (internet's data format), databases (key-value stores), caching (Redis, Memcached), configuration files - **all dictionaries**!

### Historical Context

**Associative arrays** concept from **SNOBOL** (1962) and **AWK** (1977). Python's `dict` (1991) implemented as **hash table** - same technology behind **databases**, **caches**, and **internet routing**!

**Why called "dictionary"**: **Guido van Rossum** chose the name to make it intuitive - "look up word (key) to find meaning (value)" - brilliant naming!

**Evolution**: Python 3.7 (2018) made dicts **ordered** (preserve insertion order) - combining hash table speed with list predictability. Best of both worlds!

### Real-World Analogies

**Dictionary like phonebook**:
- **Key**: Person's name
- **Value**: Phone number
- **Lookup**: "Alice" → find phone number
- **O(1)**: Direct access, no need to read entire book!

**Or like student ID system**:
- **Key**: Student ID (unique identifier)
- **Value**: Student record (name, grades, etc.)
- **Fast**: Swipe ID → instant record retrieval
- **Database**: Exactly how databases work internally!

**Or like variable names in code**:
```python
x = 10  # 'x' is KEY, 10 is VALUE
```
**Mind-blowing**: Programming languages use **symbol tables** (dictionaries!) to map variable names to values. **You've been using dictionaries all along** without knowing it!

**Or like airport code system**:
- **Key**: Airport code ('LAX', 'JFK', 'BOM')
- **Value**: Airport info (location, gates, etc.)
- **Instant lookup**: Type 'BOM' → Mumbai airport details!

### The Hash Table Foundation

**How dicts achieve O(1) lookup** (same as sets!):

**Step 1: Hash the key**
```python
hash('name')  # → 567890123 (huge number)
hash('age')   # → 234567890
# Each key → unique position
```

**Step 2: Store at computed index**
```python
# Internal array:
# Position 890: ('name', 'Alice')
# Position 567: ('age', 22)
```

**Step 3: Instant retrieval**
```python
student['name']
# hash('name') → 890
# Look at position 890 → 'Alice'
# O(1) - constant time!
```

**This is why keys must be immutable** (hashable) - if key changes, hash changes → can't find the value! Lists/sets/dicts are mutable → can't be keys. Strings/numbers/tuples are immutable → perfect keys!

### The Self-Documenting Power

**Lists require external documentation**:
```python
# Need comments to explain:
config = [True, 'postgresql', 5432, 100]
#        [debug, db_type,     port, max_conn]
# Change order → disaster!
```

**Dicts ARE the documentation**:
```python
# Self-explanatory:
config = {
    'debug': True,
    'database': 'postgresql',
    'port': 5432,
    'max_connections': 100
}
# Can add, remove, reorder - still works!
```

**This is "self-documenting code"** - code that explains itself without comments!

---

### What Is a Dictionary?

A **dictionary** stores data as **key-value pairs**. Think of it like a real dictionary: you look up a word (key) to find its definition (value).

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}
print(student)
# {'name': 'Alice', 'age': 22, 'grade': 'A'}
```

**Key Properties:**
1. **Key-value pairs** — every entry has a key and a value
2. **Keys must be unique** — no duplicate keys
3. **Keys must be immutable** — strings, numbers, tuples
4. **Values can be anything** — strings, lists, other dicts, etc.
5. **Ordered** (Python 3.7+) — insertion order is preserved

---

### Creating Dictionaries

**Method 1: Curly Braces (Most Common)**
```python
person = {'name': 'Alice', 'age': 25, 'city': 'Mumbai'}

# Numbers as keys
scores = {1: 'Gold', 2: 'Silver', 3: 'Bronze'}

# Mixed key types
data = {'name': 'Bob', 1: 'first', (0, 0): 'origin'}
```

**Method 2: `dict()` Constructor**
```python
# From keyword arguments
person = dict(name='Alice', age=25, city='Mumbai')

# From list of tuples
pairs = [('name', 'Alice'), ('age', 25), ('city', 'Mumbai')]
person = dict(pairs)

# From zip
keys = ['name', 'age', 'city']
values = ['Alice', 25, 'Mumbai']
person = dict(zip(keys, values))
```

**Method 3: Empty Dictionary**
```python
empty = {}          # curly braces
empty = dict()      # constructor
print(type(empty))  # <class 'dict'>
```

---

### Dictionary Comprehension

Create dictionaries using a compact expression:

```python
# Squares of numbers
squares = {x: x**2 for x in range(1, 6)}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# From two lists
names = ['Alice', 'Bob', 'Charlie']
ages = [25, 30, 22]
people = {name: age for name, age in zip(names, ages)}
print(people)  # {'Alice': 25, 'Bob': 30, 'Charlie': 22}

# With condition
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
print(even_squares)  # {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}
```

---

### `dict.fromkeys()` — Same Value for Multiple Keys

```python
# Initialize with default value
subjects = dict.fromkeys(['Math', 'Science', 'English'], 0)
print(subjects)  # {'Math': 0, 'Science': 0, 'English': 0}

# Default is None if not specified
fields = dict.fromkeys(['name', 'age', 'email'])
print(fields)  # {'name': None, 'age': None, 'email': None}
```

---

### Keys Must Be Unique

If you use the same key twice, the last value wins:

```python
data = {'a': 1, 'b': 2, 'a': 3}
print(data)  # {'a': 3, 'b': 2}
# The first 'a': 1 is overwritten
```

---

### What Can Be Keys?

**Valid keys (immutable):**
```python
d = {
    'name': 'Alice',      # string
    42: 'answer',          # int
    3.14: 'pi',            # float
    (1, 2): 'coordinates', # tuple
    True: 'yes',           # bool
}
```

**Invalid keys (mutable):**
```python
# d = {[1, 2]: 'list'}    # TypeError — lists are mutable
# d = {{1}: 'set'}        # TypeError — sets are mutable
```

---

### Nested Dictionaries

Dictionaries can contain other dictionaries:

```python
students = {
    'Alice': {'age': 22, 'grade': 'A', 'courses': ['Math', 'Physics']},
    'Bob': {'age': 24, 'grade': 'B', 'courses': ['Chemistry', 'Biology']}
}

print(students['Alice']['grade'])      # 'A'
print(students['Bob']['courses'][0])   # 'Chemistry'
```

---

### Practical Examples

**1. Configuration Settings:**
```python
config = {
    'debug': False,
    'database': 'postgresql',
    'port': 5432,
    'max_connections': 100
}
```

**2. Word Frequency Counter:**
```python
text = "the cat sat on the mat the cat"
words = text.split()
freq = {}
for word in words:
    freq[word] = freq.get(word, 0) + 1
print(freq)  # {'the': 3, 'cat': 2, 'sat': 1, 'on': 1, 'mat': 1}
```

**3. Student Records:**
```python
student = dict.fromkeys(['name', 'age', 'email', 'grade'], '')
student['name'] = 'Alice'
student['age'] = 22
print(student)
```

---

### Quick Reference

| Method | Example | Result |
|--------|---------|--------|
| Literal | `{'a': 1, 'b': 2}` | Dict with 2 pairs |
| `dict()` | `dict(a=1, b=2)` | Same as above |
| From tuples | `dict([('a',1), ('b',2)])` | Same as above |
| From zip | `dict(zip(keys, vals))` | Zipped pairs |
| Comprehension | `{x: x**2 for x in range(3)}` | `{0:0, 1:1, 2:4}` |
| `fromkeys()` | `dict.fromkeys(['a','b'], 0)` | `{'a':0, 'b':0}` |
| Empty | `{}` or `dict()` | Empty dict |

---

### Key Takeaways

1. Dictionaries store **key-value pairs** for structured data
2. Multiple creation methods: `{}`, `dict()`, comprehensions, `fromkeys()`
3. Keys must be **unique** and **immutable**
4. Values can be **any type** including lists and other dicts
5. Order is **preserved** (Python 3.7+)
6. Perfect for **structured data**, **lookups**, and **configurations**


---

## Accessing Dictionary Values Using Keys and get() Method

---

## Introduction

The `[]` vs `.get()` choice revisits **fail-fast vs. fail-safe** design - same philosophy as `remove()` vs `discard()` for sets! This represents different **error handling strategies**: crash loudly to catch bugs, or continue gracefully for robustness.

---

<div align="center">

![Python Dictionary get() Method Access](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.19.png)

*dict[key] and dict.get(key) both look up values by hashing the key — the difference is how they handle missing keys*

</div>

---

### Why Two Access Methods? Design Philosophy

**Why not just one?** Python provides both because **different contexts need different behaviors**:

**`dict[key]` = Fail-Fast:**
- "This key MUST exist!"
- Missing key → **crash with KeyError**
- **Use during development**: Catch bugs immediately
- **Database analogy**: Foreign key constraint - enforces data integrity

**`dict.get(key, default)` = Fail-Safe:**
- "This key might not exist, have a fallback"
- Missing key → **return default value**
- **Use in production**: Graceful degradation
- **Web server analogy**: Return 404 page instead of crashing

**Design wisdom**: One language, two philosophies - use the right tool for the context!

### Real-World Analogies

**`[]` like strict vending machine**:
- **Press A5**: "A5 exists? Dispense!"
- **Press Z9**: "Z9 doesn't exist? ERROR! Shut down!" 💥
- **Strict**: Forces you to know what buttons exist
- **Good for**: Catching configuration errors in development

**`.get()` like friendly assistant**:
- **Ask for A5**: "Here's your snack!"
- **Ask for Z9**: "Sorry, don't have that. Want the default instead?"
- **Graceful**: Provides fallback automatically
- **Good for**: User-facing features in production

**Or `[]` like password check**:
- **Correct password**: Access granted
- **Wrong password**: "Access denied!" (error)
- **No fallback**: Security requires exactness

**`.get()` like settings with defaults**:
- **Setting exists**: Use that value
- **Setting missing**: Use default value
- **Fallback**: System works either way

### The Graceful Degradation Pattern

**Problem without `.get()`** (fragile code):
```python
config = {'theme': 'dark'}

# Attempt 1: Crashes!
font_size = config['font_size']  # KeyError!

# Attempt 2: Verbose!
if 'font_size' in config:
    font_size = config['font_size']
else:
    font_size = 14  # default
# Too much code for simple fallback!
```

**Solution with `.get()`** (elegant code):
```python
config = {'theme': 'dark'}

font_size = config.get('font_size', 14)
# One line! Missing key? Use 14. Beautiful!
```

**This is "graceful degradation"** - system works even with missing data!

### The Word Counting Pattern

**Why `.get()` is perfect for counting**:

**Traditional approach** (complicated):
```python
word_count = {}
for word in words:
    if word in word_count:
        word_count[word] += 1
    else:
        word_count[word] = 1
# lines just to count!
```

**With `.get()`** (elegant):
```python
word_count = {}
for word in words:
    word_count[word] = word_count.get(word, 0) + 1
# lines! .get(word, 0) returns 0 if new word
```

**Even better** (Counter):
```python
from collections import Counter
word_count = Counter(words)
# But understanding .get() helps you understand Counter!
```

---

### Accessing with Square Brackets `[]`

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

print(student['name'])   # Alice
print(student['age'])    # 22
print(student['grade'])  # A
```

**If the key doesn't exist:**
```python
# print(student['email'])  # KeyError: 'email'
```

This crashes your program!

---

### Accessing with `.get()` — Safe Access

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

print(student.get('name'))    # Alice
print(student.get('email'))   # None — no crash!
print(student.get('email', 'not set'))  # 'not set' — custom default
```

**`.get(key, default)` returns:**
- The value if key exists
- `default` if key doesn't exist (defaults to `None`)

---

### When to Use `[]` vs `.get()`

| Situation | Use | Why |
|-----------|-----|-----|
| Key is guaranteed to exist | `dict[key]` | Clear, direct |
| Key might not exist | `dict.get(key)` | Avoids KeyError |
| Need a fallback value | `dict.get(key, default)` | Clean default handling |

```python
config = {'theme': 'dark', 'language': 'en'}

# Key exists — both work
print(config['theme'])         # 'dark'
print(config.get('theme'))     # 'dark'

# Key missing — different behavior
# print(config['font_size'])   # KeyError!
print(config.get('font_size', 14))  # 14
```

---

### Accessing All Keys, Values, and Items

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

# All keys
print(student.keys())    # dict_keys(['name', 'age', 'grade'])

# All values
print(student.values())  # dict_values(['Alice', 22, 'A'])

# All key-value pairs
print(student.items())   # dict_items([('name', 'Alice'), ('age', 22), ('grade', 'A')])
```

Convert to lists if needed:
```python
key_list = list(student.keys())
print(key_list)  # ['name', 'age', 'grade']
```

---

### Checking if a Key Exists

```python
student = {'name': 'Alice', 'age': 22}

# Using 'in'
if 'name' in student:
    print(student['name'])

# Check before access (pattern)
key = 'email'
if key in student:
    print(student[key])
else:
    print(f"'{key}' not found")
```

**Note:** `in` checks **keys**, not values:
```python
print('Alice' in student)  # False — checks keys, not values
print('name' in student)   # True
```

---

### Accessing Nested Dictionaries

```python
data = {
    'user': {
        'name': 'Alice',
        'address': {
            'city': 'Mumbai',
            'zip': '400001'
        }
    }
}

# Chain access
print(data['user']['name'])            # Alice
print(data['user']['address']['city']) # Mumbai
```

**Safe nested access:**
```python
# Using .get() chaining
city = data.get('user', {}).get('address', {}).get('city', 'Unknown')
print(city)  # Mumbai

# If path doesn't exist
phone = data.get('user', {}).get('phone', {}).get('number', 'N/A')
print(phone)  # N/A
```

---

### Practical Examples

**1. User Profile with Defaults:**
```python
profile = {'name': 'Alice', 'theme': 'dark'}

name = profile.get('name', 'Guest')
theme = profile.get('theme', 'light')
lang = profile.get('language', 'en')

print(f"Welcome {name}! Theme: {theme}, Language: {lang}")
# Welcome Alice! Theme: dark, Language: en
```

**2. Counting with `.get()`:**
```python
text = "hello world hello python hello"
word_count = {}
for word in text.split():
    word_count[word] = word_count.get(word, 0) + 1
print(word_count)  # {'hello': 3, 'world': 1, 'python': 1}
```

**3. Configuration with Fallbacks:**
```python
config = {'port': 8080, 'debug': True}

host = config.get('host', 'localhost')
port = config.get('port', 3000)
debug = config.get('debug', False)
print(f"{host}:{port} (debug={debug})")
# localhost:8080 (debug=True)
```

---

### Key Takeaways

1. `dict[key]` — direct access, raises `KeyError` if key missing
2. `dict.get(key)` — safe access, returns `None` if key missing
3. `dict.get(key, default)` — safe access with custom fallback
4. Use `in` to check if a **key** exists before accessing
5. `.keys()`, `.values()`, `.items()` give views of dictionary contents
6. Chain `.get()` for safe nested dictionary access


---

## Adding and Modifying Dictionary Entries

---

## Introduction

Dictionary modification demonstrates **mutable mapping** design - the same operation (`dict[key] = value`) can both add AND modify, making it **context-sensitive**. This represents **upsert semantics**: update if exists, insert if new - a fundamental database concept!

---

<div align="center">

![Python Dictionary Add Update Key Value Pair](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.20.png)

*dict[key] = value performs an upsert: if the key exists, its value is updated; if not, a new entry is inserted*

</div>

---

### Why Unified Add/Modify Syntax is Elegant

**Traditional approach** (separate operations): Different syntax for add vs modify:
```c
// C/Java - verbose!
if (map.containsKey("age")) {
    map.put("age", 23);  // Modify existing
} else {
    map.put("age", 23);  // Add new
}
// Same code repeated! Needless complexity.
```

**Python's approach** (unified): One syntax for both:
```python
student['age'] = 23  # Add OR modify - don't care!
# Simple, clean, no if-check needed!
```

**This is "upsert"** (UPDATE or INSERT) - databases use this pattern! MongoDB's `upsert`, SQL's `INSERT ON DUPLICATE KEY UPDATE` - Python dictionaries had it from day 1 (1991)!

### Real-World Analogies

**Dict modification like updating contacts**:
- **Add new contact**: Save "Bob" → 555-1234
- **Update existing**: "Bob" already exists? Replace with new number
- **Same button**: Phone doesn't ask "add or update?" - just saves!
- **Python dict**: Same syntax for both operations

**Or like whiteboard**:
- **Write "Score: 10"**: First time - creates entry
- **Write "Score: 20"**: Erase old, write new - updates
- **Same action**: Writing on whiteboard - adds or overwrites naturally

**Or like variable assignment**:
```python
x = 10  # First time - creates variable
x = 20  # Next time - updates variable
# Dict operations work the SAME way!
```

### The `setdefault()` Design Genius

**Problem**: Want to initialize if missing, keep if exists:
```python
# Verbose approach:
if 'count' not in data:
    data['count'] = 0
# Now use data['count']
```

**Elegant solution**:
```python
data.setdefault('count', 0)  # Initialize only if new!
# One line, returns the value, perfect!
```

**Why brilliant**: Combines **check + set + get** in one atomic operation. Used constantly for grouping/accumulation patterns!

---

### Adding New Key-Value Pairs

Simply assign a value to a new key:

```python
student = {'name': 'Alice', 'age': 22}

student['grade'] = 'A'
student['email'] = 'alice@mail.com'

print(student)
# {'name': 'Alice', 'age': 22, 'grade': 'A', 'email': 'alice@mail.com'}
```

---

### Modifying Existing Values

Same syntax — if the key exists, the value is overwritten:

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'B'}

student['grade'] = 'A'  # Update grade
student['age'] = 23      # Update age

print(student)  # {'name': 'Alice', 'age': 23, 'grade': 'A'}
```

---

### The `update()` Method

Add or modify **multiple** entries at once:

```python
student = {'name': 'Alice', 'age': 22}

# Update with another dictionary
student.update({'age': 23, 'grade': 'A', 'city': 'Mumbai'})
print(student)
# {'name': 'Alice', 'age': 23, 'grade': 'A', 'city': 'Mumbai'}

# Update with keyword arguments
student.update(email='alice@mail.com', phone='9876543210')
print(student)
```

**Behavior:**
- Existing keys → values are **overwritten**
- New keys → entries are **added**

---

### The `setdefault()` Method

Sets a value **only if the key doesn't already exist**:

```python
config = {'theme': 'dark'}

# Key exists — no change
config.setdefault('theme', 'light')
print(config['theme'])  # 'dark' — unchanged!

# Key doesn't exist — sets it
config.setdefault('language', 'en')
print(config['language'])  # 'en' — newly added
```

**Useful for initialization:**
```python
word_groups = {}
words = ['apple', 'ant', 'banana', 'avocado', 'berry']

for word in words:
    first_letter = word[0]
    word_groups.setdefault(first_letter, []).append(word)

print(word_groups)
# {'a': ['apple', 'ant', 'avocado'], 'b': ['banana', 'berry']}
```

---

### The `|=` Operator (Python 3.9+)

Merge another dictionary into the current one:

```python
defaults = {'theme': 'light', 'lang': 'en', 'font': 14}
user_prefs = {'theme': 'dark', 'font': 16}

defaults |= user_prefs
print(defaults)  # {'theme': 'dark', 'lang': 'en', 'font': 16}
```

---

### Incrementing Values

A common pattern for counters:

```python
# Count character occurrences
text = "hello"
counts = {}
for char in text:
    counts[char] = counts.get(char, 0) + 1
print(counts)  # {'h': 1, 'e': 1, 'l': 2, 'o': 1}
```

---

### Practical Examples

**1. Building a Profile Step by Step:**
```python
profile = {}
profile['name'] = input("Name: ")
profile['age'] = int(input("Age: "))
profile['skills'] = []
profile['skills'].append('Python')
profile['skills'].append('SQL')
```

**2. Merging Configurations:**
```python
default_config = {'debug': False, 'port': 3000, 'host': 'localhost'}
env_config = {'debug': True, 'port': 8080}

final = {**default_config, **env_config}  # env overrides defaults
print(final)  # {'debug': True, 'port': 8080, 'host': 'localhost'}
```

**3. Accumulating Data:**
```python
sales = {}
transactions = [('Alice', 100), ('Bob', 200), ('Alice', 150), ('Bob', 50)]

for name, amount in transactions:
    sales[name] = sales.get(name, 0) + amount

print(sales)  # {'Alice': 250, 'Bob': 250}
```

---

### Summary Table

| Method | Adds New? | Overwrites? | Multiple? |
|--------|-----------|-------------|-----------|
| `d[key] = val` | Yes | Yes | No |
| `d.update(...)` | Yes | Yes | Yes |
| `d.setdefault(key, val)` | Yes | **No** | No |
| `d \|= other` | Yes | Yes | Yes |

---

### Key Takeaways

1. `dict[key] = value` — adds or overwrites a single entry
2. `dict.update()` — adds/overwrites multiple entries at once
3. `dict.setdefault()` — only sets if key is missing (safe initialization)
4. Use `.get()` with increment for counting patterns
5. Use `{**d1, **d2}` or `|=` for merging dictionaries


---

## Removing Entries Using pop(), popitem(), and del

---

## Introduction

Multiple removal methods reflect **different use cases**: `del` for simple removal, `pop()` for removal + retrieval, `popitem()` for stack-like (LIFO) behavior. This represents **API granularity** - providing specific tools for specific patterns rather than one generic method.

---

<div align="center">

![Python Dictionary pop() Remove Entry](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.21.png)

*del removes by key, pop() removes and returns the value, popitem() removes the last inserted pair (LIFO)*

</div>

---

### Why Multiple Removal Methods?

**del** = **Statement** (not a method): Low-level, like variable deletion
**pop()** = **Retrieval + removal**: Get value before deleting (common pattern!)
**popitem()** = **LIFO stack**: Process items in reverse insertion order
**clear()** = **Bulk operation**: Empty everything at once

**Each serves a distinct purpose** - Python optimizes for common patterns!

### Real-World Analogies

**del like permanent deletion**: "Delete this file" → Gone forever, no undo
**pop() like withdrawal**: "Withdraw $50 from account" → Get money AND remove from balance
**popitem() like todo list**: "Take last item added" → LIFO (Last In, First Out) - like undoing recent changes
**clear() like factory reset**: "Erase all data" → Start fresh

---

### Three Ways to Remove Dictionary Entries

---

### `del` Statement

Removes a specific key-value pair:

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

del student['grade']
print(student)  # {'name': 'Alice', 'age': 22}
```

**If the key doesn't exist:**
```python
# del student['email']  # KeyError: 'email'
```

**Delete the entire dictionary:**
```python
del student  # student no longer exists
# print(student)  # NameError: name 'student' is not defined
```

---

### `pop()` Method

Removes and **returns** the value:

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

removed_grade = student.pop('grade')
print(removed_grade)  # 'A'
print(student)        # {'name': 'Alice', 'age': 22}
```

**With a default (no error if key missing):**
```python
result = student.pop('email', 'not found')
print(result)  # 'not found' — no KeyError!
```

**Without a default (error if key missing):**
```python
# student.pop('email')  # KeyError: 'email'
```

---

### `popitem()` Method

Removes and returns the **last** inserted key-value pair (as a tuple):

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

last = student.popitem()
print(last)     # ('grade', 'A')
print(student)  # {'name': 'Alice', 'age': 22}
```

**On empty dict:**
```python
# {}.popitem()  # KeyError: 'popitem(): dictionary is empty'
```

---

### `clear()` Method

Removes **all** entries (keeps the dict object):

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

student.clear()
print(student)  # {}
print(type(student))  # <class 'dict'> — still a dict, just empty
```

---

### Comparison Table

| Method | Returns Value? | Key Required? | Error if Missing? |
|--------|---------------|---------------|-------------------|
| `del d[key]` | No | Yes | Yes (KeyError) |
| `d.pop(key)` | Yes | Yes | Yes (unless default given) |
| `d.pop(key, default)` | Yes (or default) | Yes | No |
| `d.popitem()` | Yes (last pair) | No | Yes (if empty) |
| `d.clear()` | No | No | No |

---

### Safe Deletion Patterns

**Pattern 1: Check before delete**
```python
if 'email' in student:
    del student['email']
```

**Pattern 2: pop with default**
```python
email = student.pop('email', None)
if email:
    print(f"Removed: {email}")
```

**Pattern 3: Delete multiple keys**
```python
data = {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
keys_to_remove = ['b', 'd', 'f']

for key in keys_to_remove:
    data.pop(key, None)  # Safe — ignores missing keys

print(data)  # {'a': 1, 'c': 3, 'e': 5}
```

---

### Practical Examples

**1. Remove Sensitive Data Before Logging:**
```python
user_data = {'name': 'Alice', 'email': 'alice@mail.com',
             'password': 'secret123', 'ssn': '123-45-6789'}

sensitive_keys = ['password', 'ssn']
safe_data = user_data.copy()
for key in sensitive_keys:
    safe_data.pop(key, None)

print(f"Logging: {safe_data}")
# Logging: {'name': 'Alice', 'email': 'alice@mail.com'}
```

**2. Process and Remove Items:**
```python
tasks = {'task_1': 'Buy groceries', 'task_2': 'Clean house', 'task_3': 'Study'}

while tasks:
    task_id, description = tasks.popitem()
    print(f"Completed: {description}")

# Completed: Study
# Completed: Clean house
# Completed: Buy groceries
```

**3. Filter Out Entries:**
```python
scores = {'Alice': 92, 'Bob': 45, 'Charlie': 78, 'Diana': 38, 'Eve': 85}

# Remove failing students (below 50)
failing = [name for name, score in scores.items() if score < 50]
for name in failing:
    del scores[name]

print(f"Passing: {scores}")
# Passing: {'Alice': 92, 'Charlie': 78, 'Eve': 85}
```

---

### Key Takeaways

1. `del d[key]` — simple removal, no return value, errors on missing key
2. `pop(key)` — removes and returns the value, can provide a default
3. `popitem()` — removes and returns the last pair (LIFO order)
4. `clear()` — empties the entire dictionary
5. Use `pop(key, None)` for safe removal without error checking
6. Never modify a dict while iterating — collect keys first, then remove


---

## Checking for Key Existence Using the 'in' Keyword

---

## Introduction

The `in` operator provides **O(1) membership testing** for dictionaries - same hash table magic as sets! This enables **defensive programming**: check before access to prevent crashes. It's the **look before you leap (LBYL)** approach vs. Python's preferred **easier to ask forgiveness (EAFP)** with try-except.

---

<div align="center">

![Python Dictionary Key Existence Check](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.22.jpg)

*The `in` keyword uses hash lookup — O(1) for dicts/sets vs O(n) for lists, making it ideal for membership checks*

</div>

---

### Why Key Checking Matters

**LBYL** (Look Before You Leap): Check first, act second
```python
if key in dict:  # Check
    value = dict[key]  # Act
```

**EAFP** (Easier to Ask Forgiveness than Permission): Try, catch errors
```python
try:
    value = dict[key]  # Act
except KeyError:  # Handle failure
    value = default
```

**Python prefers EAFP**, but `in` is cleaner for simple existence checks!

---

### The `in` Operator for Dictionaries

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

print('name' in student)    # True
print('email' in student)   # False
print('email' not in student)  # True
```

**Important:** `in` checks **keys**, not values.

```python
print('Alice' in student)  # False — 'Alice' is a value, not a key
print('name' in student)   # True — 'name' is a key
```

---

### Why Check for Keys?

Without checking, accessing a missing key crashes your program:

```python
data = {'x': 10}

# DANGEROUS
# print(data['y'])  # KeyError!

# SAFE — check first
if 'y' in data:
    print(data['y'])
else:
    print("Key 'y' not found")
```

---

### `in` vs `.get()` vs `try/except`

Three approaches to handle missing keys:

**Approach 1: `in` check**
```python
if 'email' in user:
    email = user['email']
else:
    email = 'N/A'
```

**Approach 2: `.get()` with default**
```python
email = user.get('email', 'N/A')
```

**Approach 3: `try/except`**
```python
try:
    email = user['email']
except KeyError:
    email = 'N/A'
```

**When to use which:**
- `in` — when you need different logic based on existence
- `.get()` — when you just need a value with a fallback
- `try/except` — when KeyError is rare (follows EAFP principle)

---

### Checking Keys in Nested Dicts

```python
data = {
    'user': {
        'name': 'Alice',
        'settings': {'theme': 'dark'}
    }
}

# Check at each level
if 'user' in data:
    if 'settings' in data['user']:
        if 'theme' in data['user']['settings']:
            print(data['user']['settings']['theme'])
```

**Cleaner with `.get()`:**
```python
theme = data.get('user', {}).get('settings', {}).get('theme', 'light')
print(theme)  # 'dark'
```

---

### Checking Values (Not Keys)

To check if a **value** exists:

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

# Check in values
print('Alice' in student.values())  # True
print(22 in student.values())       # True
print('B' in student.values())      # False
```

---

### Common Patterns

**1. Conditional Update:**
```python
config = {'port': 8080}

if 'host' not in config:
    config['host'] = 'localhost'
```

**2. Counting (check-and-increment):**
```python
text = "hello world hello"
counts = {}
for word in text.split():
    if word in counts:
        counts[word] += 1
    else:
        counts[word] = 1
print(counts)  # {'hello': 2, 'world': 1}
```

**3. Grouping:**
```python
students = [('Alice', 'Math'), ('Bob', 'Science'), ('Alice', 'Science')]
groups = {}
for name, subject in students:
    if name not in groups:
        groups[name] = []
    groups[name].append(subject)
print(groups)  # {'Alice': ['Math', 'Science'], 'Bob': ['Science']}
```

**4. Avoid Duplicates:**
```python
seen = {}
items = [('a', 1), ('b', 2), ('a', 3)]
for key, val in items:
    if key not in seen:
        seen[key] = val  # Keep first occurrence only
print(seen)  # {'a': 1, 'b': 2}
```

**5. Required Fields Validation:**
```python
required = {'name', 'email', 'password'}
form_data = {'name': 'Alice', 'email': 'alice@mail.com'}

missing = [field for field in required if field not in form_data]
if missing:
    print(f"Missing fields: {missing}")
else:
    print("All fields present!")
# Missing fields: ['password']
```

---

### Performance

Key lookup with `in` is **O(1)** on average — just like sets. This is because dictionaries use hash tables internally.

```python
# Fast even for large dicts
big_dict = {i: i**2 for i in range(1_000_000)}
999_999 in big_dict  # Instant!
```

---

### Key Takeaways

1. `key in dict` checks if a **key** exists — O(1) operation
2. `in` does NOT check values — use `val in dict.values()` for that
3. Use `in` when you need conditional logic based on key existence
4. Use `.get()` when you just need a fallback value
5. Always check before `del` or `[]` access to avoid `KeyError`


---

## Iterating Through Dictionary Keys, Values, and Items

---

## Introduction

Dictionary iteration offers **three views** - keys, values, or items (pairs) - representing different **perspectives on the same data**. This demonstrates **dictionary views**: efficient, dynamic representations that update with the dictionary. They're **not copies** - they're **live views** into the dictionary's current state!

---

<div align="center">

![Python Dictionary keys() values() items() Iteration](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.23.png)

*.keys() iterates over keys, .values() over values, .items() over (key, value) tuples — three views of the same data*

</div>

---

### Why Three Iteration Methods?

**Keys only** (.keys() or default): When you just need identifiers
**Values only** (.values()): When you need to aggregate/analyze data
**Key-value pairs** (.items()): When you need context for each value

**All are O(n)** but with different overhead - choose based on what you actually need!

### Dictionary Views Are Live!

**Amazing property**: Views update automatically:
```python
d = {'a': 1}
view = d.items()
d['b'] = 2  # Add new item
# view now shows both items - it's LIVE!
```

**This is lazy evaluation** - no copying, just pointers!

---

### Three Ways to Iterate

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}
```

**1. Iterate over keys (default):**
```python
for key in student:
    print(key)
# name
# age
# grade
```

**2. Iterate over values:**
```python
for value in student.values():
    print(value)
# Alice
# 
# A
```

**3. Iterate over key-value pairs:**
```python
for key, value in student.items():
    print(f"{key}: {value}")
# name: Alice
# age: 22
# grade: A
```

---

### `.keys()` — All Keys

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

keys = student.keys()
print(keys)       # dict_keys(['name', 'age', 'grade'])
print(list(keys)) # ['name', 'age', 'grade']

# Iterate
for key in student.keys():
    print(key, '->', student[key])
```

**Note:** `for key in dict` and `for key in dict.keys()` are identical.

---

### `.values()` — All Values

```python
prices = {'apple': 1.50, 'banana': 0.75, 'cherry': 3.00}

# Sum all values
total = sum(prices.values())
print(f"Total: ${total:.2f}")  # Total: $5.25

# Find max value
most_expensive = max(prices.values())
print(f"Max price: ${most_expensive}")  # $3.00
```

---

### `.items()` — Key-Value Pairs

```python
scores = {'Alice': 92, 'Bob': 78, 'Charlie': 85}

# Unpack into key and value
for name, score in scores.items():
    if score >= 80:
        print(f"{name}: Pass ({score})")
    else:
        print(f"{name}: Needs improvement ({score})")
```

---

### Iterating Over Nested Dictionaries

```python
grades = {
    'Alice': {'Math': 92, 'Science': 88},
    'Bob': {'Math': 78, 'Science': 85}
}

for student, subjects in grades.items():
    print(f"\n{student}:")
    for subject, score in subjects.items():
        print(f"  {subject}: {score}")
```

Output:
Alice:
  Math: 92
  Science: 88

Bob:
  Math: 78
  Science: 85

---

### Building New Dicts While Iterating

**Filtering:**
```python
scores = {'Alice': 92, 'Bob': 45, 'Charlie': 78, 'Diana': 38}

passing = {name: score for name, score in scores.items() if score >= 50}
print(passing)  # {'Alice': 92, 'Charlie': 78}
```

**Transforming:**
```python
prices = {'apple': 1.50, 'banana': 0.75, 'cherry': 3.00}

# Apply 10% discount
discounted = {item: round(price * 0.9, 2) for item, price in prices.items()}
print(discounted)  # {'apple': 1.35, 'banana': 0.68, 'cherry': 2.7}
```

**Inverting (swap keys and values):**
```python
original = {'a': 1, 'b': 2, 'c': 3}
inverted = {v: k for k, v in original.items()}
print(inverted)  # {1: 'a', 2: 'b', 3: 'c'}
```

---

### Sorted Iteration

Dictionaries preserve insertion order, but you can iterate in sorted order:

```python
data = {'banana': 3, 'apple': 5, 'cherry': 1}

# Sort by key
for key in sorted(data):
    print(f"{key}: {data[key]}")

# Sort by value
for key in sorted(data, key=data.get):
    print(f"{key}: {data[key]}")

# Sort by value (descending)
for key in sorted(data, key=data.get, reverse=True):
    print(f"{key}: {data[key]}")
```

---

### Common Patterns

**1. Find Key with Max/Min Value:**
```python
scores = {'Alice': 92, 'Bob': 78, 'Charlie': 85}

top = max(scores, key=scores.get)
bottom = min(scores, key=scores.get)
print(f"Top: {top}, Bottom: {bottom}")
```

**2. Group Items by Value:**
```python
students = {'Alice': 'A', 'Bob': 'B', 'Charlie': 'A', 'Diana': 'B', 'Eve': 'A'}
groups = {}
for name, grade in students.items():
    groups.setdefault(grade, []).append(name)
print(groups)  # {'A': ['Alice', 'Charlie', 'Eve'], 'B': ['Bob', 'Diana']}
```

**3. Enumerate Dict Items:**
```python
colors = {'red': '#FF0000', 'green': '#00FF00', 'blue': '#0000FF'}
for i, (name, code) in enumerate(colors.items(), 1):
    print(f"{i}. {name}: {code}")
```

---

### Key Takeaways

1. `for key in dict` — iterates over keys (most common)
2. `for val in dict.values()` — iterates over values only
3. `for key, val in dict.items()` — iterates over pairs (most flexible)
4. Use comprehensions to filter/transform while iterating
5. Use `sorted()` for ordered iteration
6. Never modify a dict while iterating over it — build a new one instead


---

## Merging Dictionaries Using the update() Method

---

## Introduction

Dictionary merging implements **configuration layering** - a fundamental pattern in software engineering. Defaults → environment → user preferences → command-line arguments - each layer **overrides** the previous. This is how **all modern applications** handle configuration: Docker, Kubernetes, web frameworks, CLI tools!

---

<div align="center">

![Python Merge Dictionaries update() Method](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.2/LO-9.2.24.png)

*update() merges two dictionaries — existing keys get overwritten by the new values, new keys are added*

</div>

---

### Why Merging is Fundamental

**The configuration problem**: Apps need settings from multiple sources with precedence:
1. **Hardcoded defaults** (lowest priority)
2. **Config files** (medium priority)
3. **Environment variables** (higher priority)
4. **Command-line args** (highest priority)

**Python's solution**: Layer dictionaries with later ones overriding earlier ones!
```python
final = {**defaults, **config_file, **env_vars, **cli_args}
# Last value wins - perfect precedence!
```

**This pattern** powers every major Python framework: Django settings, Flask config, pytest configuration!

### The Evolution of Dict Merging

**Python 3.5**: `{**d1, **d2}` unpacking syntax - clean and Pythonic
**Python 3.9**: `|` operator - even cleaner, matches set union semantics!
```python
merged = d1 | d2  # Beautiful!
```

**This shows Python's design evolution** - continually improving ergonomics!

---

### The `update()` Method

Merges another dictionary (or key-value pairs) into the current dictionary:

```python
d1 = {'a': 1, 'b': 2}
d2 = {'b': 3, 'c': 4}

d1.update(d2)
print(d1)  # {'a': 1, 'b': 3, 'c': 4}
```

**Behavior:**
- New keys are **added**
- Existing keys are **overwritten** (last value wins)
- `d1` is modified in place; `d2` is unchanged

---

### Different Ways to Call `update()`

**1. With another dictionary:**
```python
base = {'host': 'localhost', 'port': 3000}
base.update({'port': 8080, 'debug': True})
print(base)  # {'host': 'localhost', 'port': 8080, 'debug': True}
```

**2. With keyword arguments:**
```python
config = {'theme': 'light'}
config.update(font_size=14, language='en')
print(config)  # {'theme': 'light', 'font_size': 14, 'language': 'en'}
```

**3. With a list of tuples:**
```python
data = {}
data.update([('name', 'Alice'), ('age', 25)])
print(data)  # {'name': 'Alice', 'age': 25}
```

---

### Merging Without Modifying Originals

**Using `{**d1, **d2}` unpacking (Python 3.5+):**
```python
defaults = {'theme': 'light', 'lang': 'en', 'font': 14}
user_prefs = {'theme': 'dark', 'font': 18}

merged = {**defaults, **user_prefs}
print(merged)   # {'theme': 'dark', 'lang': 'en', 'font': 18}
print(defaults) # unchanged
print(user_prefs) # unchanged
```

**Using `|` operator (Python 3.9+):**
```python
merged = defaults | user_prefs
print(merged)  # {'theme': 'dark', 'lang': 'en', 'font': 18}
```

**Using `|=` for in-place merge (Python 3.9+):**
```python
defaults |= user_prefs  # modifies defaults
```

---

### Merge Priority (Who Wins?)

The **later** dictionary's values take priority:

```python
d1 = {'x': 1, 'y': 2}
d2 = {'y': 20, 'z': 30}

# d2 overrides d1 for shared keys
merged = {**d1, **d2}
print(merged)  # {'x': 1, 'y': 20, 'z': 30}

# d1 overrides d2 for shared keys
merged = {**d2, **d1}
print(merged)  # {'y': 2, 'z': 30, 'x': 1}
```

---

### Merging Multiple Dictionaries

```python
base = {'a': 1}
overrides1 = {'b': 2, 'a': 10}
overrides2 = {'c': 3, 'b': 20}

# Last one wins for overlapping keys
result = {**base, **overrides1, **overrides2}
print(result)  # {'a': 10, 'b': 20, 'c': 3}

# Or with update (sequential)
final = {}
final.update(base)
final.update(overrides1)
final.update(overrides2)
print(final)  # same result
```

---

### Practical Examples

**1. Configuration Layering:**
```python
# Default < Environment < Command Line
default_config = {'debug': False, 'port': 3000, 'host': 'localhost', 'log': 'info'}
env_config = {'port': 8080, 'log': 'debug'}
cli_config = {'debug': True}

final_config = {**default_config, **env_config, **cli_config}
print(final_config)
# {'debug': True, 'port': 8080, 'host': 'localhost', 'log': 'debug'}
```

**2. Combining API Responses:**
```python
user_basic = {'id': 42, 'name': 'Alice'}
user_profile = {'bio': 'Developer', 'avatar': 'alice.png'}
user_settings = {'theme': 'dark', 'notifications': True}

full_user = {**user_basic, **user_profile, **user_settings}
```

**3. Template with Overrides:**
```python
template = {'font': 'Arial', 'size': 12, 'color': 'black', 'bold': False}

heading_style = {**template, 'size': 24, 'bold': True}
subtitle_style = {**template, 'size': 18, 'color': 'gray'}

print(heading_style)
# {'font': 'Arial', 'size': 24, 'color': 'black', 'bold': True}
```

**4. Merging with Conditional Logic:**
```python
def merge_keep_higher(d1, d2):
    """Merge dicts, keeping the higher value for shared keys."""
    result = d1.copy()
    for key, val in d2.items():
        if key in result:
            result[key] = max(result[key], val)
        else:
            result[key] = val
    return result

scores_round1 = {'Alice': 85, 'Bob': 92, 'Charlie': 78}
scores_round2 = {'Alice': 90, 'Bob': 88, 'Diana': 95}

best_scores = merge_keep_higher(scores_round1, scores_round2)
print(best_scores)
# {'Alice': 90, 'Bob': 92, 'Charlie': 78, 'Diana': 95}
```

---

### Comparison Table

| Method | Modifies Original? | Python Version |
|--------|-------------------|----------------|
| `d1.update(d2)` | Yes | All |
| `{**d1, **d2}` | No (new dict) | 3.5+ |
| `d1 \| d2` | No (new dict) | 3.9+ |
| `d1 \|= d2` | Yes | 3.9+ |

---

### Key Takeaways

1. `update()` merges another dict into the current one (in place)
2. Later values overwrite earlier ones for shared keys
3. `{**d1, **d2}` creates a new merged dict without modifying originals
4. `|` and `|=` are clean syntax for merging (Python 3.9+)
5. Layered configs: defaults → environment → user → cli
6. For custom merge logic (keep max, combine lists), write a function


---

