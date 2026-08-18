# Lecture Script: DOM Manipulation & Event Handling
**Duration:** 110 minutes | **Tools:** VS Code, Browser, DevTools Console | **Languages:** HTML + JavaScript

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Static vs interactive web |
| Why Does This Matter? | 12 min | Interactivity everywhere |
| What Is the Concept? | 25 min | DOM tree, selection, events |
| How Do We Apply It? (LOs) | 48 min | Live coding each LO |
| Interactive build lab | 12 min | Mini app: click + input |
| Recap & summary | 8 min | Bridge to React |

---

## Session Opening (5 min)

**[Script:]** "You built structure with HTML, style with CSS. Still, when you click Like on a post, the count updates without a full page reload. That is JavaScript changing the **DOM** — the browser's live version of your page. Today your pages stop being posters and start being interfaces."

**Problem hook:** Show static HTML page. Click a button — nothing happens. Add three lines of JS — button works. "This is the moment web development gets fun."

🎯 **Instructor Note:** Ensure everyone has `index.html` + empty `app.js` in same folder from prior sessions.

---

## Why Does This Matter?

🎯 **Instructor Note:** List interactive patterns on board: like button, form validation, dark mode toggle, accordion menu, live character count.

**[Script:]** "React, Vue, and every framework eventually compile down to DOM updates. If you understand selection and events at this level, framework magic becomes learnable mechanics."

**Real-world use:**
- Form validation before submit
- Shopping cart badge updates
- Search-as-you-type previews
- Modal open/close, tabs, menus

**Pain if misunderstood:**
- Script runs before HTML exists — `null` errors
- Wrong selector — silent failures
- Using `innerHTML` with user input — security risk (mention, do not deep-dive XSS)
- Inline `onclick` spaghetti — hard to maintain

---

## What Is the Concept?

### Attaching JavaScript to HTML

**External script (standard):**

```html
<body>
  <!-- content -->
  <script src="app.js"></script>
</body>
```

**Definition:** The browser downloads HTML, builds the DOM, then runs scripts at the bottom.

**Mental model:** HTML loads the stage; JavaScript is the stage manager after setup.

**Console:**

```javascript
console.log("app.js loaded");
```

🎯 **Instructor Note:** Show Console tab — errors in red. "Read errors; they tell you the line."

**Common mistakes:**
- `src="app.js"` path wrong when file in subfolder
- Script in `<head>` without defer — may query missing elements

### The DOM Tree

**Definition:** The DOM is a tree of objects representing HTML nodes.

**Mental model:** Family tree — `html` parent of `body`, `body` parent of `main`, etc.

**[Script:]** "JavaScript does not edit your `.html` file on disk during runtime. It edits the **live DOM** in memory. Refresh resets unless you saved changes in the file."

**DevTools demo:** Elements panel — expand tree, highlight nodes.

### Selecting Elements

| API | Usage |
|-----|--------|
| `document.getElementById("id")` | One id |
| `document.querySelector(".class")` | First CSS match |
| `document.querySelectorAll("p")` | All matches |

```javascript
const msg = document.getElementById("message");
const btn = document.querySelector("#submitBtn");
const cards = document.querySelectorAll(".card");
```

**Comparison (Python vs JS):** Python has no built-in DOM — this is browser-specific JS power.

**Common mistakes:**
- `#id` in querySelector but forgot `#` — selects tag name instead
- `getElementById("#msg")` — wrong, no `#` with this method

### Reading and Updating Content and Attributes

**Text:**

```javascript
el.textContent = "New text";
```

**Attributes:**

```javascript
img.setAttribute("alt", "Photo of campus");
link.href = "https://example.com";
```

🎯 **Instructor Note:** Demo `classList.add("active")` as stretch — only if time; not required by LO.

### Events and addEventListener

**Definition:** An event listener registers a function to run when an event occurs.

```javascript
btn.addEventListener("click", function () {
  console.log("clicked");
});
```

**`click`** — button, link (prevent default later), custom UI

**`input`** — fires on every keystroke in text fields

```javascript
input.addEventListener("input", function () {
  preview.textContent = input.value;
});
```

**Mental model:** Doorbell — event is the ring; handler is you answering the door.

**Common mistakes:**
- Calling handler immediately: `addEventListener("click", myFunc())` — wrong
- Correct: `addEventListener("click", myFunc)` or wrap in function

---

## How Do We Apply It?

### LO 1: Attach JavaScript file and use browser console

**Problem:** Verify JS loads and debug output.

**Write code:**

`index.html`:

```html
<p id="status">Loading...</p>
<script src="app.js"></script>
```

`app.js`:

```javascript
console.log("DOM session: script attached");
const status = document.querySelector("#status");
status.textContent = "Script ran successfully";
```

**Predict before running:** What appears in page and console?

**Explain result:** Both update — proves file linked and DOM reachable at script execution time.

---

### LO 2: Select elements with getElementById and querySelector

**Problem:** Update title and all card subtitles.

**Write code:**

```javascript
const title = document.getElementById("pageTitle");
title.textContent = "Interactive Page";

const subtitles = document.querySelectorAll(".subtitle");
subtitles.forEach(function (el) {
  el.textContent = "Updated subtitle";
});
```

**Predict:** How many elements change if two `.subtitle` exist?

**Explain result:** `querySelectorAll` returns NodeList — loop updates each.

🎯 **Instructor Note:** Briefly show `forEach` — connects to array methods from Module 1.

---

### LO 3: Read and update element content or attributes

**Problem:** Toggle link destination and label from JS.

**HTML:**

```html
<a id="cta" href="https://old.com">Old Site</a>
<button id="switch">Switch Link</button>
```

**JS:**

```javascript
const cta = document.querySelector("#cta");
document.querySelector("#switch").addEventListener("click", function () {
  cta.href = "https://new.com";
  cta.textContent = "New Site";
});
```

**Predict before click:** `href` value? After click?

**Explain result:** Attributes and text update live in DOM.

---

### LO 4: Attach click and input event listeners

**Problem:** Live greeting as user types name; button clears field.

**Write code:**

```javascript
const nameInput = document.querySelector("#name");
const output = document.querySelector("#output");
const clearBtn = document.querySelector("#clear");

nameInput.addEventListener("input", function () {
  output.textContent = "Hello, " + nameInput.value;
});

clearBtn.addEventListener("click", function () {
  nameInput.value = "";
  output.textContent = "Hello, ";
});
```

**Predict:** Typing "Asha" — what shows in `#output`?

**Explain result:** `input` event fires per keystroke; `value` reads current field content.

---

### LO 5: Build one small interactive DOM change on user action

**Problem:** Dark mode toggle for page background (no CSS file change — inline style OK for demo).

**Write code:**

```javascript
const toggle = document.querySelector("#themeToggle");
const body = document.body;
let dark = false;

toggle.addEventListener("click", function () {
  dark = !dark;
  body.style.backgroundColor = dark ? "#222" : "#fff";
  body.style.color = dark ? "#fff" : "#222";
});
```

**Predict:** After two clicks, dark or light?

**Explain result:** Boolean flag tracks state; each click flips styles.

🎯 **Instructor Note:** Discuss why toggling a CSS class is cleaner — preview React/state pattern without teaching classList deeply.

---

## Interactive Build Lab (12 min)

**Capstone mini-app — "Mood Board":**

- Heading and paragraph in HTML
- Text input: live updates paragraph (`input` event)
- Two buttons: "Happy" / "Calm" — `click` changes paragraph text and background color
- Use `querySelector`, `textContent`, `addEventListener` only
- Console.log each event type for debugging practice

**[Script:]** "One screen, three behaviors, zero page reload. That is a micro-app."

---

## Recap (8 min)

🎯 **Instructor Note:** Debug scenario — `Cannot read properties of null` — class diagnoses: selector wrong or script too early.

---

## Lecture Summary

- **External scripts** at end of `body` connect JavaScript to HTML safely
- **The DOM** is the live tree JavaScript reads and modifies
- **`getElementById` and `querySelector`** target elements for updates
- **`textContent` and attributes** change what users see and link behavior
- **`addEventListener`** wires `click` and `input` to handler functions
- **Interactive demos** combine selection, update, and events into real UI behavior
- **Practical value:** DOM + events are the foundation under every SPA, form, and dashboard you will build — including React

**[Script:]** "Module 2 web fundamentals — complete. You can structure, style, layout, and interact. Next modules add Git, then React — which wraps these same ideas in components. Today you learned the raw machinery. That knowledge never goes away."
