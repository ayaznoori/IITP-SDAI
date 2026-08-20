# Pre-Read: Loops & Iterations in JavaScript

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · Variables]</i><br/>let/const · types · operators · if/else"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · Decisions]</i><br/>Conditionals · expressions · tracing values"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Loops & Iterations in JavaScript<br/><i>Mental shift:</i> from <b>one decision</b> to <b>repeated action</b><br/>for · while · break · continue · sum/count patterns"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Unlocks arrays, list rendering, and data processing<br/>Core pattern before React lists and API loops"]
        RL["<b>Real-Life Use</b><br/>Process orders · Paginate feeds · Retry until success · Batch reports"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 1: Programming Foundations<br/><i>[JavaScript · Data Structures]</i><br/>Arrays · strings · objects · functions"]
        U2["<b>Upcoming Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTML · CSS · DOM]</i><br/>Pages · layout · interactivity"]
        U3["<b>Upcoming Module</b><br/>Module 5: React<br/><i>[Components · State]</i><br/>UI lists · re-render loops"]
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

- How to **repeat actions automatically** with `for` and `while` loops
- When to **choose `for` vs `while`** for a given problem
- How to **stop or skip** loop steps using `break` and `continue`
- How to **sum and count** using loop patterns you will reuse everywhere
- How to **solve short loop exercises** confidently in One Compiler

---

## 2. Detailed Explanation

### Why Loops Exist

Imagine printing numbers 1 to 100 without a loop. You would write 100 lines. A **loop** (code that repeats a block until a condition is met) does that work for you.

**Analogy:** A loop is like a dishwasher cycle — same steps repeated until the job is done.

### The `for` Loop — Known Repetitions

Use a **`for` loop** when you know **how many times** to repeat.

```javascript
for (let i = 1; i <= 5; i++) {
  console.log(i);
}
```

- **Initialization** — start value (`let i = 1`)
- **Condition** — keep going while true (`i <= 5`)
- **Update** — change after each round (`i++`)

### The `while` Loop — Unknown Repetitions

Use a **`while` loop** when you repeat **until a condition changes**.

```javascript
let count = 1;
while (count <= 5) {
  console.log(count);
  count++;
}
```

**Warning:** If the condition never becomes false, you get an **infinite loop** (runs forever).

### `break` and `continue`

- **`break`** — exit the loop immediately (like leaving a game early)
- **`continue`** — skip the rest of this round and go to the next one

```javascript
for (let i = 1; i <= 5; i++) {
  if (i === 3) continue;
  console.log(i);
}
// Prints 1, 2, 4, 5 — skips 3
```

### Common Patterns

**Counting:** Start at 0 or 1, increment each time.

**Summing:** Start `sum = 0`, add each value inside the loop.

```javascript
let sum = 0;
for (let n = 1; n <= 4; n++) {
  sum = sum + n;
}
console.log(sum); // 10
```

### Why It Matters

Loops power feeds, search results, game levels, and data processing. Without loops, programs cannot scale.

**Benefits:**
- Handle 10 or 10,000 items with the same few lines
- Automate boring repetitive tasks
- Build patterns used in arrays and APIs later

### Choosing `for` vs `while`

| Situation | Best choice |
|-----------|-------------|
| Fixed number of times | `for` |
| Repeat until user quits or data ends | `while` |
| Counting 1 to N | `for` |

---

## 3. Practice Exercises

**Exercise 1 — Countdown**
Use a `for` loop to print numbers from 5 down to 1.

**Exercise 2 — Sum**
Use a loop to find the sum of numbers 1 through 10. What is the answer?

**Exercise 3 — Skip evens**
Print only odd numbers from 1 to 10. Hint: use `continue` when a number is even.
