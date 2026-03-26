# Apply Logical Operators

## What You'll Learn
In this lesson, you'll learn how to combine multiple conditions using logical operators.

## Why Logical Operators?
Sometimes you need to check **multiple conditions** at once:
- "Is the user logged in AND is the password correct?"
- "Is it weekend OR is it a holiday?"
- "Is the form NOT submitted yet?"

### Real Life is Complex

Real decisions rarely depend on just one thing:
- **Bank loan**: Need good credit AND stable income AND no bankruptcies
- **Movie recommendation**: Action OR Comedy OR (Drama AND highRating)
- **Free shipping**: (price > $50) OR (isPremiumMember)

Programs model these complex decisions using logical operators!

### Simple Analogy

**AND** is like a **security checkpoint with multiple steps**:
- Show ID ✓ AND pass metal detector ✓ AND have valid ticket ✓ = enter
- Miss any one? Can't enter.

**OR** is like **multiple doors to enter a building**:
- Front door OR side door OR back door = any works!

**NOT** is like a **light switch flip**:
- Light is ON → flip switch → NOT ON (OFF)
- Statement is True → apply NOT → False

## The Three Logical Operators

### AND - Both Must Be True
```python
age = 20
has_license = True

can_drive = age >= 18 and has_license
print(can_drive)  # True (both conditions are true)
```

### OR - At Least One Must Be True
```python
is_weekend = True
is_holiday = False

can_relax = is_weekend or is_holiday
print(can_relax)  # True (one condition is true)
```

### NOT - Reverses the Value
```python
is_raining = False
should_go_out = not is_raining
print(should_go_out)  # True (reversed False to True)
```

## Quick Reference

| Operator | Meaning | Example |
|----------|---------|---------|
| `and` | Both must be True | `a and b` |
| `or` | At least one True | `a or b` |
| `not` | Reverse the value | `not a` |

## Real-World Example
```python
age = 25
income = 50000

qualifies_for_loan = age >= 21 and income >= 30000
print(qualifies_for_loan)  # True
```

## What's Next?
In the main lesson, you'll learn:
- Truth tables for each operator
- How to combine multiple logical operators
- Short-circuit evaluation
- Practical applications


---

# Write If Statements

## What You'll Learn
In this lesson, you'll learn how to make your program execute code only when certain conditions are met.

## What is an If Statement?
An `if` statement lets your program make decisions:
- "IF it's raining, bring an umbrella"
- "IF age is 18 or above, allow voting"
- "IF password is correct, log in"

### The Power of Decisions

Think about your favorite apps:
- **Instagram**: IF you double-tap, show heart animation
- **Netflix**: IF you've watched 3 episodes, suggest next season
- **Gmail**: IF message contains "attachment" but no file attached, warn user
- **Games**: IF health <= 0, game over screen

Every smart behavior comes from if statements making decisions!

### Simple Analogy

If statements are like **conditional instructions**:
- Recipe: "IF dough is sticky, add more flour"
- Alarm: "IF time == 7:00 AM, ring"
- Thermostat: "IF temperature < 68°F, turn on heat"

Programs work the same way - do something only when a condition is met.

## Basic Syntax

```python
if condition:
    # Code to run if condition is True
    print("Condition was True!")
```

**Important**: Notice the colon `:` and the indentation!

## Simple Example

```python
age = 20

if age >= 18:
    print("You are an adult")
```

This will print "You are an adult" because 20 >= 18 is True.

## The Condition Can Be False

```python
age = 15

if age >= 18:
    print("You are an adult")

# Nothing prints because condition is False
```

## Indentation Matters!

```python
# Correct - indented code runs if condition is True
if age >= 18:
    print("You can vote")
    print("You are an adult")

# Wrong - IndentationError
if age >= 18:
print("You can vote")  # Missing indentation!
```

## Using Comparison Operators

```python
score = 85

if score >= 60:
    print("You passed!")

if score >= 90:
    print("Excellent work!")
```

## Using Logical Operators

```python
age = 20
has_license = True

if age >= 18 and has_license:
    print("You can drive")
```

## What's Next?
In the main lesson, you'll learn:
- Detailed syntax rules
- Multiple statements in if blocks
- Common mistakes and how to avoid them
- Real-world applications


---

# Write Elif Statements

## Why Elif?

Real life has more than two options:
- **Weather**: Sunny, Cloudy, Rainy, Snowy (not just good/bad)
- **Grades**: A, B, C, D, F (not just pass/fail)
- **Age groups**: Child, Teen, Adult, Senior

Elif lets you handle multiple possibilities elegantly!

### Simple Analogy

Think of elif like **choosing an outfit based on weather**:
- IF temperature > 80: wear shorts
- ELIF temperature > 60: wear jeans
- ELIF temperature > 40: wear jacket
- ELSE: wear heavy coat

The program checks from top to bottom and stops at the first match.

## What You'll Learn
In this lesson, you'll learn how to handle multiple different conditions using elif (else if) statements.

## The Problem with Multiple If Statements

With only `if` statements, every condition is checked:

```python
score = 85

if score >= 90:
    print("Grade: A")

if score >= 80:
    print("Grade: B")  # This also runs!

if score >= 70:
    print("Grade: C")

# Output: Grade: B (and we also check unnecessary conditions)
```

This works, but it's inefficient and can give unexpected results.

## Enter elif (Else If)

`elif` lets you check multiple conditions, but **stops as soon as one is True**:

```python
score = 85

if score >= 90:
    print("Grade: A")
elif score >= 80:
    print("Grade: B")  # This runs and stops!
elif score >= 70:
    print("Grade: C")  # Never checked

# Output: Grade: B
```

## Basic Syntax

```python
if condition1:
    # Runs if condition1 is True
elif condition2:
    # Runs if condition1 is False and condition2 is True
elif condition3:
    # Runs if condition1 and condition2 are False, and condition3 is True
```

**Key Points**:
- `elif` comes after `if`
- You can have multiple `elif` statements
- Only ONE block runs (the first True condition)
- Once a condition is True, the rest are skipped

## Real-World Example

```python
age = 15

if age >= 18:
    print("Adult ticket: $12")
elif age >= 13:
    print("Teen ticket: $8")  # This runs!
elif age >= 5:
    print("Child ticket: $5")
else:
    print("Free for under 5")

# Output: Teen ticket: $8
```

## What's Next?
In the main lesson, you'll learn:
- Detailed elif syntax and rules
- When to use elif vs multiple if statements
- Common patterns and best practices
- Debugging elif chains


---

# Write Else Statements

## What is Else?

`else` handles "everything else" - what happens when all other conditions are False.

### Simple Analogy

Think of else like **"Plan B"**:
- IF it's sunny, go to beach; ELSE stay home and watch movies
- IF have cash, pay with cash; ELSE use credit card
- IF item in stock, ship it; ELSE send backorder notice

It's your fallback option!

## What You'll Learn
In this lesson, you'll learn how to use else statements to handle cases when all previous conditions are False.

## The Else Statement

`else` provides a **default action** when no previous conditions match:

```python
age = 10

if age >= 18:
    print("Adult ticket")
elif age >= 13:
    print("Teen ticket")
else:
    print("Child ticket")  # Runs for ages < 13

# Output: Child ticket
```

## Key Characteristics

1. **No Condition**: else doesn't check anything - it runs when all above conditions are False
2. **Must Be Last**: else always comes at the end
3. **Catches Everything Else**: Handles all remaining cases

## Syntax

```python
if condition1:
    # Code if condition1 is True
elif condition2:
    # Code if condition2 is True
else:
    # Code if ALL above are False
```

## Examples

### Example 1: Pass/Fail

```python
score = 55

if score >= 60:
    print("Pass")
else:
    print("Fail")  # Runs for score < 60
```

### Example 2: Complete Grade System

```python
score = 45

if score >= 90:
    print("A")
elif score >= 80:
    print("B")
elif score >= 70:
    print("C")
elif score >= 60:
    print("D")
else:
    print("F")  # Handles everything below 60

# Output: F
```

## What's Next?
In the main lesson, you'll learn:
- Detailed else syntax
- When to use else vs explicit conditions
- Complete if-elif-else patterns
- Real-world applications


---

# Writing Nested Conditionals

## What You'll Learn
In this lesson, you'll learn how to write if statements inside other if statements (nested conditionals) to handle complex decision-making logic.

## Why This Matters
Real-world programs often need to make decisions based on multiple conditions that depend on each other. For example:
- A login system checks if the username exists, then checks if the password matches
- An online store checks if you have enough money, then checks if the item is in stock
- A game checks if you pressed a key, then checks which key it was

Nested conditionals let you create these layered decision trees.

---

## What are Nested Conditionals?

A **nested conditional** is simply an if statement placed inside another if statement. Think of it like a decision tree where you only check some conditions if previous conditions are met.

```python
if outer_condition:
    if inner_condition:
        # Runs only if BOTH conditions are True
```

## Simple Example

Imagine you're checking if someone can drive:

```python
age = 20
has_license = True

if age >= 18:
    print("You're old enough")
    if has_license:
        print("You can drive!")
    else:
        print("You need to get a license first")
else:
    print("You're too young to drive")
```

**How it works:**
1. First, check if age >= 18
2. **Only if** that's True, then check if they have a license
3. If age < 18, we never check the license (because it doesn't matter)

---

## When to Use Nested Conditionals

Use nested conditionals when:
- The second condition only makes sense if the first is True
- You need different messages or actions at each level
- Conditions depend on each other sequentially

**Example: File Upload System**

```python
file_size = 2.5  # in MB
file_format = "jpg"

if file_size <= 5:
    if file_format in ["jpg", "png", "gif"]:
        print("Upload successful!")
    else:
        print("Invalid format. Use JPG, PNG, or GIF")
else:
    print("File too large. Maximum 5MB")
```

---

## Nested Conditionals vs Logical Operators

Sometimes you can use nested conditionals OR logical operators (`and`). They're not always the same!

### When They're Equivalent

```python
# Using nested if
if age >= 18:
    if has_license:
        print("Can drive")

# Using and operator
if age >= 18 and has_license:
    print("Can drive")
```

### When Nested Is Better

When you need different actions at each level:

```python
# Nested allows different messages
if age >= 18:
    if has_license:
        print("You can drive")
    else:
        print("Get a license first")
else:
    print("Wait until you're 18")

# With 'and', you only get one message
if age >= 18 and has_license:
    print("You can drive")
else:
    print("You can't drive")  # Why? We don't know!
```

---

## Visual Example: Login System

```python
username = "alice"
password = "secret123"

if username == "alice":
    print("Username found")
    if password == "secret123":
        print("Login successful!")
        print("Welcome to your account")
    else:
        print("Wrong password")
else:
    print("Username not found")
```

**Decision tree:**
Is username correct?
├─ NO → "Username not found"
└─ YES → "Username found"
    │
    Is password correct?
    ├─ NO → "Wrong password"
    └─ YES → "Login successful!"

---

## Be Careful with Indentation!

Python uses indentation to show which code belongs to which if statement:

```python
# Correct
if condition1:
    if condition2:      # Inside first if
        print("Both True")

# Wrong - IndentationError
if condition1:
if condition2:          # Not indented - Python doesn't know this is nested
    print("Both True")
```

---

## Try It Yourself (Before Class)

Run this code and see what happens:

```python
score = 75
bonus_completed = True

if score >= 60:
    print("You passed!")
    if bonus_completed:
        print("Bonus points added!")
        score = score + 10
        print(f"New score: {score}")
    else:
        print("No bonus")
else:
    print("You need to score at least 60")

print(f"Final score: {score}")
```

**Try changing:**
- `score` to 50 - what happens?
- `bonus_completed` to False - what happens?
- Both values - can you predict the output?

---

## Key Takeaways

Before class, remember:
1. **Nested if = if inside if** - for layered decision-making
2. **Inner code runs only if outer condition is True** - sequential checking
3. **Indentation matters** - shows which if contains which code
4. **Use for dependent conditions** - when second check only makes sense if first is True
5. **Alternative exists** - sometimes `and` operator is simpler

## Questions to Think About

Come to class ready to discuss:
- When would you use nested conditionals instead of `and`/`or` operators?
- Can you have an if nested inside an else block?
- How many levels deep can you nest conditionals? Should you?

## What's Next?

In the live session, we'll:
- Write complex nested conditional structures
- Learn when to use nesting vs logical operators
- Avoid common mistakes like too-deep nesting
- Build programs with multiple decision levels
- Practice debugging nested conditionals

See you in class!


---

# Accept User Input

## The input() Function

Makes programs interactive!

```python
name = input("What's your name? ")
print("Hello, " + name + "!")
```

### What Makes Programs Useful?

Think about the apps you use:
- **Calculator**: Takes YOUR numbers, not fixed numbers
- **Google**: Searches for YOUR query
- **Games**: Respond to YOUR actions
- **Chat apps**: Send YOUR messages

Without input, programs would be like movies - you just watch. With input, they're like video games - you participate!

### Simple Analogy

**Without input():**
```python
print("Hello Alice!")  # Always greets Alice
```
Like a recording: "Hello Alice!" plays the same way every time.

**With input():**
```python
name = input("Your name: ")
print("Hello " + name + "!")  # Greets anyone!
```
Like a live conversation: "What's your name?" → You answer → "Hello [your name]!"

### Why This Matters

Before input, all your variables had fixed values:
```python
age = 25  # Hard-coded
score = 100  # Fixed
name = "Alice"  # Same every run
```

With input, your programs work with ANY data:
```python
age = int(input("Your age: "))  # Different each time!
```

This is the difference between a **demo** and a **real application**.

## Important: input() Returns String

```python
age = input("Age: ")  # User types: 25
print(type(age))      # <class 'str'>  "25", not 25!
```

Must convert for math:
```python
age = input("Age: ")
age_int = int(age)
next_year = age_int + 1
```

## Example: Interactive Calculator
```python
num1 = input("First number: ")
num2 = input("Second number: ")
sum_result = int(num1) + int(num2)
print("Sum:", sum_result)
```

## Try It
```python
name = input("Your name: ")
age = input("Your age: ")
print(f"Hello {name}, you are {age} years old!")
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

