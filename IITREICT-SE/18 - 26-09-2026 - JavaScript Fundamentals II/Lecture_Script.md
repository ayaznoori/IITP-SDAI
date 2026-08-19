# Lecture Script: JavaScript Fundamentals II
**Duration:** 110 minutes | **Tools:** Browser, VS Code | **Project:** Portfolio app.js

---

## Opening (8 min)

**[Script:]** "Session 17 you logged data and branched with if/else. Real apps bundle behavior into **functions** — one place to fix GST calculation, one place to validate email. **DRY** is not a buzzword; it is how **Amazon** avoids fixing the same tax bug on 50 pages."

---

## Real-World Hooks

- **PhonePe** amount formatter function reused in send money + history
- **Zerodha** Kite — scope prevents accidental global variable collisions in large scripts
- Refactoring copy-paste → function = junior dev daily task

---

## LO Walkthroughs

### LO 1–2: Functions and return values

```javascript
function formatINR(amount) {
  return "₹" + amount.toLocaleString("en-IN");
}

const formatINRArrow = (amount) => `₹${amount}`;
```

**Predict:** `formatINR(1500)` output?

---

### LO 3: Scope

```javascript
const TAX = 0.18;
function lineTotal(price) {
  const tax = price * TAX;
  return price + tax;
}
```

**Interview scenario:** Why not make everything global?

---

### LO 4–5: Arrays/objects + refactor

**Before:** Three handlers duplicate email regex check.  
**After:**

```javascript
function isValidEmail(email) {
  return email.includes("@") && email.includes(".");
}
```

Cart total function over array of `{ price }` objects.

---

## Live Lab

Refactor portfolio script: extract `highlightNav()`, `validateContactForm()`, `calculateQuote()` (dummy GST). Each under 15 lines.

---

## Lecture Summary

- Functions package reusable behavior
- Arrow syntax for concise logic
- Scope prevents accidental coupling
- Refactoring reduces bug surface
- **Practical value:** Function design is how scripts become maintainable apps
