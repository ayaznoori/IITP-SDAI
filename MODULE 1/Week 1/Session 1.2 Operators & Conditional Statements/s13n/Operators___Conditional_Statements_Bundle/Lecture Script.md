### Apply Logical Operators (20 minutes)


### CS Theory Bite

> **Origin**: Logical operators (`and`, `or`, `not`) implement **Boolean algebra** (George Boole, 1854) and **De Morgan's Laws**: `not (A and B)` = `not A or not B`. These same operations are hardwired into CPU logic gates.
>
> **Analogy**: Logical operators are like **bouncers at a club** — `and` means BOTH conditions pass, `or` means EITHER passes, `not` reverses the decision.
>
> **Why it matters**: Complex conditions require combining simple comparisons — logical operators are the glue.


### Hook (2 minutes)

**Say**: "You're at a restaurant. You can order dessert if you finished your meal AND you're still hungry. OR if it's your birthday. That's logical operators in real life!"

### The `and` Operator (6 minutes)

**Say**: "`and` means BOTH conditions must be true."

```python
age = 20
has_license = True

can_drive = age >= 18 and has_license
print(can_drive)  # True
```

### The `or` Operator (6 minutes)

**Say**: "`or` means AT LEAST ONE condition must be true."

```python
is_weekend = True
is_holiday = False

can_sleep_in = is_weekend or is_holiday
print(can_sleep_in)  # True
```

### The `not` Operator (4 minutes)

**Say**: "`not` flips True to False and False to True."

```python
is_raining = False
is_sunny = not is_raining
print(is_sunny)  # True
```

**Real-World**: ```python
form_submitted = False
can_edit = not form_submitted
print(f"Can edit: {can_edit}")  # True

### Combining Operators (4 minutes)

**Say**: "Now let's combine them all!"

```python
age = 17
has_parent_consent = True
has_id = True

# Can register if 18+ OR (under 18 but has consent AND ID)
can_register = age >= 18 or (age < 18 and has_parent_consent and has_id)
print(f"Can register: {can_register}")  # True
```

### Practice & Wrap-up (3 minutes)

```python
a = True
b = False
print(a and b)
print(a or b)
print(not b)
```



---

### Write If Statements (20 minutes)


### CS Theory Bite

> **Origin**: Conditional branching is fundamental to **Turing completeness** — without `if`, programs can't make decisions. CPUs use **branch prediction** to guess which path code will take for speed.
>
> **Analogy**: An `if` statement is like a **fork in the road** — based on a condition, the program takes one path or the other.
>
> **Why it matters**: Programs without conditionals are just calculators — `if` makes programs intelligent.


### Hook (2 minutes)

**Say**: "Imagine a security guard at a concert. They check: 'IF you have a ticket, you can enter.' That's exactly what an if statement does in Python - it guards a block of code!"

### Basic If Statement (6 minutes)

**Say**: "An if statement executes code ONLY when a condition is True."

```python
age = 20

if age >= 18:
    print("You are an adult")
```

### Indentation (4 minutes)

**Say**: "In Python, indentation is REQUIRED. It's not just for looks!"

```python
if age >= 18:
    print("You can vote")
    print("You are an adult")
print("End")
```

### Using Conditions (4 minutes)

**Say**: "You can use any boolean expression as the condition."

```python
score = 85

if score >= 60:
    print("You passed!")

if score >= 90:
    print("Excellent!")
```

### Multiple If Statements (4 minutes)

**Say**: "You can have multiple if statements. Each one is checked independently!"

```python
temperature = 30

if temperature > 25:
    print("It's hot")

if temperature > 20:
    print("It's warm")

if temperature > 15:
    print("It's mild")
```

### Real-World Example (3 minutes)

**Say**: "Let's build a simple login checker!"

```python
username = input("Enter username: ")
password = input("Enter password: ")

if username == "admin" and password == "secret":
    print("Login successful!")
    print("Welcome, admin!")

if username != "admin":
    print("Wrong username")

```

### Common Mistakes & Wrap-up (2 minutes)

**Say**: "Let me show you the most common mistakes!"

```python
# Mistake 1: Forgetting colon
if age >= 18  # SyntaxError!

# Mistake 2: Using = instead of ==
if age = 18:  # SyntaxError!

# Mistake 3: No indentation
if age >= 18:
print("Adult")  # IndentationError!
```



---

### Write Elif Statements (20 minutes)


### CS Theory Bite

> **Origin**: `elif` is Python's elegant alternative to **switch/case** statements found in C/Java. It implements **decision trees** — sequential condition checking until a match is found.
>
> **Analogy**: `elif` is like a **multiple-choice exam** — check option A, if not then B, if not then C, until one matches.
>
> **Why it matters**: Real decisions rarely have just two options — `elif` handles multi-branch logic cleanly.


### Hook (2 minutes)

**Say**: "You walk into a coffee shop. Small is $3, Medium is $4, Large is $5. How does the cashier know which price? They check: IF small, ELIF medium, ELIF large. That's elif in action!"

### Problem with Multiple If (6 minutes)

**Say**: "First, let's see what happens with only if statements."

```python
score = 85

if score >= 90:
    print("Grade: A")

if score >= 80:
    print("Grade: B")

if score >= 70:
    print("Grade: C")
```

### Introducing Elif (6 minutes)

**Say**: "elif means 'else if'. It checks a condition ONLY if previous ones were False."

```python
score = 85

if score >= 90:
    print("Grade: A")
elif score >= 80:
    print("Grade: B")  # Runs and STOPS!
elif score >= 70:
    print("Grade: C")  # Never checked

# Output: Grade: B
```

### Order Matters (4 minutes)

**Say**: "The order of conditions is CRITICAL!"

```python
score = 95

if score >= 70:
    print("Pass")  # This catches 95!
elif score >= 90:
    print("Excellent")  # Never reached
```

### Real-World Example (4 minutes)

**Say**: "Let's build a ticket pricing system!"

```python
age = int(input("Enter age: "))

if age >= 65:
    price = 8
    print("Senior ticket: $8")
elif age >= 18:
    price = 12
    print("Adult ticket: $12")
elif age >= 13:
    price = 10
```

### Practice & Wrap-up (3 minutes)

```python
x = 15

if x > 20:
    print("A")
elif x > 10:
    print("B")
elif x > 5:
    print("C")
```



---

### Write Else Statements (20 minutes)


### CS Theory Bite

> **Origin**: The `else` clause ensures **exhaustive handling** — every possible path has a response. This relates to the **guard clause** pattern — handling edge cases before the main logic.
>
> **Analogy**: `else` is like a **safety net** — if none of the specific conditions match, the default action catches everything else.
>
> **Why it matters**: Without `else`, some inputs silently produce no output — `else` guarantees a response.


### Hook (2 minutes)

**Say**: "At a restaurant: IF you want pasta, we make pasta. ELIF you want pizza, we make pizza. ELSE we make our house special! That's else - the default option!"

```python
choice = "burger"

if choice == "pasta":
    print("Making pasta")
elif choice == "pizza":
    print("Making pizza")
else:
    print("House special!")  # Default!
```

### Basic Else (6 minutes)

**Say**: "else handles everything that didn't match previous conditions."

```python
age = 16

if age >= 18:
    print("Can vote")
else:
    print("Cannot vote yet")

# Output: Cannot vote yet
```

**Key Points**:
- `else` doesn't need a condition
- Catches EVERYTHING not caught by if/elif
- Must come after if (and optional elif)
- Only executes if all previous conditions were False

```python
score = 45

if score >= 60:
    print("Pass")
else:
    print("Fail")  # Runs for ANY score < 60
```

### Complete If-Elif-Else (6 minutes)

**Say**: "else catches EVERYTHING below 60. We don't need to write 'else score < 60'!"

```python
score = 55

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"  # Anything below 60

print(f"Grade: {grade}")  # F
```

**Real-World**: Login validation
```python
password = input("Enter password: ")

if len(password) >= 8:
    print("Password accepted")
else:
    print("Too short! Must be 8+ characters")
```

### When to Use Else (4 minutes)

**Say**: "Use else when you NEED a default action. Skip it when you don't!"

```python
# With else - always get output
if temperature > 30:
    print("Hot")
else:
    print("Not hot")

# Without else - may print nothing
if temperature > 30:
    print("Hot")
# Nothing prints if temperature <= 30
```

**Best Practice**: Use else for complete coverage
```python
status = "pending"

if status == "active":
    print("Process")
elif status == "inactive":
    print("Skip")
else:
    print(f"Unknown status: {status}")  # Catch unexpected values!
```

### Practice (2 minutes)

```python
x = 5

if x > 10:
    print("Big")
elif x > 5:
    print("Medium")
else:
    print("Small")  # Runs!

# Output: Small
```


---

### Write Nested Conditionals (25 minutes)


### CS Theory Bite

> **Origin**: Nested conditionals increase **cyclomatic complexity** (Thomas McCabe, 1976) — a measure of code complexity. High nesting depth signals code that's hard to test and maintain.
>
> **Analogy**: Nested ifs are like **Russian nesting dolls** — decisions inside decisions. Too many layers become hard to follow.
>
> **Why it matters**: Sometimes complex logic requires nesting, but prefer flattening with `elif` or early returns when possible.


### Hook (3 minutes)

**Say**: "To drive, you need: age 18+ AND a license AND a car. That's checking conditions INSIDE conditions - nested logic!"

```python
age = 25
has_license = True
has_car = True

if age >= 18:
    if has_license:
        if has_car:
            print("You can drive!")
        else:
            print("You need a car")
    else:
        print("You need a license")
else:
    print("Too young to drive")
```

### Basic Nested Conditionals (6 minutes)

**Say**: "Nested means putting one if statement INSIDE another. Each level adds more detail."

```python
temperature = 85
is_sunny = True

if temperature > 80:
    print("It's hot!")
    if is_sunny:
        print("And it's sunny - perfect beach weather!")
    else:
        print("But it's cloudy")
else:
    print("Not too hot today")
```

**Key Points**:
- Inner if only runs if outer if is True
- Each level needs proper indentation
- Each if block can have its own elif/else

### Decision Trees (6 minutes)

**Say**: "Nested conditionals create decision trees - each branch leads to more decisions."

```python
age = 25

if age >= 0:
    if age < 13:
        category = "Child"
    elif age < 20:
        category = "Teenager"
    elif age < 60:
        category = "Adult"
    else:
        category = "Senior"
    print(f"Category: {category}")
else:
    print("Invalid age")
```

**Real-World**: Discount calculator
```python
price = 100
is_member = True
is_sale = True

if is_member:
    if is_sale:
        discount = 0.30  # Member + Sale
    else:
        discount = 0.10  # Member only
else:
    if is_sale:
        discount = 0.15  # Sale only
    else:
        discount = 0  # No discount

final_price = price * (1 - discount)
print(f"Price: ${final_price}")
```

### Avoiding Deep Nesting (5 minutes)

**Say**: "More than 2-3 levels? Too complex! Use 'and' instead."

```python
# TOO DEEP - hard to read!
if condition1:
    if condition2:
        if condition3:
            if condition4:
                print("All conditions met")

# BETTER - use 'and'
if condition1 and condition2 and condition3 and condition4:
    print("All conditions met")
```

**When to nest**: When the inner logic DEPENDS on the outer condition
```python
# Good nesting - inner logic depends on outer
if has_account:
    if balance > 100:
        print("Can withdraw")
    else:
        print("Insufficient funds")
else:
    print("Please create account")
```

### Practice (3 minutes)

Calculate tip based on bill and service:
```python
bill = 75
service = "excellent"

if bill > 50:
    if service == "excellent":
        tip_percent = 0.20
    else:
        tip_percent = 0.15
else:
    if service == "excellent":
        tip_percent = 0.15
    else:
        tip_percent = 0.10

tip = bill * tip_percent
print(f"Tip: ${tip}")
```

### Wrap-up (2 minutes)

**Key Takeaways**:
- Nest when inner conditions depend on outer ones
- Keep nesting shallow (2-3 levels max)
- Use `and`/`or` for independent conditions
- Proper indentation is critical!


---

### Accept User Input (16 minutes)


### CS Theory Bite

> **Origin**: Interactive input comes from Unix's **stdin/stdout model** (1971). `input()` reads from **standard input** — the same stream whether it's a keyboard, file, or pipe.
>
> **Analogy**: `input()` is like a **waiter taking your order** — the program pauses, listens, and records what you say as a string.
>
> **Why it matters**: User input makes programs interactive — from CLI tools to form validation, input handling is essential.


### Hook (3 minutes)

**Say**: "Want to make programs that talk to users? The input() function makes programs interactive!"

```python
name = input("What's your name? ")
print(f"Hello, {name}!")
```

### Input Basics (5 minutes)

**Say**: "Input() displays a prompt, waits for user to type, then returns what they typed as a string."

```python
name = input("Enter your name: ")
print(f"Welcome, {name}!")

city = input("Where do you live? ")
print(f"{name} lives in {city}")
```

**Key Points**:
- `input()` always returns a string
- The prompt message is optional but helpful
- Program pauses until user presses Enter
- Works in terminal/console programs

### Input with Type Conversion (5 minutes)

**Say**: "Remember: input() gives strings! Convert for math."

```python
# Wrong way
age = input("Enter age: ")
# age + 1  # Error! Can't add string and number

# Right way
age = int(input("Enter age: "))
next_year = age + 1
print(f"Next year you'll be {next_year}")
```

**Common Pattern**:
```python
price = float(input("Enter price: $"))
quantity = int(input("Enter quantity: "))
total = price * quantity
print(f"Total: ${total:.2f}")
```

**Real-World**: Calculators, games, forms - all use input() to interact with users!

### Practice (3 minutes)

Build an interactive calculator:
```python
num1 = int(input("First number: "))
num2 = int(input("Second number: "))
sum_result = num1 + num2
print(f"{num1} + {num2} = {sum_result}")
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

