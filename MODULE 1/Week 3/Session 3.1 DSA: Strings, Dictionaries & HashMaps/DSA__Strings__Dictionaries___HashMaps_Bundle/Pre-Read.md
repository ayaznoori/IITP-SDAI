# Implement String Data Types

## What are Strings?

**Strings** are text data - any characters in quotes.

```python
name = "Alice"
message = 'Hello, World!'
email = "user@example.com"
```

Single `'` or double `"` quotes both work!

### Why Strings Are Different

You've learned numbers (int, float). Why do we need a separate type for text?

**Computers only understand numbers** (0s and 1s). When you write "Hello":
- The computer doesn't see letters
- It sees numbers: [72, 101, 108, 108, 111]
- Each letter has a number code
- Python handles this translation automatically!

This is why you can't do `"5" + 5` - one is the text character "5", the other is the number 5. They're stored completely differently.

### Simple Analogy

Think of a string like a **necklace**:
- Each character is a bead
- The beads are strung together in order
- "Hello" is 5 beads: H-e-l-l-o
- You can't have half a bead (can't split a character)
- The order matters: "Hello" ≠ "olleH"

### Why Order Matters

Unlike numbers (5 + 3 = 3 + 5), strings care about order:
- "cat" ≠ "act"
- "hello" ≠ "olleh"

This is because strings represent **language**, and language has grammar and meaning tied to order.

## String Operations

### Concatenation (Combining)
```python
first = "Hello"
second = "World"
combined = first + " " + second
print(combined)  # "Hello World"
```

### Repetition
```python
laugh = "ha" * 3
print(laugh)  # "hahaha"
```

### Length
```python
name = "Alice"
print(len(name))  # 5
```

## Special Characters
```python
# Use opposite quotes inside
message = "He said, 'Hello!'"
message = 'She said, "Hi!"'

# Or escape quotes
message = "He said, \"Hello!\""
```

## When to Use Strings
✅ Names, messages, text, emails, URLs, addresses

## Try It
```python
first_name = "Alice"
last_name = "Johnson"
full_name = first_name + " " + last_name
print(full_name)
```


---

# Format Output with F-strings

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

Without f-strings, combining text and variables is painful:
```python
name = "Bob"
score = 95
level = 10

# Concatenation - lots of + and str()
msg = "Player " + name + " scored " + str(score) + " at level " + str(level)
# 😩 Hard to read! Easy to mess up!
```

With f-strings, it's natural:
```python
msg = f"Player {name} scored {score} at level {level}"
# 😊 Clean and obvious!
```

### Simple Analogy

Think of f-strings like **filling in blanks**:
- You write a sentence with blanks: "My name is _____ and I'm _____ years old"
- Python fills them in: "My name is Alice and I'm 25 years old"

Or think of them like **customizable t-shirts**:
- Template: "I ❤️ {city}"
- For you: "I ❤️ New York"
- For me: "I ❤️ Boston"
- Same template, different data!

### Why "F" String?

The `f` stands for **"formatted"** - it tells Python: "This string has special formatting with `{}` placeholders."

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

That little `f` makes all the difference!

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

`.2f` means: 2 decimal places, float

## Try It
```python
name = "Bob"
age = 30
city = "Boston"
print(f"{name} is {age} years old and lives in {city}")
```


---

## Creating and Initializing Dictionaries

**Duration:** 5 minutes

---

## What Are Dictionaries?

Dictionaries are Python's **"smart label maker"** - instead of remembering that "position 2 = grade", you just say `student['grade']`! This is **data with meaning** instead of **data in positions**.

### Simple Analogy

Think of dictionaries like **contact list on your phone**:
- **Key**: Person's name ("Mom", "Boss", "Pizza Place")
- **Value**: Phone number, email, address
- **Lookup**: Tap "Mom" → instant info!
- **No positions**: Don't need to remember "Mom is contact #47"

Or like **locker system at gym**:
- **Key**: Locker number (unique ID)
- **Value**: Your stuff inside
- **Access**: Enter #42 → your locker opens
- **Fast**: Don't search all 200 lockers!

### Why This Changes Everything

**The "magic position" problem** (lists):
```python
# You have to MEMORIZE what each position means!
student = ['Alice', 22, 'A', 'alice@email.com']
# 1    2     3
# What's index 2? Grade? Email? Have to remember!
# Someone changes order → everything breaks!
```

**The "labeled data" solution** (dicts):
```python
# Labels tell you EXACTLY what each value means!
student = {
    'name': 'Alice',
    'age': 22,
    'grade': 'A',
    'email': 'alice@email.com'
}
# Crystal clear! Add fields, reorder - still works!
```

**No memorization needed!** Code documents itself.

### The Real-World Connection

**Everything digital uses dictionaries**:
- **JSON** (internet data) → dictionaries
- **Databases** → key-value lookups (dictionaries!)
- **Web APIs** → return dictionaries
- **Configuration files** → dictionaries
- **Your variable names** → Python uses dictionaries internally!

**Mind-blowing**: When you type `my_variable = 10`, Python stores that in a **dictionary** mapping `'my_variable'` → `10`. Dictionaries are **everywhere**!

---

### What You'll Learn

Dictionaries are Python's most versatile data structure. They store data as **key-value pairs** — like a real dictionary where you look up a word (key) to find its definition (value).

---

### Quick Example

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}
print(student['name'])   # Alice
print(student['grade'])  # A
```

Each key maps to a value. Keys are like labels for your data.

---

### Why Not Just Use Lists?

```python
# With a list — what does index 2 mean?
student = ['Alice', 22, 'A']
print(student[2])  # 'A' — but you have to remember that 2 = grade

# With a dict — self-documenting!
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}
print(student['grade'])  # 'A' — clear!
```

---

### Creating Dictionaries

```python
# Method 1: Curly braces
person = {'name': 'Alice', 'age': 25}

# Method 2: dict() constructor
person = dict(name='Alice', age=25)

# Method 3: Empty dictionary
empty = {}
```

---

### Try This

```python
# Create a dictionary for your favorite book
book = {
    'title': 'Your Book Title',
    'author': 'Author Name',
    'year': 2024,
    'rating': 4.5
}
print(book)
print(book['title'])
```

---

### What to Expect

In class, you'll learn:
- All the ways to create dictionaries
- Dictionary comprehensions
- Nested dictionaries
- Rules for keys and values
- Practical applications


---

## Accessing Dictionary Values

**Duration:** 5 minutes

---

## What's the Difference Between `[]` and `.get()`?

These are Python's **"crash vs. continue"** methods - one throws an error if key is missing (strict!), the other gives you a default value (friendly!).

### Simple Analogy

**Using `[]` like strict password system**:
- **Right password**: "Welcome!"
- **Wrong password**: "ERROR! Access denied!" 💥 (program crashes)
- **No second chances**: Must be exact
- **Use when**: Data MUST exist (critical settings)

**Using `.get()` like voice assistant**:
- **Ask for timer**: "Timer is 5 minutes!"
- **Ask for unknown**: "I don't have that. Want the default?"
- **Helpful**: Gives fallback automatically
- **Use when**: Data might be missing (user preferences)

### The "Missing Key" Problem

**Scenario**: User settings where some fields might not exist yet.

**Using `[]` (crashes!):**
```python
settings = {'theme': 'dark'}

# This works:
theme = settings['theme']  # 'dark'

# This crashes!
font_size = settings['font_size']  # KeyError! 💥
# Your program just died!
```

**Using `.get()` (safe!):**
```python
settings = {'theme': 'dark'}

# Both work:
theme = settings.get('theme', 'light')      # 'dark' (exists)
font_size = settings.get('font_size', 14)   # 14 (default!)
# No crashes! Program continues!
```

### When to Use Which?

**Use `[]` when**:
- Key is **guaranteed** to exist
- Missing key = **bug in your code** → crash is good!
- Example: `required_config['database_url']`

**Use `.get()` when**:
- Key **might not** exist
- Missing key = **normal** → use default instead
- Example: `user_prefs.get('language', 'en')`

**Golden rule**: If you'd write `if key in dict:` before accessing, just use `.get()` instead!

---

### Two Ways to Access Values

```python
student = {'name': 'Alice', 'age': 22}

# Way 1: Square brackets — errors if key missing
print(student['name'])  # Alice

# Way 2: .get() — returns None if key missing
print(student.get('name'))    # Alice
print(student.get('email'))   # None (no error!)
print(student.get('email', 'N/A'))  # N/A (custom default)
```

---

### The Problem with `[]`

```python
data = {'x': 10}
# print(data['y'])  # KeyError: 'y' — CRASH!
```

---

### The Solution: `.get()`

```python
data = {'x': 10}
print(data.get('y', 0))  # 0 — safe!
```

---

### Quick Exercise

```python
config = {'port': 8080}

# What does each print?
print(config.get('port', 3000))     # ?
print(config.get('host', 'localhost'))  # ?
```

**Answers:** `8080` (key exists, uses actual value), `localhost` (key missing, uses default)


---

## Adding and Modifying Dictionary Entries

**Duration:** 5 minutes

---

## What Does Add/Modify Mean?

Dictionaries have a **"save button"** that's smart - use `dict[key] = value` and it automatically adds if new or updates if existing! This is **one syntax for two operations**.

### Simple Analogy

**Like editing a document**:
- **Type new heading**: Creates new section
- **Type existing heading**: Replaces old content
- **Same action**: Just type - document figures out add vs update!
- **Dict**: Same with `student['grade'] = 'A'` - adds or updates!

**Or like phone contacts**:
- **Save "Mom"**: First time → creates new contact
- **Save "Mom" again**: Updates existing contact
- **No mode switch**: Phone automatically handles both
- **Dict**: Works identically!

### The "No Error" Magic

**Beautiful thing**: You never get errors!
```python
person = {}  # Empty dict

# Add (first time) - works!
person['name'] = 'Alice'

# Modify (exists) - works!
person['name'] = 'Alice B.'

# Never crashes! Just works!
```

**Compare to lists** (need to check index exists):
```python
items = []
# items[0] = 'x'  # IndexError! List crashes!
items.append('x')  # Must use append for new items
```

**Dicts are more forgiving!**

---

### Adding to a Dictionary

```python
person = {'name': 'Alice'}
person['age'] = 25        # Add new key
person['name'] = 'Alice B.'  # Modify existing key
```

---

### Multiple Updates

```python
person.update({'city': 'Mumbai', 'email': 'alice@mail.com'})
```

---

### Safe Setting

```python
# setdefault — only sets if key doesn't exist
config = {'port': 8080}
config.setdefault('port', 3000)  # No change — key exists
config.setdefault('host', 'localhost')  # Added — key was new
```

---

### Common Pattern: Counting

```python
counts = {}
for item in ['a', 'b', 'a', 'c', 'a']:
    counts[item] = counts.get(item, 0) + 1
# {'a': 3, 'b': 1, 'c': 1}
```

---

### Try This

```python
cart = {}
cart['apple'] = 3
cart['banana'] = 5
cart['apple'] = 10  # What happens?
print(cart)
```

**Answer:** `{'apple': 10, 'banana': 5}` — the value is overwritten.


---

## Removing Dictionary Entries

**Duration:** 5 minutes

---

### Four Ways to Remove

```python
d = {'a': 1, 'b': 2, 'c': 3}

# del — remove by key
del d['a']       # d = {'b': 2, 'c': 3}

# pop() — remove and return
val = d.pop('b') # val = 2, d = {'c': 3}

# popitem() — remove last pair
pair = d.popitem()  # pair = ('c', 3), d = {}

# clear() — remove everything
d = {'x': 1, 'y': 2}
d.clear()  # d = {}
```

---

### Safe Removal

```python
d = {'a': 1}

# pop() with default — no error
val = d.pop('z', None)  # val = None, no crash
```

---

### Key Point

`del` and `pop()` without default raise `KeyError` if the key is missing. Use `pop(key, default)` for safe removal.


---

## Checking for Key Existence

**Duration:** 5 minutes

---

### The `in` Keyword

```python
data = {'name': 'Alice', 'age': 25}

print('name' in data)   # True — key exists
print('email' in data)  # False — key doesn't exist
```

---

### Keys vs Values

```python
# 'in' checks KEYS, not values
print('Alice' in data)          # False — Alice is a value
print('Alice' in data.values()) # True — check values explicitly
```

---

### Why It Matters

```python
data = {'x': 10}

# Without checking:
# print(data['y'])  # KeyError — crash!

# With checking:
if 'y' in data:
    print(data['y'])
else:
    print("Not found")
```

---

### Try This

```python
config = {'debug': True, 'port': 8080}
print('debug' in config)    # ?
print(True in config)       # ?
print('host' not in config) # ?
```

**Answers:** `True`, `False`, `True`


---

## Iterating Through Dictionaries

**Duration:** 5 minutes

---

### Three Iteration Methods

```python
d = {'name': 'Alice', 'age': 25}

# Keys
for key in d:
    print(key)       # name, age

# Values
for val in d.values():
    print(val)       # Alice, 25

# Both
for key, val in d.items():
    print(key, val)  # name Alice, age 25
```

---

### Most Useful: .items()

```python
scores = {'Alice': 90, 'Bob': 80}
for name, score in scores.items():
    print(f"{name} scored {score}")
```

---

### Try Before Class

```python
prices = {'apple': 1.5, 'banana': 0.75, 'cherry': 3.0}

# What does this print?
for item, price in prices.items():
    if price > 1:
        print(item)
```

**Answer:** `apple` and `cherry`


---

## Merging Dictionaries

**Duration:** 5 minutes

---

### Why Merge Dictionaries?

Combining data from multiple sources into one dictionary:

```python
defaults = {'color': 'blue', 'size': 'medium'}
user_choice = {'color': 'red'}

# User choice overrides defaults
final = {**defaults, **user_choice}
print(final)  # {'color': 'red', 'size': 'medium'}
```

---

### Two Main Methods

**`update()` — modifies in place:**
```python
d1 = {'a': 1}
d1.update({'b': 2})
print(d1)  # {'a': 1, 'b': 2}
```

**`{**d1, **d2}` — creates a new dict:**
```python
d1 = {'a': 1}
d2 = {'b': 2}
merged = {**d1, **d2}  # d1 unchanged
```

---

### The Rule: Last One Wins

```python
d1 = {'x': 1}
d2 = {'x': 99}
print({**d1, **d2})  # {'x': 99}
print({**d2, **d1})  # {'x': 1}
```

---

### Try This

```python
base = {'a': 1, 'b': 2}
override = {'b': 20, 'c': 30}
result = {**base, **override}
print(result)  # ?
```

**Answer:** `{'a': 1, 'b': 20, 'c': 30}`


---

