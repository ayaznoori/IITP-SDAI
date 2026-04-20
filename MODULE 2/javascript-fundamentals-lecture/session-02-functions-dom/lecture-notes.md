# JavaScript Functions & DOM Fundamentals — Detailed Lecture Notes

**Prerequisites:** Python; JS basics; HTML structure (tags, attributes, `id`, `class`).  
**Environment:** Modern browser with Developer Tools.

---

## Part A — Functions

### A.1 Why functions?

Functions bundle logic you can **call** with different inputs — same idea as Python `def`. Benefits: reuse, testing, naming, and separating “what” from “how.”

---

### A.2 Function declaration

```javascript
function add(a, b) {
  return a + b;
}

add(2, 3); // 5
```

- **Hoisting:** the **name** `add` is available throughout its enclosing scope even if the declaration appears lower in the file (the whole function body is hoisted).  
- Common for top-level utilities and traditional object methods in older code.

---

### A.3 Function expression

```javascript
const add = function (a, b) {
  return a + b;
};
```

- Here `add` is a **`const` binding** assigned a function value.  
- **Not hoisted** like a declaration: you cannot call `add` before this line runs.  
- **Anonymous function:** `function (a, b) { ... }` has no name (stack traces may show `(anonymous)` — optional **named function expression** for debugging in advanced style).

---

### A.4 Parameters, arguments, and return

- **Parameters** are the names in the header; **arguments** are the values at the call site.  
- If you **omit** `return`, the function returns **`undefined`**.

```javascript
function logOnly(x) {
  console.log(x);
}
logOnly(1); // undefined (implicit return)
```

- **Extra arguments** are ignored (unless you use `...rest` — later topic).  
- **Missing arguments** yield `undefined` inside the function.

```javascript
function greet(name, title) {
  return `${title ?? ""} ${name}`.trim();
}
```

**Default parameters (ES6):**

```javascript
function pow(x, exp = 2) {
  return x ** exp;
}
```

---

### A.5 Arrow functions

**Syntax:**

```javascript
const add = (a, b) => {
  return a + b;
};

// Concise body — implicit return of the expression
const add2 = (a, b) => a + b;

// Single parameter — parentheses optional
const square = (n) => n * n;

// No parameters
const roll = () => Math.floor(Math.random() * 6) + 1;
```

**Returning an object literal** — wrap in parentheses so `{` starts an object, not a block:

```javascript
const point = (x, y) => ({ x, y });
```

---

### A.6 Normal `function` vs arrow — practical comparison

| Topic | `function` | Arrow `=>` |
|--------|------------|------------|
| **`this` binding** | Dynamic — own `this` (important in object methods / DOM handlers in some patterns) | Lexical — inherits `this` from enclosing scope |
| **`arguments` object** | Available | Not own `arguments` (use `...args` in modern code) |
| **Hoisting** | Declaration form is hoisted | Not hoisted (typical `const` assignment) |
| **Use as constructor** | Can use with `new` (when designed for it) | Cannot use with `new` |
| **Readability** | Verbose; familiar | Compact for short lambdas |

**Heuristic for beginners:**

- Prefer **arrow** for short **pure** helpers and array callbacks: `items.map(x => x.price)`.  
- Use **`function`** when you need **stable `this`** for methods or will teach `class` / prototypes next: `const obj = { name: "A", greet() { return this.name; } };`

*Note:* DOM `addEventListener` callbacks are often arrows **or** bound functions — event module will formalize.

---

### A.7 Scope recap (link to prior module)

- `const` / `let` in blocks behave as before; **nested functions** close over outer variables (**closure** — name only today).

---

## Part B — DOM fundamentals

### B.1 What is the DOM?

- **Document Object Model:** browser’s structured representation of the page.  
- **Tree** of **nodes** (element nodes, text nodes, comments, etc.).  
- JavaScript exposes the tree via APIs starting from the global **`document`** object.

---

### B.2 The `document` object

- Represents the loaded HTML document.  
- Entry points: `document.documentElement`, `document.head`, `document.body`.  
- Selection methods: `getElementById`, `querySelector`, `querySelectorAll`, and legacy helpers (`getElementsByTagName`, etc.).

---

### B.3 Selecting elements

#### `getElementById`

```javascript
const el = document.getElementById("app");
// el is Element or null
```

- **Fast** and explicit for ids.  
- **No** `#` prefix.

#### `querySelector`

```javascript
const first = document.querySelector(".card.featured");
const only = document.querySelector("#main nav a");
```

- Returns the **first** matching **Element**, or **`null`**.  
- String is a **CSS selector**.

#### `querySelectorAll`

```javascript
const nodes = document.querySelectorAll(".item");
```

- Returns a **NodeList** of all matches (may be empty).  
- **NodeList** is array-*like*; modern JS: iterate with `for...of`.

```javascript
for (const item of nodes) {
  console.log(item.textContent);
}
```

---

### B.4 Traversing the DOM

Common **element-only** properties (skip pure text noise between tags):

| Property | Meaning |
|----------|---------|
| `parentElement` | Parent element |
| `children` | Live HTMLCollection of child **elements** |
| `firstElementChild` / `lastElementChild` | First/last child **element** |
| `previousElementSibling` / `nextElementSibling` | Adjacent element siblings |

**Why not `firstChild`?** It may be a **text node** (whitespace). For markup walking, `*Element*` properties are usually clearer.

```javascript
const h2 = document.querySelector("h2.title");
const section = h2.parentElement;
const next = section.nextElementSibling;
```

---

### B.5 Reading element content

| Property | Typical use |
|----------|-------------|
| `textContent` | All text inside node + descendants; **not** HTML parsing; good for **reading** plain text safely |
| `innerHTML` | String of HTML inside element; parsing HTML — **risky** if assigning untrusted strings (XSS) |
| `innerText` | Rendered text as displayed (CSS-aware); slower; layout-dependent |

**Today’s focus:** prefer **`textContent`** for “what text is in this subtree?”

```javascript
const label = document.querySelector(".label");
console.log(label.textContent.trim());
```

**Attributes** (awareness): `el.getAttribute("data-id")`, `el.id`, `el.className`, `el.classList`.

---

### B.6 Inspecting the DOM in Developer Tools

1. **Elements / Inspector** panel shows the live tree.  
2. **Picker** mode selects a node on the page and focuses it in the tree.  
3. Edit attributes temporarily to test selectors (refresh loses edits unless saved in source).  
4. **Console helpers (Chrome):** `$0` is the currently selected element — useful with `console.log($0.textContent)`.

---

## Part C — Worked mini-example (functions + DOM)

```javascript
function readHeadline(rootSelector) {
  const root = document.querySelector(rootSelector);
  if (!root) {
    return null;
  }
  const heading = root.querySelector("h2");
  return heading ? heading.textContent.trim() : null;
}

// Usage after DOM is loaded:
console.log(readHeadline("#news"));
```

- **Guard clause** for missing root.  
- **Scoped search:** `root.querySelector` limits search to subtree.

---

## Part D — Common pitfalls

1. Running script **before** DOM exists — selection returns `null` (fix: `defer`, place script at end, or `DOMContentLoaded`).  
2. Confusing **live** vs **static** collections (advanced) — `querySelectorAll` returns **static** NodeList in modern browsers.  
3. Forgetting `querySelector` returns **`null`** — always check before use.  
4. Using `innerHTML` with user input — security hole.  
5. Arrow methods on objects when you need **`this`** — use method shorthand or `function`.

---

## Part E — Glossary

| Term | Meaning |
|------|---------|
| **Arity** | Number of parameters a function expects |
| **Hoisting** | Engine’s compile-time behavior moving declarations |
| **Lexical scope** | Where variables are visible based on source structure |
| **Node** | Generic DOM tree item |
| **Element** | Node that is an HTML element |
| **NodeList** | Collection returned by `querySelectorAll` |
| **CSS selector** | Pattern to match elements (`#id`, `.class`, `nav a`) |

---

**Aligned with the 2.5-hour lecture script and pre-read.**
