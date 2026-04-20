# JavaScript Functions & DOM Fundamentals — Assessment

**Instructions:**  
- **MCQ 1–6:** Select the **one** best answer (single correct).  
- **MCQ 7–8:** Select **all** that apply (multiple correct).  
- **Subjective:** Prefer valid JavaScript; explain briefly where asked.

---

## Part A — Multiple choice (single correct)

**1.** Which statement best describes **hoisting** for a classic **function declaration** in an outer scope?

- A) It is not hoisted; it behaves like a `const` assignment.  
- B) The binding is hoisted but the body stays unreachable until runtime.  
- C) The function can be called before its declaration line in the same scope.  
- D) Hoisting only applies to arrow functions.  

---

**2.** What does a function return if it finishes without a `return` statement?

- A) `null`  
- B) `0`  
- C) `undefined`  
- D) It throws a `ReferenceError`  

---

**3.** Which arrow function correctly returns the object `{ id: 1 }`?

- A) `(id) => { id: 1 }`  
- B) `(id) => { return { id: 1 } }`  
- C) `(id) => { id: 1, name: "a" }`  
- D) `(id) => id => { id: 1 }`  

---

**4.** Compared with a traditional `function` expression, an **arrow function** created in the same outer scope typically:

- A) Always has its own `this` binding like a method on an object  
- B) Inherits `this` from the surrounding lexical scope  
- C) Cannot capture variables from outer scopes  
- D) Must always use a block body with explicit `return`  

---

**5.** `document.querySelector(".item")` returns:

- A) A `NodeList` of every `.item`  
- B) The **first** matching element, or `null`  
- C) An array of matching elements  
- D) Always the element with class exactly `"item"` even if multiple exist  

---

**6.** You want to skip whitespace-only **text nodes** between elements when walking the tree. Which property pair is most appropriate?

- A) `firstChild` and `nextSibling`  
- B) `firstElementChild` and `nextElementSibling`  
- C) `childNodes` and `parentNode` only  
- D) `children[0]` and `children[1]` only  

---

## Part B — Multiple choice (multiple correct)

**7.** Which of the following are **valid** ways to define a callable function `f` that returns the number `5`? *(Select all that apply.)*

- A) `function f() { return 5; }`  
- B) `const f = () => 5;`  
- C) `const f = function () { return 5; };`  
- D) `const f = () => { 5; };`  

---

**8.** Given a non-null `Element` node `el`, which expressions are generally **safe and appropriate** for **reading visible text content** without parsing HTML? *(Select all that apply.)*

- A) `el.textContent`  
- B) `el.innerHTML` when you only *read* and never assign untrusted data  
- C) `el.outerHTML` for reading a short label inside a span  
- D) `el.innerText`  

---

## Part C — Subjective question

### Problem: `buildStats` + DOM summary

**Part 1 — Pure function**

Write a function `buildStats(numbers)` where `numbers` is an array of **numbers** (may be empty). It must return a **new object** with:

- `count`: how many items in the array  
- `sum`: total sum  
- `average`: arithmetic mean as a number, or `null` if `count === 0` (avoid dividing by zero)  

Use **one** arrow function for `buildStats` (your choice of concise or block body). Do **not** mutate the input array.

**Part 2 — DOM**

Assume HTML:

```html
<section id="dashboard">
  <h2 class="title">Sales</h2>
  <ul class="metrics">
    <li class="metric">100</li>
    <li class="metric">50</li>
    <li class="metric">25</li>
  </ul>
</section>
```

Write a function `readMetricValues()` that:

1. Selects `#dashboard`.  
2. If missing, returns `[]`.  
3. Otherwise selects **all** `.metric` elements **inside** that dashboard only (not elsewhere on the page).  
4. Returns an **array of numbers** by parsing each item’s `textContent` with `Number(...)` or `parseFloat` (either is fine if you state which you chose).  
5. Skips elements that parse to `NaN` (do not include them in the result).

**Part 3 — Short explanation**

In **2–3 sentences**, explain why `root.querySelectorAll(".metric")` is better than `document.querySelectorAll(".metric")` when multiple dashboards might exist on the same page.

---

### Answer key (instructor — remove when sharing with students)

**Single correct:** 1-C, 2-C, 3-B, 4-B, 5-B, 6-B  

**Multiple correct:** 7-A,B,C | 8-A,D  

*Note on 8:* `innerHTML` *read* does not execute scripts, but it returns markup, not “plain text,” and is the wrong tool for labels; `outerHTML` likewise. Accept A and D as best answers for “reading text”; if you discuss `innerHTML` reads in class, clarify your rubric.

**Subjective — sample solution:**

```javascript
const buildStats = (numbers) => {
  const count = numbers.length;
  const sum = numbers.reduce((a, b) => a + b, 0);
  const average = count === 0 ? null : sum / count;
  return { count, sum, average };
};

function readMetricValues() {
  const root = document.querySelector("#dashboard");
  if (!root) return [];

  const values = [];
  for (const li of root.querySelectorAll(".metric")) {
    const n = Number(li.textContent.trim());
    if (!Number.isNaN(n)) values.push(n);
  }
  return values;
}
```

**Explanation sample:** Scoping the query to `root` ensures you only read metrics belonging to this dashboard; a global `document.querySelectorAll` could mix in `.metric` nodes from other sections.

---

**Total MCQ:** 8 (6 single-select + 2 multi-select)  
**Subjective:** 1
