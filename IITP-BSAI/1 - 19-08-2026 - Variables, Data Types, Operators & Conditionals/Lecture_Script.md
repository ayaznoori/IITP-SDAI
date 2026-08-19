# Lecture Script: Variables, Data Types, Operators & Conditionals
**Duration:** 110 minutes | **Tool:** One Compiler | **Language:** JavaScript

---

## Session Opening (5 min)

**[Script:]** "Welcome to your first real coding session. By the end of today, you will write JavaScript that stores data, compares values, and makes decisions. No installs today — we use One Compiler. Let's go."

**Problem hook:** A food delivery app needs to know: Is the user logged in? Is the cart total above ₹500 for free delivery? That is variables + conditionals in action.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask the room — "Name one app you used today. What decision did it make for you?" (suggestions: OTP check, dark mode, sale badge)

**[Script:]** "Every product you touch runs on stored data and if/else logic. Miss a type? Your price shows as `'99' + '1' = '991'` instead of 100. Skip a condition? You ship the wrong discount. Today we learn the grammar every program speaks."

- **Real-world use:** Login gates, pricing rules, form validation, game scores
- **Pain if misunderstood:** Silent bugs from wrong types, broken comparisons with `==` vs `===`, variables that change when they should not

---

## What Is the Concept?

### Variables: `let` vs `const`

**Definition:** A variable is a named reference to a value in memory.

| Keyword | When to use | Mental model |
|---------|-------------|--------------|
| `let` | Value may change | Rewritable label |
| `const` | Value must not be reassigned | Locked label |

🎯 **Instructor Note:** Pause — "Why not use `let` everywhere?" Let someone answer: const prevents accidental overwrites and makes intent clear.

**Common mistake:** Reassigning a `const` — show the error live.

### Primitive Data Types

**[Script:]** "JavaScript has a small set of primitive types. Master these five today."

- `string` — text in quotes
- `number` — integers and decimals
- `boolean` — `true` or `false`
- `null` — intentional empty
- `undefined` — not yet assigned

**Comparison (Python vs JS — mention briefly):** Python uses `True`/`False` capitalized; JS uses lowercase `true`/`false`. Python has no `undefined`; JS does.

### Operators

- **Arithmetic:** `+ - * / %`
- **Comparison:** prefer `===` and `!==` (strict equality)
- **Logical:** `&&` `||` `!`
- **Assignment:** `=` `+=` `-=`

🎯 **Instructor Note:** Whiteboard `5 == '5'` vs `5 === '5'`. Predict before revealing.

### Conditionals: `if` / `else if` / `else`

**Mental model:** A fork in the road. One path runs; the others are skipped.

**Common mistakes:**
- Using `=` instead of `===` inside conditions
- Missing braces when adding a second line inside a block
- Forgetting that only the first true `else if` runs

---

## How Do We Apply It?

### LO 1: Set up and run JavaScript programs in One Compiler

**Problem:** You need a place to write and run code instantly.

**Translate logic:** Open tool → pick JavaScript → write `console.log` → click Run.

**Write code:**
```javascript
console.log("Hello, Masai!");
```

**Predict before running:** What will appear in the output panel?

**Explain result:** `console.log` prints to the output area. This is your feedback loop for the whole course.

---

### LO 2: Declare variables with `let` and `const` and use core primitive data types

**Problem:** Store a user's name (fixed) and score (changes).

**Translate logic:** Name → `const string`. Score → `let number`.

**Write code:**
```javascript
const name = "Ravi";
let score = 0;
score = score + 10;
console.log(name, score);
```

**Predict before running:** What is printed after `score = score + 10`?

**Explain result:** `name` stays `"Ravi"`. `score` becomes `10`. Types matter: `"10" + 5` is `"105"` (string), but `10 + 5` is `15` (number).

🎯 **Instructor Note:** Demo `typeof` on each variable — quick type check habit.

---

### LO 3: Apply arithmetic, comparison, logical, and assignment operators

**Problem:** A cart has 3 items at ₹199 each. Is the total ≥ ₹500 for free delivery?

**Translate logic:** Multiply → compare with `>=` → combine with `&&` if a coupon is also needed.

**Write code:**
```javascript
let items = 3;
let price = 199;
let total = items * price;
let hasCoupon = true;
let freeDelivery = total >= 500 && hasCoupon;
console.log(total, freeDelivery);
```

**Predict before running:** Is `freeDelivery` true or false?

**Explain result:** `597 >= 500` is true, and `hasCoupon` is true, so `freeDelivery` is `true`.

**Short demo (assignment operator):**
```javascript
let points = 50;
points += 25;
console.log(points);
```
**Predict:** `75`

---

### LO 4: Write basic `if` / `else` / `else if` programs for simple decisions

**Problem:** Assign a grade band from marks.

**Translate logic:** If marks ≥ 90 → A; else if ≥ 75 → B; else → C.

**Write code:**
```javascript
let marks = 82;
if (marks >= 90) {
  console.log("A");
} else if (marks >= 75) {
  console.log("B");
} else {
  console.log("C");
}
```

**Predict before running:** Which branch runs?

**Explain result:** `82 >= 75` is true, so `"B"` prints. The `else` is skipped.

🎯 **Instructor Note:** Change `marks` to 90 live — show boundary behavior.

---

### LO 5: Trace variable values for sample inputs

**Problem:** Given code, predict output without running.

**Translate logic:** Table on whiteboard: line | variable values | output.

**Walkthrough (board trace):**
```javascript
let a = 4;
let b = a + 2;
if (b > 5) {
  a = a * 2;
}
console.log(a, b);
```

**Predict before running:** `a = ?`, `b = ?`

**Explain result:** `b` starts as `6`. Condition true, so `a` becomes `8`. Prints `8 6`.

🎯 **Instructor Note:** Pair activity — 3 minutes, partners trace a 6-line snippet you provide.

---

## Live Demo Block (15 min)

Build a mini "ticket checker" in One Compiler:
1. `const` for event name
2. `let` for age
3. Operators for price calculation
4. `if/else if/else` for child / adult / senior pricing

**[Script:]** "Watch me think aloud: what is fixed? what changes? what is the decision?"

---

## Recap (10 min)

🎯 **Instructor Note:** Cold-call three students with prediction questions from today's demos.

---

## Lecture Summary

- **One Compiler** is your run-and-check environment for JavaScript
- **`let` and `const`** store data; choose based on whether the value changes
- **Primitives** — string, number, boolean, null, undefined — are the atoms of every program
- **Operators** let you calculate, compare, and combine conditions; prefer `===`
- **`if / else if / else`** routes your program based on true/false checks
- **Tracing** builds the skill to debug and reason before you run code
- **Practical value:** Every feature you will ever build — web, mobile, AI tools — starts with storing values and making decisions

**[Script:]** "Next session we repeat actions with loops. Today you learned to decide once; soon you'll decide many times automatically. Great work."
