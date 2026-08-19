# Lecture Script: JavaScript Fundamentals I
**Duration:** 110 minutes | **Tools:** VS Code, Browser Console | **Project:** Portfolio + app.js

---

## Session Timeline | Opening (8 min)

**[Script:]** "Your portfolio looks professional with HTML, CSS, and Grid. Still static. **JavaScript** makes it respond. Every 'Add to Cart', every form check, every animation — JS. Today you move from Python-on-server thinking to **JS-in-browser** thinking."

> **In the Real World:** **Ola** fare estimates, **BookMyShow** seat selection — browser JS before API calls. You learn the same language.

---

## Why Does This Matter? (14 min)

| Without JS | With JS |
|------------|---------|
| Static brochure site | Interactive product |
| Server round-trip for every check | Instant validation |
| Python skills only backend | Full-stack path open |

🎯 **Instructor Note:** Demo broken form — no JS validation — submit empty email. Then add 5 lines JS — instant fix.

---

## How Do We Apply It? (LOs)

### LO 1: Attach JS and use console

```html
<script src="app.js"></script>
```

```javascript
console.log("Portfolio JS loaded");
console.table({ page: "home", version: 1 });
```

**Real-world:** Production sites use console + monitoring tools (Sentry preview).

---

### LO 2: let, const, primitives

```javascript
const AUTHOR = "Your Name";
let visitorCount = 0;
```

**Case study:** E-commerce `const TAX_RATE = 0.18` — never reassigned.

---

### LO 3: Conditionals and loops

**Scenario:** Highlight nav link for current page section using `if` + `window.location.hash`.

```javascript
for (let i = 0; i < skills.length; i++) {
  console.log(skills[i]);
}
```

---

### LO 4: Strings, arrays, objects

```javascript
const skills = ["HTML", "CSS", "Git"];
const profile = { name: "Asha", role: "Learner" };
```

**API preview:** `fetch` returns JSON objects later — same shape.

---

### LO 5: var vs let vs const

**Live bug demo:** `var` in loop + setTimeout — classic interview question. Show why `let` fixes it (brief).

**Rule:** `const` default, `let` when reassigning, avoid `var`.

---

## Live Lab

Add `app.js` to portfolio: log profile object, loop skills to console, `if` check for empty `title` in form on submit (preventDefault preview).

---

## Lecture Summary

- Browser JS + console are your frontend debugger
- `const`/`let` replace legacy `var`
- Control flow mirrors Python — syntax differs
- Objects/arrays model real API data
- **Practical value:** JavaScript is the language of the interactive web — session 1 of becoming frontend-capable
