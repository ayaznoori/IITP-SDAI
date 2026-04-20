# JavaScript Functions & DOM Fundamentals — Lecture Script (2.5 hours)

**Total duration:** 2 hours 30 minutes (150 minutes)  
**Prerequisites:** Python module; JS basics (variables, types, control flow); basic HTML.  
**Materials:** Browser + DevTools, editor, simple `index.html` with nested structure for live demos.

---

## Schedule overview

| Segment | Topic | Time |
|--------|--------|------|
| 1 | Opening, objectives, Python `def` → JS functions | 10 min |
| 2 | Function declarations vs expressions; hoisting (awareness) | 25 min |
| 3 | Parameters, arguments, return values; early `return` | 20 min |
| 4 | Arrow functions; concise body; comparison with `function` (`this` preview) | 30 min |
| 5 | DOM intro, `document`; `getElementById`, `querySelector`, `querySelectorAll` | 28 min |
| 6 | Traversing DOM; reading `textContent` / `innerHTML`; live mini-lab | 27 min |
| 7 | Inspecting DOM in DevTools (Elements, picker, structure) | 10 min |
| 8 | Recap, practice pointers, Q&A, buffer | 10 min |
| **Total** | | **150 min** |

---

## Segment 1 — Opening, objectives, Python bridge (10 min)

**Say / do:**

- Outcomes: define functions three ways; choose arrow vs `function` with basic criteria; select and traverse DOM nodes; read element text safely; use DevTools to verify structure.  
- **Python map:** `def foo(a, b): return a + b` → JS declaration/expression/arrow with same shape.  
- Emphasize: DOM is **not** a string — it’s **objects** in memory linked like a tree.

---

## Segment 2 — Declarations vs expressions (25 min)

**Subtopics (internal timing):**

| Subtopic | ~Time |
|----------|--------|
| Function declaration syntax; naming; calling | 8 min |
| Function expression; anonymous vs named (brief) | 7 min |
| Hoisting: declarations lifted, `const` bindings not | 10 min |

**Say / do:**

- Live: declaration called **before** line in file — works.  
- Expression with `const` called before assignment — **ReferenceError**.  
- **Teaching rule:** prefer **one style per file** for consistency; many teams default to `const fn = () => {}` for short helpers.

**Micro-check:** “Fix the order” slide.

---

## Segment 3 — Parameters and return values (20 min)

**Subtopics:**

| Subtopic | ~Time |
|----------|--------|
| Parameters vs arguments; arity | 5 min |
| `return`; functions without `return` → `undefined` | 6 min |
| Extra args ignored (awareness); default parameters (demo1–2) | 5 min |
| Early `return` for guard clauses | 4 min |

**Say / do:**

- Contrast Python: fewer/more args errors vs JS permissiveness.  
- Show `undefined` from missing return — common student bug.

**Micro-check:** write `clamp(x, min, max)` on board.

---

## Segment 4 — Arrow functions and comparison (30 min)

**Subtopics:**

| Subtopic | ~Time |
|----------|--------|
| Syntax variants: no params, one param, block body, concise body | 10 min |
| Returning objects: wrap in `()` | 5 min |
| **No** own `this`, `arguments` (awareness — not deep) | 8 min |
| When to prefer `function` (methods, constructors) vs arrows (callbacks) — heuristic | 7 min |

**Say / do:**

- Live: `map` with arrow (if arrays covered) or simple `nums.forEach(n => ...)` — keep dependency light; alternatively pure `const sq = x => x * x`.  
- **Do not** over-teach `this` — one clear sentence: “arrow inherits `this` from surrounding scope; `function` has its own `this`.”

**Micro-check:** “Convert to concise arrow” exercise.

---

## Segment 5 — DOM intro and selecting elements (28 min)

**Subtopics:**

| Subtopic | ~Time |
|----------|--------|
| DOM as tree; nodes vs elements | 6 min |
| `document` global; connection to loaded HTML | 5 min |
| `getElementById` | 4 min |
| `querySelector` (first match), CSS selectors recap | 8 min |
| `querySelectorAll` → **NodeList**; iterate with `for...of` | 5 min |

**Say / do:**

- Live HTML with ids/classes/nesting; demo wrong selector → `null` / empty.  
- **Security note (brief):** prefer `.textContent` over `innerHTML` when setting user text later.

**Micro-check:** “Write the selector for the second `.card`” — discuss limitation; introduce idea of `querySelectorAll()[1]` cautiously.

---

## Segment 6 — Traversal and reading content (27 min)

**Subtopics:**

| Subtopic | ~Time |
|----------|--------|
| Parent/child/sibling: `parentElement`, `children`, `firstElementChild`, `nextElementSibling` | 12 min |
| `textContent` vs `innerHTML` vs `innerText` (high level) | 10 min |
| Mini-lab: from a clicked element reference (or preset `const el`), walk to title | 5 min |

**Say / do:**

- Show **whitespace text nodes** problem — why `*Element*` properties help.  
- Read-only focus today; optional one-line “we’ll mutate in next lesson” if needed.

**Micro-check:** given markup diagram, trace `h2` → `article` → next sibling.

---

## Segment 7 — DevTools inspection (10 min)

**Say / do:**

- Open **Elements** (Inspector); expand tree; highlight on hover.  
- **Select element** tool (picker); show matching selector in panel.  
- **Console** cross-link: `$0` previous selection (Chrome) — optional shortcut.  
- **Styles** tab: not today’s depth — “structure first.”

---

## Segment 8 — Recap and buffer (10 min)

**Takeaways:**

1. Three function forms — know hoisting and **`this`** heuristic.  
2. DOM = tree; selection is **CSS-shaped**.  
3. Traversal + `textContent` = reading what users see without regex on HTML strings.  
4. DevTools **prove** your assumptions.

**Buffer:** trim default parameters or named-expression edge case if over time.

---

## Instructor checklist

- [ ] Hoisting demo: declaration vs `const` expression  
- [ ] One arrow concise vs block body example  
- [ ] `querySelector` vs `querySelectorAll` return types  
- [ ] One traversal from child to parent to sibling  
- [ ] `textContent` read on nested element  
- [ ] DevTools: pick element, show in tree

---

**End of lecture script.**
