# Event Handling & DOM Manipulation — Assessment

**Instructions:**  
- **MCQ 1–6:** Select the **one** best answer (single correct).  
- **MCQ 7–8:** Select **all** that apply (multiple correct).  
- **Subjective:** Use valid JavaScript; you may assume the DOM is ready (script at end of body or deferred).

---

## Part A — Multiple choice (single correct)

**1.** Which API is generally **preferred** in structured projects for attaching a `click` handler?

- A) `onclick` attribute in HTML only  
- B) `element.click = function () { }` exclusively  
- C) `element.addEventListener("click", handler)`  
- D) `document.write("<script>...</script>")`  

---

**2.** On a nested structure, a listener is attached to a parent `<div>`. Inside the handler, **`event.target`** usually refers to:

- A) The `<div>` that owns the listener  
- B) The innermost element where the event originated  
- C) Always `document.body`  
- D) The `window` object  

---

**3.** You want to handle form submission with **pure JavaScript** (no full page navigation). What must you typically call in a `"submit"` listener?

- A) `event.stopPropagation()` only  
- B) `event.preventDefault()`  
- C) `form.reset()`  
- D) `return false` in HTML attribute only  

---

**4.** Which event fires **repeatedly** as the user types in a text `<input>`?

- A) `change` only  
- B) `submit`  
- C) `input`  
- D) `load`  

---

**5.** What is the safest default for inserting **plain user-typed text** into the DOM?

- A) `element.innerHTML = userText`  
- B) `element.outerHTML = userText`  
- C) `element.insertAdjacentHTML("beforeend", userText)`  
- D) `element.textContent = userText`  

---

**6.** After `const copy = node.cloneNode(true)`, which statement is **most accurate**?

- A) All JavaScript event listeners on `node` are copied to `copy`  
- B) Inline HTML `onclick` attributes may be duplicated as attributes, but JS `addEventListener` handlers are not cloned onto `copy`  
- C) `copy` is automatically inserted into the document  
- D) `cloneNode(true)` only copies attributes, not child elements  

---

## Part B — Multiple choice (multiple correct)

**7.** Which of the following are **valid** reasons to avoid inline `onclick="..."` for production-style code? *(Select all that apply.)*

- A) Mixes behavior into markup and is harder to maintain  
- B) Inline handlers are the **only** way to support touch devices  
- C) CSP or team standards may disallow inline scripts  
- D) `addEventListener` cannot be removed once attached  

---

**8.** Which statements about **`removeEventListener`** are **true**? *(Select all that apply.)*

- A) You must pass the **same function reference** that was used with `addEventListener`  
- B) It works with anonymous arrow functions created inline in `addEventListener`  
- C) The event type string must match the one used when adding  
- D) Calling it with a new function that has identical code removes the old listener  

---

## Part C — Subjective question

### Problem: Task list with validation

You are building a small task list UI. Assume this HTML **already exists** in the document:

```html
<section id="tasks-app">
  <form id="task-form">
    <label>
      New task
      <input id="task-input" type="text" autocomplete="off" />
    </label>
    <button type="submit">Add</button>
    <p id="task-error" class="error" aria-live="polite"></p>
  </form>
  <ul id="task-list"></ul>
</section>
```

**Requirements:**

1. **Submit handler:** On `#task-form` `submit`, prevent the default form submission. Read the trimmed value from `#task-input`. If it is empty or shorter than 3 characters, set `#task-error`’s `textContent` to a clear message, `focus()` the input, and **do not** add a task. If valid, clear the error text, create a new `li`, set its `textContent` to the task string, append it to `#task-list`, and clear the input (set `value` to `""`).

2. **Delegation (bonus behavior):** Attach **one** `click` listener on `#task-list`. When a click originates on a `button` with `class="remove"` inside an `li`, remove **that** `li` from the list. (If you implement this, each new `li` must include a remove button, e.g. `<button type="button" class="remove" aria-label="Remove task">×</button>` alongside the text — you may use a `span` for the label if you prefer.)

3. **Short answer:** In **2–3 sentences**, explain why listening on `#task-list` for remove clicks can be better than calling `addEventListener` on every remove button each time you add a task.

---

### Answer key (instructor — remove when sharing with students)

**Single correct:** 1-C, 2-B, 3-B, 4-C, 5-D, 6-B  

**Multiple correct:** 7-A,C | 8-A,C  

**Subjective — sample solution:**

```javascript
const form = document.querySelector("#task-form");
const input = document.querySelector("#task-input");
const error = document.querySelector("#task-error");
const list = document.querySelector("#task-list");

form.addEventListener("submit", (event) => {
  event.preventDefault();
  const text = input.value.trim();
  if (text.length < 3) {
    error.textContent = "Task must be at least 3 characters.";
    input.focus();
    return;
  }
  error.textContent = "";

  const li = document.createElement("li");
  const label = document.createElement("span");
  label.textContent = text;

  const removeBtn = document.createElement("button");
  removeBtn.type = "button";
  removeBtn.className = "remove";
  removeBtn.setAttribute("aria-label", "Remove task");
  removeBtn.textContent = "×";

  li.append(label, " ", removeBtn);
  list.append(li);
  input.value = "";
  input.focus();
});

list.addEventListener("click", (event) => {
  const btn = event.target.closest("button.remove");
  if (!btn || !list.contains(btn)) return;
  const li = btn.closest("li");
  if (li) li.remove();
});
```

**Explanation sample:** One parent listener scales when items are added or removed; you avoid registering/removing handlers per button and reduce memory leaks from duplicate listeners.

---

**Total MCQ:** 8 (6 single-select + 2 multi-select)  
**Subjective:** 1
