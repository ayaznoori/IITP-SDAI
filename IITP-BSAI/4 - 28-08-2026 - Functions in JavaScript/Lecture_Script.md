# Lecture Script: Functions in JavaScript
**Duration:** 110 minutes | **Tool:** One Compiler | **Language:** JavaScript

---

## Session Opening (5 min)

**[Script:]** "You can store data and repeat steps. Now we package steps into named, reusable tools. Functions are how professional code stays readable at 10,000 lines."

**Problem hook:** Three places in your app calculate shipping the same way. Bug in the formula? Fix it three times — or once inside a function.

---

## Why Does This Matter?

🎯 **Instructor Note:** "React components are functions. API route handlers are functions. Tests call functions. This session is not a detour — it is the shape of everything ahead."

**[Script:]** "Without functions, code becomes copy-paste soup. With functions, you build a toolbox. Each tool does one job well."

- **Real-world use:** Validation, formatting, pricing, auth checks
- **Pain if misunderstood:** Missing `return`, scope leaks, giant functions that do everything

---

## What Is the Concept?

### Function Declaration

```javascript
function functionName(param1, param2) {
  // body
  return result;
}
```

**Mental model:** Vending machine — input slots, internal process, output slot.

### Parameters vs Arguments

- Definition: `function add(a, b)` — `a`, `b` are parameters
- Call: `add(2, 3)` — `2`, `3` are arguments

### Return

**Definition:** `return` passes a value back and ends the function immediately.

**Common mistake:** Forgetting `return` — function yields `undefined`.

### Scope

- **Local:** variables inside `{ }` of a function
- **Global:** declared outside any function

🎯 **Instructor Note:** Demo shadowing — local `let x` inside function vs global `x`. Beginner confusion point.

### Arrow Functions

```javascript
const multiply = (a, b) => a * b;
```

Use for short, expression-like logic. Same rules for parameters and return.

---

## How Do We Apply It?

### LO 1: Define and call functions with a clear purpose

**Problem:** Format a price with a rupee symbol.

**Write code:**
```javascript
function formatPrice(amount) {
  return "₹" + amount;
}
console.log(formatPrice(499));
```

**Predict:** `"₹499"`

---

### LO 2: Pass parameters and use return values in calling code

**Problem:** Compute total with tax.

**Write code:**
```javascript
function withTax(price, rate) {
  return price + price * rate;
}
const total = withTax(1000, 0.18);
console.log(total);
```

**Predict:** `1180`

🎯 **Instructor Note:** "Where does 1180 get stored?" — introduce using return in an expression.

---

### LO 3: Explain local vs global scope at a beginner level

**Problem:** Why does this fail?

**Write code:**
```javascript
const app = "Shop";
function showApp() {
  const app = "Cart";
  console.log(app);
}
showApp();
console.log(app);
```

**Predict:** First log? Second log?

**Explain result:** Inside function, local `app` is `"Cart"`. Outside, global `app` is still `"Shop"`.

---

### LO 4: Write simple arrow functions for short logic

**Problem:** Check if a score is passing (≥ 40).

**Write code:**
```javascript
const isPass = (score) => score >= 40;
console.log(isPass(55), isPass(30));
```

**Predict:** `true false`

---

### LO 5: Refactor repeated code into reusable functions

**Problem:** Same area calculation copied twice.

**Before (on board):** `const a = 5 * 3;` and later `const b = 5 * 3;`

**After:**
```javascript
function area(w, h) {
  return w * h;
}
console.log(area(5, 3), area(5, 3));
```

**Guided refactor (10 min):** Give students 8 lines with duplicated discount logic — extract `applyTenPercentOff(price)`.

---

## Live Demo Block (15 min)

Mini "grade helper" module:
- `getGrade(marks)` with if/else if
- `formatReport(name, marks)` calling `getGrade`
- One arrow function `isHonors(marks) => marks >= 90`

Show how composition makes `main` readable.

---

## Recap (10 min)

🎯 **Instructor Note:** "What happens if you omit return?" "When is arrow syntax a good fit?"

---

## Lecture Summary

- **Functions** package reusable behavior with a clear name and purpose
- **Parameters and return** move data in and results out
- **Local vs global scope** controls visibility — prefer local when possible
- **Arrow functions** offer concise syntax for small operations
- **Refactoring** into functions removes duplication and bugs
- **Practical value:** Functions are the unit of work in JS, React, Node, and every library you will use

**[Script:]** "Module 1 JavaScript foundations — done. Next we open the browser and build pages. You now have variables, loops, data structures, and functions. That is a real programming toolkit."
