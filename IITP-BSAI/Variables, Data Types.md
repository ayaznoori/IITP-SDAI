# Lecture Script: Variables, Data Types, Operators & Conditionals
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? and Setting Up One Compiler | 15 min |
| 2 | Variables — let, const, and Primitive Data Types | 25 min |
| 3 | Operators — Arithmetic, Comparison, Logical, Assignment | 30 min |
| 4 | Conditionals — if / else / else if | 20 min |
| 5 | Tracing Variable Values | 10 min |
| 6 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter? and Setting Up One Compiler

> 🎯 **Instructor Note:** This is likely an early session in a JavaScript fundamentals track. Treat the setup portion as genuinely important — do not rush it, and confirm every learner has a working environment before moving on. Nothing in the rest of the session works if setup is broken. Wait after the opening question, and wait again after setup before moving to Block 2.

**[Script:]**

"Every program you will ever write — a website that responds to a click, an app that calculates a total, a game that tracks a score — comes down to the same small set of building blocks. Something needs to be remembered. That is a variable. Something needs to be compared, calculated, or combined. That is an operator. And based on those comparisons, the program needs to make a decision and do different things depending on the situation. That is a conditional.

These three ideas — remembering, calculating, and deciding — are not just JavaScript concepts. They are the foundation of programming in any language. Once you understand them clearly here, you will recognize the same patterns everywhere you go next.

Today we start writing and running real JavaScript code, using an online tool called One Compiler, so nothing needs to be installed on your machine before we begin. By the end of today you will be able to declare variables, work with the core data types JavaScript gives you, use the main categories of operators, write programs that make decisions with if and else, and — just as importantly — read through a piece of code and correctly predict what value a variable holds at any given point, which is a skill you will rely on constantly when something in your own code is not behaving the way you expect."

---

### 1A — Setting Up and Running Code in One Compiler

**[Script:]**

"One Compiler is a website that lets you write and run code directly in your browser, with no installation required. Let us set this up together, right now, step by step."

> 🎯 **Instructor Note:** Walk through this live, on screen, and have every learner follow along on their own machine simultaneously. Do not proceed until everyone confirms their setup works.

**Demo 1 — Setting up and running a first program (whiteboard-friendly)**

```
Step 1: Open a web browser and navigate to onecompiler.com

Step 2: Select JavaScript as the language from the language menu

Step 3: You will see two main areas:
  - The editor pane, where you type your code
  - The output/console pane, where results appear after running

Step 4: Type the following into the editor:

console.log("Hello, world!");

Step 5: Click the Run button (or press the equivalent shortcut)

Step 6: Confirm the output pane displays: Hello, world!
```

**[Script:]**

"`console.log(...)` is how you print a value so you can see it — it writes whatever is inside the parentheses to the output pane. You will use this constantly throughout today, and throughout the rest of this course, to check what your code is actually doing at any given point. If your output pane shows 'Hello, world!' exactly as written, your setup is working correctly."

> 🎯 **Instructor Note:** Ask the room directly: "Everyone, please run this now and confirm you see the output. Raise a hand if your output pane does not show 'Hello, world!'." Do not proceed until every learner has a confirmed working setup — troubleshooting individual setup issues now saves much larger problems later in the session.

**Recap of Block 1 before moving on:**

- One Compiler lets you write and run JavaScript directly in a browser, with no installation needed
- The editor pane is where code is written; the output pane shows what happens when it runs
- `console.log(...)` prints a value to the output pane and will be used constantly to check what code is actually doing
- Variables, operators, and conditionals — remembering, calculating, and deciding — are the foundation this entire session builds

---

## Block 2 — Variables: let, const, and Primitive Data Types

### 2A — What a Variable Is

**[Script:]**

"A variable is a named container that holds a value, which your program can refer to by that name later. Instead of writing the number 25 directly every time you need it, you can store it in a variable called `age`, and use `age` anywhere you would have used that number — and if the value ever needs to change, you only need to update it in one place."

---

### 2B — Declaring Variables with let and const

**[Script:]**

"JavaScript gives you two main ways to declare a variable: `let` and `const`. The difference between them is whether the value can be reassigned after it is first set."

> 🎯 **Instructor Note:** Write this distinction on the board and keep it visible for the rest of the block.

```
let    → declares a variable whose value CAN change later
const  → declares a variable whose value CANNOT change after 
         it is first set (a "constant")
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I declare `const score = 10;` and then later try to write `score = 20;`, what do you expect to happen when the code runs?" Answer: an error — `const` does not allow reassignment after the initial value is set. Let learners guess before confirming with the demo.

**Demo 2 — let vs const (whiteboard-friendly)**

```javascript
let score = 10;
score = 20;              // this works — let allows reassignment
console.log(score);      // 20

const maxPlayers = 4;
maxPlayers = 6;           // this causes an error — const does not allow this
```

**[Script:]**

"`score` was declared with `let`, so reassigning it to 20 works without any problem — the output confirms `score` is now 20. `maxPlayers` was declared with `const`, so attempting to reassign it produces an error, and the program stops running at that line.

A practical rule of thumb: use `const` by default, since it prevents you from accidentally changing a value you did not mean to change. Only use `let` when you genuinely expect the variable's value to change later in your program — a running total, a counter, a score that updates as the game progresses."

> 🎯 **Instructor Note:** Ask: "Why might defaulting to `const` and only switching to `let` when needed be a good habit, rather than just using `let` for everything to be safe?" Answer: `const` acts as a signal to yourself and anyone else reading the code that this value is not meant to change — if you later see it being reassigned somewhere, that immediately stands out as either a mistake or a sign the variable was declared with the wrong keyword.

---

### 2C — Primitive Data Types

**[Script:]**

"Every value in JavaScript has a data type, which determines what kind of value it is and what you can do with it. There are several core primitive — meaning basic, not made up of other types — data types you will use constantly."

> 🎯 **Instructor Note:** Write this table on the board and keep it visible for the rest of the session.

```
Type        Example                  Description
--------    ---------------------    --------------------------------
Number      25, 3.14, -7             Any numeric value, whole or decimal
String      "hello", 'world'         Text, wrapped in quotes
Boolean     true, false              Exactly one of two values
Undefined   (a declared, unset var)  A variable that has no value yet
Null        null                     An intentional "no value"
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I declare `let x;` without assigning it any value, and then print it with `console.log(x)`, what do you expect to see?" Answer: `undefined` — a variable that has been declared but never assigned a value automatically holds this special value.

**Demo 3 — Checking data types with typeof (whiteboard-friendly)**

```javascript
let age = 25;
let name = "Alice";
let isStudent = true;
let nickname;

console.log(typeof age);        // "number"
console.log(typeof name);       // "string"
console.log(typeof isStudent);  // "boolean"
console.log(typeof nickname);   // "undefined"
```

**[Script:]**

"`typeof` is a keyword that tells you the data type of whatever follows it. This is a genuinely useful tool while learning — whenever you are unsure what type a value actually is, `typeof` gives you a direct answer instead of guessing.

Notice `nickname` was declared with `let` but never given a value — its type comes back as `'undefined'`, confirming exactly what we predicted. This is different from deliberately setting a variable to `null`, which means 'I am intentionally saying there is no value here,' as opposed to `undefined`, which usually means a value was simply never assigned."

> 🎯 **Instructor Note:** Ask: "What is the practical difference between a variable being `undefined` versus a variable deliberately set to `null`?" Answer: `undefined` typically happens by omission — you forgot to assign a value, or it has not been assigned yet. `null` is a deliberate choice by the programmer to represent the intentional absence of a value. Both represent "no value" in some sense, but one is usually accidental and one is usually intentional.

**Recap of Block 2 before moving on:**

- A variable is a named container for a value that your program can refer to later
- `let` allows a variable's value to be reassigned; `const` does not — default to `const` unless reassignment is genuinely needed
- The core primitive types are Number, String, Boolean, Undefined, and Null
- `typeof` reports a value's data type, which is useful both for learning and for debugging real code

---

## Block 3 — Operators: Arithmetic, Comparison, Logical, Assignment

### 3A — Arithmetic Operators

**[Script:]**

"Arithmetic operators perform mathematical calculations on numbers — this is likely the most intuitive category, since it closely matches math you already know."

> 🎯 **Instructor Note:** Write this table on the board.

```
Operator   Meaning            Example        Result
--------   ----------------   -----------    ------
+          Addition           5 + 3          8
-          Subtraction        5 - 3          2
*          Multiplication     5 * 3          15
/          Division           6 / 3          2
%          Remainder (modulo) 7 % 3          1
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask specifically about the modulo operator, since it is usually the least familiar. Ask: "What do you think `10 % 3` evaluates to?" Answer: 1 — modulo gives the remainder after division; 10 divided by 3 is 3 with a remainder of 1.

**Demo 4 — Arithmetic operators (whiteboard-friendly)**

```javascript
let total = 10 + 5;      // 15
let difference = 10 - 5; // 5
let product = 10 * 5;    // 50
let quotient = 10 / 5;   // 2
let remainder = 10 % 3;  // 1

console.log(total, difference, product, quotient, remainder);
```

**[Script:]**

"The modulo operator, `%`, is worth pausing on since it is new to most learners. It does not calculate a percentage — it calculates what is left over after division. This turns out to be extremely useful for things like checking whether a number is even or odd — `number % 2` is 0 for even numbers and 1 for odd numbers — a pattern you will use often."

---

### 3B — Comparison Operators

**[Script:]**

"Comparison operators compare two values and produce a Boolean result — either `true` or `false`. This is the foundation of every decision your program will ever make, which is exactly what Block 4 builds on directly."

> 🎯 **Instructor Note:** Write this table on the board. The `==` versus `===` distinction deserves particular emphasis — it is the single most common early JavaScript mistake.

```
Operator   Meaning                        Example        Result
--------   ---------------------------    -----------    ------
===        Equal to (strict)              5 === 5        true
!==        Not equal to (strict)          5 !== 3        true
>          Greater than                   5 > 3          true
<          Less than                      5 < 3          false
>=         Greater than or equal to       5 >= 5         true
<=         Less than or equal to          5 <= 3         false
```

**[Script:]**

"You may also encounter `==` and `!=`, without the extra equals sign — these are the 'loose' equality operators, and they will attempt to convert values of different types before comparing them, which can produce surprising results. `'5' == 5` evaluates to `true`, even though one side is text and the other is a number, because loose equality converts them to match before comparing.

The strong recommendation, and the convention we will follow throughout this course, is to always use `===` and `!==` — the strict versions — which compare both value and type together, with no automatic conversion. This avoids an entire category of confusing bugs."

> 🎯 **Instructor Note:** This is worth stating as a firm rule, not just a suggestion. Say directly: "Use `===` and `!==` by default, always. Only reach for `==` or `!=` if you have a specific, deliberate reason to want type conversion during comparison — and as a beginner, that reason essentially never comes up."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What does `'5' === 5` evaluate to — true or false? What about `'5' == 5`?" Answer: `'5' === 5` is `false`, because one side is a string and the other is a number, and strict equality requires matching types. `'5' == 5` is `true`, because loose equality converts the string to a number before comparing. This concretely demonstrates why strict equality is the safer default.

**Demo 5 — Strict vs loose equality (whiteboard-friendly)**

```javascript
console.log('5' === 5);    // false — different types
console.log('5' == 5);     // true  — loose equality converts types
console.log(5 === 5);      // true  — same value, same type
```

---

### 3C — Logical Operators

**[Script:]**

"Logical operators combine or invert Boolean values, letting you express more complex conditions than a single comparison alone."

> 🎯 **Instructor Note:** Write this table on the board.

```
Operator   Meaning                            Example
--------   --------------------------------   ---------------------------
&&         AND — true only if both sides       (age >= 18 && hasID)
           are true                             both must be true

||         OR — true if at least one side       (isMember || isGuest)
           is true                              at least one must be true

!          NOT — inverts a Boolean value        !isLoggedIn
                                                 true becomes false
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If `age` is 20 and `hasTicket` is `false`, what does `age >= 18 && hasTicket` evaluate to?" Answer: `false` — even though the age condition is true, `&&` requires both sides to be true, and `hasTicket` is false.

**Demo 6 — Logical operators (whiteboard-friendly)**

```javascript
let age = 20;
let hasTicket = false;

console.log(age >= 18 && hasTicket);   // false — hasTicket is false
console.log(age >= 18 || hasTicket);   // true — age condition alone is enough
console.log(!hasTicket);               // true — inverts false to true
```

**[Script:]**

"`&&` requires both sides to be true for the whole expression to be true — as soon as one side is false, the whole thing is false, regardless of the other side. `||` only needs one side to be true. `!` simply flips a Boolean — `true` becomes `false` and `false` becomes `true`.

These become essential the moment your decisions depend on more than one condition at once — which is nearly always, once your programs go beyond the simplest examples."

---

### 3D — Assignment Operators

**[Script:]**

"Assignment operators assign a value to a variable, and several shorthand versions combine assignment with an arithmetic operation in one step."

> 🎯 **Instructor Note:** Write this table on the board.

```
Operator   Meaning                     Equivalent to
--------   ------------------------    -------------------
=          Assign a value              x = 5
+=         Add and reassign            x = x + 5
-=         Subtract and reassign       x = x - 5
*=         Multiply and reassign       x = x * 5
/=         Divide and reassign         x = x / 5
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If `score` starts at 10, what does `score` equal after running `score += 5;`?" Answer: 15 — `+=` is shorthand for taking the current value, adding 5, and reassigning the result back to `score`.

**Demo 7 — Assignment operators (whiteboard-friendly)**

```javascript
let score = 10;
score += 5;    // same as: score = score + 5
console.log(score);   // 15

score -= 3;    // same as: score = score - 3
console.log(score);   // 12
```

**[Script:]**

"These shorthand operators are extremely common in real code, especially inside loops — accumulating a total, incrementing a counter — so it is worth becoming comfortable reading `+=` and immediately recognizing it as 'take the current value, add this amount, and store the result back.'"

**Recap of Block 3 before moving on:**

- Arithmetic operators perform calculations; `%` returns the remainder after division, useful for checks like even versus odd
- Comparison operators produce a Boolean result; always prefer `===` and `!==` over `==` and `!=` to avoid unexpected type conversion
- Logical operators combine conditions: `&&` requires both sides true, `||` requires at least one side true, `!` inverts a Boolean
- Assignment operators like `+=` and `-=` combine reassignment with an arithmetic operation in a single step

---

## Block 4 — Conditionals: if / else / else if

### 4A — Making Decisions with if

**[Script:]**

"Everything up to this point — variables holding values, operators comparing and combining them — has been leading here. A conditional lets your program actually make a decision: run one block of code if a condition is true, and potentially something different if it is not.

The `if` statement is the most basic form: a condition in parentheses, followed by a block of code in curly braces that only runs if that condition evaluates to `true`."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If `temperature` is 15, what do you expect this code to print, if anything at all?" Answer: nothing — the condition `temperature > 30` is false, so the code inside the `if` block never runs.

**Demo 8 — A basic if statement (whiteboard-friendly)**

```javascript
let temperature = 15;

if (temperature > 30) {
    console.log("It's hot outside!");
}

console.log("Program continues here regardless.");
```

**[Script:]**

"Since `temperature` is 15, the condition `temperature > 30` evaluates to `false`, so the line inside the curly braces never runs — nothing is printed from inside that block. The final `console.log` outside the block still runs regardless, since it is not inside the `if` statement's braces at all."

---

### 4B — Adding an Alternative with else

**[Script:]**

"`else` provides a block of code that runs specifically when the `if` condition is false — giving your program exactly two paths, one or the other, never both."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Using the same `temperature = 15`, what do you expect this version to print, now that there is an `else` block?" Answer: "It's cool outside." — since the condition is false, execution moves to the `else` block instead of skipping entirely.

**Demo 9 — if / else (whiteboard-friendly)**

```javascript
let temperature = 15;

if (temperature > 30) {
    console.log("It's hot outside!");
} else {
    console.log("It's cool outside.");
}
```

**[Script:]**

"Now, instead of nothing printing, the `else` block runs since the `if` condition was false. Exactly one of these two blocks runs — never both, and never neither."

---

### 4C — Multiple Conditions with else if

**[Script:]**

"Real decisions often involve more than two possible outcomes. `else if` lets you chain additional conditions, checked in order, top to bottom, with the first true condition's block being the one that runs — every condition after that is skipped entirely, even if it would also have been true."

> 🎯 **Instructor Note:** This "checked in order, first match wins" behavior is a common source of confusion. Emphasize it directly and demonstrate it concretely in the demo below.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "With `score = 85`, walk through this code condition by condition. Which block actually runs?" Have the room trace it verbally, condition by condition, before revealing the answer: the `score >= 80` block runs, printing "Grade: B" — even though `score >= 70` and `score >= 60` are also technically true, they are never reached because the first true condition already claimed execution.

**Demo 10 — if / else if / else (whiteboard-friendly)**

```javascript
let score = 85;

if (score >= 90) {
    console.log("Grade: A");
} else if (score >= 80) {
    console.log("Grade: B");
} else if (score >= 70) {
    console.log("Grade: C");
} else {
    console.log("Grade: F");
}
```

**[Script:]**

"With `score` equal to 85: the first condition, `score >= 90`, is false, so we move to the next. `score >= 80` is true — 85 is indeed greater than or equal to 80 — so 'Grade: B' prints, and execution skips every remaining `else if` and the final `else`, even though `score >= 70` would also have evaluated to true if it had been checked. Order matters — conditions are evaluated top to bottom, and the first one that is true wins."

> 🎯 **Instructor Note:** Ask: "What would happen if the conditions in this chain were written in the opposite order — checking `score >= 70` first, then `score >= 80`, then `score >= 90`?" Answer: with `score = 85`, the very first condition, `score >= 70`, would already be true, so "Grade: C" would incorrectly print instead of "Grade: B" — the chain would never even reach the more specific, higher conditions. This demonstrates concretely why condition order matters when ranges overlap.

**Recap of Block 4 before moving on:**

- `if` runs a block of code only when its condition evaluates to `true`
- `else` provides an alternative block that runs specifically when the `if` condition is `false`
- `else if` chains additional conditions, checked in order from top to bottom; the first true condition's block runs, and every condition after it is skipped
- When conditions could overlap, their order in the chain determines the outcome — write more specific conditions before more general ones

---

## Block 5 — Tracing Variable Values

### 5A — Why Tracing Matters

**[Script:]**

"Tracing means reading through code line by line, without running it, and writing down what each variable's value actually is at each point — this is one of the most valuable debugging skills you can build, because it lets you find exactly where your understanding of the code diverges from what it is actually doing, without needing to run anything at all."

---

### 5B — Tracing a Program Step by Step

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before revealing the trace table, put this code on the board and ask the room to trace it themselves first — line by line, on paper or out loud — before you reveal the answer together. This active attempt matters more than watching the answer appear.

**Demo 11 — Tracing a multi-step program (whiteboard-friendly)**

```javascript
let balance = 100;
let deposit = 50;

balance += deposit;

let isEligible = balance >= 100 && deposit > 0;

if (isEligible) {
    balance -= 20;
} else {
    balance -= 5;
}

console.log(balance);
```

**[Script:]**

"Let us trace this line by line, tracking every variable's value as it changes."

> 🎯 **Instructor Note:** Build this trace table on the board live, one row at a time, confirming each value with the room before writing it down.

```
Line                              balance    deposit    isEligible
let balance = 100;                100        —          —
let deposit = 50;                 100        50         —
balance += deposit;               150        50         —
let isEligible = balance >= 100   150        50         true
  && deposit > 0;
if (isEligible) { ... }           → condition is true, enters if block
  balance -= 20;                  130        50         true

Final: console.log(balance) prints 130
```

**[Script:]**

"Walking through this: `balance` starts at 100, `deposit` starts at 50. After `balance += deposit`, `balance` becomes 150. `isEligible` checks two things combined with `&&`: is `balance >= 100`? Yes, 150 is. Is `deposit > 0`? Yes, 50 is. Both sides are true, so `isEligible` is `true`. Since `isEligible` is true, the `if` block runs, not the `else` block — `balance -= 20` executes, bringing `balance` down to 130. That final value, 130, is what actually prints."

> 🎯 **Instructor Note:** Ask a variation to check transfer of the skill: "If `deposit` had started at 0 instead of 50, what would the final printed value of `balance` be? Trace it yourself before answering." Answer: with `deposit = 0`, after `balance += deposit`, `balance` stays at 100. `isEligible` checks `balance >= 100` (true) `&&` `deposit > 0` (false, since 0 is not greater than 0) — combined with `&&`, this makes `isEligible` false. The `else` block runs instead: `balance -= 5`, giving a final value of 95.

**Recap of Block 5 before moving on:**

- Tracing means reading code line by line and recording each variable's actual value at each point, without running it
- A trace table tracks every relevant variable across each line of execution, updated only when that line actually changes it
- Conditionals branch the trace — only one path's effects actually apply, based on what the condition evaluated to at that point
- Tracing is a core debugging skill: it lets you find exactly where your expectation of the code's behavior diverges from what it is actually doing

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What is the difference between let and const? Why should === be preferred over ==? What does && require compared to ||? What determines which block runs in an if / else if / else chain when multiple conditions are true? What is tracing, and why is it useful even without running code?"

**Setting Up and Running JavaScript**

- One Compiler runs JavaScript directly in the browser with no installation needed
- `console.log(...)` prints a value to the output pane and is used constantly to check what code is doing

**Variables — let, const, and Primitive Data Types**

- `let` allows reassignment; `const` does not — default to `const` unless a value genuinely needs to change
- Core primitive types: Number, String, Boolean, Undefined, and Null
- `typeof` reports a value's data type, useful for both learning and debugging

**Operators**

- Arithmetic operators calculate; `%` returns the remainder, useful for checks like even versus odd
- Comparison operators produce a Boolean; prefer `===` and `!==` over `==` and `!=` to avoid unexpected type conversion
- Logical operators combine conditions: `&&` needs both sides true, `||` needs at least one, `!` inverts a Boolean
- Assignment shorthand like `+=` combines reassignment with an arithmetic operation in one step

**Conditionals — if / else / else if**

- `if` runs a block only when its condition is true; `else` provides the alternative when it is false
- `else if` chains additional conditions checked top to bottom; the first true condition wins, and the rest are skipped
- Condition order matters when ranges overlap — write more specific conditions before more general ones

**Tracing Variable Values**

- Tracing means reading code line by line and recording each variable's actual value without running it
- A trace table tracks variables across lines, updated only when a line changes them
- Conditionals branch a trace — only the path actually taken affects the final values

**Why All of This Matters Together**

- Variables give a program memory, operators let it calculate and compare, and conditionals let it decide — together these three concepts are the smallest complete toolkit for writing a program that does something genuinely useful rather than just running the same fixed steps every time; and the ability to trace through code by hand, confirming exactly what a program does at each step, is the skill that will let you find and fix problems in every program you write from here forward, in this course and beyond it

---

*End of script.*