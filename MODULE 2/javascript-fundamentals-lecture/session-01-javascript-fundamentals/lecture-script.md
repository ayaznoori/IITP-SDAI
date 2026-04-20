# JavaScript Fundamentals — Lecture Script (2.5 hours)

**Total duration:** 2 hours 30 minutes (150 minutes)  
**Assumptions:** Students completed Python; map concepts (lists → arrays, dicts → objects, etc.).  
**Materials:** Browser with DevTools, code editor, projector; optional simple static HTML/JS files.

---

## Schedule overview


| Segment   | Topic                                                                       | Time        |
| --------- | --------------------------------------------------------------------------- | ----------- |
| 1         | Opening, objectives, Python → JS bridge                                     | 10 min      |
| 2         | What is JS; HTML + execution model; attach scripts; `console.log`; comments | 25 min      |
| 3         | Variables (`let`, `const`, `var`), types, scope (global & block)            | 30 min      |
| 4         | Operators; conditionals; loops                                              | 35 min      |
| 5         | Strings; arrays; objects — practical mini-examples                          | 40 min      |
| 6         | Recap, practice pointers, Q&A, buffer                                       | 10 min      |
| **Total** |                                                                             | **150 min** |


---

## Segment 1 — Opening, objectives, Python → JS bridge (10 min)

**Say / do:**

- Welcome; safety and participation norms (questions anytime).  
- **Objectives by end of session:** Attach JS to HTML, run in browser, use console for debugging, declare variables with correct scope, use conditionals and loops, manipulate strings/arrays/objects in small examples.  
- **Bridge from Python:**  
  - Python: interpreter, `.py` files, REPL.  
  - JS in browser: **engine inside browser**, tied to **HTML**; no separate “run” step beyond loading the page (for simple scripts).
- Quick poll: “Who has opened Inspect / DevTools before?”

**Instructor note:** Keep energy practical — “today you make a page *do* something.”

---

## Segment 2 — Introduction; HTML attachment; console; comments (25 min)

**Subtopics and timing (internal):**


| Subtopic                                                          | ~Time |
| ----------------------------------------------------------------- | ----- |
| What JS is / isn’t (vs Java); where it runs                       | 5 min |
| `<script>` inline vs external `src=`; load order; `defer` mention | 8 min |
| DevTools → Console; `console.log`, `console.error` teaser         | 7 min |
| Comments: `//` and `/* */`                                        | 5 min |


**Say / do:**

- Live demo: minimal `index.html` + `main.js` with `console.log("Hello")`.  
- Show **wrong path** on purpose → Network/console error; fix it.  
- **Why console.log:** first tool for “what is my program thinking?”  
- **Comments:** document intent; don’t narrate obvious code.

**Micro-check:** “Change the message; reload; find the log.”

---

## Segment 3 — Variables, types, scope (30 min)

**Subtopics and timing:**


| Subtopic                                                                    | ~Time  |
| --------------------------------------------------------------------------- | ------ |
| `let` vs `const` vs `var` — modern default: `const`, need rebinding → `let` | 10 min |
| Dynamic typing; primitives vs objects (high level)                          | 6 min  |
| `typeof` teaser; coercion mention (avoid deep rabbit hole)                  | 4 min  |
| Scope: global, block `{ }`, `let`/`const` vs `var`                          | 10 min |


**Say / do:**

- **Python parallel:** assignment, rebinding, “names” vs “values.”  
- `**const`:** binding is fixed; **object/array contents** can still mutate (preview for Segment 5).  
- Live: shadowing with blocks; contrast `var` in loop (classic footgun) if time — at least **one** clear example.

**Micro-check:** “Predict the output” slide — two blocks with same variable name.

---

## Segment 4 — Operators; conditionals; loops (35 min)

**Subtopics and timing:**


| Subtopic                                             | ~Time  |
| ---------------------------------------------------- | ------ |
| Arithmetic; comparison; `===` vs `==` (prefer `===`) | 8 min  |
| Logical `&&`, `||`, `!`; truthy/falsy (short list)   | 7 min  |
| `if` / `else if` / `else`; ternary (optional, brief) | 8 min  |
| `for`, `while`; `for...of` for iterables (arrays)    | 12 min |


**Say / do:**

- **Python parallels:** `elif` → `else if`; `and`/`or`/`not` → `&&`/`\|\|`/`!`.  
- Emphasize `**===`**: no implicit type coercion.  
- **Truthy/falsy:** `0`, `""`, `null`, `undefined`, `NaN`, `false` — don’t memorize exhaustively; know they exist.  
- Loop demo: print/index small array; **off-by-one** caution.

**Micro-check:** Fizz-style or “sum1..n” on board — students trace.

---

## Segment 5 — Strings, arrays, objects (40 min)

**Subtopics and timing:**


| Subtopic                                                                                               | ~Time  |
| ------------------------------------------------------------------------------------------------------ | ------ |
| Strings: literals, `.length`, slice-like methods (`slice`, `substring` mention), template literals ``` | 12 min |
| Arrays: literal `[]`, `push`/`pop`, `length`, index access, `for...of`                                 | 14 min |
| Objects: `{ key: value }`, dot vs bracket, nested objects, array of objects                            | 14 min |


**Say / do:**

- **Python mapping:** `str` / f-strings → template literals; `list` → array; `dict` → object (keys are strings or symbols in practice — stay with string keys today).  
- **Mini practical example:** “Student roster” — array of objects; filter or print one field in loop.  
- **Common bug:** mutating shared object references (light touch).

**Micro-check:** “Add a new property to one student; log the full roster.”

---

## Segment 6 — Recap, Q&A, buffer (10 min)

**Say / do:**

- **Three takeaways:** (1) JS runs in the browser with HTML as host. (2) Prefer `const`/`let` and block scope. (3) Arrays + objects are your daily data bread.  
- Point to detailed notes + problem set.  
- **Buffer:** if running over, trim ternary + deep `var` history; if under, live-code one small interactive button (optional teaser for next time).

---

## Instructor checklist (print-friendly)

- HTML + external JS loads without path errors  
- Console open; students replicate `console.log`  
- `let`/`const`/`var` contrast **one** memorable example  
- `===` shown in an example where `==` would surprise  
- One loop + one `if` on real mini-data  
- One array-of-objects example tied to a realistic label (“users”, “products”, “students”)

---

**End of lecture script.**