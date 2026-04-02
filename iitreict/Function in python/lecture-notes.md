# Define Functions

## Introduction

Functions introduce **code reusability** and **abstraction** - two of the most fundamental concepts in programming. They represent the shift from **linear, repetitive code** to **modular, maintainable software**.

### Why Functions Are Revolutionary

**The repetition problem**: Early programs repeated the same code blocks hundreds of times. A bug meant fixing it everywhere. A change meant updating hundreds of locations.
**Functions solution**: Write code once, call it anywhere. Fix bugs in one place. Update features once.

**Historical note**: Subroutines (early functions) appeared in FORTRAN (1954) and revolutionized programming. Before this, code was a tangled mess. After, programs became organized, maintainable systems.

### Real-World Analogy

Functions are like **recipes in a cookbook**:
- **Define once**: Write the recipe for "make pasta" once
- **Use many times**: Cook pasta Monday, Wednesday, Friday without rewriting steps
- **Share**: Others can use your recipe (code reuse!)
- **Modify**: Update recipe in one place, everyone gets the improvement

Or like **keyboard shortcuts**:
- **Ctrl+C (copy)**: A complex series of operations wrapped into one simple action
- **Function**: Complex code wrapped into one simple name
- **Result**: Simpler, more readable programs

### The Power of Abstraction

Functions hide **implementation details** behind a simple name:
```python
calculate_tax(income)  # Don't need to know HOW tax is calculated!
send_email(to, subject, body)  # Don't need to know HOW email works!
```

This **abstraction** lets you work at a higher level - thinking in terms of "what" not "how".

### DRY Principle

Functions embody **DRY: Don't Repeat Yourself**:
- **Without functions**: Same code copied 50 times (nightmare to maintain!)
- **With functions**: One function called 50 times (update once, fixed everywhere!)

This is the foundation of professional software engineering.

---

<div align="center">

![Python Function Definition Syntax](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.26.jpg)

*A function acts as a reusable machine: it accepts input, processes it according to defined instructions, and produces output*

</div>

---

## Functions

A function is a reusable block of code that performs a specific task.

### Basic Syntax

```python
def function_name():
    # Function body
    # Code to execute
```

## Simple Examples

### Example 1: Hello Function

```python
def say_hello():
    print("Hello, World!")

# Call the function
say_hello()
# Output: Hello, World!
```

### Example 2: Multiple Statements

```python
def greet_user():
    print("Welcome!")
    print("Thanks for using our program")
    print("Have a great day!")

greet_user()
```

# Output:
# ----------------------------------------
# Header
# ----------------------------------------

## Why Use Functions?

1. **Reusability**: Write once, use many times
2. **Organization**: Break complex code into manageable pieces
3. **Maintainability**: Easier to debug and update
4. **Readability**: Makes code self-documenting

## Function Naming

```python
# Good names (verb-based, descriptive)
def calculate_total():
    pass

def send_email():
    pass

def validate_input():
    pass

# Avoid (not descriptive)
def func1():
    pass

def do_stuff():
    pass
```

## Calling Functions

```python
def display_menu():
    print("1. New Game")
    print("2. Load Game")
    print("3. Quit")

# Call it multiple times
display_menu()
display_menu()
```

## Functions Calling Functions

```python
def print_header():
    print("=" * 40)
    print("Welcome to My Program")
    print("=" * 40)

def print_footer():
    print("=" * 40)
    print("Thank you!")
    print("=" * 40)

def show_welcome():
    print_header()
    print("\nPlease choose an option:")
    print_footer()

show_welcome()
```

## Key Takeaways

1. **def keyword**: Defines a function
2. **Colon and indent**: Required syntax
3. **Call with ()**: parentheses needed to execute
4. **Reusable**: Can call multiple times
5. **Organization**: Breaks code into logical units


---

# Use Function Parameters

## Introduction

Function parameters introduce **flexibility** and **generalization** to functions. They transform functions from **single-purpose tools** into **versatile, reusable components** that work with any data.

### Why Parameters Exist

**The rigidity problem**: Functions without parameters do one specific thing with hardcoded values. Need to greet different people? Need 100 different functions!
**Parameters solution**: One function, infinite uses. Pass different data each time.

**Historical perspective**: Early programming languages didn't have sophisticated parameter passing. FORTRAN (1954) introduced subroutine parameters, revolutionizing code reuse. Modern languages (including Python) have refined this into elegant, powerful systems.

### Real-World Analogy

Parameters are like **slots in a coffee machine**:
- **Coffee type slot**: Parameter for type (espresso, latte, cappuccino)
- **Size slot**: Parameter for size (small, medium, large)
- **Sugar slot**: Parameter for sweetness (none, 1 spoon, 2 spoons)
- **One machine**: Works with any combination of inputs!

Or like **a form**:
- **Name field**: Parameter
- **Age field**: Parameter
- **Address field**: Parameter
- **Same form**, different people fill in different values

### The Power of Abstraction

Parameters let you write **generic**, **reusable** code:
```python
# One function for ALL calculations!
def calculate(a, b, operation):
    # Works for any numbers, any operation
```

This is **parametric polymorphism** - one function, many uses through different inputs.

### Information Flow

Parameters are **inputs** to functions - the data they need to do their job:
- **Function**: A machine that processes data
- **Parameters**: The raw materials/ingredients
- **Processing**: The function's code
- **Result**: Output (we'll learn about `return` soon!)

---

<div align="center">

![Python Function Parameters and Arguments](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.27.jpg)

*Function parameters define named input slots that receive argument values when the function is called, enabling flexible and reusable code*

</div>

---

## Function Parameters

Parameters allow functions to accept input values.

### Basic Syntax

```python
def function_name(parameter):
    # Use parameter in function body
```

## Examples

### Example 1: Single Parameter

```python
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")  # Hello, Alice!
greet("Bob")    # Hello, Bob!
```

### Example 2: Multiple Parameters

```python
def add_numbers(a, b):
    result = a + b
    print(f"{a} + {b} = {result}")

add_numbers(5, 3)   # 5 + 3 = 8
add_numbers(10, 20) # 10 + 20 = 30
```

## Parameters vs Arguments

```python
def greet(name):  # 'name' is a parameter
    print(f"Hello, {name}")

greet("Alice")  # "Alice" is an argument
```

- **Parameter**: Variable in function definition
- **Argument**: Actual value passed when calling

## Order Matters

```python
def introduce(name, age):
    print(f"{name} is {age} years old")

introduce("Alice", 25)  # Alice is 25 years old
introduce(25, "Alice")  # 25 is Alice years old (wrong!)
```

## Key Takeaways

1. **Parameters**: Variables in function definition
2. **Arguments**: Values passed when calling
3. **Multiple parameters**: Separated by commas
4. **Order matters**: Arguments match parameters by position
5. **Flexibility**: Same function, different inputs


---

# Return Values from Functions

## Introduction

The `return` statement introduces **output** from functions - the ability to send computed values back to the caller. This completes the **input-process-output** cycle that makes functions powerful building blocks.

### Why Return Exists

**The usability problem**: Functions that only print are limited - you can't use their results in calculations, store them, or pass them to other functions.
**Return solution**: Functions compute values AND send them back for further use. This enables **function composition** - using outputs of one function as inputs to another.

**Mathematical parallel**: Functions in math return values: f(x) = x² returns a value you can use. Programming functions work the same way!

### Real-World Analogy

Return is like **a calculator**:
- **Input**: You press 5 + 3
- **Processing**: Calculator computes internally
- **Return**: Shows "8" on display (returns value you can copy/use)
- **vs Display only**: If calculator just showed "8" but you couldn't copy it, useless!

Or like **a vending machine**:
- **Input**: You insert money, press button
- **Processing**: Machine finds item
- **Return**: Item drops into slot (returns physical product)
- **You get output**: Can now eat/use the item elsewhere!

### Return vs Print: The Critical Difference

This is one of the most confusing concepts for beginners:
- **print()**: Shows something to the user (side effect, no value returned)
- **return**: Sends value back to the code (for further computation)

```python
def bad_add(a, b):
    print(a + b)  # Just displays, returns None

result = bad_add(5, 3)  # Displays 8
# result is None! Can't use it!
```

**Professional code uses return** - printing is only for final user output.

### The Power of Composition

Return enables **chaining functions** - outputs become inputs:
```python
result = function3(function2(function1(data)))
```

This **functional composition** is fundamental to modern programming - building complex operations from simple, reusable pieces.

---

<div align="center">

![Python return Statement Function Return Value](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.28.png)

*The return statement sends a computed value back to the caller — like a function machine that takes input and produces output*

</div>

---

## Return Statement

Functions can send values back using `return`.

### Basic Syntax

```python
def function_name():
    return value
```

## Examples

### Example 1: Return a Number

```python
def add(a, b):
    return a + b

result = add(5, 3)
print(result)  # 8
```

### Example 2: Return a String

```python
def make_greeting(name):
    return f"Hello, {name}!"

message = make_greeting("Alice")
print(message)  # Hello, Alice!
```

# Output: Can vote

## Return vs Print

```python
# Print (displays, returns None)
def add_print(a, b):
    print(a + b)

result = add_print(5, 3)  # Displays 8
print(result)  # None

# Return (sends value back)
def add_return(a, b):
    return a + b

result = add_return(5, 3)  # Returns 8
print(result)  # 8
```

## Multiple Return Statements

```python
def check_number(num):
    if num > 0:
        return "Positive"
    elif num < 0:
        return "Negative"
    else:
        return "Zero"

print(check_number(5))   # Positive
print(check_number(-3))  # Negative
print(check_number(0))   # Zero
```

## Key Takeaways

1. **return**: Sends value back to caller
2. **Exits function**: Stops execution immediately
3. **Store result**: Assign to variable
4. **Use in expressions**: Can use directly in calculations
5. **return vs print**: return gives value, print displays


---

# Apply Default Parameters

## Introduction

Default parameters introduce **optional arguments** - the ability to call functions with fewer arguments by providing sensible defaults. This represents a balance between **flexibility** and **convenience** in API design.

### Why Default Parameters Exist

**The convenience problem**: Some functions have common use cases. Requiring all parameters every time is tedious and error-prone.
**Default parameters solution**: Provide sensible defaults for common cases. Users can override when needed, but don't have to specify everything every time.

**Real-world API design**: Python's own `print()` function uses defaults: `print(value, sep=' ', end='\n')`. You rarely need to change `sep` or `end`, so they have defaults!

### Real-World Analogy

Default parameters are like **ordering at a restaurant**:
- **Required**: "I want a burger" (must specify main item)
- **Optional/defaults**: Comes with lettuce, tomato, pickles (default toppings)
- **Override**: "No pickles, extra cheese" (customize when needed)
- **Same kitchen**, flexible ordering!

Or like **a thermostat with preset modes**:
- **Cool to 72°F**: Default setting most people want
- **Override**: Can adjust to 68°F or 75°F if needed
- **Convenience**: One button for common case, detailed control when wanted

### The Power of Sensible Defaults

Default parameters embody **convention over configuration**:
```python
def send_email(to, subject, body, cc=None, bcc=None, priority='normal'):
    # Most emails don't need cc/bcc/priority changes
    # But they're available when needed!
```

This makes **common cases simple**, while keeping **complex cases possible**.

### Design Principle

Good defaults follow the **Principle of Least Surprise**:
- Defaults should be what users expect 90% of the time
- Reduces cognitive load
- Makes functions easier to use
- Professional libraries use this extensively

---

<div align="center">

![Python Default Parameter Types Diagram](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.29.png)

*Default parameters provide preset values that are used automatically unless the caller explicitly overrides them*

</div>

---

## Apply Default Parameters

Defining functions with default argument values

### Key Concepts

**Core principle**: def func(arg1, arg2=default_value):

### Syntax and Usage

```python
# Basic example will be shown in practical examples below
```

### Practical Examples

#### Example 1: Basic Default Parameters

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(greet("Alice"))              # Hello, Alice!
print(greet("Bob", "Hi"))          # Hi, Bob!
print(greet("Charlie", greeting="Hey"))  # Hey, Charlie!
```

#### Example 2: Multiple Defaults

```python
def create_user(username, email, role="user", active=True):
    return {
        "username": username,
        "email": email,
        "role": role,
        "active": active
    }

user1 = create_user("alice", "alice@example.com")
user2 = create_user("bob", "bob@example.com", role="admin")
user3 = create_user("charlie", "charlie@example.com", active=False)
```

# Bad - mutable default
def add_item_bad(item, items=[]):
    items.append(item)
    return items

print(add_item_bad(1))  # [1]
print(add_item_bad(2))  # [1, 2] - Unexpected!

# Good - use None
def add_item_good(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items

print(add_item_good(1))  # [1]
print(add_item_good(2))  # [2] - Expected!

### Best Practices

1. Write clear, readable code
2. Handle errors appropriately
3. Follow Python conventions
4. Document your code
5. Test thoroughly

### Common Mistakes

1. Not handling edge cases
2. Overcomplicating simple tasks
3. Not following naming conventions

### Key Takeaways

1. Understanding the core concept is essential
2. Practice with real examples
3. Apply best practices
4. Avoid common pitfalls
5. Write clean, maintainable code


---

