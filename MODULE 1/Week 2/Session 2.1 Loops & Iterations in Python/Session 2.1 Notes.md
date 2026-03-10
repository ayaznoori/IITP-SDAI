## Title

Loops & Iterations in Python

## Objective

Automating repetitive tasks with loops; Understanding when to use For vs While; Implementing loop control statements

## Topics & Subtopics Covered

- **Loops**
  - Using `for` loops and `while` loops
  - Controlling execution with `break` and `continue`
  - Automating repetitive sequences
- **Problem Solving**
  - Iterative problem solving
  - Breaking problems into small testable parts

---

# 1. Why Do We Need Loops?

So far, our programs have executed each line once.
But in real problems, we often need to **repeat** an action:

- Print numbers from 1 to 100  
- Check each item (for example, each value in a sequence)  
- Ask user input multiple times  

Without loops, we would repeat code manually:

```python
print(1)
print(2)
print(3)
```

This is:
- Hard to maintain  
- Error–prone  
- Not scalable  

➡️ Loops allow us to **automate repetitive sequences** with fewer lines.

---

# 2. `for` Loop — Iterate Over a Sequence

The `for` loop is used when:

- We know **how many times** we want to repeat  
- We are iterating over a **sequence** (range, string, etc.)

### Basic Syntax

```python
for variable in sequence:
    # repeated block
```

### Example: Print 1 to 5

```python
for num in range(1, 6):
    print(num)
```

Explanation:
- `range(1, 6)` gives 1, 2, 3, 4, 5  
- `num` takes each value one by one  
- `print(num)` runs 5 times  

### Example: Loop Through a String

```python
name = "Python"

for ch in name:
    print(ch)
```

Each character of `"Python"` is printed on a new line.

---

# 3. `while` Loop — Repeat While Condition Is True

The `while` loop is used when:

- We **do not know** the exact number of repetitions  
- We want to repeat until a **condition becomes False**  

### Basic Syntax

```python
while condition:
    # repeated block
```

### Example: Print 1 to 5 With `while`

```python
num = 1

while num <= 5:
    print(num)
    num = num + 1
```

Key ideas:
- We must **update** the variable (`num = num + 1`)  
- If we forget to update, loop may become **infinite**  

---

# 4. Choosing Between `for` and `while`

**Use `for` when:**
- You know the count (e.g., 10 times, range)
- You are iterating over a sequence such as a string or a range

**Use `while` when:**
- You loop until a **condition changes** (e.g., user chooses to exit)
- You don’t know the exact number of iterations

Example — Menu Until User Exits (good for `while`):

```python
choice = ""

while choice != "q":
    choice = input("Enter option (q to quit): ")
```

---

# 5. Loop Control: `break` and `continue`

Sometimes we need to **control** the loop from inside.

## `break` — Stop the Loop Early

Used to **exit** the loop when a condition is met.

```python
for num in range(1, 11):
    if num == 5:
        break
    print(num)
```

Output:

```text
1
2
3
4
```

Loop stops when `num == 5`.

---

## `continue` — Skip Current Iteration

Used to **skip** the rest of the block for the current value and continue with next.

```python
for num in range(1, 6):
    if num == 3:
        continue
    print(num)
```

Output:

```text
1
2
4
5
```

The number 3 is **skipped**.

---

# 6. Automating Repetitive Sequences

Loops help us automate tasks like:

- Printing tables  
- Repeating calculations  
- Processing each item in a sequence  

### Example: Multiplication Table

```python
n = int(input("Enter number: "))

for i in range(1, 11):
    print(n, "x", i, "=", n * i)
```

### Example: Sum of First N Numbers

```python
n = int(input("Enter N: "))
total = 0

for i in range(1, n + 1):
    total = total + i

print("Sum:", total)
```

---

# 7. Iterative Problem Solving

**Iterative problem solving** means solving a problem step-by-step, improving or updating values in each loop.

General pattern:

1. Initialize a variable (counter / accumulator)  
2. Loop over a range or sequence  
3. Update the variable on each iteration  
4. Use the final result after the loop  

### Example: Count Even Numbers Between 1 and 20

```python
count_even = 0

for num in range(1, 21):
    if num % 2 == 0:
        count_even = count_even + 1

print("Even numbers:", count_even)
```

---

# 8. Breaking Problems Into Small Testable Parts

When solving a loop-based problem:

1. **Write the goal clearly**  
2. **Decide the loop type** (`for` vs `while`)  
3. **Write a small version** and test  
4. Add conditions (`if`, `break`, `continue`) if needed  

### Example Problem

> Take numbers from user until they enter 0.  
> Then print the total sum.

Step-by-step approach:

1. Initialize `total = 0`  
2. Use `while True:` for continuous input  
3. Break when input is 0  
4. Add each number to total  

Code:

```python
total = 0

while True:
    num = int(input("Enter number (0 to stop): "))
    if num == 0:
        break
    total = total + num

print("Total sum:", total)
```

---

# 9. Common Beginner Mistakes

- Forgetting to update the loop variable in `while` → **infinite loop**  
- Using `range(1, n)` but expecting `n` to be included (need `range(1, n + 1)`)  
- Misusing `break` inside nested loops and expecting all loops to stop  

---

# 10. Quick Practice Ideas

1. Print all odd numbers from 1 to 50.  
2. Count how many numbers between 1 and 100 are divisible by 3.  
3. Ask user for 5 marks and print the average using a loop.  

These will help you apply:
- `for` loops  
- Conditions inside loops  
- Iterative thinking  

