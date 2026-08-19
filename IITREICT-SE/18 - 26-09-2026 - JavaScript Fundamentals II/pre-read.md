# Pre-Read: JavaScript Fundamentals II

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to write **functions** three ways — declaration, expression, and arrow
- How **parameters and return values** make code reusable (like Python `def`)
- How **scope** works — global vs block — and why it prevents bugs
- How to **manipulate arrays and objects** for structured browser data
- How to **refactor repeated logic** into clean functions
- Why functions are the building block for DOM scripts, APIs, and React components ahead

---

## 2. Detailed Explanation

### Why Functions Matter

You wrote loops and conditionals in one long script. When the same logic appears twice — calculating tax, formatting a name, filtering a list — you copy-paste. Then you fix a bug in one copy and forget the other.

A **function** (a reusable block of code with a name, inputs, and optional output) solves this. You learned `def` in Python. JavaScript has the same idea with different syntax.

**Analogy:** A function is a vending machine. You input coins (arguments), it outputs a snack (return value). You don't rebuild the machine every time you're hungry.

> **In the Real World:** **Stripe's** payment SDK exposes functions like `confirmPayment()`. **Google Maps** API uses functions you call with parameters. Every JavaScript library is a collection of functions you compose.

### Function Declarations

```javascript
function greet(name) {
  return "Hello, " + name + "!";
}

console.log(greet("Anita"));  // "Hello, Anita!"
```

**Hoisting:** Declarations can be called before they appear in the file (useful but can confuse beginners — prefer clear order).

### Function Expressions

```javascript
const greet = function(name) {
  return "Hello, " + name + "!";
};
```

Stored in a variable. Not hoisted like declarations.

### Arrow Functions

```javascript
const greet = (name) => {
  return "Hello, " + name + "!";
};

// Shorthand when body is one expression:
const double = (n) => n * 2;
```

**When to use:** Short callbacks, array methods (`map`, `filter` — next session). For named top-level logic, declarations are fine too.

### Parameters and Return Values

```javascript
function calculateBill(price, quantity, taxRate = 0.18) {
  const subtotal = price * quantity;
  const tax = subtotal * taxRate;
  return subtotal + tax;
}

console.log(calculateBill(100, 2));        // 236
console.log(calculateBill(100, 2, 0.05));    // 210
```

| Concept | Meaning |
|---------|---------|
| **Parameter** | Variable in function definition |
| **Argument** | Actual value passed when calling |
| **Default parameter** | Fallback if argument omitted |
| **return** | Sends value back to caller |

**Python comparison:**

```python
def calculate_bill(price, quantity, tax_rate=0.18):
    subtotal = price * quantity
    return subtotal + (subtotal * tax_rate)
```

Same logic, different syntax.

### Scope — Global vs Block

```javascript
const appName = "Course Portal";  // global — entire file

function showApp() {
  const greeting = "Welcome";   // local to function
  console.log(appName, greeting);
}

// console.log(greeting);  // Error — not in scope
```

**Block scope with let/const:**

```javascript
if (true) {
  let temp = 42;
}
// console.log(temp);  // Error
```

**var** ignores block scope — another reason to avoid it.

### Working with Arrays

```javascript
const scores = [78, 92, 85, 64];

function average(list) {
  let sum = 0;
  for (const score of list) {
    sum += score;
  }
  return sum / list.length;
}

console.log(average(scores));  // 79.75
```

**Useful methods (preview for Modern JS session):**
- `push`, `pop` — add/remove end
- `length` — count items
- `includes` — check membership

### Working with Objects

```javascript
const student = {
  name: "Kiran",
  scores: [80, 90, 88],
  active: true,
};

function getTopScore(person) {
  let max = person.scores[0];
  for (const s of person.scores) {
    if (s > max) max = s;
  }
  return max;
}

console.log(getTopScore(student));  // 90
```

**Dot notation:** `student.name`
**Bracket notation:** `student["name"]` — needed for dynamic keys

### Refactoring Repeated Logic

**Before:**

```javascript
const a = 10, b = 20;
console.log((a + b) / 2);
const c = 4, d = 8;
console.log((c + d) / 2);
```

**After:**

```javascript
function averageTwo(x, y) {
  return (x + y) / 2;
}
console.log(averageTwo(10, 20));
console.log(averageTwo(4, 8));
```

### Why It Matters

Interactive pages need organized logic — validate email, compute cart total, filter search results. Functions keep scripts readable and testable. React components are functions. FastAPI route handlers are functions. The pattern is universal.

**Benefits:**
- DRY — Don't Repeat Yourself
- Easier debugging — isolate one function
- Interview staple — "write a function that..."

### Messy to Clear

**Messy:**
- 200-line script with no functions
- Functions that do five unrelated things
- Global variables mutated everywhere
- Missing `return` — gets `undefined` silently

**Clean:**
- One function, one job
- Parameters instead of globals
- Explicit `return`
- Descriptive names: `calculateBill` not `fn1`

### Building Blocks Checklist

- [ ] I can write a function declaration with parameters and return
- [ ] I can write a simple arrow function
- [ ] I understand block scope with let/const
- [ ] I can pass arrays and objects into functions
- [ ] I can refactor duplicated code into one function

---

## 3. Practice Exercises

**Exercise 1 — Declaration**
Write `function celsiusToFahrenheit(c)` returning `(c * 9/5) + 32`. Test with 0, 25, and 100. Log results.

**Exercise 2 — Arrow function**
Rewrite as arrow function `const isEven = (n) => ...`. Return true/false. Test with 4 and 7.

**Exercise 3 — Scope**
Write a function `createCounter()` that declares `let count = 0` inside, returns a function that increments and returns count. Call twice — observe closure preview (mentor will explain in class).

**Exercise 4 — Array function**
Write `findMax(numbers)` that loops an array and returns highest value. Test with `[3, 9, 1, 27, 5]`.

**Exercise 5 — Refactor**
Take a script with repeated "format currency" logic (₹ symbol + two decimals). Extract `formatINR(amount)` function. Use it three times with different amounts.
