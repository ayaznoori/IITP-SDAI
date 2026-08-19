# Pre-Read: DOM Manipulation & Event Handling

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **attach a JavaScript file** to an HTML page and use the **browser console**
- How the **DOM tree** represents your HTML in the browser's memory
- How to **select elements** with `getElementById` and `querySelector`
- How to **read and update** text content and attributes like `src` or `href`
- How to **listen for click and input events** and build one small interactive change

---

## 2. Detailed Explanation

### Static Pages Are Done — Time to Make Them React

You built HTML structure. You styled it with CSS and Grid. But the page still cannot **respond**. Click a button — nothing happens unless it is a link. Type in a field — the page does not react.

**JavaScript in the browser** connects your code to the live page through the **DOM** (Document Object Model — the browser's tree-shaped representation of your HTML).

**Analogy:** HTML is the blueprint. CSS is interior design. The DOM is the **actual house the browser built** from the blueprint — and JavaScript is the electrician who rewires lights when you flip switches.

**One-line definition:** The DOM is a live tree of objects the browser creates from HTML; JavaScript can read and change that tree.

### Why DOM Manipulation Matters

**Real-world hook:** Every "Add to Cart," dark mode toggle, form validation message, and like button uses DOM manipulation and events. React, Vue, and Angular eventually compile down to DOM updates — understanding the raw version makes frameworks click later.

**Benefits:**
- **Interactivity** — users get feedback when they act
- **Dynamic content** — show/hide sections without reloading the page
- **Foundation for React** — `useState` re-renders are advanced DOM updates

### Connecting JavaScript to HTML

#### The script Tag

**External file (preferred):**

```html
<body>
  <h1 id="title">Hello</h1>
  <script src="app.js"></script>
</body>
```

**Rule:** Put `<script>` at the **end of `<body>`** so HTML loads before JavaScript runs.

**Internal script (for tiny tests):**

```html
<script>
  console.log("Page loaded");
</script>
```

#### The Browser Console

Open DevTools → **Console** tab. Type expressions and see output. `console.log()` prints messages from your code.

```javascript
console.log("Debug check");
console.log(2 + 2);
```

**Why It Matters:** Console is your JavaScript feedback loop — like Run in One Compiler, but for the live page.

### DOM Tree Basics

The browser parses HTML into a tree:

```
document
 └── html
      ├── head
      └── body
           ├── header
           ├── main
           │    └── p
           └── footer
```

Each tag becomes a **node**. JavaScript starts at `document` and searches down.

**Mental model:** Family tree. `document` is the root ancestor. Each element is a relative you can find by id, class, or tag.

**Common mistake:** Thinking JavaScript edits your `.html` file on disk. It changes the **live DOM in memory**. Refresh resets unless you save HTML separately.

### Selecting Elements

| Method | Selects | Example |
|--------|---------|---------|
| **`getElementById`** | One element by `id` | `document.getElementById("title")` |
| **`querySelector`** | First match for any CSS selector | `document.querySelector(".btn")` |

**HTML:**

```html
<h1 id="page-title">Welcome</h1>
<button class="btn">Click me</button>
```

**JavaScript:**

```javascript
const title = document.getElementById("page-title");
const btn = document.querySelector(".btn");
```

**Why both exist:** `getElementById` is simple for unique ids. `querySelector` accepts class, tag, or compound selectors — same syntax as CSS.

**Common mistakes:**
- Typo in id string — returns `null`
- `getElementById("#title")` — wrong; no `#` in id argument
- Running script in `<head>` before element exists — `null` error

### Reading and Updating Content

**Text content:**

```javascript
const title = document.getElementById("page-title");
console.log(title.textContent);  // read
title.textContent = "Hello, BSAI!";  // update
```

**Attributes:**

```javascript
const link = document.querySelector("a");
link.href = "https://masaischool.com";
link.setAttribute("target", "_blank");

const img = document.querySelector("img");
img.src = "new-photo.jpg";
img.alt = "Updated photo description";
```

**Why It Matters:** Shopping sites update prices, images, and labels without reloading. You are learning the same primitive operations.

### Events — When the User Acts

An **event** (something that happens in the browser — click, key press, input change) can trigger JavaScript.

**`addEventListener`** attaches a function to run when the event fires:

```javascript
const btn = document.querySelector(".btn");

btn.addEventListener("click", function () {
  console.log("Button was clicked!");
});
```

**Common events today:**

| Event | Fires when |
|-------|------------|
| **`click`** | Element is clicked |
| **`input`** | Input or textarea value changes |

**Input example:**

```javascript
const nameInput = document.getElementById("username");

nameInput.addEventListener("input", function () {
  console.log(nameInput.value);
});
```

**Mental model:** Doorbell. Event listener is you waiting. `click` is someone pressing the bell. Your function is opening the door.

### One Small Interactive Demo — Pattern

**HTML:**

```html
<p id="greeting">Hello, guest!</p>
<input id="name" type="text" placeholder="Your name">
<button id="say-hi">Say Hi</button>
```

**JavaScript:**

```javascript
const greeting = document.getElementById("greeting");
const nameInput = document.getElementById("name");
const btn = document.getElementById("say-hi");

btn.addEventListener("click", function () {
  const name = nameInput.value;
  greeting.textContent = "Hello, " + name + "!";
});
```

**Walkthrough:**
1. User types in input
2. User clicks button
3. Handler reads `nameInput.value`
4. Handler updates `greeting.textContent`
5. Paragraph changes **without** page reload

**Why It Matters:** This is the seed of every interactive UI — forms, toggles, counters, games.

### JavaScript vs One Compiler — Quick Bridge

| One Compiler | Browser |
|--------------|---------|
| `console.log` output in panel | `console.log` in DevTools Console |
| Run button executes all code | Script runs when page loads |
| No HTML | JS talks to HTML through DOM |

You already know variables, functions, and strings from Module 1. Today you apply them to **web pages**.

### Building Blocks Checklist

Before the live session, you should recognize:

- [ ] `<script src="app.js"></script>` at end of `<body>`
- [ ] Open DevTools Console
- [ ] DOM as tree of nodes from HTML
- [ ] `document.getElementById("id")` and `document.querySelector(".class")`
- [ ] `element.textContent` to read/write text
- [ ] `element.value` on inputs
- [ ] `addEventListener("click", fn)` and `addEventListener("input", fn)`
- [ ] One interactive update on user action

### Small Code Example

```javascript
const counter = document.getElementById("count");
const btn = document.getElementById("add-btn");
let total = 0;

btn.addEventListener("click", function () {
  total = total + 1;
  counter.textContent = total;
});
```

---

## 3. Practice Exercises

**Exercise 1 — Script link and console**
Create `app.js` with `console.log("JS connected");`. Link it from your HTML at the end of `<body>`. Open the page, open Console, confirm the message appears.

**Exercise 2 — Select and read**
Add `<p id="status">Loading...</p>`. In `app.js`, select it with `getElementById`, log its `textContent` to the console. Change the text in HTML, refresh, log again.

**Exercise 3 — Click to update**
Add a button `#dark-toggle`. On click, change the `<body>` background color to `#1a202c` and text color to `#fff` using `document.body.style.backgroundColor` and `document.body.style.color`. Click again behavior optional — live session may add toggle logic.

**Exercise 4 — Input live preview**
Add `<input id="live-name">` and `<p id="preview">Your name will appear here.</p>`. On `input` event, set `preview.textContent` to `"Hello, " + liveNameInput.value + "!"`. Type slowly and watch the paragraph update.
