# JavaScript Fundamentals — Pre-Read

**Audience:** Students who completed the Python module.  
**Goal:** Arrive ready to connect familiar ideas (variables, types, control flow, collections) to how browsers run JavaScript and how the web stack fits together.

---

## What is this topic?

**JavaScript** is the programming language that makes web pages *interactive* in the browser. While **HTML** describes structure and **CSS** describes presentation, **JavaScript** handles behavior: reacting to clicks, validating forms, fetching data, updating the page without a full reload, and powering modern front-end frameworks.

Unlike Python, which you often run as scripts on a computer or server, **browser JavaScript runs inside a sandboxed environment** provided by the browser (the **JavaScript engine**). You will attach scripts to HTML, open the page, and use tools like the **console** to inspect output and debug.

This module introduces:

- How JS connects to HTML and runs in the browser  
- Debugging basics with `console.log` and comments  
- Core syntax: variables (`let`, `const`, `var`), types, scope, operators, `if`/`else`, loops  
- Everyday structures: strings, arrays, and objects — with small, practical examples  

---

## Why are we learning this?

1. **Industry relevance:** Most user-facing software touches the web. JavaScript is central to front-end development and increasingly used on the server (Node.js) and in tooling.  
2. **Completes the web literacy picture:** You already think like a programmer (Python). Now you learn how *the same ideas* appear in the environment billions of users interact with daily.  
3. **Bridge to modern stacks:** Frameworks (React, Vue, etc.), APIs, and full-stack work all assume solid JS fundamentals.  
4. **Debugging mindset:** The browser **DevTools** console is where front-end issues are diagnosed; `console.log` is your first step into that workflow.

---

## Real-world examples

| Context | Role of JavaScript |
|--------|---------------------|
| **E-commerce checkout** | Validate card/expiry format client-side, update cart totals, show errors before submit. |
| **Dashboards (analytics, admin)** | Fetch JSON from an API, render tables/charts, filter data without reloading the page. |
| **Forms (signup, contact)** | Instant feedback (“password too short”), conditional fields, async checks (e.g. username taken). |
| **Single-page apps (SPAs)** | One HTML shell; JS swaps views and state — feels like a desktop app in the browser. |
| **Games & interactive media** | Canvas/WebGL, animation loops, input handling — all driven by JS in the browser. |

---

## Problem-style examples (to think about before class)

Use these as mental warm-ups. You do **not** need perfect answers — we will solve similar patterns in class.

### Problem A — “Where does this run?”

You have a file `app.js` and `index.html`. The HTML contains:

```html
<script src="app.js"></script>
```

**Question:** When you double-click `index.html` or serve it from a small local server, *who* executes `app.js` — the operating system, Python, or the browser? What happens if the script path is wrong?

*Hint (Python analogy):* Running `python script.py` uses the Python interpreter; loading a page uses the browser’s JS engine.

---

### Problem B — Mutability vs rebinding (Python vs JS mental model)

In Python you might write:

```python
name = "Ada"
name = "Grace"  # name now refers to a different string
```

In JavaScript you will see:

```javascript
let name = "Ada";
name = "Grace";
```

**Question:** In plain words, what is the difference between **changing what a variable points to** and **mutating the contents of a value**? (We will connect this to `const`, arrays, and objects.)

---

### Problem C — “Global vs block”

Consider nested blocks (conceptually):

```text
BLOCK A
  declare x
  BLOCK B
    declare x again?
```

**Question:** Should inner `x` be the same as outer `x`, or a separate binding? What bugs appear if the rules are unclear?

*Python prepares you:* You have seen indentation-scoped blocks and `global` / nonlocal ideas in advanced cases. JavaScript has **function scope** (historically with `var`) and **block scope** (with `let` and `const`).

---

### Problem D — Collections

You need to store:

1. A list of student scores in order.  
2. A single student’s record: name, id, email.

**Question:** Which JavaScript structures fit (1) and (2)? How would you access the third score, and the student’s email?

---

### What to bring to class

- Curiosity about how browsers work.  
- Your Python mental model (variables, loops, lists, dicts) — we will map terms.  
- Optional: install **Chrome** or **Edge** (or Firefox) and know how to open **Developer Tools** (we will practice).

---

**Estimated pre-read time:** 25–35 minutes.
