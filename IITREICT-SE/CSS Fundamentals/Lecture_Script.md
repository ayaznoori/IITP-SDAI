# Lecture Script: CSS Fundamentals
**Duration:** 110 minutes | **Tools:** VS Code, Browser, DevTools | **Languages:** HTML + CSS

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Unstyled vs styled portfolio |
| Why Does This Matter? | 12 min | UX, credibility, layout |
| What Is the Concept? | 30 min | Selectors, box model, Flexbox |
| How Do We Apply It? (LOs) | 50 min | Style Session 14 HTML |
| Portfolio polish lab | 8 min | Nav + cards + typography |
| Recap & summary | 5 min | Grid/JS preview |

---

## Session Opening (5 min)

**[Script:]** "Open your portfolio HTML from last session. Honest reaction — would you send this link to a recruiter today? Structure is correct. Visual design is missing. CSS is how you communicate professionalism before anyone reads a word."

**Problem hook:** Side-by-side same HTML, +15 lines of CSS — dramatic improvement. "Small CSS investment, large perception change."

🎯 **Instructor Note:** Students must have Session 14 HTML open. Provide starter `styles.css` link only for absentees.

---

## Why Does This Matter?

🎯 **Instructor Note:** "Trust" discussion — banking site styled like 1990s plain HTML vs modern UI.

**[Script:]** "CSS is not optional decoration in product teams. Design systems — spacing scales, color tokens, component classes — are CSS architecture at scale. Today you learn the primitives: select, style, space, align."

**Real-world use:**
- Portfolio differentiation in hiring
- Consistent UI in dashboards and forms
- Flexbox for navbars and toolbars everywhere

**Pain if misunderstood:**
- Box model breaking layouts
- Specificity wars — rules not applying
- Flexbox on wrong element
- Inline style duplication

---

## What Is the Concept?

### Linking CSS

**External:**

```html
<link rel="stylesheet" href="styles.css">
```

**Project structure:**

```
portfolio/
  index.html
  styles.css
  images/
```

**[Script:]** "One CSS file can style multiple HTML pages — DRY for design."

### Selectors

```css
/* element */
body { font-family: system-ui, sans-serif; }

/* class */
.project-card { border: 1px solid #e5e7eb; }

/* id */
#contact { padding: 2rem 0; }
```

**Specificity (simple):** id > class > element. Prefer classes for components.

🎯 **Instructor Note:** Demo typo — `.project-card` vs `.project-crad` — find in DevTools.

### Box Model Deep Dive

```css
.project-card {
  width: 280px;
  padding: 1rem;
  margin: 1rem;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  background: #fff;
}
```

**DevTools:** Hover box model diagram — narrate each layer.

**Optional global fix:**

```css
*, *::before, *::after { box-sizing: border-box; }
```

### Typography, Color, Background

```css
:root {
  --color-primary: #2563eb;
  --color-bg: #f9fafb;
  --color-text: #1f2937;
}

body {
  background: var(--color-bg);
  color: var(--color-text);
  line-height: 1.6;
}

h1, h2, h3 {
  color: #111827;
}
```

**[Script:]** "CSS variables preview — optional sugar; focus on properties if time short."

### Flexbox

**Nav bar pattern:**

```css
.site-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background: #111827;
  color: white;
}

.site-nav {
  display: flex;
  gap: 1.5rem;
}

.site-nav a {
  color: white;
  text-decoration: none;
}
```

**Card row:**

```css
.project-list {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}
```

**Mental model:** Parent = flex container; children = flex items.

---

## How Do We Apply It?

### LO 1: Apply CSS with internal and external stylesheets

**Problem:** Quick header color test vs project-wide theme.

**Internal (demo only):**

```html
<style>
  h1 { color: navy; }
</style>
```

**External (keep):**

```css
/* styles.css */
h1 { color: var(--color-primary, #2563eb); }
```

**Predict:** Which approach scales to 10 pages?

**Explain result:** External — single source of truth.

---

### LO 2: Use element, class, and id selectors

**Problem:** Style all paragraphs, project cards reusable, contact section unique spacing.

```css
p { margin-bottom: 1rem; }

.project-card { padding: 1rem; }

#contact { border-top: 2px solid #e5e7eb; margin-top: 2rem; }
```

**HTML:**

```html
<article class="project-card">
  <h3>Text Utility</h3>
  <p>Python file tool.</p>
</article>
```

**Predict:** Which rule styles project title `h3` — element or class?

**Explain result:** Element `h3` unless overridden; card padding from class on `article`.

---

### LO 3: Explain and implement box model; introduce Flexbox

**Problem:** Cards stuck to edges; nav links cramped.

**Box model fix:**

```css
main {
  max-width: 960px;
  margin: 0 auto;
  padding: 2rem 1rem;
}
```

**Flexbox fix:**

```css
.site-nav {
  display: flex;
  gap: 1.5rem;
}
```

**Predict:** Does `gap` work without `display: flex`?

**Explain result:** No — Flexbox must be enabled on parent.

🎯 **Instructor Note:** Students measure padding in DevTools — change live, observe content area.

---

### LO 4: Style typography, colour, and background on complete page

**Problem:** Portfolio looks like default browser stylesheet.

**Full-page pass (checklist on slide):**
- [ ] `body` font and background
- [ ] Heading colors and spacing
- [ ] Link styles (`a { color: ... }`)
- [ ] `footer` subtle background
- [ ] Form inputs readable width

```css
input, textarea {
  display: block;
  width: 100%;
  max-width: 400px;
  padding: 0.5rem;
  margin-bottom: 1rem;
}
```

**Predict:** Why `display: block` on inputs?

**Explain result:** Stack labels above fields — clearer form layout before Grid.

---

### LO 5: Debug CSS with DevTools

**Problem:** Nav links stay blue and underlined despite "CSS not working."

**Investigation:**
1. Inspect `<a>` in nav
2. See rule not applied — wrong selector `.nav a` vs `.site-nav a`
3. Fix selector
4. Confirm white text, no underline

**Second bug:** `margin` collapse confusion — demonstrate with DevTools box model.

**[Script:]** "Strikethrough rule in DevTools means overridden — read specificity, do not guess."

---

## Portfolio Polish Lab (8 min)

Minimum polish requirements:
1. External `styles.css` linked
2. Flexbox header/nav
3. At least two `.project-card` with box model spacing
4. Cohesive color palette (2–3 colors max)
5. One bug fixed via DevTools

---

## Recap (5 min)

---

## Lecture Summary

- **External CSS** keeps presentation separate and maintainable
- **Selectors** target elements at global, reusable, or unique levels
- **Box model** explains spacing and sizing behavior
- **Flexbox** aligns navigation and card rows efficiently
- **Typography and color** establish professional visual hierarchy
- **DevTools** reveal applied rules and box model debugging
- **Practical value:** CSS turns structured HTML into a portfolio you are proud to share

**[Script:]** "Coming up: CSS Grid, JavaScript in the browser, APIs — your stack grows. Commit today's portfolio to Git on a `style-portfolio` branch. You know what to do."
