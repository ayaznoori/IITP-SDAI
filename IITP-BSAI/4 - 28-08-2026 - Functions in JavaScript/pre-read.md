# Pre-Read: Functions in JavaScript

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · Loops]</i><br/>for/while · patterns · repetition"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · Data]</i><br/>Arrays · strings · objects · map"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Functions in JavaScript<br/><i>Mental shift:</i> from <b>inline steps</b> to <b>reusable units</b><br/>Parameters · return · scope · arrow functions · refactor"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Prepares for React components, utilities, and clean modules<br/>Ends Module 1 JS foundations before web stack"]
        RL["<b>Real-Life Use</b><br/>Shared validators · Pricing helpers · Auth checks · Testable business logic"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS · DOM]</i><br/>Structure pages · style · events"]
        U2["<b>Upcoming Module</b><br/>Module 5: React<br/><i>[Components · Props]</i><br/>Functions as UI building blocks"]
        U3["<b>Upcoming Module</b><br/>Module 6: Backend FastAPI<br/><i>[Python · APIs]</i><br/>Route handlers as functions"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| CM
    CM ==>|&nbsp;Builds on&nbsp;| CS
    CS ==>|&nbsp;Course Path&nbsp;| CV
    CS ==>|&nbsp;Real-Life Use&nbsp;| RL
    CS ==>|&nbsp;Next Module&nbsp;| U1
    U1 -.-> U2
    U2 -.-> U3

    classDef previous fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px,color:#1a1a1a
    classDef current fill:#FFF3CD,stroke:#E6A817,stroke-width:3px,color:#1a1a1a
    classDef value fill:#D4EDDA,stroke:#28A745,stroke-width:2px,color:#1a1a1a
    classDef future fill:#F3E8FF,stroke:#9B59B6,stroke-width:2px,color:#1a1a1a

    class P1,CM previous
    class CS current
    class CV,RL value
    class U1,U2,U3 future

    linkStyle default stroke-width:3px
```

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **define and call functions** that do one clear job
- How to **pass data in** with parameters and **get results back** with `return`
- How **local and global scope** affect which variables you can use
- How to write **arrow functions** for short, readable logic
- How to **refactor repeated code** into reusable functions

---

## 2. Detailed Explanation

### What Is a Function?

A **function** (a reusable block of code with a name) is like a kitchen appliance. You give it ingredients (inputs), it does one job, and sometimes gives you a result (output).

Without functions, you copy-paste the same logic everywhere. That is hard to fix and easy to break.

### Defining and Calling

```javascript
function greet(name) {
  return "Hello, " + name;
}

console.log(greet("Asha"));
```

- **Define** once with `function name(params) { ... }`
- **Call** whenever needed: `greet("Asha")`

### Parameters and Return Values

- **Parameters** — placeholders in the function definition
- **Arguments** — actual values you pass when calling
- **`return`** — sends a value back to the caller; also stops the function

```javascript
function add(a, b) {
  return a + b;
}
console.log(add(3, 5)); // 8
```

### Scope: Who Can See What?

- **Local scope** — variables created inside a function exist only there
- **Global scope** — variables declared outside functions are visible everywhere (use carefully)

```javascript
const appName = "LearnJS"; // global

function demo() {
  const x = 10; // local to demo
  console.log(appName); // OK
}
// console.log(x); // Error — x is not visible here
```

### Arrow Functions

Short syntax for small functions:

```javascript
const double = (n) => n * 2;
console.log(double(4)); // 8
```

Use arrow functions when the logic is brief and clear.

### Why It Matters

Functions are how teams build large apps — small tested pieces composed together.

**Benefits:**
- Write once, use many times
- Easier debugging — fix one place
- Cleaner, more readable programs

### Refactoring

**Refactoring** means improving code structure without changing what it does. If you see the same 4 lines twice, make a function.

---

## 3. Practice Exercises

**Exercise 1 — Basic function**
Write `square(n)` that returns `n * n`. Test with `square(6)`.

**Exercise 2 — Arrow function**
Write `isAdult(age)` as an arrow function that returns `true` if age ≥ 18.

**Exercise 3 — Refactor**
You have the same discount calculation twice: `price * 0.9`. Move it into `applyDiscount(price)` and call it twice.
