# Pre-Read — Loops & Iterations in Python

Before the live session, this pre-read will help you:

- Understand **why** we need loops  
- Get a basic idea of `for` and `while`  
- Start thinking about problems that require **repetition**  

You do **not** need to memorize syntax yet — just understand the ideas.

---

## 1. Where Do We See Repetition?

In daily life:

- Counting 1 to 100  
- Calling 10 students one by one  
- Sending reminder messages to many people  

In programming, we also repeat actions:

- Check every element of a list  
- Process each line from a file  
- Print a pattern  

Without loops, we would repeat the same code many times.

---

## 2. Idea of a Loop

A **loop** means:

> “Do this action again and again until we decide to stop.”

Conceptually:

```text
Start
↓
Do something
↓
Check condition
↓
Repeat or Stop
```

Python mainly gives us:

- `for` loop  
- `while` loop  

You will learn when to use each.

---

## 3. `for` Loop — Repeat For Each Item

Think of:

> “For each student in the class, call their name.”

In Python idea:

```python
for item in collection:
    # use item
```

You can loop over:

- A range of numbers  
- A list of marks  
- A string of characters  

In class, we will convert these ideas into working code.

---

## 4. `while` Loop — Repeat While Condition Is True

Think of:

> “While battery is above 20%, keep playing video.”

In Python idea:

```python
while condition_is_true:
    # repeat this block
```

This is useful when:

- We **don’t know** how many times we will repeat  
- We just know when to **stop**  

Example idea (not full code yet):

- Keep asking password **while** it is wrong  
- Keep asking user for numbers **until** they type 0  

---

## 5. Controlling the Loop: `break` and `continue`

Sometimes we want more control inside the loop:

- **Stop early** when something happens → `break`  
- **Skip** just one repetition → `continue`  

You will see:

- `break` = “I’m done, exit the loop now.”  
- `continue` = “Skip this one, go to the next.”  

---

## 6. Iterative Problem Solving Mindset

Loops are not just syntax — they change how you think.

When solving a problem:

1. Identify what is **repeating**  
2. Decide what changes on each step (counter, index, total, etc.)  
3. Decide when to **stop**  

Example problem to think about:

> “Add numbers from 1 to 10.”

Questions to reflect:

- What is repeating?  
- What value is changing each time?  
- How do we know we are finished?  

We will turn this thought process into Python code in the session.

---

## 7. What You Should Be Comfortable With Before Class

Just recognize these words:

- Loop  
- Iteration  
- `for`  
- `while`  
- Condition  
- `break` / `continue`  

You don’t need to know exact syntax — that’s what we will practice together.

