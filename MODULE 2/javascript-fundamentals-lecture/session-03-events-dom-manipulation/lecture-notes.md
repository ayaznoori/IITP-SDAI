# Event Handling & DOM Manipulation — Detailed Lecture Notes

**Prerequisites:** Functions; DOM selection and traversal; basic HTML forms.

---

## Part A — Event handling

### A.1 What is an event?

An **event** is a signal that something occurred: a user **clicked**, **typed**, **submitted** a form, the window **resized**, the DOM **finished loading**, etc. The browser **dispatches** events on objects (often **elements** or `document` / `window`).

---

### A.2 Registering listeners with `addEventListener`

```javascript
const btn = document.querySelector("#save");

btn.addEventListener("click", (event) => {
  console.log("clicked", event.type);
});
```

- **First argument:** event type string (`"click"`, `"input"`, …).  
- **Second argument:** **listener** — a function invoked when the event fires.  
- **Third argument (optional):** options object, e.g. `{ once: true }`, `{ capture: true }`.

**Multiple listeners** on the same element and event type are allowed and run in registration order (bubble phase by default).

---

### A.3 Inline handlers vs JavaScript listeners

**Inline (HTML attribute):**

```html
<button onclick="handleClick()">Go</button>
```

- Tight coupling; harder to test; string is not real JS module scope; CSP may block.  
- OK for quick demos; **avoid** as default in production-style teaching.

**`addEventListener` (preferred):**

```javascript
button.addEventListener("click", handleClick);
```

- Keeps markup clean; easy to attach/detach; multiple handlers.

---

### A.4 Removing listeners

```javascript
function onClick(e) {
  /* ... */
}

btn.addEventListener("click", onClick);
btn.removeEventListener("click", onClick); // same function reference required
```

Anonymous arrows cannot be removed unless stored in a variable.

---

### A.5 Common event types (class focus)

| Event | When it fires |
|-------|----------------|
| `click` | Press + release on element (also activated by keyboard on focusable controls in accessible ways) |
| `input` | Value of `<input>`, `<textarea>`, or `contenteditable` changes |
| `change` | Control loses focus after value changed (for many inputs); semantics differ by control |
| `submit` | Form submit (button `type="submit"`, Enter in single-line fields, etc.) |

---

### A.6 The `Event` object

When the listener runs, the browser passes an **`event`** (or `e`) object:

| Property / method | Meaning |
|-------------------|---------|
| `type` | Event name, e.g. `"click"` |
| `target` | Deepest element where the event originated |
| `currentTarget` | Element that has this listener attached |
| `preventDefault()` | Cancel default browser action (e.g. form navigation) |
| `stopPropagation()` | Stop event from bubbling further |

**Delegation pattern:** attach **one** listener on a parent; use `event.target` (often with `Element.closest`) to see which child was activated.

```javascript
list.addEventListener("click", (event) => {
  const button = event.target.closest("button[data-action]");
  if (!button) return;
  const action = button.dataset.action;
  // ...
});
```

---

### A.7 `submit` and `preventDefault`

```javascript
form.addEventListener("submit", (event) => {
  event.preventDefault();
  // read fields, validate, maybe fetch() later
});
```

Without `preventDefault`, the browser may **navigate** (full page load) depending on `action` / `method`. For SPA-style or in-page validation, you typically prevent default and handle data in JS.

---

### A.8 Interactive exercises (class lab ideas)

- **Counter:** buttons increment/decrement state stored in a variable; update a `<span>`.  
- **Tabs / panels (light):** `data-*` attributes + `classList` on sections.  
- **Delegated list:** add/remove items with one list-level listener (advanced variant).

---

## Part B — DOM manipulation

### B.1 Creating elements

```javascript
const li = document.createElement("li");
li.textContent = "New item";
list.append(li);
```

- **`append`** can take multiple nodes and strings; **`appendChild`** accepts one node only.  
- Prefer **`textContent`** when the string is **user-controlled** to avoid HTML injection.

---

### B.2 Modifying content and structure

| API | Use |
|-----|-----|
| `element.textContent = "..."` | Plain text; no HTML parsing |
| `element.innerHTML = "..."` | Parses HTML — **dangerous** for untrusted strings |
| `element.insertAdjacentHTML(...)` | Similar caution as `innerHTML` |

**Attributes:**

```javascript
img.setAttribute("alt", "Logo");
img.alt = "Logo"; // property shortcut for standard attributes
div.dataset.userId = "42"; // data-user-id="42"
```

**Classes:**

```javascript
box.classList.add("active");
box.classList.toggle("hidden");
box.classList.contains("active");
```

---

### B.3 Removing elements

```javascript
node.remove(); // removes node from tree
```

Legacy: `parent.removeChild(child)`.

---

### B.4 Cloning

```javascript
const copy = template.cloneNode(true); // true = deep clone
```

- **Cloning copies attributes and structure**, not properties like `.value` unless you set them again.  
- **Event listeners** registered in JS are **not** cloned — re-attach if needed.

---

### B.5 `DocumentFragment` (optional optimization)

Build many nodes off-DOM, then append once:

```javascript
const frag = document.createDocumentFragment();
for (const name of names) {
  const li = document.createElement("li");
  li.textContent = name;
  frag.append(li);
}
list.append(frag);
```

Reduces layout thrash when inserting many siblings.

---

## Part C — Basic form validation (client-side)

### C.1 Strategy

1. On **`submit`**, or on **`input`** for live feedback, read values from controls.  
2. Apply rules (required, min length, regex, equality with another field).  
3. Display errors next to fields or in a summary region.  
4. If invalid: `preventDefault()`, `focus()` first bad field, `reportValidity()` if using Constraint Validation API (awareness).

### C.2 Minimal imperative example

```javascript
form.addEventListener("submit", (event) => {
  const email = emailInput.value.trim();
  let valid = true;
  emailError.textContent = "";

  if (!email.includes("@")) {
    emailError.textContent = "Enter a valid email.";
    valid = false;
  }

  if (!valid) {
    event.preventDefault();
    emailInput.focus();
  }
});
```

### C.3 Security note

**Never trust** client-side validation alone. Always validate on the server.

---

## Part D — DevTools

- **Elements:** see live DOM after your scripts run (not just static source).  
- **Event Listeners** (Chrome): inspect handlers on selected node.  
- **Console:** log `event` objects during development.

---

## Part E — Common pitfalls

1. **Stale NodeList** — `querySelectorAll` result won’t auto-update; re-query or build from current parent.  
2. **Forgetting `preventDefault`** on forms when doing in-page handling.  
3. **Assuming `cloneNode` copies listeners** — it does not.  
4. **Setting `innerHTML` with user input** — XSS risk.  
5. **Confusing `input` vs `change`** — pick the right UX for live vs commit semantics.

---

## Part F — Glossary

| Term | Meaning |
|------|---------|
| **Listener / handler** | Function run when event fires |
| **Bubble** | Event travels from target up to ancestors |
| **Delegation** | One parent listener for many children |
| **Mutation** | Change to DOM tree |
| **XSS** | Cross-site scripting via unsafe HTML injection |

---

**Aligned with the 2.5-hour lecture script and pre-read.**
