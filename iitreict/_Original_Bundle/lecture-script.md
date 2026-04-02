### Defining Reusable Functions in Python


### CS Theory Bite

> **Origin**: Functions (called **subroutines** in the 1950s) were formalized to enable **code reuse**. The **DRY principle** (Don't Repeat Yourself) — Andy Hunt & Dave Thomas, 1999 — is built on functions.
>
> **Analogy**: A function is like a **recipe** — define it once, cook it anytime you want. Change the recipe, and every dish improves.
>
> **Why it matters**: Functions are the primary unit of code organization — every large program is built from small functions.


### Hook (3 minutes)

"Imagine you're making breakfast. Every morning, you follow the same routine to make coffee - you don't relearn the steps each time. Functions in Python work the same way: define once, use many times!"

Think about your calculator's 'add' button - programmers wrote ONE function and reused it thousands of times!

### Section 1: What is a Function? (4 minutes)

A function is a named block of reusable code that performs a specific task.

**Without functions - repetitive code:**
```python
# Print header - First time
print("=" * 40)
print("   Welcome to My Program")
print("=" * 40)

# Later, print header again (copy-paste!)
print("=" * 40)
print("   Welcome to My Program")
print("=" * 40)
```

This is repetitive! If we change the header, we update it everywhere. Functions solve this.

### Section 2: Function Syntax (5 minutes)

```python
def function_name():
    # Function body (indented)
    # Code to execute
    pass
```

**Components:**
- `def` - keyword to start function definition
- `function_name` - descriptive name (PEP 8 conventions)
- `()` - parentheses (for parameters later)
- `:` - colon to start the block
- Indented code - function body

**Example:**
```python
def greet():
    print("Hello, World!")
    print("Welcome to Python!")
```

Note: Defining doesn't run it! We must CALL it.

### Section 3: Calling Functions (4 minutes)

```python
# STEP 1: Define
def greet():
    print("Hello, World!")
    print("Welcome to Python!")

# STEP 2: Call
greet()  # Output: Hello, World!
         #         Welcome to Python!

# Call multiple times
greet()
greet()  # Each call executes the entire function!
```

**Practical example:**
```python
def print_header():
    print("=" * 40)
    print("   Welcome to My Program")
    print("=" * 40)

print_header()  # Use anywhere
print("Loading...")
print_header()  # Use again
```

### Section 4: Functions Can Contain Any Code (4 minutes)

**Conditionals:**
```python
def check_age():
    age = 20
    if age >= 18:
        print("You are an adult")
    else:
        print("You are a minor")

check_age()
```

**Loops:**
```python
def print_countdown():
    for i in range(5, 0, -1):
        print(i)
    print("Blast off!")

print_countdown()
```

**Multiple operations:**
```python
def daily_report():
    print("Generating report...")
    total = 100 + 200 + 300
    print(f"Total: {total}")
```

### Section 5: Why Use Functions? (3 minutes)

**Benefits:**
1. Reusability - Write once, use many times
2. Organization - Break large programs into manageable pieces
3. Maintainability - Update code in one place
4. Readability - Descriptive names make code self-documenting

**Example:**
```python
def show_menu():
    print("=" * 30)
    print("1. New Game")
    print("2. Load Game")
    print("3. Exit")

show_menu()  # On startup
# ... code ...
show_menu()  # After game over
# To change menu, update function once!
```

### Section 6: Common Mistakes (3 minutes)

**Mistake 1: Forgetting to call**
```python
def greet():
    print("Hello!")
# Nothing happens - not called!
```

**Mistake 2: Indentation errors**
```python
def greet():
print("Hello!")  # IndentationError!
```

**Mistake 3: Calling before defining**
```python
greet()  # NameError!

def greet():
    print("Hello!")
```

### Section 7: Practice (2 minutes)

Write a function `print_stars` that prints 5 stars:
```python
def print_stars():
    print("*" * 5)

print_stars()  # *****
```

### Wrap-Up (2 minutes)

**Key points:**
- Functions are reusable blocks of code
- Define with `def` keyword
- Call using function name with parentheses
- Make code organized and maintainable

**Template:**
```python
def function_name():
    # Your code here
    pass

function_name()  # Call it
```


---

### Using Function Parameters to Accept Inputs


### CS Theory Bite

> **Origin**: Parameters implement **parameterized abstraction** — functions that adapt to different inputs. Python uses **pass-by-object-reference** (neither pure pass-by-value nor pass-by-reference).
>
> **Analogy**: Parameters are like **power outlets** — the function is an appliance, and parameters are the sockets where you plug in different data.
>
> **Why it matters**: Parameters make functions flexible — one function, many uses with different inputs.


### Hook (3 minutes)

"Yesterday we learned functions, but they always did the SAME thing. Imagine if your calculator's 'add' button could only add 2+3 every time - not very useful!"

Today: Make functions flexible by passing data to them!

Think of a vending machine - ONE machine accepts INPUT (your selection) and gives different OUTPUT. Functions work the same way!

### Section 1: The Problem Without Parameters (4 minutes)

**Inflexible function:**
```python
def greet_alice():
    print("Hello, Alice!")

greet_alice()  # Hello, Alice!
greet_alice()  # Hello, Alice! (same every time)
```

To greet different people, we'd need many functions - impractical! **Solution: Parameters!**

### Section 2: What Are Parameters? (5 minutes)

A parameter is a variable in the function definition that acts as a placeholder for data.

**Syntax:**
```python
def function_name(parameter):
    # Use parameter here
    print(parameter)
```

**Example:**
```python
def greet(name):  # 'name' is a parameter
    print(f"Hello, {name}!")

greet("Alice")    # Hello, Alice!
greet("Bob")      # Hello, Bob!
greet("Charlie")  # Hello, Charlie!
```

**Key points:**
- Parameter is a variable name
- Gets its value when function is called
- Can be used anywhere in function body

### Section 3: Parameters vs Arguments (3 minutes)

```python
def greet(name):  # 'name' is a PARAMETER
    print(f"Hello, {name}")

greet("Alice")  # "Alice" is an ARGUMENT
```

**Remember:**
- Parameter = placeholder in definition
- Argument = actual value when calling

**Example:**
```python
def square(number):  # parameter
    print(number ** 2)

square(5)   # 5 is argument -> 25
square(10)  # 10 is argument -> 100
```

### Section 4: Multiple Parameters (5 minutes)

**Syntax:**
```python
def function_name(param1, param2, param3):
    pass
```

**Examples:**
```python
def greet_full(first_name, last_name):
    print(f"Hello, {first_name} {last_name}!")

greet_full("Alice", "Smith")  # Hello, Alice Smith!

def add(a, b):
    print(f"{a} + {b} = {a + b}")

add(5, 3)   # 5 + 3 = 8
```

**Order matters!**
```python
def introduce(name, age):
    print(f"My name is {name} and I'm {age} years old")

introduce("Alice", 25)  # Correct
introduce(25, "Alice")  # Wrong! "My name is 25..."
```

### Section 5: Different Data Types (4 minutes)

**Strings:**
```python
def shout(message):
    print(message.upper() + "!")

shout("hello")  # HELLO!
```

**Numbers:**
```python
def print_times(text, count):
    for i in range(count):
        print(text)

print_times("Python", 3)
```

**Mixed types:**
```python
def make_sentence(name, age, city):
    print(f"{name} is {age} years old and lives in {city}.")

make_sentence("Alice", 25, "New York")
```

### Section 6: Common Mistakes (3 minutes)

**Wrong number of arguments:**
```python
def add(a, b):
    print(a + b)

add(5)        # Error: missing argument
add(5, 3, 2)  # Error: too many arguments
add(5, 3)     # Correct
```

**Wrong order:**
```python
def subtract(a, b):
    print(a - b)

subtract(10, 3)  # 7
subtract(3, 10)  # -7 (different!)
```

### Section 7: Real-World Application (3 minutes)

**Validation:**
```python
def validate_username(username):
    if len(username) < 3:
        print("Username too short!")
    elif len(username) > 20:
        print("Username too long!")
    else:
        print("Username valid!")

validate_username("ab")        # Too short!
validate_username("alice123")  # Valid!
```

### Section 8: Practice (2 minutes)

Write `calculate_bmi` that takes weight and height:
```python
def calculate_bmi(weight, height):
    bmi = weight / (height ** 2)
    print(f"BMI: {bmi:.2f}")

calculate_bmi(70, 1.75)  # BMI: 22.86
```

### Wrap-Up (2 minutes)

**Key takeaways:**
1. Parameters = placeholders in definition
2. Arguments = actual values when calling
3. Multiple parameters = separate with commas
4. Order matters
5. Any data type can be a parameter

**Template:**
```python
def function_name(param1, param2):
    # Use parameters
    pass

function_name(value1, value2)
```


---

### Returning Values from Functions Using Return


### CS Theory Bite

> **Origin**: Return values connect to **functional programming** (lambda calculus, 1930s) — functions as mathematical mappings: input → output. **Pure functions** (same input → same output) are the gold standard.
>
> **Analogy**: `return` is like a **vending machine** — you insert input (money + selection), and the machine gives back a specific output (snack).
>
> **Why it matters**: Functions without return values are limited to side effects — return values enable composition and data flow.


### Hook (3 minutes)

"Our functions print results, but imagine if your calculator showed the answer but didn't let you USE it in another calculation. Frustrating, right?"

Today: Learn how to make functions send data BACK using return statements!

Think of a vending machine - it doesn't just SHOW you the snack, it GIVES it to you. The `return` statement works the same way!

### Section 1: The Problem - Functions That Only Print (4 minutes)

**Current limitation:**
```python
def add(a, b):
    result = a + b
    print(result)  # Just prints

add(5, 3)  # Prints: 8

# But we can't use the result!
total = add(5, 3)  # Prints: 8
print(total)       # Prints: None
```

**What we need:**
```python
def add(a, b):
    return a + b  # Give back the result

total = add(5, 3)   # total = 8
print(total)        # Prints: 8
doubled = total * 2 # We can use it!
```

### Section 2: What is Return? (5 minutes)

The `return` statement sends a value back to the caller and immediately exits the function.

**Syntax:**
```python
def function_name():
    return value  # Send value back
```

**Example:**
```python
def get_greeting():
    return "Hello, World!"

message = get_greeting()
print(message)  # Hello, World!
```

**Key points:**
- `return` gives a value back
- Function stops when it hits `return`
- Store returned value in a variable
- Use returned value in expressions

### Section 3: Return vs Print (4 minutes)

**Print version:**
```python
def add_print(a, b):
    print(a + b)  # Shows on screen

x = add_print(3, 4)  # Prints: 7
print(x)             # Prints: None
```

**Return version:**
```python
def add_return(a, b):
    return a + b  # Gives value back

x = add_return(3, 4)  # x = 7
print(x)              # Prints: 7
```

**Key difference:**
- `print()` displays to screen (for humans)
- `return` gives data back (for programs)

### Section 4: Using Returned Values (5 minutes)

**In calculations:**
```python
def square(n):
    return n * n

result = square(5)                # 25
total = square(3) + square(4)     # 9 + 16 = 25
```

**In conditionals:**
```python
def is_adult(age):
    return age >= 18

if is_adult(20):
    print("Can vote!")
```

### Section 5: Returning Different Data Types (4 minutes)

**Strings, numbers, booleans:**
```python
def make_greeting(name):
    return f"Hello, {name}!"

def is_even(num):
    return num % 2 == 0
```

**Multiple values:**
```python
def get_min_max(numbers):
    return min(numbers), max(numbers)

minimum, maximum = get_min_max([1, 5, 3, 9])
```

### Section 6: Return Stops Execution (3 minutes)

```python
def check_number(num):
    if num > 10:
        return "Too big!"
    if num < 0:
        return "Negative!"
    return "Perfect!"

def divide(a, b):
    if b == 0:
        return "Error: Cannot divide by zero"
    return a / b
```

### Section 7: Common Mistakes (3 minutes)

```python
# Forgetting to return
def add(a, b):
    result = a + b  # Forgot return!

# Not capturing
square(5)  # Value lost!
result = square(5)  # Correct

# Code after return
def greet():
    return "Hello"
    print("World")  # Never executes!
```

### Section 8: Real-World Application (3 minutes)

```python
def is_valid_password(password):
    if len(password) < 8:
        return False
    return True

def calculate_total(price, quantity, tax_rate):
    return price * quantity * (1 + tax_rate)

order_total = calculate_total(10.99, 3, 0.08)
```

### Section 9: Practice (2 minutes)

Write `calculate_discount` that returns final price:
```python
def calculate_discount(price, discount_percent):
    discount = price * (discount_percent / 100)
    return price - discount

final = calculate_discount(100, 20)
print(f"Final: ${final}")  # $80.00
```

### Wrap-Up (2 minutes)

**Key takeaways:**
1. `return` sends value back to caller
2. Return stops execution immediately
3. Returned values can be stored and used
4. Print shows output; return gives usable data
5. No return means function returns `None`

**Template:**
```python
def function_name(params):
    result = some_value
    return result

value = function_name(args)
```


---

### Applying Default Parameters in Function Definitions


### CS Theory Bite

> **Origin**: Default parameters are Python's alternative to **function overloading** (C++/Java). They enable **progressive disclosure** — simple calls for common cases, detailed calls for edge cases.
>
> **Analogy**: Default parameters are like **restaurant menu defaults** — "How do you want your steak?" "Medium" is the default, but you can specify rare or well-done.
>
> **Why it matters**: Defaults simplify function calls — users only specify what differs from the common case.


### Hook (3 minutes)

"Ever use a copy machine? The settings are pre-configured: single-sided, one copy, black and white. But you CAN change them! This is how default parameters work - sensible defaults with customization when needed."

Think about ordering coffee - default is medium with no sugar, but you can say "large with 2 sugars" to override!

### Section 1: The Problem - Too Many Required Arguments (4 minutes)

**Inflexible:**
```python
def greet(name, greeting, punctuation):
    print(f"{greeting}, {name}{punctuation}")

greet("Alice", "Hello", "!")
greet("Bob", "Hello", "!")    # Repeating "Hello" and "!"
greet("Charlie", "Hello", "!")
```

**Solution - default parameters:**
```python
def greet(name, greeting="Hello", punctuation="!"):
    print(f"{greeting}, {name}{punctuation}")

greet("Alice")              # Hello, Alice!
greet("Bob")                # Hello, Bob!
greet("Charlie", "Hi")      # Hi, Charlie!
greet("David", "Hey", "?")  # Hey, David?
```

### Section 2: What Are Default Parameters? (5 minutes)

A default parameter has a pre-assigned value in the function definition. If no argument is provided, the default is used.

**Syntax:**
```python
def function_name(param1, param2=default_value):
    pass
```

**Example:**
```python
def power(base, exponent=2):  # exponent defaults to 2
    return base ** exponent

print(power(5))      # 5^2 = 25
print(power(5, 3))   # 5^3 = 125
print(power(2, 4))   # 2^4 = 16
```

**Key points:**
- Default parameters make arguments optional
- If not provided, default value is used
- Can override by passing a value
- Defaults must come after regular parameters

### Section 3: Syntax Rules (4 minutes)

**Rule 1: Defaults after required parameters**
```python
# WRONG
def greet(greeting="Hello", name):  # SyntaxError!
    pass

# CORRECT
def greet(name, greeting="Hello"):
    pass
```

**Rule 2: Multiple defaults**
```python
def make_coffee(size="medium", sugar=0, milk=False):
    print(f"{size} coffee, {sugar} sugar, milk: {milk}")

make_coffee()               # All defaults
make_coffee("large")        # Override size
make_coffee("small", 2)     # Override size and sugar
```

**Rule 3: Mixing required and default**
```python
def order_pizza(size, topping="cheese", extra=False):
    print(f"{size} pizza with {topping}")

order_pizza("large")              # Must provide size
order_pizza("medium", "pepperoni")
```

### Section 4: Using Default Parameters (5 minutes)

**Configuration:**
```python
def connect_to_server(host, port=8080, timeout=30):
    print(f"Connecting to {host}:{port}, timeout: {timeout}s")

connect_to_server("localhost")
connect_to_server("example.com", 443)
```

**Formatting:**
```python
def format_price(amount, currency="USD", decimals=2):
    return f"{currency} {amount:.{decimals}f}"

print(format_price(19.99))        # USD 19.99
print(format_price(19.99, "EUR")) # EUR 19.99
```

### Section 5: Keyword Arguments with Defaults (4 minutes)

```python
def create_user(username, role="user", active=True, notifications=True):
    print(f"User: {username}, Role: {role}")
    print(f"Active: {active}, Notifications: {notifications}")

create_user("alice", notifications=False)  # Skip some defaults
create_user("bob", active=False)
```

### Section 6: Common Use Cases (3 minutes)

```python
def print_header(text, width=50, char="="):
    print(char * width)
    print(text.center(width))
    print(char * width)

print_header("Welcome")        # Defaults
print_header("Hello", 30, "*") # Custom

def calculate_shipping(weight, distance, express=False):
    cost = weight * 0.5 + distance * 0.1
    return cost * 2 if express else cost
```

### Section 7: Common Mistakes (3 minutes)

**Mistake 1: Mutable defaults**
```python
# DANGEROUS
def add_item(item, items=[]):  # BAD!
    items.append(item)
    return items

# CORRECT
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

**Mistake 2: Wrong order**
```python
# WRONG
def greet(greeting="Hello", name):  # SyntaxError!
    pass

# CORRECT
def greet(name, greeting="Hello"):
    pass
```

### Section 8: Practice (2 minutes)

Write `send_email` with recipient (required), subject (default "No Subject"), urgent (default False):
```python
def send_email(recipient, subject="No Subject", urgent=False):
    priority = "HIGH" if urgent else "NORMAL"
    print(f"To: {recipient}")
    print(f"Subject: {subject}")
    print(f"Priority: {priority}")

send_email("alice@example.com")
send_email("bob@example.com", "Meeting", True)
```

### Wrap-Up (2 minutes)

**Key takeaways:**
1. Default parameters provide fallback values
2. Make arguments optional and functions flexible
3. Required parameters before defaults
4. Override by position or keyword
5. Avoid mutable defaults (use None)

**Template:**
```python
def function_name(required, optional=default):
    pass

function_name(value1)         # Uses default
function_name(value1, value2) # Overrides
```

Congratulations! You now know Python function fundamentals!


---

