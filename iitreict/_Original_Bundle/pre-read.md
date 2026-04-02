# Define Functions

## What are Functions?

Functions are **reusable blocks of code** that perform a specific task. They're like creating your own custom commands!

### Simple Analogy

Think of functions like **remote control buttons**:
- **Power button**: One press does a complex series of actions (turn on screen, load settings, connect to input, etc.)
- **You**: Just press button (call function)
- **Inside**: Complex code runs automatically
- **Result**: You work at a higher level - thinking in terms of actions, not implementation

Or like **a vending machine**:
- **Button**: You press "A3"
- **Hidden process**: Machine finds item, charges card, dispenses snack
- **You see**: Simple interface, complex internals

Functions let you create **custom actions** for your program!

### Why Functions Matter

Before functions, programmers copied the same code everywhere:
```python
# Without functions - repetitive nightmare!
print("-" * 40)
print("Welcome!")
print("-" * 40)

# ...100 lines later...
print("-" * 40)  # Same code again!
print("Welcome!")
print("-" * 40)
```

With functions - write once, use anywhere:
```python
def show_welcome():
    print("-" * 40)
    print("Welcome!")
    print("-" * 40)

show_welcome()  # Use anywhere, anytime!
```

## What are Functions?

Functions are reusable blocks of code that perform a specific task.

```python
def greet():
    print("Hello!")

greet()  # Calls the function
# Output: Hello!
```

## Why Use Functions?

1. **Reusability**: Write once, use many times
2. **Organization**: Break code into logical pieces
3. **Maintainability**: Easier to update and debug

## Basic Syntax

```python
def function_name():
    # Code here
    pass
```

## Example

```python
def say_hello():
    print("Hello, World!")

# Call the function
say_hello()
say_hello()
say_hello()

# Output: Hello, World! (3 times)
```


---

# Using Function Parameters to Accept Inputs

## Why Parameters?

Imagine having a different calculator for every pair of numbers you want to add - one for "5+3", another for "10+20", etc. Ridiculous, right? **Parameters** solve this - one function, infinite uses!

### Simple Analogy

Think of parameters like **placeholders in a Mad Libs game**:
- **Template**: "Hello, [NAME]! You are [AGE] years old."
- **Parameters**: NAME and AGE are slots you fill in
- **Infinite variations**: Change the values, get different results
- **Same structure**, different data each time

Or like **a blender**:
- **Machine**: The function (same every time)
- **Ingredients**: Parameters (different each time)
- **Banana smoothie**: blend(banana, milk, ice)
- **Berry smoothie**: blend(berries, yogurt, ice)
- **One blender**, many recipes!

## What You'll Learn
In this lesson, you'll learn how to define functions that accept parameters (inputs), making your functions flexible and reusable for different data.

## Why This Matters
Functions without parameters can only do one specific thing. Parameters make functions powerful:
- A calculator function that works with any two numbers
- A greeting function that works with any name
- A discount function that works with any price
- A validation function that checks any password

Parameters turn functions from single-use tools into versatile, reusable building blocks.

---

## What are Function Parameters?

**Parameters** are variables listed in the function definition that accept values when the function is called.

```python
def greet(name):  # 'name' is a parameter
    print(f"Hello, {name}!")

greet("Alice")  # "Alice" is an argument
greet("Bob")    # "Bob" is an argument
```

**Key terms:**
- **Parameter**: Variable in function definition (`name`)
- **Argument**: Actual value passed when calling (`"Alice"`, `"Bob"`)

---

## Simple Example

```python
def add(a, b):  # a and b are parameters
    sum_result = a + b
    print(f"{a} + {b} = {sum_result}")

add(5, 3)   # 5 + 3 = 8
add(10, 20) # 10 + 20 = 30
add(7, 2)   # 7 + 2 = 9
```

**One function, many uses!** The same function works with different numbers.

---

## Multiple Parameters

You can have as many parameters as you need:

```python
def introduce(name, age, city):
    print(f"Hi, I'm {name}")
    print(f"I'm {age} years old")
    print(f"I live in {city}")

introduce("Alice", 25, "New York")
introduce("Bob", 30, "London")
```

**Order matters!** Arguments must match parameter order.

---

## Real-World Examples

### Example 1: Calculate Area

```python
def rectangle_area(length, width):
    area = length * width
    print(f"Area: {area} square units")

rectangle_area(5, 3)   # Area: 15 square units
rectangle_area(10, 8)  # Area: 80 square units
```

### Example 2: Check Password Strength

```python
def check_password(password, min_length):
    if len(password) >= min_length:
        print("Password is strong enough")
    else:
        print(f"Password too short. Need {min_length} characters")

check_password("abc", 8)        # Too short
check_password("secure123", 8)  # Strong enough
```

---

## Parameters Make Functions Flexible

### Without Parameters (Limited)

```python
def greet_alice():
    print("Hello, Alice!")

greet_alice()  # Only works for Alice!
```

### With Parameters (Flexible)

```python
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")   # Hello, Alice!
greet("Bob")     # Hello, Bob!
greet("Charlie") # Hello, Charlie!
```

---

## Try It Yourself (Before Class)

Write and run this function:

```python
def multiply(x, y):
    result = x * y
    print(f"{x} × {y} = {result}")

multiply(4, 5)
multiply(10, 3)
multiply(7, 7)
```

**Questions:**
1. What does each call print?
2. Try adding a third parameter `z` - what changes?
3. What happens if you call `multiply(5)` (only one argument)?

---

## Key Takeaways

Before class, remember:
1. **Parameters = inputs** - make functions flexible
2. **Defined in ()** - after function name
3. **Order matters** - arguments match parameter positions
4. **Multiple parameters** - separate with commas
5. **Reusability** - one function, many uses

## What's Next?

In the live session, we'll:
- Write functions with various parameter counts
- Learn about return values (sending data back)
- Explore default parameters
- Understand parameter vs argument terminology

See you in class!


---

# Returning Values from Functions Using Return

## Why Return?

Functions without return are like **ovens that cook but never open** - food cooks inside, but you can't get it out to eat! Return lets functions **give back** their results.

### Simple Analogy

Think of return like **an ATM dispensing cash**:
- **Input**: You insert card, enter PIN, request $100
- **Processing**: ATM validates, checks balance, prepares money
- **Return**: Money comes out of slot (this is the return!)
- **You use it**: Take money, buy things elsewhere

Or like **a microwave**:
- **Input**: Put food in, set timer
- **Processing**: Heats food inside
- **Return**: Beep! Food is ready (you take it out)
- **vs. No return**: Food cooks but stays locked inside forever!

### Return vs Print: The Big Confusion

**Beginners' biggest mistake**: Thinking `print()` and `return` are the same.
- **print()**: Shows something to YOU (the human)
- **return**: Gives something to THE CODE (for further use)

```python
def bad_calculator(a, b):
    print(a + b)  # Shows answer, but code can't use it!

# This doesn't work!
total = bad_calculator(5, 3) + 10  # Error! Can't add None + 10
```

Return makes functions **useful**, not just pretty displays!

## What You'll Learn
In this lesson, you'll learn how to use the `return` statement to send values back from functions, allowing you to use function results in other parts of your code.

## Why This Matters
So far, functions just print output. But what if you want to:
- Use a function's result in a calculation
- Store a function's result in a variable
- Pass a function's result to another function
- Make decisions based on a function's result

The `return` statement lets functions send data back to the caller, making them infinitely more useful.

---

## What is Return?

The `return` statement sends a value back from a function to wherever it was called.

```python
def add(a, b):
    return a + b  # Send the sum back

result = add(5, 3)  # result gets 8
print(result)       # 8
```

**Key difference:**
- `print()` → Shows output on screen
- `return` → Sends value back to use in code

---

## Return vs Print

### With Print (Limited)

```python
def add(a, b):
    print(a + b)  # Just displays

add(5, 3)  # Shows: 8
x = add(5, 3)  # x gets None (not the sum!)
print(x)  # None
```

### With Return (Flexible)

```python
def add(a, b):
    return a + b  # Send value back

result = add(5, 3)  # result gets 8
x = add(10, 20)     # x gets 30
total = add(result, x)  # Can use in more calculations!
print(total)  # 38
```

---

## Simple Examples

### Example 1: Calculate and Return

```python
def square(n):
    return n * n

x = square(5)  # x = 25
y = square(10) # y = 100
z = x + y      # z = 125
print(z)       # 125
```

### Example 2: Return in Conditions

```python
def is_even(n):
    if n % 2 == 0:
        return True
    else:
        return False

if is_even(4):
    print("4 is even")  # This prints!

if is_even(7):
    print("7 is even")  # This doesn't run
```

---

## Return Stops Function Execution

Once `return` runs, the function exits immediately:

```python
def check_age(age):
    if age < 18:
        return "Too young"  # Exit here if True
    return "Old enough"     # Only runs if age >= 18

print(check_age(15))  # "Too young"
print(check_age(25))  # "Old enough"
```

---

## Real-World Examples

### Example 1: Calculate Discount

```python
def apply_discount(price, discount_percent):
    discount = price * (discount_percent / 100)
    final_price = price - discount
    return final_price

shirt_price = apply_discount(50, 20)   # $40
shoe_price = apply_discount(100, 15)   # $85
total = shirt_price + shoe_price       # $125
print(f"Total: ${total}")
```

### Example 2: Validate Password

```python
def is_strong_password(password):
    if len(password) < 8:
        return False
    if not any(c.isdigit() for c in password):
        return False
    return True

password = input("Enter password: ")
if is_strong_password(password):
    print("Password accepted!")
else:
    print("Password too weak!")
```

---

## Returning Multiple Values

Python can return multiple values using tuples:

```python
def get_user_info():
    name = "Alice"
    age = 25
    city = "NYC"
    return name, age, city  # Returns tuple

name, age, city = get_user_info()
print(f"{name}, {age}, {city}")  # Alice, 25, NYC
```

---

## Try It Yourself (Before Class)

Run this code:

```python
def calculate_rectangle(length, width):
    area = length * width
    perimeter = 2 * (length + width)
    return area, perimeter

a, p = calculate_rectangle(5, 3)
print(f"Area: {a}, Perimeter: {p}")
```

**Questions:**
1. What values do `a` and `p` get?
2. Try returning just `area` without `perimeter` - what changes?
3. What happens if you don't return anything?

---

## Key Takeaways

Before class, remember:
1. **return sends values back** - for use in other code
2. **return exits function** - code after return doesn't run
3. **return vs print** - return for data, print for display
4. **Can return anything** - numbers, strings, booleans, tuples
5. **No return = None** - functions return None by default

## What's Next?

In the live session, we'll:
- Practice writing functions with return values
- Use returned values in complex calculations
- Understand when to return vs print
- Learn about returning multiple values
- Handle functions that return None

See you in class!


---

# Applying Default Parameters in Function Definitions

## Why Default Parameters?

Functions with lots of required parameters are annoying to use. Default parameters make functions **easy to use** for common cases while staying **powerful** for complex cases!

### Simple Analogy

Think of default parameters like **ordering coffee**:
- **Required**: "I want a coffee" (must specify)
- **Defaults**: Medium size, regular milk, normal temperature (most people want this)
- **Override when needed**: "Make it large, almond milk, extra hot" (customize!)
- **Result**: Quick ordering for most, detailed control when needed

Or like **phone settings**:
- **Auto-brightness**: ON by default (works for 90% of users)
- **Override**: Turn off and set manually if you want
- **Smart design**: Common case is easy, customization available

Default parameters = **convenience with flexibility**!

## What You'll Learn
In this lesson, you'll learn how to provide default values for function parameters, making some arguments optional when calling the function.

## Why This Matters
Default parameters make functions more flexible:
- Users can provide custom values OR use sensible defaults
- Reduces the number of required arguments
- Makes functions easier to use
- Common in professional Python code

Example: `print()` has default parameter `end="\n"` (newline). You can override it: `print("Hi", end="")`

---

## What are Default Parameters?

**Default parameters** have predefined values. If the caller doesn't provide a value, the default is used.

```python
def greet(name, greeting="Hello"):  # greeting has default
    print(f"{greeting}, {name}!")

greet("Alice")              # Hello, Alice! (uses default)
greet("Bob", "Hi")          # Hi, Bob! (overrides default)
greet("Charlie", "Hey")     # Hey, Charlie!
```

---

## Syntax

```python
def function_name(required_param, optional_param=default_value):
    # code
```

**Rules:**
1. Default parameters come AFTER required parameters
2. Can have multiple default parameters
3. Caller can override any default

---

## Simple Examples

### Example 1: Power Function

```python
def power(base, exponent=2):  # exponent defaults to 2 (square)
    return base ** exponent

print(power(5))      # 25 (5²)
print(power(5, 3))   # 125 (5³)
print(power(2, 4))   # 16 (2⁴)
```

### Example 2: Greeting with Time

```python
def greet(name, time_of_day="day"):
    if time_of_day == "morning":
        print(f"Good morning, {name}!")
    elif time_of_day == "evening":
        print(f"Good evening, {name}!")
    else:
        print(f"Good day, {name}!")

greet("Alice")                # Good day, Alice!
greet("Bob", "morning")       # Good morning, Bob!
greet("Charlie", "evening")   # Good evening, Charlie!
```

---

## Real-World Examples

### Example 1: Calculate Discount

```python
def apply_discount(price, discount=10):  # 10% default
    final_price = price * (1 - discount/100)
    return final_price

print(apply_discount(100))      # 90 (10% off)
print(apply_discount(100, 20))  # 80 (20% off)
print(apply_discount(50, 5))    # 47.5 (5% off)
```

### Example 2: Print Message with Separator

```python
def print_message(message, sep="-", length=20):
    print(sep * length)
    print(message)
    print(sep * length)

print_message("Hello")           # Uses defaults
print_message("Python", "*")     # Custom separator
print_message("Code", "=", 30)   # Custom both
```

---

## Order Matters!

**Correct:** Required parameters first, then defaults

```python
def greet(name, greeting="Hello"):  # ✓ Correct
    print(f"{greeting}, {name}")
```

**Incorrect:** Defaults before required

```python
def greet(greeting="Hello", name):  # ✗ SyntaxError!
    print(f"{greeting}, {name}")
```

---

## Multiple Defaults

```python
def create_profile(name, age=18, city="Unknown", country="USA"):
    print(f"{name}, {age}, {city}, {country}")

create_profile("Alice")                    # Alice, 18, Unknown, USA
create_profile("Bob", 25)                  # Bob, 25, Unknown, USA
create_profile("Charlie", 30, "NYC")       # Charlie, 30, NYC, USA
create_profile("David", 22, "LA", "USA")   # David, 22, LA, USA
```

---

## Try It Yourself (Before Class)

```python
def repeat_text(text, times=3, separator=" "):
    return (text + separator) * times

print(repeat_text("Hi"))
print(repeat_text("Hello", 5))
print(repeat_text("Python", 2, "-"))
```

**Questions:**
1. What does each call output?
2. Can you call it with just one argument?
3. What happens if you provide all three arguments?

---

## Key Takeaways

Before class, remember:
1. **Default parameters** - have predefined values
2. **Optional arguments** - caller can skip or override
3. **Order matters** - required first, defaults after
4. **Multiple defaults** - can have many
5. **Common pattern** - makes functions flexible

## What's Next?

In the live session, we'll:
- Write functions with multiple default parameters
- Learn about keyword arguments
- Understand when to use defaults
- Combine defaults with return values

See you in class!


---

