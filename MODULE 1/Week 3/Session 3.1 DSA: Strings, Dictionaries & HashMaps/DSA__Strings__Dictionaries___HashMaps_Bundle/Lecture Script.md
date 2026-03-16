### Implement String Data Types (21 minutes)


### CS Theory Bite

> **Origin**: Text encoding evolved from **ASCII** (1963, 128 characters) to **Unicode** (1991, 150K+ characters). Python 3 strings are Unicode by default — supporting every human language.
>
> **Analogy**: A string is like a **necklace of letter beads** — each character is one bead, and you can slice, count, or rearrange them.
>
> **Why it matters**: Strings are everywhere — user input, file content, API responses, database records all flow as text.


### Hook (3 minutes)

**Say**: "Every app you use has text - usernames, messages, posts. In Python, that's all strings!"

```python
username = "alice_2024"
message = "Hello World!"
email = "user@example.com"
```

### String Basics (6 minutes)

**Say**: "Strings are text in quotes. Use single or double quotes - both work the same!"

```python
name = "Alice"
city = 'New York'
message = "Hello World"

print(type(name))  # <class 'str'>
```

**Key Points**:
- Strings can use "" or ''
- Must match opening and closing quotes
- Use quotes inside strings: "She said 'Hi'"

**String Concatenation**:
```python
first = "John"
last = "Doe"
full_name = first + " " + last  # John Doe
```

### String Methods (6 minutes)

**Say**: "Strings have built-in methods to modify and check text. These are super useful for data cleaning!"

```python
name = "alice"
print(name.upper())      # ALICE
print(name.capitalize()) # Alice
print(name.title())      # Alice

text = "  hello  "
print(text.strip())      # hello (removes spaces)

email = "USER@EXAMPLE.COM"
print(email.lower())     # user@example.com
```

**Real-World**: Always lowercase emails before saving to database!

### Practice (6 minutes)

**Say**: "Let's build an email address from parts."

```python
first = "john"
last = "doe"
domain = "example.com"

# Solution
email = first + "." + last + "@" + domain
print(email)  # john.doe@example.com

# Better way with f-string
email = f"{first}.{last}@{domain}"
print(email)
```



---

### Format Output with F-strings (16 minutes)


### CS Theory Bite

> **Origin**: String formatting evolved from C's `printf()` (1970s) → Python's `%` operator → `.format()` → **f-strings** (Python 3.6, PEP 498). Each generation got more readable.
>
> **Analogy**: F-strings are like **Mad Libs** — the template has blanks `{name}` that get filled in with actual values at runtime.
>
> **Why it matters**: f-strings are the fastest and most readable way to build dynamic strings in Python.


### Hook (3 minutes)

**Say**: "Tired of writing `'text' + str(variable) + 'more text'`? F-strings are cleaner and faster!"

```python
# Old way (messy!)
name = "Alice"
age = 25
print("Hi " + name + ", you are " + str(age))

# F-string way (clean!)
print(f"Hi {name}, you are {age}")
```

### F-string Basics (6 minutes)

**Say**: "Put 'f' before the string, then use {variable} to insert values."

```python
name = "Alice"
age = 25
city = "NYC"

print(f"Hi {name}, age {age}, from {city}")
```

**Key Points**:
- Prefix string with `f` (f-string = formatted string)
- Use `{variable}` to embed variables
- Automatically converts types to strings
- Much cleaner than concatenation

```python
score = 95
total = 100
print(f"You scored {score} out of {total}")
```

**Real-World**: Every modern Python app uses f-strings for output!

### Expressions and Formatting (5 minutes)

**Say**: "F-strings can do calculations and format numbers!"

```python
price = 19.99
quantity = 3

# Expressions inside {}
print(f"Total: ${price * quantity}")

# Format decimals with :.2f
print(f"Price: ${price:.2f}")
print(f"Total: ${price * quantity:.2f}")

# Other examples
name = "alice"
print(f"Hello {name.upper()}")  # ALICE
```

**Formatting Options**:
- `:.2f` - Two decimal places
- `:.0f` - No decimal places
- `:,` - Add commas for thousands

### Practice (2 minutes)

Create formatted output:
```python
first = "John"
last = "Doe"
age = 30
salary = 75000.50

print(f"Name: {first} {last}")
print(f"Age: {age}")
print(f"Salary: ${salary:,.2f}")  # $75,000.50
```


---

## Creating and Initializing Dictionaries


---

### CS Theory Bite

> **Origin**: Dictionaries implement **hash maps** — the most important data structure in computing. Invented for **LISP** (1958) as association lists, evolved into O(1) hash tables. **Python 3.7+** guarantees insertion order.
>
> **Analogy**: A dictionary is like a **real dictionary** — look up a word (key) and instantly find its definition (value). No scanning page by page.
>
> **Why it matters**: Dicts are Python's Swiss Army knife — used for JSON, caching, counting, configuration, and database records.



### Hook (2 minutes)

**Scenario:**

You need to store information about a student — name, age, grade, email, courses. With lists:

```python
student = ['Alice', 22, 'A', 'alice@mail.com', ['Math', 'Physics']]
# What is student[2]? Hard to remember!
```

With a dictionary:
```python
student = {
    'name': 'Alice',
    'age': 22,
    'grade': 'A',
    'email': 'alice@mail.com',
    'courses': ['Math', 'Physics']
}
print(student['grade'])  # 'A' — clear and readable!
```

Dictionaries give your data **meaning** by labeling each value with a key.

---

### Section 1: Creating Dictionaries (3 minutes)

**Curly Braces:**
```python
person = {'name': 'Alice', 'age': 25}

# Numbers as keys
medals = {1: 'Gold', 2: 'Silver', 3: 'Bronze'}

# Empty dictionary
empty = {}
```

**`dict()` Constructor:**
```python
# Keyword arguments (keys must be valid variable names)
person = dict(name='Alice', age=25)

# From list of tuples
person = dict([('name', 'Alice'), ('age', 25)])

# From zip
keys = ['name', 'age', 'city']
vals = ['Alice', 25, 'Mumbai']
person = dict(zip(keys, vals))
```

---

### Section 2: Comprehensions and fromkeys (3 minutes)

**Dictionary Comprehension:**
```python
# Create a mapping of numbers to their squares
squares = {n: n**2 for n in range(1, 6)}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# With filtering
even_sq = {n: n**2 for n in range(10) if n % 2 == 0}
```

**`fromkeys()`** — when all keys share the same initial value:
```python
subjects = dict.fromkeys(['Math', 'Science', 'English'], 0)
# {'Math': 0, 'Science': 0, 'English': 0}
```

---

### Section 3: Keys and Values Rules (3 minutes)

**Keys must be unique:**
```python
d = {'x': 1, 'y': 2, 'x': 3}
print(d)  # {'x': 3, 'y': 2} — last value wins
```

**Keys must be immutable:**
```python
ok = {'name': 1, 42: 2, (1,2): 3}  # strings, ints, tuples — fine
# bad = {[1,2]: 3}  # TypeError — lists can't be keys
```

**Values can be anything:**
```python
data = {
    'name': 'Alice',          # string
    'scores': [85, 92, 78],   # list
    'address': {'city': 'Mumbai', 'zip': '400001'},  # dict
    'active': True             # bool
}
```

---

### Section 4: Nested Dictionaries (2 minutes)

```python
company = {
    'engineering': {
        'Alice': {'role': 'Lead', 'salary': 120000},
        'Bob': {'role': 'Developer', 'salary': 95000}
    },
    'marketing': {
        'Charlie': {'role': 'Manager', 'salary': 100000}
    }
}

# Access nested values
print(company['engineering']['Alice']['role'])  # 'Lead'
```

---

### Section 5: Practical Uses (2 minutes)

**Config files:**
```python
config = dict(debug=False, port=8080, host='localhost')
```

**Counting things:**
```python
text = "apple banana apple cherry banana apple"
freq = {}
for word in text.split():
    freq[word] = freq.get(word, 0) + 1
print(freq)  # {'apple': 3, 'banana': 2, 'cherry': 1}
```

**Mapping data:**
```python
status_codes = {200: 'OK', 404: 'Not Found', 500: 'Server Error'}
print(status_codes[404])  # 'Not Found'
```

---

### Summary (1 minute)

1. Dictionaries store **key-value pairs** — labeled, structured data
2. Create with `{}`, `dict()`, comprehensions, or `fromkeys()`
3. Keys: **unique** and **immutable** (strings, numbers, tuples)
4. Values: **any type** — including lists and nested dicts
5. Dictionaries are the backbone of structured data in Python


---

## Accessing Dictionary Values


---

### CS Theory Bite

> **Origin**: Key access uses **hash functions** for O(1) lookup. `get()` was added for **safe access** — returns a default instead of raising `KeyError`. This is the **LBYL** (Look Before You Leap) approach.
>
> **Analogy**: Direct access `d[key]` is like **demanding a file** — crashes if it doesn't exist. `d.get(key)` is like **politely asking** — returns None if unavailable.
>
> **Why it matters**: `get()` with a default value eliminates entire categories of KeyError bugs.



### Hook (2 minutes)

Your app receives user data from an API:

```python
user_data = {'name': 'Alice', 'age': 25}
```

You try to access the email:
```python
email = user_data['email']  # CRASH! KeyError: 'email'
```

Your entire program stops because one field is missing. This happens in real applications all the time — APIs return partial data, configs are incomplete, user input varies.

The solution? `.get()` — your safety net for dictionary access.

```python
email = user_data.get('email', 'no-email@default.com')
# No crash, has a sensible default
```

---

### Section 1: Square Bracket Access (2 minutes)

```python
student = {'name': 'Alice', 'age': 22, 'grade': 'A'}

# Direct access
print(student['name'])   # Alice
print(student['grade'])  # A

# KeyError if missing
# print(student['email'])  # KeyError: 'email'
```

Use `[]` when you're **certain** the key exists.

---

### Section 2: The .get() Method (3 minutes)

```python
student = {'name': 'Alice', 'age': 22}

# Returns None for missing keys
print(student.get('name'))    # Alice
print(student.get('email'))   # None

# Custom default value
print(student.get('email', 'N/A'))     # N/A
print(student.get('grade', 'Pending')) # Pending
```

**Common pattern — counting:**
```python
counts = {}
words = ['apple', 'banana', 'apple', 'cherry', 'banana', 'apple']
for word in words:
    counts[word] = counts.get(word, 0) + 1
print(counts)  # {'apple': 3, 'banana': 2, 'cherry': 1}
```

---

### Section 3: Checking Key Existence (2 minutes)

```python
data = {'x': 10, 'y': 20}

# 'in' checks keys
print('x' in data)   # True
print(10 in data)     # False — checks keys, not values!

# Pattern: check then access
if 'z' in data:
    print(data['z'])
else:
    print("z not found")
```

---

### Section 4: keys(), values(), items() (3 minutes)

```python
person = {'name': 'Alice', 'age': 25, 'city': 'Mumbai'}

# Keys
for key in person.keys():
    print(key)  # name, age, city

# Values
for val in person.values():
    print(val)  # Alice, 25, Mumbai

# Key-value pairs
for key, val in person.items():
    print(f"{key}: {val}")
```

---

### Section 5: Nested Access (2 minutes)

```python
data = {
    'user': {
        'name': 'Alice',
        'settings': {'theme': 'dark'}
    }
}

# Direct (risky)
print(data['user']['settings']['theme'])  # dark

# Safe (with .get() chain)
theme = data.get('user', {}).get('settings', {}).get('theme', 'light')
print(theme)  # dark
```

---

### Summary (1 minute)

1. `dict[key]` — fast but crashes on missing keys
2. `dict.get(key, default)` — safe, returns default for missing keys
3. `key in dict` — checks key existence
4. `.keys()`, `.values()`, `.items()` — iterate over contents
5. Chain `.get()` for safe nested access


---

## Adding and Modifying Dictionary Entries


---

### CS Theory Bite

> **Origin**: Dict insertion and update are both O(1) average. Python dicts use **dynamic resizing** — when the **load factor** exceeds ~2/3, the hash table doubles in size and rehashes all entries.
>
> **Analogy**: Adding to a dict is like **filing a document** — place it in the right folder (hash bucket). Updating is like **replacing the document** in the same folder.
>
> **Why it matters**: Dicts are mutable by design — build them incrementally as data arrives.



### Hook (2 minutes)

You're building a user registration system. Users fill out a form step by step:

```python
user = {}
user['name'] = 'Alice'          # Step 1
user['email'] = 'alice@mail.com' # Step 2
user['plan'] = 'free'           # Step 3

# User upgrades their plan
user['plan'] = 'premium'        # Overwrite!
```

Dictionaries grow and change — just like real data. Today we learn every way to add and modify entries.

---

### Section 1: Basic Assignment (2 minutes)

```python
d = {'a': 1}

# Add new key
d['b'] = 2
print(d)  # {'a': 1, 'b': 2}

# Modify existing key
d['a'] = 10
print(d)  # {'a': 10, 'b': 2}
```

Same syntax for both — Python decides based on whether the key exists.

---

### Section 2: update() (3 minutes)

```python
user = {'name': 'Alice'}

# Add/modify multiple at once
user.update({'age': 25, 'city': 'Mumbai', 'name': 'Alice B.'})
print(user)  # {'name': 'Alice B.', 'age': 25, 'city': 'Mumbai'}

# With keyword arguments
user.update(email='alice@mail.com')
```

`update()` is like batch processing — one call, many changes.

---

### Section 3: setdefault() (3 minutes)

"Set this value, but ONLY if the key doesn't exist yet."

```python
config = {'port': 8080}

config.setdefault('port', 3000)    # Key exists — no change
config.setdefault('host', 'localhost')  # Key missing — sets it

print(config)  # {'port': 8080, 'host': 'localhost'}
```

**Killer use case — grouping:**
```python
words = ['apple', 'ant', 'banana', 'avocado', 'berry']
groups = {}

for word in words:
    groups.setdefault(word[0], []).append(word)

print(groups)
# {'a': ['apple', 'ant', 'avocado'], 'b': ['banana', 'berry']}
```

---

### Section 4: Counter Pattern (3 minutes)

The most common dict modification pattern:

```python
text = "the cat sat on the mat"
counts = {}
for word in text.split():
    counts[word] = counts.get(word, 0) + 1
print(counts)  # {'the': 2, 'cat': 1, 'sat': 1, 'on': 1, 'mat': 1}
```

Also works for accumulating totals:
```python
sales = {}
data = [('Alice', 100), ('Bob', 200), ('Alice', 50)]
for name, amount in data:
    sales[name] = sales.get(name, 0) + amount
print(sales)  # {'Alice': 150, 'Bob': 200}
```

---

### Section 5: Merging Dicts (1 minute)

```python
d1 = {'a': 1, 'b': 2}
d2 = {'b': 3, 'c': 4}

# Unpacking (creates new dict)
merged = {**d1, **d2}
print(merged)  # {'a': 1, 'b': 3, 'c': 4}

# |= operator (modifies in place, Python 3.9+)
d1 |= d2
print(d1)  # {'a': 1, 'b': 3, 'c': 4}
```

---

### Summary (1 minute)

1. `d[key] = value` — add or overwrite one entry
2. `d.update(...)` — add/overwrite multiple at once
3. `d.setdefault(key, val)` — set only if key is new
4. `.get(key, 0) + 1` — the counter pattern
5. `{**d1, **d2}` — merge dicts (last wins)


---

## Removing Dictionary Entries


---

### CS Theory Bite

> **Origin**: `pop()` returns the removed value (useful for processing), `del` just removes (faster). `popitem()` removes the last inserted pair — useful for **stack-like** dict processing (LIFO since Python 3.7).
>
> **Analogy**: `pop()` is like **taking a book off the shelf and reading the title**. `del` is like **throwing it away without looking**.
>
> **Why it matters**: Choose the right removal method based on whether you need the removed value.



### Hook (2 minutes)

You're processing user data before sending it to a third-party API:

```python
user = {'name': 'Alice', 'email': 'alice@mail.com',
        'password': 'secret123', 'credit_card': '4111-1111-1111-1111'}
```

You need to remove sensitive fields. How?

```python
safe_user = user.copy()
safe_user.pop('password', None)
safe_user.pop('credit_card', None)
print(safe_user)  # {'name': 'Alice', 'email': 'alice@mail.com'}
```

Removing entries is as important as adding them. Today we learn every way to do it.

---

### Section 1: del Statement (2 minutes)

```python
d = {'a': 1, 'b': 2, 'c': 3}

del d['b']
print(d)  # {'a': 1, 'c': 3}

# Missing key → error
# del d['z']  # KeyError: 'z'

# Safe pattern
if 'z' in d:
    del d['z']
```

---

### Section 2: pop() Method (3 minutes)

```python
d = {'a': 1, 'b': 2, 'c': 3}

# Remove and get value
val = d.pop('b')
print(val)  # 2
print(d)    # {'a': 1, 'c': 3}

# With default — no error
val = d.pop('z', -1)
print(val)  # -1

# Without default — error
# d.pop('z')  # KeyError
```

**pop() is preferred** when you need the removed value or want safe removal.

---

### Section 3: popitem() (2 minutes)

Removes the **last** inserted pair:

```python
d = {'x': 10, 'y': 20, 'z': 30}

pair = d.popitem()
print(pair)  # ('z', 30)
print(d)     # {'x': 10, 'y': 20}

# Process all items
while d:
    key, val = d.popitem()
    print(f"Processed {key}: {val}")
```

---

### Section 4: clear() (1 minute)

```python
d = {'a': 1, 'b': 2, 'c': 3}
d.clear()
print(d)  # {}
```

Dict still exists, just empty.

---

### Section 5: Practical Patterns (4 minutes)

**Remove multiple keys safely:**
```python
data = {'a': 1, 'b': 2, 'c': 3, 'd': 4}
for key in ['b', 'd', 'f']:
    data.pop(key, None)
print(data)  # {'a': 1, 'c': 3}
```

**Filter by condition (collect then remove):**
```python
scores = {'Alice': 90, 'Bob': 40, 'Charlie': 75, 'Diana': 30}
failing = [k for k, v in scores.items() if v < 50]
for k in failing:
    del scores[k]
print(scores)  # {'Alice': 90, 'Charlie': 75}
```

**Never modify while iterating:**
```python
# WRONG — RuntimeError
# for k in d:
#     if condition:
#         del d[k]

# RIGHT — collect first, then delete
to_remove = [k for k in d if condition(k)]
for k in to_remove:
    del d[k]
```

---

### Summary (1 minute)

| Method | Use When |
|--------|----------|
| `del d[key]` | Simple removal, don't need the value |
| `d.pop(key, default)` | Need the value or want safe removal |
| `d.popitem()` | Processing items one at a time (LIFO) |
| `d.clear()` | Reset the entire dictionary |


---

## Checking for Key Existence


---

### CS Theory Bite

> **Origin**: The `in` operator checks dict **keys** (not values) using O(1) hash lookup. Before Python 3, you'd use `dict.has_key()` — now deprecated in favor of the cleaner `in` syntax.
>
> **Analogy**: Checking `key in dict` is like **checking if a word exists in the dictionary index** — instant, without reading every definition.
>
> **Why it matters**: Always check before accessing to avoid KeyError — or use `get()` for the same safety.



### Hook (2 minutes)

Your app receives JSON data from an API. Sometimes fields are missing:

```python
response = {'status': 'ok', 'data': {'name': 'Alice'}}

# This works:
print(response['status'])  # 'ok'

# This crashes:
# print(response['error'])  # KeyError!
```

In real applications, data is messy. Keys might be missing. You need to check before accessing.

---

### Section 1: Basic Key Checking (2 minutes)

```python
user = {'name': 'Alice', 'age': 25}

# Check keys
print('name' in user)   # True
print('email' in user)  # False

# NOT in
if 'email' not in user:
    print("No email on file")
```

**Remember:** `in` checks **keys only**, not values.

---

### Section 2: Keys vs Values (2 minutes)

```python
scores = {'Alice': 90, 'Bob': 85}

# Key check
print('Alice' in scores)          # True

# Value check
print(90 in scores.values())      # True
print(90 in scores)               # False — 90 is not a key!
```

---

### Section 3: Patterns with `in` (3 minutes)

**Conditional access:**
```python
if 'email' in user:
    send_notification(user['email'])
```

**Counting:**
```python
counts = {}
for item in data:
    if item in counts:
        counts[item] += 1
    else:
        counts[item] = 1
```

**Validation:**
```python
required = {'name', 'email', 'password'}
missing = [f for f in required if f not in form]
```

---

### Section 4: in vs get() (2 minutes)

```python
# Use 'in' when you need different logic paths
if 'role' in user:
    grant_access(user['role'])
else:
    deny_access()

# Use .get() when you just need a value with fallback
role = user.get('role', 'guest')
```

---

### Summary (1 minute)

1. `key in dict` — checks if key exists (O(1))
2. `val in dict.values()` — checks if value exists
3. Use `in` for conditional logic, `.get()` for fallback values
4. Always check before `del` or `[]` to prevent `KeyError`


---

## Iterating Through Dictionaries


---

### CS Theory Bite

> **Origin**: `.keys()`, `.values()`, `.items()` return **view objects** (Python 3) — live windows into the dict that update automatically. In Python 2, these returned static lists (more memory).
>
> **Analogy**: Dict views are like **security camera feeds** — they show the current state of the dict in real-time, not a snapshot.
>
> **Why it matters**: `.items()` is the most common — it gives you both key and value in each iteration.



### Hook (2 minutes)

You have student grades and need to:
- Print a report card
- Calculate averages
- Find the top student

All of this requires **iterating** through the dictionary:

```python
grades = {'Alice': 92, 'Bob': 78, 'Charlie': 85}

for name, score in grades.items():
    status = 'Pass' if score >= 80 else 'Needs work'
    print(f"{name}: {score} - {status}")
```

Three lines, full report. Let's learn every way to loop through dictionaries.

---

### Section 1: Iterating Keys (2 minutes)

```python
data = {'a': 1, 'b': 2, 'c': 3}

# Default — iterates keys
for key in data:
    print(key, data[key])

# Explicit — same result
for key in data.keys():
    print(key, data[key])
```

---

### Section 2: Iterating Values (2 minutes)

```python
prices = {'apple': 1.50, 'banana': 0.75, 'cherry': 3.00}

# Sum all prices
total = sum(prices.values())
print(f"Total: ${total}")

# Find average
avg = sum(prices.values()) / len(prices)
```

---

### Section 3: Iterating Items (3 minutes)

```python
user = {'name': 'Alice', 'age': 25, 'city': 'Mumbai'}

for key, val in user.items():
    print(f"{key}: {val}")
```

**Nested dicts:**
```python
grades = {
    'Alice': {'Math': 92, 'Science': 88},
    'Bob': {'Math': 78, 'Science': 85}
}

for student, subjects in grades.items():
    avg = sum(subjects.values()) / len(subjects)
    print(f"{student}: avg = {avg}")
```

---

### Section 4: Sorting and Filtering (3 minutes)

**Sorted by key:**
```python
for k in sorted(data):
    print(k, data[k])
```

**Sorted by value:**
```python
scores = {'Alice': 92, 'Bob': 78, 'Charlie': 85}
for name in sorted(scores, key=scores.get, reverse=True):
    print(f"{name}: {scores[name]}")
```

**Filtering with comprehension:**
```python
passing = {n: s for n, s in scores.items() if s >= 80}
```

---

### Section 5: Common Patterns (2 minutes)

**Find max/min:**
```python
top = max(scores, key=scores.get)
```

**Group by value:**
```python
groups = {}
for name, grade in students.items():
    groups.setdefault(grade, []).append(name)
```

**Invert dict:**
```python
inverted = {v: k for k, v in original.items()}
```

---

### Summary (1 minute)

1. `for key in dict` — keys
2. `for val in dict.values()` — values
3. `for k, v in dict.items()` — pairs (most useful)
4. Use `sorted()` for ordered iteration
5. Comprehensions for filtering and transforming


---

## Merging Dictionaries Using update()


---

### CS Theory Bite

> **Origin**: `update()` merges dicts — later values overwrite earlier ones. **Python 3.9+** added the `|` merge operator: `merged = dict1 | dict2` — cleaner syntax inspired by set union.
>
> **Analogy**: `update()` is like **merging two address books** — if both have "Alice", the newer entry wins.
>
> **Why it matters**: Dict merging is essential for combining configuration, API responses, and default + custom settings.



### Hook (2 minutes)

Your app has a layered configuration:

```python
defaults = {'debug': False, 'port': 3000, 'host': 'localhost'}
# User says: "I want port 8080 and debug on"
user = {'port': 8080, 'debug': True}
```

How do you combine them, with user preferences overriding defaults?

```python
config = {**defaults, **user}
print(config)  # {'debug': True, 'port': 8080, 'host': 'localhost'}
```

One line. User preferences win. Defaults fill in the gaps. This is dictionary merging.

---

### Section 1: update() Basics (3 minutes)

```python
d1 = {'a': 1, 'b': 2}
d2 = {'b': 20, 'c': 30}

d1.update(d2)
print(d1)  # {'a': 1, 'b': 20, 'c': 30}
# d2 values override d1 for shared keys
# d2 is unchanged
```

`update()` can also take keyword args:
```python
d1.update(x=100, y=200)
```

Or tuples:
```python
d1.update([('m', 10), ('n', 20)])
```

---

### Section 2: Creating New Merged Dicts (3 minutes)

`update()` modifies in place. To create a new dict:

```python
d1 = {'a': 1, 'b': 2}
d2 = {'b': 20, 'c': 30}

# Unpacking
merged = {**d1, **d2}
print(merged)  # {'a': 1, 'b': 20, 'c': 30}
print(d1)      # {'a': 1, 'b': 2} — unchanged!

# | operator (Python 3.9+)
merged = d1 | d2  # same result
```

---

### Section 3: Merge Priority (2 minutes)

Later dicts override earlier ones:

```python
layer1 = {'x': 1, 'y': 2}
layer2 = {'y': 20, 'z': 30}
layer3 = {'z': 300}

result = {**layer1, **layer2, **layer3}
print(result)  # {'x': 1, 'y': 20, 'z': 300}
```

To reverse priority, reverse the order:
```python
result = {**layer3, **layer2, **layer1}
# layer1 wins for shared keys
```

---

### Section 4: Practical Applications (3 minutes)

**Config layering:**
```python
final = {**defaults, **env_config, **cli_args}
```

**Template system:**
```python
base_style = {'font': 'Arial', 'size': 12, 'color': 'black'}
heading = {**base_style, 'size': 24, 'bold': True}
```

**Custom merge logic:**
```python
def merge_sum(d1, d2):
    result = d1.copy()
    for k, v in d2.items():
        result[k] = result.get(k, 0) + v
    return result
```

---

### Section 5: Pitfalls (1 minute)

**Shallow merge only:**
```python
d1 = {'settings': {'theme': 'dark', 'lang': 'en'}}
d2 = {'settings': {'theme': 'light'}}

merged = {**d1, **d2}
print(merged)  # {'settings': {'theme': 'light'}}
# 'lang' is LOST! Nested dicts are replaced, not merged
```

For deep merging, you need a custom function or a library.

---

### Summary (1 minute)

1. `d1.update(d2)` — merge in place, d2 wins for shared keys
2. `{**d1, **d2}` — new dict, d2 wins
3. Order determines priority — last one wins
4. Merging is **shallow** — nested dicts are replaced, not merged
5. For custom logic (sum, max, lists), write a merge function


---

