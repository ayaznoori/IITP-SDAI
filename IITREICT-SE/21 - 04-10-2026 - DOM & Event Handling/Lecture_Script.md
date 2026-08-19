# Lecture Script: DOM & Event Handling
**Duration:** 110 minutes | **Tools:** VS Code, Browser, DevTools Elements + Console | **Project:** Contact / search snippet on student page

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 6 min | Dead button vs live page |
| Why Does This Matter? | 12 min | Cart, search, login |
| What Is the Concept? | 24 min | DOM tree, selectors, events |
| How Do We Apply It? (LOs) | 56 min | Live form + trio of layers |
| Recap | 12 min | Preview Advanced DOM |

---

## Session Opening (6 min)

**Problem:** A beautiful HTML button does nothing. Students think the browser is broken.

**[Script:]** "HTML is the **nouns**. CSS is the **adjectives**. JavaScript is the **verbs**. The **DOM** is the live tree. Today we **select**, **update**, **listen**, **validate**, and see the three layers work as one."

> **In the Real World:** **Amazon** quantity stepper, **Zomato** search box, **Instagram** like button — all DOM + events.

🎯 **Instructor Note:** Click "Add to cart" on a live site with DevTools open. Show the node highlight. Do not reverse-engineer their JS. Make the point: the tree changes.

---

## Why Does This Matter?

🎯 **Instructor Note:** Hook — "Raise a hand if your portfolio form currently reloads and loses data." Most will.

**[Script:]** "Without the DOM, JS cannot find the button. Without events, JS never runs at the right time. Without validation, empty emails hit a future API. **Paytm** login is this session at product scale."

**Pain if misunderstood:**
- `getElementById` before the element exists (script in `head` without `defer`)
- Using `querySelector` when they needed **all** matches
- Forgetting `preventDefault` on submit

---

## What Is the Concept?

### DOM tree

**Definition:** Browser parse of HTML into **nodes** (elements, text). JS reads and writes that tree.

**Mental model:** Family tree. `querySelector("ul li")` is "first list item in a ul."

### Selectors

`getElementById` — fastest to teach for unique widgets.  
`querySelector` — CSS selector power.  
`querySelectorAll` — NodeList; loop with `forEach`.

### Content and attributes

`textContent` vs `innerHTML`. Attributes: `value`, `src`, `disabled`, `class` (classList comes next session — here `setAttribute`/`getAttribute` is enough; `className` optional).

### Events

`addEventListener(type, handler)`. Types in LO: **click**, **input**, **submit**.

### HTML + CSS + JS

Same document. CSS can style `.error`. JS adds the class or sets text.

**Python vs JS:** Python had no page tree. The browser **is** the environment.

**Common mistakes:** Typo in id. Listening on `button` click instead of `form` submit for validation. Validating only CSS `:required` and thinking JS is unused — still teach JS checks.

---

## How Do We Apply It?

### LO 1: Select with getElementById, querySelector, querySelectorAll

**Problem:** Highlight all prices on a fake catalog.

**Translate logic:** One title by id. First card with `.card`. All `.price` with `querySelectorAll`.

**Write code:**

```javascript
const header = document.getElementById("shop");
const first = document.querySelector(".card");
const prices = document.querySelectorAll(".price");
console.log(header, first, prices.length);
```

**Predict before running:** If two `.card` exist, what does `querySelector` return?

**Explain result:** Only the **first**. Need `querySelectorAll` for a list.

> **In the Real World:** **Myntra** filter chips are many nodes. You select a **list**, not one.

---

### LO 2: Read and update content and attributes

**Problem:** Product image missing alt. Title should show stock.

**Write code:**

```javascript
const img = document.querySelector("#hero");
img.setAttribute("alt", "Red running shoes");
document.getElementById("stock").textContent = "In stock";
```

**Predict before running:** Does `textContent` change the HTML source file on disk?

**Explain result:** No. It changes the **live DOM**. Refresh restores the file.

**Demo:** Toggle `disabled` on a button with `setAttribute` / `removeAttribute`.

---

### LO 3: Attach click, input, submit listeners

**Problem:** Live character count + save click + form submit.

**Write code:**

```javascript
nameInput.addEventListener("input", (e) => {
  count.textContent = e.target.value.length;
});
saveBtn.addEventListener("click", () => {
  status.textContent = "Saved locally";
});
form.addEventListener("submit", (e) => {
  e.preventDefault();
  status.textContent = "Submitted";
});
```

**Predict before running:** Without `preventDefault`, what happens on Enter?

**Explain result:** Browser default — often a reload. The listener must stop it for in-page apps.

> **In the Real World:** **Twitter/X** compose box is `input`. Tweet button is `click`. Posting is `submit` or click that runs the same logic.

---

### LO 4: Implement basic form validation

**Problem:** Contact form: email required, message min 10 chars.

**Translate logic:** On submit, prevent default. Trim values. Set an error paragraph. Return early.

**Write code:**

```javascript
form.addEventListener("submit", (e) => {
  e.preventDefault();
  const email = emailEl.value.trim();
  const msg = msgEl.value.trim();
  if (!email) {
    err.textContent = "Email is required";
    return;
  }
  if (msg.length < 10) {
    err.textContent = "Message too short";
    return;
  }
  err.textContent = "";
  ok.textContent = "Looks good";
});
```

**Predict before running:** Empty email — do we still check message length?

**Explain result:** No, if we `return` early. Teach **order of checks**.

🎯 **Instructor Note:** Pair lab: 8 minutes, validate a login of username + password length >= 6.

---

### LO 5: Explain how HTML, CSS, and JS work together

**Problem:** Error text is invisible because no CSS. Or JS sets text but class `error` is never applied.

**Walkthrough:** Three files or one HTML with `<style>` and `<script>`.

```html
<p class="hint" id="err"></p>
```

```css
.hint { min-height: 1.2em; }
.error { color: #b91c1c; font-weight: 700; }
```

```javascript
err.className = "hint error";
err.textContent = "Email is required";
```

**Predict before running:** If JS is disabled, does HTML still show the form?

**Explain result:** Yes. Progressive idea: structure survives; behavior enriches. CSS without JS still styles the empty hint box.

> **In the Real World:** **GOV.UK** style forms keep HTML working; JS adds live validation. Same split.

---

## Recap (12 min)

Students demo neighbor's form. Checklist: three selectors used, one attribute change, three event types, validation messages, CSS class for error.

**[Script:]** "Next session we **create and remove** nodes — lists and tabs. Today you **found** nodes. Tomorrow you **build** them."

---

## Lecture Summary

- **Select** with `getElementById`, `querySelector`, `querySelectorAll`
- **Read/update** `textContent` and attributes on the live DOM
- **Listen** for click, input, and submit
- **Validate** forms with `preventDefault` and clear messages
- **HTML, CSS, JS** split structure, style, and behavior
- **Practical value:** Every interactive page you use is this trio talking to the DOM
