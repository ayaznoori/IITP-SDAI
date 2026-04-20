# Event Handling & DOM Manipulation — Lecture Script (2.5 hours)

**Total duration:** 2 hours 30 minutes (150 minutes)  
**Prerequisites:** Session 1–2 (JS basics, functions, DOM selection/traversal).  
**Materials:** `index.html` with buttons, form, list; editor; DevTools.

---

## Schedule overview

| Segment | Topic | Time |
|--------|--------|------|
| 1 | Opening, objectives, recap “static DOM → reactive DOM” | 8 min |
| 2 | Events model; listeners; common event types; phases (brief) | 20 min |
| 3 | Inline handlers vs `addEventListener`; removing listeners (awareness) | 10 min |
| 4 | `click`, `input`, `submit`; `preventDefault`; `event` object essentials | 28 min |
| 5 | Interactive button exercises (counter, toggle, delegated list) | 12 min |
| 6 | Creating elements; `textContent`; `append` / `appendChild` | 20 min |
| 7 | Modifying content; attributes; `classList`; `innerHTML` cautions | 18 min |
| 8 | Removing and cloning nodes | 10 min |
| 9 | Basic client-side form validation (pattern, live errors, submit gate) | 14 min |
| 10 | Recap, DevTools **Event Listeners** panel teaser, Q&A, buffer | 10 min |
| **Total** | | **150 min** |

---

## Segment 1 — Opening (8 min)

**Say / do:**

- Outcomes: register listeners; handle click/input/submit; use `event.target` / `preventDefault`; create/append/modify/remove/clone nodes; validate a simple form in JS.  
- Bridge: “Session 2 you *read* the tree; today you *react* and *reshape* it.”

---

## Segment 2 — Events: model, listeners, types (20 min)

**Subtopics (internal):**

| Subtopic | ~Time |
|----------|--------|
| Events as messages; who dispatches (user, browser, your code) | 6 min |
| `addEventListener(type, handler, options?)` | 8 min |
| Survey: `click`, `input`, `change`, `submit`, `keydown` (mention) | 6 min |

**Say / do:**

- **Bubble vs capture:** one diagram; default is bubble — deep dive later.  
- Handler is a **function** — arrow vs `function` only if `this` matters (link Session 2).

**Micro-check:** name three events you expect on a search box vs a form.

---

## Segment 3 — Inline vs `addEventListener` (10 min)

**Say / do:**

- Inline: `onclick="..."` — works, but mixes concerns; CSP/strategy in real apps.  
- `addEventListener`: multiple handlers, clearer testing, separation of HTML/JS.  
- Mention `removeEventListener` needs **same function reference**.

---

## Segment 4 — `click`, `input`, `submit` + event object (28 min)

**Subtopics:**

| Subtopic | ~Time |
|----------|--------|
| `event.type`, `event.target`, `event.currentTarget` | 10 min |
| `click` demo: button vs delegated parent | 6 min |
| `input` / `change` difference (high level) | 6 min |
| `submit` on form; **`preventDefault()`** | 6 min |

**Say / do:**

- Live: log `target` vs `currentTarget` on nested markup.  
- Form: show navigation/reload without `preventDefault` when action/method default applies.

**Micro-check:** “Which property identifies the element that originated the event?”

---

## Segment 5 — Interactive button exercises (12 min)

**Ideas (pick 2 fast or assign 1 as homework):**

1. **Counter:** + / − buttons update a `<span>`.  
2. **Theme toggle:** flip a class on `<body>`.  
3. **Delegation:** one listener on `<ul>`; `closest("button")` or tag check.

**Say / do:**

- Emphasize **guards** if `target` is not the expected element.

---

## Segment 6 — Creating elements & appending (20 min)

**Say / do:**

- `document.createElement("li")`  
- Set `textContent` or build children  
- `parent.append(node)` (modern) vs `appendChild`  
- **`DocumentFragment`** mention if building many rows (optional 2 min)

**Micro-check:** build one `<article>` with heading + paragraph from data object.

---

## Segment 7 — Modifying content & classes (18 min)

**Say / do:**

- `textContent` vs `innerHTML` — XSS when **assigning** user strings to `innerHTML`.  
- Attributes: `setAttribute`, `id`, `dataset` (data-* attributes)  
- `classList.add/remove/toggle/contains`

---

## Segment 8 — Remove & clone (10 min)

**Say / do:**

- `element.remove()`  
- `parent.removeChild` (legacy awareness)  
- `cloneNode(true)` deep clone — event listeners **not** copied (important)

---

## Segment 9 — Basic form validation (14 min)

**Say / do:**

- HTML5 attributes: `required`, `minlength`, `pattern` (awareness)  
- JS: on `submit`, read fields, show `<p class="error">`, focus first invalid  
- `input` for live feedback (e.g. password length)  
- **Message:** client validation is UX; server must still validate

---

## Segment 10 — Recap & buffer (10 min)

**Takeaways:**

1. Prefer `addEventListener`; know when `preventDefault` is mandatory.  
2. `target` vs `currentTarget` resolves “who fired?” vs “who is listening?”  
3. Prefer `createElement` + safe text APIs over HTML string soup for user data.  
4. `cloneNode` duplicates structure, not listeners.

**Buffer:** trim delegation deep-dive or `DocumentFragment` if over time.

---

## Instructor checklist

- [ ] Inline vs listener side-by-side  
- [ ] `submit` + `preventDefault` demo  
- [ ] Log `event.target` on delegated click  
- [ ] `createElement` + `append` live  
- [ ] One `classList.toggle` interaction  
- [ ] `remove()` and `cloneNode(true)` with listener caveat  
- [ ] Form: block invalid submit, show error message  

---

**End of lecture script.**
