# JavaScript Fundamentals — Detailed Lecture Notes

**Prerequisite:** Python module (variables, types, control flow, lists, dictionaries, functions introduction).  
**Environment:** Code editor + modern browser (Chrome / Edge / Firefox) with Developer Tools.

---

## 1. Introduction to JavaScript

### 1.1 What is JavaScript?

- **High-level, dynamic language** standardized as **ECMAScript**.  
- **Primary role in this module:** scripting in the **browser** to control page behavior.  
- **Not the same as Java** (different language; name is historical marketing).

### 1.2 How Python compares (mental map)


| Idea        | Python (typical)     | JavaScript (browser)                             |
| ----------- | -------------------- | ------------------------------------------------ |
| Run command | `python file.py`     | Load HTML page; browser runs `<script>`          |
| REPL        | `python` interactive | DevTools **Console**                             |
| Types       | Dynamic, strong-ish  | Dynamic; **coercion** appears in some operations |
| Collections | `list`, `dict`       | `Array`, `Object`                                |


### 1.3 Why browsers?

- Browsers provide: **DOM** (document structure), **CSS**, **JS engine**, **network**, **storage APIs**.  
- Today we focus on: **loading scripts**, **printing to console**, **basic syntax**.

---

## 2. Attaching JavaScript to HTML and executing in the browser

### 2.1 Inline script

```html
<script>
  console.log("Inline script ran");
</script>
```

- Runs when the parser reaches the script (unless `defer`/`async` — see below).

### 2.2 External script (preferred for real projects)

```html
<script src="./main.js"></script>
```

- `**src` path** must resolve correctly (relative to the HTML file or server root).  
- Wrong path → **404** in Network tab, script does not run.

### 2.3 Placement

- Traditionally before `</body>` to avoid touching DOM before it exists.  
- Modern option: `<script src="app.js" defer></script>` in `<head>` — downloads early, runs **after** HTML parse.  
- `**async`:** download in parallel; run **as soon as** ready — order not guaranteed; use for independent scripts.

### 2.4 Execution model (simplified)

1. Browser fetches HTML.
2. Parses HTML; when it hits `<script>`, it **runs** (or fetches and runs external file).
3. JS can call browser APIs (e.g. `console`, `document` later in course).

---

## 3. Using `console.log` for debugging

### 3.1 Basic logging

```javascript
console.log("Debug:",42, { ok: true });
```

- **Multiple arguments** are printed space-separated.  
- Objects are **live** in some consoles — expand to inspect (DevTools behavior).

### 3.2 Other useful methods (awareness)

- `console.warn`, `console.error` — different severity / styling.  
- `console.table(arrayOfObjects)` — readable tabular output for homework/debug.

### 3.3 Debugging mindset

1. Reproduce the issue.
2. **Log inputs and outputs** at boundaries (start of function, after a loop).
3. Use DevTools **Sources** / breakpoints later for larger apps.

---

## 4. Comments in JavaScript

```javascript
// Single-line comment

/*
  Multi-line comment
  Often used for doc blocks or temporarily disabling code
*/
```

- **Best practice:** explain **why**, not what the syntax already states.  
- Block comments **do not nest** in standard JS.

---

## 5. Core concepts — variables

### 5.1 `let` — block-scoped, reassignable

```javascript
let score = 0;
score = 10;
```

### 5.2 `const` — block-scoped, binding cannot be reassigned

```javascript
const maxUsers = 100;
// maxUsers = 200; // TypeError
```

**Important:** `const` only fixes the **binding**. The **contents** of objects/arrays can still change:

```javascript
const user = { name: "Ada" };
user.name = "Grace"; // allowed
```

### 5.3 `var` — function-scoped (legacy)

- Hoisting and loop quirks make it error-prone.  
- **Teaching stance:** recognize in old code; **do not default to `var` in new code.**

### 5.4 Naming

- Use **camelCase** for variables and functions in JS conventions.  
- Avoid reserved words (`let`, `class`, `return`, etc.).

---

## 6. Data types (essentials)

**Primitive types (conceptual list for this course):**

- `string`, `number`, `boolean`, `bigint` (mention), `symbol` (mention), `undefined`, `null`

**Reference types:**

- `Object` (including plain objects, arrays, functions)

### 6.1 `typeof`

```javascript
typeof "hi";    // "string"
typeof 42;      // "number"
typeof true;    // "boolean"
typeof {}; // "object"
typeof [];      // "object"  (arrays are objects)
typeof null;    // "object"  (historical quirk — memorize as oddity)
```

### 6.2 `undefined` vs `null`

- `**undefined`:** variable declared but not assigned; missing property; function returns nothing.  
- `**null`:** intentional “no value” by programmer.

---

## 7. Scope — global and block

### 7.1 Global scope

Bindings declared **outside** any function/block (or sloppy `var` at top level) live in global scope — **avoid polluting** global in real apps.

### 7.2 Block scope

```javascript
{
  let x = 1;
  console.log(x); // 1
}
// console.log(x); // ReferenceError
```

- `**let` and `const**` are **block-scoped**.  
- `**var`** ignores block scope (function-scoped only) — classic source of bugs in loops.

### 7.3 Shadowing

Inner block can **declare** a new binding with the same name — hides outer binding within inner block.

---

## 8. Operators

### 8.1 Arithmetic

`+`, `-`, `*`, `/`, `%`, `*`* (exponent)

- `**+` with strings:** concatenation if either side is string (watch types).

### 8.2 Comparison

- `**===` strict equality** (prefer): same value **and** same type.  
- `**==` loose equality:** applies coercion — **avoid** until you fully understand coercion tables.

```javascript
0 === false // false
0 == false   // true (coercion)
```

### 8.3 Logical

- `&&`, `||`, `!`  
- **Short-circuiting:** `a && b` does not evaluate `b` if `a` is falsy.

### 8.4 Truthy and falsy (practical subset)

**Falsy:** `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN`.  
**Truthy:** everything else (including `"0"`, empty arrays/objects).

---

## 9. Conditional statements

```javascript
const age = 20;

if (age >= 18) {
  console.log("Adult");
} else if (age >= 13) {
  console.log("Teen");
} else {
  console.log("Child");
}
```

- **Ternary (optional):** `const label = age >= 18 ? "adult" : "minor";`

### 9.1 `switch` (awareness)

- Useful for many discrete values; **always `break`** (or `return`) unless fall-through is intentional.

---

## 10. Loops

### 10.1 `while`

```javascript
let i = 0;
while (i < 3) {
  console.log(i);
  i++;
}
```

### 10.2 `for`

```javascript
for (let i = 0; i < 3; i++) {
  console.log(i);
}
```

### 10.3 `for...of` (arrays, strings, iterables)

```javascript
const nums = [10, 20, 30];
for (const n of nums) {
  console.log(n);
}
```

**Python parallel:** `for n in nums:`

---

## 11. Data structures — strings

### 11.1 Creation and immutability

- Strings are **immutable** — methods return **new** strings.

### 11.2 Useful properties/methods

```javascript
const s = "hello";
s.length;           // 5
s.toUpperCase();    // "HELLO"
s.includes("ell");  // true
s.slice(1, 4);      // "ell"
```

### 11.3 Template literals

```javascript
const name = "Ada";
const score = 95;
console.log(`${name} scored ${score}`);
```

**Python parallel:** f-strings `f"{name} scored {score}"`

---

## 12. Data structures — arrays

### 12.1 Literals and access

```javascript
const scores = [88, 92, 77];
scores[0];       // 88
scores.length;   // 3
```

### 12.2 Common mutations

```javascript
scores.push(100);   // add end
scores.pop(); // remove end
```

### 12.3 Iteration

- `for`, `for...of`, `forEach` (method — functions module later).

### 12.4 Mental model

- **Ordered list** — like Python `list` (but typed as object under the hood).  
- **Reference type:** two variables can refer to **same** array.

---

## 13. Data structures — objects

### 13.1 Plain object as key-value map

```javascript
const student = {
  id: 42,
  name: "Sam",
  email: "sam@example.com",
  active: true,
};
```

### 13.2 Access

```javascript
student.name;     // dot notation
student["email"]; // bracket — needed for dynamic keys
const key = "id";
student[key];
```

### 13.3 Nested structures

```javascript
const course = {
  title: "JS 101",
  instructor: { name: "Alex", id: 7 },
  roster: [{ id: 1 }, { id: 2 }],
};
```

### 13.4 Python parallel

- Plain objects map cleanly to Python **dict** (with string keys in beginner usage).  
- Arrays map to Python **list** of **dict** — a very common JSON shape from APIs.

---

## 14. Practical mini-patterns (classroom examples)

### 14.1 Validate and branch

```javascript
const password = "short";
if (password.length < 8) {
  console.log("Password too short");
} else {
  console.log("OK");
}
```

### 14.2 Sum an array

```javascript
const values = [10, 20, 30];
let total = 0;
for (const v of values) {
  total += v;
}
console.log(total);
```

### 14.3 Roster: array of objects

```javascript
const users = [
  { name: "Ada", role: "admin" },
  { name: "Bob", role: "guest" },
];

for (const u of users) {
  if (u.role === "admin") {
    console.log(u.name, "can manage settings");
  }
}
```

---

## 15. Common pitfalls (from Python habits)

1. Using `==` and being surprised by coercion — default to `===`.
2. Forgetting that `const` objects are still **mutable**.
3. **Off-by-one** in `for` loops; `length` is one past last index.
4. Thinking `[]` or `{}` copy by value — assignment copies **reference**.
5. Global variables for everything — hard to test and reason about.

---

## 16. Glossary (quick reference)


| Term           | Meaning                                                              |
| -------------- | -------------------------------------------------------------------- |
| **ECMAScript** | Language standard JavaScript implements                              |
| **Engine**     | Browser component that runs JS (V8, SpiderMonkey, etc.)              |
| **DOM**        | Tree representation of HTML (deeper in later modules)                |
| **Scope**      | Where a binding is visible                                           |
| **Hoisting**   | `var`/`function` declaration behavior (advanced)                     |
| **JSON**       | Text format — looks like nested objects/arrays (later: `JSON.parse`) |


---

**These notes align with the 2.5-hour lecture script and pre-read.**