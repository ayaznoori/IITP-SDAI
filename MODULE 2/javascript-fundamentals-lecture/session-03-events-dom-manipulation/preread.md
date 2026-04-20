# Event Handling & DOM Manipulation — Pre-Read

**Audience:** Students who know JS basics, functions, and DOM selection/traversal.  
**Goal:** Understand how browsers **dispatch events**, how you **subscribe** with listeners, and how to **change the document** safely (create, update, remove) including simple **form validation**.

---

## What is this topic?

### Event handling

User actions (click, typing, submitting) and browser lifecycle moments generate **events**. Your code can **react** by registering **listeners** that run when an event fires. You will compare **inline handlers** in HTML with **`addEventListener`**, handle **`click`**, **`input`**, and **`submit`**, and read the **`event`** object (target, type, defaults like form submit).

### DOM manipulation

The DOM tree is **mutable**. JavaScript can **create** new nodes, **change** text or structure, **remove** nodes, and **clone** existing subtrees — without reloading the page. Together with events, this is how buttons, counters, and dynamic lists work.

### Form validation (client-side)

Before data reaches a server, you can **check** fields in the browser: required fields, length, patterns, matching passwords — and show messages. This improves UX but is **not** a substitute for server validation.

---

## Why are we learning this?

1. **Interactivity:** Events are the wiring between user intent and your logic.  
2. **Maintainability:** `addEventListener` scales better than scattered inline attributes.  
3. **Control:** `preventDefault`, `stopPropagation`, and reading `event.target` let you build precise UX (e.g. SPA-style forms, live search).  
4. **Dynamic UIs:** Creating and updating DOM is how dashboards, chats, and carts feel “live.”

---

## Real-world examples

| Context | Events | Manipulation |
|--------|--------|--------------|
| **Add to cart** | `click` on button | Clone template row, append to list, update badge count |
| **Live search** | `input` on search box | Filter list: hide/show items or rebuild fragment |
| **Checkout form** | `submit` on `<form>` | Validate; block submit with `preventDefault` if invalid |
| **Modal / toast** | `click` outside / Escape | Create div, append, remove after timeout |
| **Comment thread** | `submit` on nested form | `preventDefault`, append new `<li>`, clear textarea |

---

## Problem-style examples (before class)

### Problem A — Inline vs listener

```html
<button onclick="alert('hi')">A</button>
<button id="b">B</button>
```

```javascript
document.querySelector("#b").addEventListener("click", () => alert("hi"));
```

**Question:** Which approach keeps behavior mostly in JavaScript files? What breaks if the inline string has a typo?

---

### Problem B — `submit` default behavior

**Question:** What does a `<form>` do by default when the user submits? Why would you call `event.preventDefault()` in a submit handler?

---

### Problem C — `event.target`

A list has three buttons with class `vote`. One listener is attached to the `<ul>`.

**Question:** In the handler, how do you know **which** button was pressed? (Hint: `event.target` vs `event.currentTarget`.)

---

### Problem D — Create vs innerHTML

You need to add ten similar `<li>` items from an array of strings.

**Question:** Why might `document.createElement` + `append` be preferable to one big `innerHTML += ...` in a loop? (Think: security, performance, control.)

---

### What to bring

- Browser with DevTools.  
- Comfort with `querySelector`, arrow functions, and template literals.  
- A simple local HTML file you can open for experiments (or a static server).

---

**Estimated pre-read time:** 30–40 minutes.
