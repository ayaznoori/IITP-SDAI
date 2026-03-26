# Apply Logical Operators

## Introduction

Logical operators are the **building blocks of complex decision-making**. They allow programs to combine multiple conditions and make sophisticated choices.

---

<div align="center">

![Python Logical Operators Truth Table](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.14.png)

*Truth tables for AND, OR, and NOT — Python's logical operators (and, or, not) follow these exact rules*

</div>

---

### The Foundation: Boolean Algebra

These operators implement **Boolean algebra**, invented by George Boole in 1847. This mathematical system forms the foundation of:
- All digital logic circuits (AND, OR, NOT gates)
- Database queries (SQL WHERE clauses)
- Search engines (Google's AND, OR, NOT operators)
- Every programming language's conditional logic

**Why this matters**: The logic gates in your CPU physically implement these operations billions of times per second!

### Real-World Analogy

Think of logical operators like **decision-making rules**:

**AND** is like **requirements** that ALL must be met:
- To drive: Need license AND age >= 16 AND vehicle
- To graduate: Pass all courses AND complete project AND attend ceremony

**OR** is like **alternatives** where ANY works:
- To pay: Cash OR card OR mobile payment
- To contact: Email OR phone OR in-person

**NOT** is like **opposite/negation**:
- NOT raining = sunny
- NOT logged_in = logged_out
- NOT available = busy

Or think of them like **filters stacked on each other**:
- Photo filter 1 AND filter 2 AND filter 3 = all must apply
- Show results from source A OR source B = either is fine

## Logical Operators

Logical operators combine or modify boolean values. They're essential for creating complex conditions.

### The Three Logical Operators

| Operator | Description | Returns True When |
|----------|-------------|-------------------|
| `and` | Both conditions must be True | Both are True |
| `or` | At least one condition must be True | One or both are True |
| `not` | Reverses the boolean value | The value is False |

## The `and` Operator

Returns `True` only if **both** conditions are True.

### Truth Table

| A | B | A and B |
|---|---|---------|
| True | True | **True** |
| True | False | False |
| False | True | False |
| False | False | False |

### Examples

```python
# Simple example
age = 20
has_license = True
can_drive = age >= 18 and has_license
print(can_drive)  # True

# Multiple conditions
temperature = 25
is_sunny = True
is_weekend = True
perfect_day = temperature > 20 and is_sunny and is_weekend
print(perfect_day)  # True

# When one is False
score = 85
passed_exam = score >= 60
submitted_assignment = False
can_pass_course = passed_exam and submitted_assignment
print(can_pass_course)  # False (one is False!)
```

### Real-World: Loan Qualification

```python
age = 25
income = 50000
has_good_credit = True

qualifies = age >= 21 and income >= 30000 and has_good_credit
print(f"Qualifies for loan: {qualifies}")  # True
```

## The `or` Operator

Returns `True` if **at least one** condition is True.

### Truth Table

| A | B | A or B |
|---|---|--------|
| True | True | **True** |
| True | False | **True** |
| False | True | **True** |
| False | False | False |

### Examples

```python
# Simple example
is_weekend = True
is_holiday = False
can_sleep_in = is_weekend or is_holiday
print(can_sleep_in)  # True (one is True!)

# Both are True
has_cash = True
has_card = True
can_pay = has_cash or has_card
print(can_pay)  # True

# Both are False
is_raining = False
is_snowing = False
is_bad_weather = is_raining or is_snowing
print(is_bad_weather)  # False (both are False)
```

### Real-World: Access Control

```python
is_admin = False
is_owner = True
has_access = is_admin or is_owner
print(f"Has access: {has_access}")  # True
```

## The `not` Operator

Reverses the boolean value.

### Truth Table

| A | not A |
|---|-------|
| True | False |
| False | True |

### Examples

```python
# Simple reversal
is_raining = False
is_sunny = not is_raining
print(is_sunny)  # True

# With comparisons
age = 16
is_adult = not (age < 18)  # Same as age >= 18
print(is_adult)  # False

# Double negative
is_logged_in = True
is_not_logged_in = not is_logged_in
is_logged_in_again = not is_not_logged_in
print(is_logged_in_again)  # True (double negative!)
```

### Real-World: Form Validation

```python
form_submitted = False
can_edit = not form_submitted
print(f"Can edit: {can_edit}")  # True
```

## Combining Logical Operators

You can combine multiple logical operators to create complex conditions.

### Example 1: Age Range Check

```python
age = 25
is_adult = age >= 18 and age < 65
print(f"Working age adult: {is_adult}")  # True
```

### Example 2: Access Control

```python
is_admin = False
is_moderator = True
is_owner = False

has_special_access = is_admin or is_moderator or is_owner
print(f"Has special access: {has_special_access}")  # True
```

# Can register if (18+) OR (under 18 but has parent consent)
can_register = (age >= 18) or (age < 18 and has_parent_consent and has_id)
print(f"Can register: {can_register}")  # True

## Operator Precedence

When combining operators, Python evaluates in this order:

1. **Parentheses** `()`
2. **not**
3. **and**
4. **or**

### Examples

```python
# Without parentheses
result = True or True and False
# Evaluates as: True or (True and False) = True or False = True
print(result)  # True

# With parentheses to force different order
result = (True or True) and False
# Evaluates as: True and False = False
print(result)  # False
```

**Best Practice**: Use parentheses for clarity!

```python
# Harder to read
can_proceed = age >= 18 and has_license or has_permit and has_supervisor

# Clearer with parentheses
can_proceed = (age >= 18 and has_license) or (has_permit and has_supervisor)
```

## Short-Circuit Evaluation

Python evaluates logical operators **left to right** and stops as soon as the result is determined.

### With `and`

```python
# If first is False, second is never checked
def expensive_check():
    print("Expensive check called")
    return True

age = 16
result = age >= 18 and expensive_check()
# expensive_check() is NEVER called because age >= 18 is False
print(result)  # False
```

### With `or`

```python
# If first is True, second is never checked
is_admin = True
result = is_admin or expensive_check()
# expensive_check() is NEVER called because is_admin is already True
print(result)  # True
```

**Why This Matters**: You can safely check conditions without errors:

```python
username = None
# Safe: won't error because len() is never called if username is None
is_valid = username is not None and len(username) > 0
print(is_valid)  # False
```

## Common Patterns

### Checking Ranges

```python
# Age between 18 and 65
age = 30
is_working_age = age >= 18 and age <= 65
# Or more concisely:
is_working_age = 18 <= age <= 65  # Python allows chaining!
print(is_working_age)  # True
```

### Validating Multiple Conditions

```python
password = "secure123"
username = "john_doe"

# All must be true
is_valid = (
    len(username) >= 3 and
    len(password) >= 8 and
    username != password
)
print(f"Valid registration: {is_valid}")  # True
```

### Default Values with `or`

```python
name = ""
display_name = name or "Guest"
print(display_name)  # "Guest" (empty string is falsy)

name = "Alice"
display_name = name or "Guest"
print(display_name)  # "Alice"
```

## Common Mistakes

### Using `&` or `|` Instead of `and`/`or`

```python
# Wrong (these are bitwise operators)
result = True & False  # Works but not recommended for booleans

# Correct
result = True and False
```

### Forgetting Parentheses

```python
# Confusing
age = 20
has_license = True
can_drive = age >= 18 and has_license or not has_license

# Clear
can_drive = (age >= 18 and has_license) or (not has_license)
```

### Redundant Comparisons

```python
# Redundant
is_adult = (age >= 18) == True

# Better
is_adult = age >= 18
```

## Practice Exercise

Predict the output:

```python
a = True
b = False
c = True

print(a and b)           # ?
print(a or b)            # ?
print(not b)             # ?
print(a and b or c)      # ?
print(a and (b or c))    # ?
print(not (a and b))     # ?
```

<details>
<summary>Answer</summary>

```python
print(a and b)           # False
print(a or b)            # True
print(not b)             # True
print(a and b or c)      # True (evaluates as (a and b) or c)
print(a and (b or c))    # True
print(not (a and b))     # True
```
</details>

## Key Takeaways

1. **`and`**: Both conditions must be True
2. **`or`**: At least one condition must be True
3. **`not`**: Reverses the boolean value
4. **Precedence**: `not` > `and` > `or`
5. **Short-circuit**: Evaluation stops when result is determined
6. **Use parentheses**: Makes complex conditions clearer


---

# Write If Statements

## Introduction

The `if` statement is the most fundamental **control flow** structure in programming. It allows programs to make decisions and respond differently to different situations - the essence of intelligence.

---

<div align="center">

<div style="overflow: hidden; display: inline-block;">
  <img src="https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.15.png" 
       style="margin-top: -50px; display: block;" 
       alt="Python if Statement Flowchart">
</div>

*The if statement evaluates a condition: if True, execute one block; if False, skip it or execute an alternative*

</div>

---

### Why If Statements Are Revolutionary

**Before if statements** (early computers, 1940s):
- Programs executed every instruction in sequence
- No decisions, no branches
- Same output every time for same input

**After if statements** (FORTRAN, 1957):
- Programs could choose different paths
- Respond to varying conditions
- Adaptive, intelligent behavior

This single feature transformed computers from calculators into decision-making machines!

### Real-World Analogy

If statements are like **forks in a road**:
- You're driving: IF road is blocked, take detour
- Otherwise, continue straight

Or like **automated doors**:
- IF person approaches, open door
- IF no one nearby, stay closed

Or like **your brain's decisions**:
- IF hungry, eat
- IF tired, sleep
- IF danger, run

Programs use if statements the same way - constant decision-making based on conditions.

## If Statements

An `if` statement allows your program to execute code **only when a condition is True**.

### Basic Syntax

```python
if condition:
    # Code block (indented)
    # Runs only if condition is True
```

**Key Components**:
1. The `if` keyword
2. A condition (boolean expression)
3. A colon `:`
4. Indented code block

## Simple Examples

### Example 1: Basic If Statement

```python
age = 20

if age >= 18:
    print("You are an adult")

# Output: You are an adult
```

### Example 2: Condition is False

```python
age = 15

if age >= 18:
    print("You are an adult")

# No output (condition is False, code doesn't run)
```

# All three lines print because condition is True

## Indentation Rules

Python uses **indentation** (spaces or tabs) to group code blocks. This is **required**, not optional!

### Correct Indentation

```python
if age >= 18:
    print("You can vote")      # Indented - part of if block
    print("You are an adult")  # Indented - part of if block
print("Program continues")     # Not indented - always runs
```

### Indentation Error

```python
if age >= 18:
print("You can vote")  # IndentationError: expected an indented block
```

### Inconsistent Indentation

```python
if age >= 18:
    print("Line 1")  # 4 spaces
        print("Line 2")  # 8 spaces - IndentationError!
```

**Best Practice**: Use **4 spaces** for each indentation level (standard Python convention)

## Using Conditions

### With Comparison Operators

```python
temperature = 30

if temperature > 25:
    print("It's hot outside")

score = 85
if score >= 60:
    print("You passed")

password = "secret"
if password == "secret":
    print("Access granted")
```

### With Logical Operators

```python
age = 20
has_license = True

if age >= 18 and has_license:
    print("You can drive")

is_weekend = True
is_holiday = False

if is_weekend or is_holiday:
    print("You can sleep in")

form_submitted = False
if not form_submitted:
    print("You can still edit")
```

### With Variables

```python
is_logged_in = True

if is_logged_in:
    print("Welcome back!")

is_admin = False
if is_admin:
    print("Admin panel")  # Doesn't print (False)
```

## Multiple If Statements

You can have multiple independent if statements:

```python
score = 85

if score >= 60:
    print("You passed")

if score >= 70:
    print("You got a C or better")

if score >= 80:
    print("You got a B or better")

if score >= 90:
    print("You got an A")

# Output:
# You passed
# You got a C or better
# You got a B or better
```

**Note**: Each if statement is checked independently!

## Real-World Examples

### Example 1: Age Verification

```python
age = int(input("Enter your age: "))

if age >= 18:
    print("You can create an account")
    print("Welcome!")

if age >= 21:
    print("You can access premium features")
```

### Example 2: Password Validation

```python
password = input("Enter password: ")

if len(password) < 8:
    print("Password too short")

if len(password) >= 8:
    print("Password length OK")

if len(password) >= 12:
    print("Strong password!")
```

## Flow Control Visualization

```python
age = 20

print("Start")

if age >= 18:
    print("Adult")  # This runs

print("End")

# Output:
# Start
# Adult
# End
```

```python
age = 15

print("Start")

if age >= 18:
    print("Adult")  # This does NOT run

print("End")

# Output:
# Start
# End
```

## Common Mistakes

### Forgetting the Colon

```python
# Wrong
if age >= 18
    print("Adult")  # SyntaxError: invalid syntax

# Correct
if age >= 18:
    print("Adult")
```

### No Indentation

```python
# Wrong
if age >= 18:
print("Adult")  # IndentationError

# Correct
if age >= 18:
    print("Adult")
```

### Using `=` Instead of `==`

```python
# Wrong - This assigns 18 to age!
if age = 18:  # SyntaxError
    print("18 years old")

# Correct - This compares
if age == 18:
    print("18 years old")
```

### Forgetting Indentation for Multiple Lines

```python
# Wrong - Only first line is in if block
if age >= 18:
    print("You are an adult")
print("You can vote")  # Always runs!

# Correct - Both lines in if block
if age >= 18:
    print("You are an adult")
    print("You can vote")  # Both indented
```

### Empty If Block

```python
# Wrong - If block can't be empty
if age >= 18:

# Correct - Use pass as placeholder
if age >= 18:
    pass  # Do nothing (placeholder)
```

## Practice Exercise

Predict what will be printed:

```python
x = 10

if x > 5:
    print("A")
    print("B")

if x > 15:
    print("C")

print("D")
```

<details>
<summary>Answer</summary>

A
B
D

Explanation:
- x > 5 is True (10 > 5), so "A" and "B" print
- x > 15 is False (10 is not > 15), so "C" doesn't print
- "D" always prints (not in any if block)
</details>

## Key Takeaways

1. **Syntax**: `if condition:`
2. **Colon required**: After the condition
3. **Indentation required**: 4 spaces is standard
4. **Condition**: Any boolean expression
5. **Code block**: All indented lines run if condition is True
6. **Independent checks**: Multiple if statements check independently

## What's Next?

In the next lessons, you'll learn:
- `elif` (else if) for multiple conditions
- `else` for default cases
- Nested if statements
- More complex decision-making


---

# Write Elif Statements

## Introduction

The `elif` (else-if) statement allows programs to check **multiple conditions in sequence** and execute different code for each case. This is essential for modeling real-world scenarios with more than two outcomes.

---

<div align="center">

<div style="overflow: hidden; display: inline-block;">
  <img src="https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/images/1773052261494-94552dbe71da.jpg" alt="if-elseif-ladder.jpg" 
       style="margin-right: -40px; display: block;" 
       alt="Python elif Multiple Conditions Flowchart">
</div>

*elif chains check conditions sequentially — the first True condition executes, then the rest are skipped*

</div>

---

### Why Elif Exists

Real decisions often have multiple outcomes:
- Grade: A, B, C, D, or F (not just pass/fail)
- Traffic light: Red, Yellow, or Green
- Temperature: Too cold, comfortable, or too hot

**The problem with multiple ifs**: All conditions are checked even after finding a match (inefficient)
**The solution: elif**: Stop checking once a condition is True (efficient, clear logic)

### Real-World Analogy

Elif is like a **customer service menu**:
- "Press 1 for billing" → IF choice == 1
- "Press 2 for technical support" → ELIF choice == 2
- "Press 3 for sales" → ELIF choice == 3

Once you press 2, the system doesn't check options 3, 4, 5... It found its match and proceeds!

## Elif Statements

The `elif` (else if) statement allows you to check multiple conditions where **only one block of code will execute**.

### Basic Syntax

```python
if condition1:
    # Runs if condition1 is True
elif condition2:
    # Runs if condition1 is False and condition2 is True
elif condition3:
    # Runs if condition1 and condition2 are False and condition3 is True
```

**Key Characteristics**:
1. `elif` must come after an `if` statement
2. You can have multiple `elif` statements
3. Only the **first True condition** executes
4. Once a match is found, remaining conditions are skipped

## Why Use Elif?

### Problem with Multiple If Statements

```python
score = 85

if score >= 90:
    grade = "A"

if score >= 80:
    grade = "B"  # This runs!

if score >= 70:
    grade = "C"  # This also runs!

print(grade)  # C (last assignment wins!)
```

**Issues**:
- All conditions are checked (inefficient)
- Multiple blocks might execute
- Results can be unexpected

### Solution with Elif

```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"  # This runs and STOPS
elif score >= 70:
    grade = "C"  # Never checked

print(grade)  # B (correct!)
```

**Benefits**:
- Only one block executes
- More efficient (stops after first match)
- Clearer intent

## Examples

### Example 1: Grade Calculator

```python
score = 75

if score >= 90:
    print("Grade: A")
elif score >= 80:
    print("Grade: B")
elif score >= 70:
    print("Grade: C")
elif score >= 60:
    print("Grade: D")

# Output: Grade: C
```

### Example 2: Age Categories

```python
age = 15

if age >= 65:
    print("Senior citizen")
elif age >= 18:
    print("Adult")
elif age >= 13:
    print("Teenager")  # This runs
elif age >= 3:
    print("Child")

# Output: Teenager
```

## Order Matters!

The order of conditions is crucial because Python checks from top to bottom:

### Incorrect Order

```python
score = 95

if score >= 60:  # This is checked first
    print("Pass")  # This runs for 95!
elif score >= 90:  # Never reached!
    print("Excellent")
```

### Correct Order

```python
score = 95

if score >= 90:  # Check specific conditions first
    print("Excellent")  # This runs
elif score >= 60:  # More general conditions later
    print("Pass")
```

**Rule**: Put more **specific** conditions before more **general** ones.

## Real-World Applications

### BMI Calculator

```python
bmi = 27.5

if bmi < 18.5:
    category = "Underweight"
elif bmi < 25:
    category = "Normal weight"
elif bmi < 30:
    category = "Overweight"  # This runs
else:
    category = "Obese"

print(f"BMI Category: {category}")
```

## Key Takeaways

1. **Mutually exclusive**: Only one block executes in an if-elif chain
2. **Order matters**: Check specific conditions before general ones
3. **First match wins**: Stops at first True condition
4. **Must follow if**: `elif` can't be used alone
5. **More efficient**: Stops checking once a match is found


---

# Write Else Statements

## Introduction

The `else` statement provides a **catch-all** for when all other conditions fail. It represents "everything else" or "the default case" - ensuring your program always has a response.

---

<div align="center">

<div style="overflow: hidden; display: inline-block;">
  <img src="https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.17.png" 
       style="margin-top: -100px; display: block;" 
       alt="Python if-else Statement Flowchart">
</div>

*The else block executes when the condition is False — it's the default path that guarantees a response*

</div>

---

### Why Else is Essential

Real decisions need a default:
- **ATM**: IF PIN correct, proceed; ELSE reject
- **Login**: IF credentials match, login; ELSE show error
- **Game**: IF answer correct, award points; ELSE try again

Without else, programs would silently do nothing when conditions fail - confusing for users!

### Real-World Analogy

Else is like a **catch-all bin**:
- Sort laundry: IF white → bin 1, IF colors → bin 2, ELSE (mixed/unknown) → bin 3
- Customer service: IF billing → dept A, IF tech → dept B, ELSE → general support

Or like **"otherwise" in instructions**:
- Recipe: "IF dough is too wet, add flour; OTHERWISE continue to next step"

## Else Statements

The `else` statement provides a **default action** when all previous conditions in an if-elif chain are False.

### Basic Syntax

```python
if condition:
    # Runs if condition is True
else:
    # Runs if condition is False
```

With elif:

```python
if condition1:
    # Code
elif condition2:
    # Code
else:
    # Runs if ALL above conditions are False
```

**Key Points**:
1. No condition needed (no `else score > 50:`)
2. Always comes last
3. Optional - not required
4. Catches all remaining cases

## Simple Examples

### Example 1: Binary Choice

```python
age = 16

if age >= 18:
    print("Can vote")
else:
    print("Cannot vote yet")

# Output: Cannot vote yet
```

### Example 2: Complete Grade System

```python
score = 75

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"Grade: {grade}")  # Grade: C
```

# Output: Negative

## When to Use Else

### Use Else When:
- You want a default action for unmatched cases
- You need to handle "everything else"
- Two possible paths (if-else)

### Skip Else When:
- Not all cases need handling
- You only care about specific conditions

```python
# With else
age = 15
if age >= 18:
    print("Adult")
else:
    print("Minor")

# Without else (also valid)
age = 15
if age >= 18:
    print("Adult")
# Nothing prints if age < 18
```

## Real-World Applications

### Login Validation

```python
username = "admin"
password = "secret123"

if username == "admin" and password == "secret123":
    print("Login successful")
else:
    print("Invalid credentials")
```

### Ticket Pricing

```python
age = 8

if age >= 65:
    price = 8
elif age >= 18:
    price = 12
elif age >= 5:
    price = 6
else:
    price = 0  # Free for under 5

print(f"Ticket price: ${price}")
```

## Common Mistakes

### Condition in Else

```python
# Wrong
if score >= 60:
    print("Pass")
else score < 60:  # SyntaxError
    print("Fail")

# Correct
if score >= 60:
    print("Pass")
else:
    print("Fail")
```

### Else Before Elif

```python
# Wrong
if score >= 90:
    print("A")
else:
    print("Not A")
elif score >= 80:  # SyntaxError
    print("B")

# Correct
if score >= 90:
    print("A")
elif score >= 80:
    print("B")
else:
    print("Below B")
```

## Key Takeaways

1. **No condition**: else doesn't check anything
2. **Always last**: Must come after if/elif
3. **Default handler**: Catches all unmatched cases
4. **Optional**: Not always needed
5. **One else max**: Only one else per if-elif chain


---

# Write Nested Conditionals

## Introduction

Nested conditionals create **decision trees** - multi-level logic where each branch can split further. This mirrors how humans make complex decisions with multiple factors.

---

<div align="center">

![Python Nested if-else Conditional Control Flow](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.18.png)

*Nested conditionals create multi-level decision trees — each branch can contain further if/else checks*

</div>

---

### Why Nesting is Powerful

Real decisions are hierarchical:
- **Security system**: IF motion detected → THEN IF after hours → THEN IF not authorized → sound alarm
- **Email filter**: IF from known sender → THEN IF has attachment → THEN IF attachment type safe → allow

Each decision depends on previous ones - nesting models this perfectly.

### Real-World Analogy

Nested conditionals are like **nested boxes**:
- Open big box → IF it contains small box → open small box → IF it contains item → retrieve item

Or like **navigation instructions**:
- "IF you see the gas station, turn right. THEN IF you see the school, turn left. THEN IF you see the blue house, you've arrived."

## Nested Conditionals

A nested conditional is an if statement inside another if statement.

### Basic Syntax

```python
if condition1:
    if condition2:
        # Runs if both condition1 AND condition2 are True
    else:
        # Runs if condition1 is True but condition2 is False
else:
    # Runs if condition1 is False
```

## Why Use Nested Conditionals?

When you need to check a second condition **only if** the first one is True.

### Example 1: Age and License Check

```python
age = 20
has_license = True

if age >= 18:
    if has_license:
        print("You can drive")
    else:
        print("You need a license")
else:
    print("Too young to drive")
```

### Example 2: Login System

```python
username = "admin"
password = "secret"

if username == "admin":
    if password == "secret":
        print("Login successful")
    else:
        print("Wrong password")
else:
    print("User not found")
```

## Multiple Levels of Nesting

```python
score = 85
submitted = True
on_time = True

if score >= 60:
    if submitted:
        if on_time:
            print("Full credit")
        else:
            print("Late penalty")
    else:
        print("Missing assignment")
else:
    print("Failed")
```

## Nested vs Logical Operators

These are equivalent:

```python
# Using nested if
if age >= 18:
    if has_license:
        print("Can drive")

# Using and operator
if age >= 18 and has_license:
    print("Can drive")
```

**When to use nested**: When you need different actions at each level

**When to use and**: When you just need to check multiple conditions

## Real-World Application: Discount Calculator

```python
total = 150
is_member = True
has_coupon = True

if total >= 100:
    discount = 10  # Base discount
    if is_member:
        discount += 5  # Member bonus
        if has_coupon:
            discount += 10  # Coupon bonus
    print(f"Total discount: {discount}%")
else:
    print("No discount (minimum $100)")
```

## Common Mistakes

### Too Much Nesting

```python
# Avoid - too deep
if a:
    if b:
        if c:
            if d:
                if e:  # Hard to read!
                    print("Something")

# Better - use logical operators
if a and b and c and d and e:
    print("Something")
```

### Forgetting Indentation

```python
# Wrong
if age >= 18:
if has_license:  # IndentationError
    print("Can drive")

# Correct
if age >= 18:
    if has_license:
        print("Can drive")
```

## Key Takeaways

1. **Nested if**: if inside another if
2. **Sequential checking**: Checks inner condition only if outer is True
3. **Use sparingly**: Too much nesting is hard to read
4. **Alternative**: Consider using logical operators (and, or)
5. **Indentation**: Each level needs proper indentation


---

# Accept User Input

## Introduction

User input transforms programs from static scripts to **interactive applications**. The `input()` function is Python's way of creating a dialogue between your program and its users.

---

<div align="center">

![Python input() Function Diagram](https://s13n-curr-images-bucket.s3.ap-south-1.amazonaws.com/python-lectures/9.1/LO-9.1.9.jpg)

*input() reads user data as a string → your program processes it → print() displays the result*

</div>

---

### The Evolution of Human-Computer Interaction

**1940s-1950s**: Batch processing
- Feed punch cards into computer
- Wait hours for results
- No interaction during execution

**1960s-1970s**: Command-line interfaces
- Type commands, get immediate responses
- Programs could ask questions and wait for answers
- Revolutionary for productivity!

**1980s-Present**: GUIs, touchscreens, voice
- Windows, buttons, touch interfaces
- But command-line input remains fundamental
- Web forms, chat apps - all variations of input/output

The `input()` function continues this tradition of **interactive computing**.

### Understanding I/O (Input/Output)

Programs have three components:
1. **Input**: Data coming in (user input, files, network)
2. **Processing**: Computation and logic
3. **Output**: Results going out (screen, files, network)

[User types "Bob"] → Input → [Program processes] → Output → [Displays "Hello Bob!"]

Think of your program as a **function** in the mathematical sense:
- Input: User provides data
- Function: Your code processes it
- Output: Program returns results

### Real-World Analogy

The `input()` function is like a **conversation**:
- **Program asks**: "What's your name?"
- **User responds**: "Alice"
- **Program continues**: "Hello, Alice!"

Or like a **restaurant order**:
- **Waiter asks**: "What would you like to drink?"
- **You say**: "Water"
- **Waiter processes**: Brings water

Without `input()`, programs are like reading a book - one-way communication. With `input()`, they're like having a conversation - two-way interaction.

---

## The input() Function

### Basic Syntax
```python
variable = input("Prompt message: ")
```

### Example
```python
name = input("Enter your name: ")
print("Hello, " + name + "!")
```

**When run:**
Enter your name: Alice
Hello, Alice!

## Input Always Returns String

```python
age = input("Enter age: ")
print(type(age))  # <class 'str'>
```

Even if user types a number, it's stored as string!

### Convert for Math
```python
age_str = input("Enter age: ")
age = int(age_str)
next_year = age + 1
print("Next year you'll be", next_year)
```

## Multiple Inputs
```python
first = input("First name: ")
last = input("Last name: ")
age = int(input("Age: "))  # Convert immediately

print(f"{first} {last}, age {age}")
```

## Practical Examples

### Calculator
```python
num1 = float(input("First number: "))
num2 = float(input("Second number: "))
result = num1 + num2
print("Sum:", result)
```

### Profile Creator
```python
name = input("Name: ")
city = input("City: ")
age = int(input("Age: "))

print(f"{name} from {city}, age {age}")
```

## Why input() Always Returns Strings

This design choice might seem inconvenient, but it's intentional:

**Reason 1: Safety**
- User could type anything: numbers, text, symbols
- Treating everything as text first prevents crashes
- You explicitly convert to the type you expect

**Reason 2: Flexibility**
- Some inputs are ambiguous: "007" could be string "007" or int 7
- You decide the meaning through conversion

**Reason 3: Validation**
- You can check the string before converting
- Prevents crashes from invalid input

**Design principle**: "Accept anything, validate carefully, then convert"

## The Power of Interactive Programs

Before `input()`, your programs could only work with hardcoded values:
```python
name = "Alice"  # Fixed - same every time
```

With `input()`, programs become tools that adapt to each user:
```python
name = input("Your name: ")  # Different every time!
```

This is what makes applications useful: They work with *your* data, not just the programmer's sample data.

---

## Key Takeaways
1. **input() enables interaction** - programs become conversations
2. **Always returns string** - by design, for safety and flexibility
3. **Convert explicitly** - use int(), float(), etc. for numeric operations
4. **Use clear prompts** - help users understand what to enter
5. **I/O is fundamental** - Input → Processing → Output is the core program structure
6. **Inline conversion is common**: `int(input("Age: "))` combines input and conversion
7. **Interactive = useful** - Real applications respond to user needs


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

