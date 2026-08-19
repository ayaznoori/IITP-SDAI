# Lecture Script: Loops & Iterations in JavaScript
**Duration:** 110 minutes | **Tool:** One Compiler | **Language:** JavaScript

---

## Session Opening (5 min)

**[Script:]** "Last session you made one decision at a time. Today we teach the computer to repeat work for us. Loops are how software handles thousands of items without thousands of copy-paste lines."

**Problem hook:** Print the first 50 multiples of 7. Without a loop? Painful. With a loop? Ten seconds.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "What in Instagram or YouTube repeats for every post or video?" (scroll feed, load more, count views)

**[Script:]** "Every list you scroll, every report that totals sales, every game enemy that spawns — loops. Get loops wrong and you freeze the browser with an infinite loop or skip half your data."

- **Real-world use:** Pagination, batch jobs, animations, aggregations
- **Pain if misunderstood:** Infinite loops, off-by-one errors (`<` vs `<=`), wrong counter updates

---

## What Is the Concept?

### `for` Loop

**Definition:** A control structure that repeats a block a specific number of times using init → condition → update.

**Mental model:** A treadmill with a set number of laps.

```javascript
for (let i = 0; i < 3; i++) {
  console.log("Lap", i);
}
```

### `while` Loop

**Definition:** Repeats while a condition is true; update must happen inside the body.

**Mental model:** "Keep asking until the user types quit."

**Common mistake:** Forgetting `count++` inside `while` → infinite loop. Demo this briefly, then fix.

### `break` and `continue`

- `break` — stop the entire loop
- `continue` — skip to next iteration

🎯 **Instructor Note:** "Search until found, then break" — search analogy lands well.

### `for` vs `while`

| Use `for` | Use `while` |
|-----------|-------------|
| Known count | Unknown count |
| Index-based | Condition-driven |

---

## How Do We Apply It?

### LO 1: Write `for` loops to repeat actions a set number of times

**Problem:** Print squares of 1 through 5.

**Translate logic:** Loop `i` from 1 to 5; print `i * i`.

**Write code:**
```javascript
for (let i = 1; i <= 5; i++) {
  console.log(i * i);
}
```

**Predict before running:** First three outputs?

**Explain result:** 1, 4, 9, 16, 25 — condition checked before each body run.

---

### LO 2: Write `while` loops that repeat until a condition changes

**Problem:** Double a number until it exceeds 100.

**Translate logic:** Start at 5; while `n <= 100`, print and double.

**Write code:**
```javascript
let n = 5;
while (n <= 100) {
  console.log(n);
  n = n * 2;
}
```

**Predict before running:** Last number printed?

**Explain result:** 5, 10, 20, 40, 80 — next would be 160, so loop stops.

🎯 **Instructor Note:** Pause — "Where must `n = n * 2` live? Inside the body."

---

### LO 3: Control loop execution using `break` and `continue`

**Problem:** Find first number divisible by 7 between 1 and 50.

**Translate logic:** Loop 1–50; if divisible by 7, print and break.

**Write code:**
```javascript
for (let i = 1; i <= 50; i++) {
  if (i % 7 !== 0) continue;
  console.log(i);
  break;
}
```

**Predict before running:** What prints?

**Explain result:** `7` — `continue` skips non-multiples; `break` exits after first hit.

---

### LO 4: Choose the correct loop type for a given repetitive problem

**Problem:** User keeps entering numbers until they enter 0. (Simulate with an array.)

**Translate logic:** Unknown iterations → `while`; known list length → `for`.

**Write code (simulated input):**
```javascript
const inputs = [4, 8, 0];
let idx = 0;
while (inputs[idx] !== 0) {
  console.log(inputs[idx]);
  idx++;
}
```

**Predict:** Prints 4, then 8, then stops before 0.

🎯 **Instructor Note:** Poll class — "for or while for printing 1 to 10?" Quick show of hands.

---

### LO 5: Solve beginner loop exercises in One Compiler

**Problem:** Sum numbers 1 to 10.

**Translate logic:** `sum = 0`; loop and add.

**Write code:**
```javascript
let sum = 0;
for (let i = 1; i <= 10; i++) {
  sum += i;
}
console.log(sum);
```

**Predict:** `55`

**Guided exercise (10 min):** Students solve: count how many numbers from 1–20 are divisible by 3.

---

## Live Demo Block (15 min)

**FizzBuzz lite (1–15):** Print number; if divisible by 3 print "Fizz" instead. Uses `for`, `%`, `if`, optional `continue`.

---

## Recap (10 min)

Cold-call: "What causes an infinite `while` loop?" "When do you pick `break` over `continue`?"

---

## Lecture Summary

- **`for` loops** handle counted repetition with init, condition, and update
- **`while` loops** repeat until a condition becomes false — watch your updates
- **`break`** exits early; **`continue`** skips to the next round
- **Sum and count patterns** appear in almost every real program
- **Choosing the right loop** makes code clearer and safer
- **Practical value:** Loops are how software scales from one record to millions

**[Script:]** "Next up: arrays, strings, and objects — data you will loop over every day. Today you learned repetition; next you learn what to repeat on."
