# JavaScript Functions & DOM Fundamentals — Pre-Read

**Audience:** Students who completed Python and JavaScript fundamentals (variables, types, control flow, basic HTML).  
**Goal:** Connect Python’s idea of **functions** to JavaScript’s multiple syntaxes, then understand how JS **reads and navigates** the live page structure (the **DOM**) using the `document` object and browser tools.

---

## What is this topic?

### Functions in JavaScript

A **function** packages reusable logic: named inputs (**parameters**), optional **return values**, and a body of statements. In JS you can define the *same idea* in more than one way:

- **Function declaration:** `function name() { }`  
- **Function expression:** `const name = function() { };`  
- **Arrow function:** `const name = () => { };`  

Each form has rules the engine follows (e.g. **hoisting** for declarations) and subtle differences (especially around **`this`** in methods — we touch this at a preview level).

### The DOM (Document Object Model)

When the browser loads HTML, it builds an in-memory **tree** of **nodes**: elements, text, attributes. That tree API is the **DOM**. JavaScript in the page uses **`document`** (and element references) to **select** nodes, **move** between parents and children, and **read** what appears on screen (and later: change it).

**Developer Tools** let you **inspect** that tree live: see HTML, styles, and the structure your selectors must match.

---

## Why are we learning this?

1. **Reusable logic:** Functions are how you avoid copy-paste and structure programs — same motivation as `def` in Python.  
2. **Interactivity:** Almost every behavior on a page starts with “get the right element(s), then do something.” DOM selection is step one.  
3. **Industry alignment:** Interviews and real codebases use **arrow functions** heavily; you must also **read** traditional `function` syntax in older modules.  
4. **Debugging:** If your selector is wrong, nothing “breaks” loudly — you get `null` or empty lists. DevTools inspection is how you **see** the real structure.

---

## Real-world examples

| Context | Functions | DOM |
|--------|-----------|-----|
| **Login form** | `validateEmail(s)`, `hashInput` (later modules) | Select `#email`, read `.value`, show error `<span>` |
| **Todo list** | `addTodo(title)`, `toggleDone(id)` | `querySelectorAll('.todo')`, traverse to find checkbox |
| **Product gallery** | `formatPrice(n)`, `thumbnailUrl(id)` | Select container, loop cards, read `data-product-id` |
| **Analytics / tagging** | Small pure helpers | Find buttons by role or class, attach listeners (next unit) |
| **Accessibility fixes** | `expandSection(el)` | Walk DOM to find headings, update `aria-expanded` |

---

## Problem-style examples (before class)

### Problem A — Declaration vs expression (predict)

```javascript
console.log(square(3));

function square(n) {
  return n * n;
}
```

vs

```javascript
console.log(double(3));

const double = function (n) {
  return n * n;
};
```

**Question:** Which snippet is safe as written? Why might the other fail?

*Python hint:* In Python, `def` is defined for the whole block; JS **hoists** declarations differently than `const` bindings.

---

### Problem B — Arrow functions and braces

```javascript
const add = (a, b) => a + b;
const greet = (name) => { return "Hi, " + name; };
```

**Question:** When can you **omit** `return` and curly braces in an arrow function? What does `{ }` without `return` actually return?

---

### Problem C — Selectors

HTML:

```html
<article id="post-1" class="post featured">
  <h2 class="title">Hello</h2>
  <p class="excerpt">Short text</p>
</article>
```

**Question:** What is the difference between `document.querySelector('.post')` and `document.querySelectorAll('.post')`? What does each return if **no** match exists?

---

### Problem D — Traversal vocabulary

**Question:** In plain English, what is the difference between **parentElement**, **firstElementChild**, and **nextElementSibling**? Why use `*Element*` variants instead of `firstChild` when you only care about HTML elements?

---

### What to bring

- Browser with **DevTools** (Chrome / Edge / Firefox).  
- Comfort with HTML tags and **classes** vs **ids**.  
- Python memory: `def`, arguments, `return`, lambda (optional — we’ll relate to arrows).

---

**Estimated pre-read time:** 30–40 minutes.
