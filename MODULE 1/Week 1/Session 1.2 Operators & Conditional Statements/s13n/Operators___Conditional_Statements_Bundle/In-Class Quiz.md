# Apply Logical Operators

## What's the output?
```python
print(True and False)
```
A) True
B) False
C) Error
D) None

<details><summary>Answer</summary>
B - False (both must be True for `and` to return True)
</details>

## What's the output?
```python
print(True or False)
```
A) True
B) False
C) Error
D) None

<details><summary>Answer</summary>
A - True (at least one is True, so `or` returns True)
</details>

## What does `not True` return?
A) True
B) False
C) None
D) Error

<details><summary>Answer</summary>
B - False (`not` reverses the boolean value)
</details>

## Operator precedence?
```python
print(True or False and False)
```
A) True
B) False
C) Error
D) None

<details><summary>Answer</summary>
A - True (evaluates as `True or (False and False)` = `True or False` = `True`)
</details>

## What's the output?
```python
age = 20
has_id = True
can_enter = age >= 18 and has_id
print(can_enter)
```
A) True
B) False
C) Error
D) 20

<details><summary>Answer</summary>
A - True (both conditions are True: 20 >= 18 is True, has_id is True)
</details>


---

# Write If Statements

## What's missing?
```python
if age >= 18
    print("Adult")
```
A) Colon after condition
B) Equals sign
C) Parentheses
D) Semicolon after condition

<details><summary>Answer</summary>
A - Colon after condition (should be `if age >= 18:`)
</details>

## What's wrong?
```python
if age >= 18:
print("Adult")
```
A) Missing colon
B) Missing indentation
C) Wrong operator
D) Missing parentheses around condition

<details><summary>Answer</summary>
B - Missing indentation (print statement must be indented)
</details>

## What prints?
```python
x = 10
if x > 15:
    print("A")
print("B")
```
A) A B
B) B
C) A
D) Nothing prints

<details><summary>Answer</summary>
B - Only "B" prints (x is not > 15, so "A" doesn't print. "B" always prints)
</details>

## How many lines run?
```python
if True:
    print("Line 1")
    print("Line 2")
```
A) 0
B) 1
C) 2
D) Error

<details><summary>Answer</summary>
C - Both lines run (both are indented under the if, and condition is True)
</details>

## What's the correct syntax?
A) `if x = 5:`
B) `if x == 5:`
C) `if x == 5`
D) `if (x == 5)`

<details><summary>Answer</summary>
B - `if x == 5:` (use `==` for comparison and include colon)
</details>


---

# Write Elif Statements

## What runs?
```python
x = 15
if x > 20:
    print("A")
elif x > 10:
    print("B")
elif x > 5:
    print("C")
```
A) A
B) B
C) B and C
D) C

<details><summary>Answer</summary>
B - Only "B" prints (first True condition, then stops)
</details>

## What's wrong?
```python
elif x > 10:
    print("Big")
```
A) Missing if statement before elif
B) Missing colon
C) Wrong indentation
D) elif cannot be used with print

<details><summary>Answer</summary>
A - elif must come after an if statement
</details>

## What prints?
```python
score = 95
if score >= 70:
    print("Pass")
elif score >= 90:
    print("Excellent")
```
A) Pass
B) Excellent
C) Pass and Excellent
D) No output

<details><summary>Answer</summary>
A - "Pass" (first condition is True, so elif never runs)
</details>

## Best practice for order?
A) General conditions first, specific last
B) Specific conditions first, general last
C) Order doesn't matter
D) Alphabetical order of variable names

<details><summary>Answer</summary>
B - Specific conditions first, general last
</details>

## How many elif statements can you have?
A) Only one
B) Maximum two
C) As many as needed
D) Maximum five

<details><summary>Answer</summary>
C - You can chain as many elif statements as needed
</details>


---

# Write Else Statements

## What's wrong?
```python
if x > 10:
    print("Big")
else x <= 10:
    print("Small")
```
A) else can't have a condition
B) Missing colon
C) Wrong indentation
D) else must use parentheses around the condition

<details><summary>Answer</summary>
A - else should not have a condition (remove `x <= 10`)
</details>

## When does else run?
```python
if x > 10:
    print("A")
elif x > 5:
    print("B")
else:
    print("C")
```
A) When x > 10
B) When x > 5
C) When x <= 5
D) When x equals 5

<details><summary>Answer</summary>
C - else runs when ALL previous conditions are False (x <= 5)
</details>

## What prints?
```python
score = 75
if score >= 60:
    print("Pass")
else:
    print("Fail")
```
A) Pass
B) Fail
C) Both
D) No output

<details><summary>Answer</summary>
A - Pass (75 >= 60 is True)
</details>

## Can you have multiple else blocks?
```python
if x > 10:
    print("A")
else:
    print("B")
else:
    print("C")
```
A) Yes
B) No
C) Only with elif
D) Yes, but only if each else has a different condition

<details><summary>Answer</summary>
B - No, only ONE else per if-elif chain (this is a SyntaxError)
</details>

## Where must else go?
A) Before if
B) Before elif
C) After all if/elif
D) Anywhere inside the if-elif chain

<details><summary>Answer</summary>
C - else must come AFTER all if/elif statements
</details>


---

# Writing Nested Conditionals for Complex Logic

Test your understanding of nested if statements and complex decision-making logic.

---

## What is the output of this code?
```python
x = 10
y = 5

if x > 5:
    if y > 3:
        print("A")
    else:
        print("B")
else:
    print("C")
```

A) A
B) B
C) C
D) No output

<details><summary>Answer</summary>
**A) A**

**Explanation:** x > 5 is True (10 > 5), so we enter the outer if block. Then y > 3 is also True (5 > 3), so we print "A".
</details>

---

## What is the key purpose of indentation in nested conditionals?

A) To make code look pretty
B) To define which code belongs to which condition
C) To improve code performance
D) To satisfy Python syntax requirements only

<details><summary>Answer</summary>
**B) To define which code belongs to which condition**

**Explanation:** Indentation in Python determines code structure. It shows which statements are inside which blocks. While it is a syntax requirement (D), the PURPOSE is to define structure (B).
</details>

---

## What is the output?
```python
age = 15
has_permission = False

if age >= 18:
    print("Adult")
else:
    if has_permission:
        print("Allowed with permission")
    else:
        print("Not allowed")
```

A) Adult
B) Allowed with permission
C) Not allowed
D) Error

<details><summary>Answer</summary>
**C) Not allowed**

**Explanation:** age >= 18 is False (15 < 18), so we go to the outer else block. Inside, has_permission is False, so we go to the inner else and print "Not allowed".
</details>

---

## Which code structure is generally MORE readable?
```python
# Option A
if a:
    if b:
        if c:
            print("Success")

# Option B
if a and b and c:
    print("Success")
```

A) Option A is more readable
B) Option B is more readable
C) Both are equally readable
D) Neither is readable

<details><summary>Answer</summary>
**B) Option B is more readable**

**Explanation:** When you simply need all conditions to be True with a single action, using `and` is clearer and more concise than deep nesting. Nested ifs are better when you need different actions at each level.
</details>

---

## What happens if the outer condition is False in a nested if?

A) Python checks the inner condition anyway
B) Python skips the entire outer block including inner conditions
C) Python raises an error
D) Python executes the outer else, then checks inner condition

<details><summary>Answer</summary>
**B) Python skips the entire outer block including inner conditions**

**Explanation:** If the outer condition is False, Python NEVER evaluates the inner conditions. It skips the entire outer if block and goes to the outer else (if present).
</details>

---

## What is the output?
```python
score = 75

if score >= 0 and score <= 100:
    if score >= 60:
        print("Pass")
    else:
        print("Fail")
else:
    print("Invalid")
```

A) Pass
B) Fail
C) Invalid
D) No output

<details><summary>Answer</summary>
**A) Pass**

**Explanation:** score is 75, which is between 0 and 100 (outer condition True). Then score >= 60 is True (75 >= 60), so we print "Pass".
</details>

---

## How many possible execution paths are there in this code?
```python
if condition_A:
    if condition_B:
        print("Path 1")
    else:
        print("Path 2")
else:
    print("Path 3")
```

A) 2
B) 3
C) 4
D) 5

<details><summary>Answer</summary>
**B) 3**

**Explanation:**
- Path 1: condition_A True AND condition_B True → "Path 1"
- Path 2: condition_A True AND condition_B False → "Path 2"
- Path 3: condition_A False → "Path 3"

Total: 3 paths
</details>

---

## What is the output?
```python
temperature = 95
humidity = 70

if temperature > 90:
    if humidity > 60:
        print("Dangerous")
    else:
        print("Hot")
else:
    print("Normal")
```

A) Dangerous
B) Hot
C) Normal
D) Error

<details><summary>Answer</summary>
**A) Dangerous**

**Explanation:** temperature > 90 is True (95 > 90), and humidity > 60 is also True (70 > 60), so we print "Dangerous".
</details>

---

## Which is TRUE about nested conditionals?

A) The inner condition is always checked first
B) You can only nest 2 levels deep
C) The outer condition must be True for inner condition to be checked
D) else is required for every if in nested structures

<details><summary>Answer</summary>
**C) The outer condition must be True for inner condition to be checked**

**Explanation:** The outer condition guards the inner condition. If outer is False, inner is never evaluated. Options A, B, and D are false.
</details>

---

## What is the output?
```python
username = "admin"
password = "wrong"

if username == "admin":
    if password == "secret":
        print("Login successful")
    else:
        print("Wrong password")
else:
    print("Unknown user")
```

A) Login successful
B) Wrong password
C) Unknown user
D) No output

<details><summary>Answer</summary>
**B) Wrong password**

**Explanation:** username == "admin" is True, so we enter outer if. Then password == "secret" is False ("wrong" != "secret"), so we go to the inner else and print "Wrong password".
</details>

---

## What does this code validate?
```python
if age >= 0:
    if age < 18:
        category = "Minor"
    else:
        category = "Adult"
else:
    category = "Invalid"
```

A) Age is a positive number
B) Age is between 0 and 18
C) Age is a valid non-negative number before classification
D) Age is exactly 18

<details><summary>Answer</summary>
**C) Age is a valid non-negative number before classification**

**Explanation:** The outer if checks age >= 0 (validation). Only if valid does it proceed to classify as Minor or Adult. This is a validation-first pattern.
</details>

---

## What is the maximum recommended nesting depth?

A) 1 level
B) 2 levels
C) 3-4 levels
D) Unlimited

<details><summary>Answer</summary>
**C) 3-4 levels**

**Explanation:** While technically you can nest infinitely, best practice is to keep it to 3-4 levels maximum for readability. Beyond that, consider refactoring using `and`/`or` operators or functions.
</details>

---

## What is the output?
```python
x = 5
y = 10

if x > 0:
    if y > 0:
        result = x + y
    else:
        result = x - y
else:
    result = 0

print(result)
```

A) 0
B) 5
C) 10
D) 15

<details><summary>Answer</summary>
**D) 15**

**Explanation:** x > 0 is True (5 > 0), and y > 0 is also True (10 > 0), so result = x + y = 5 + 10 = 15.
</details>

---

## When should you use nested if instead of `and` operator?

A) Never - always use `and`
B) When you need different actions at each condition level
C) When you want slower code
D) Only when you have more than 5 conditions

<details><summary>Answer</summary>
**B) When you need different actions at each condition level**

**Explanation:** Nested ifs are useful when you need to provide specific feedback or take different actions based on which condition failed. For example, different error messages for missing username vs wrong password.
</details>

---

## What is the output?
```python
score = 50
bonus = True

if score >= 60:
    print("Pass")
else:
    if bonus:
        print("Pass with bonus")
    else:
        print("Fail")
```

A) Pass
B) Pass with bonus
C) Fail
D) No output

<details><summary>Answer</summary>
**B) Pass with bonus**

**Explanation:** score >= 60 is False (50 < 60), so we go to outer else. Inside, bonus is True, so we print "Pass with bonus".
</details>

---




---

# Accept User Input

## What does input() return?
A) int
B) str
C) Depends on input

<details><summary>Answer</summary>
B - Always returns string
</details>

## Fix this:
```python
age = input("Age: ")
result = age + 5
```
A) int(age) + 5
B) age + str(5)
C) float(age) + str(5)
D) int(age) + int("5")

<details><summary>Answer</summary>
A - Convert string to int
</details>

## Get number from user?
A) num = input("Number: ")
B) num = int(input("Number: "))
C) num = str(input("Number: "))
D) num = get("Number: ")

<details><summary>Answer</summary>
B - Convert to int immediately
</details>

## What's wrong?
```python
x = input()
```
A) Missing prompt
B) Nothing wrong
C) Missing quotes

<details><summary>Answer</summary>
B - Prompt is optional (but recommended)
</details>

## Multiple inputs?
A) Can only use input() once
B) Can call input() multiple times
C) Need special function

<details><summary>Answer</summary>
B - Call input() for each value needed
</details>


---

# Format Output with F-strings

## Valid f-string?
A) f"Hello {name}"
B) "Hello {name}"f
C) "f Hello {name}"
D) f{"Hello name"}

<details><summary>Answer</summary>
A - f goes before quotes
</details>

## What prints?
```python
x = 10
print(f"{x * 2}")
```
A) x * 2
B) 20
C) {20}
D) {x * 2}

<details><summary>Answer</summary>
B - Expressions are evaluated
</details>

## Format to 2 decimals?
```python
pi = 3.14159
```
A) f"{pi:.2f}"
B) f"{pi:2f}"
C) f"{pi.2f}"
D) f"{round(pi)}"

<details><summary>Answer</summary>
A - :.2f format specifier
</details>

## What's wrong?
```python
name = "Bob"
print("Hello {name}")
```
A) Missing f
B) Wrong brackets
C) Nothing
D) name should be in parentheses instead of braces

<details><summary>Answer</summary>
A - Need f before string
</details>

## Can you do math in {}?
A) Yes
B) No
C) Only addition
D) Only with integers

<details><summary>Answer</summary>
A - Any expression works
</details>


---

