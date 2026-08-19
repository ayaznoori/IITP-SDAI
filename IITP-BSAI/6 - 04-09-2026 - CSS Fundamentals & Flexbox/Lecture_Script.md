# Lecture Script: CSS Fundamentals & Flexbox
**Duration:** 110 minutes | **Tools:** VS Code, Browser, DevTools | **Languages:** HTML + CSS

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | From structure to style |
| Why Does This Matter? | 12 min | UX, branding, layout |
| What Is the Concept? | 28 min | Selectors, box model, Flexbox |
| How Do We Apply It? (LOs) | 45 min | Style last session's HTML |
| Live styling lab | 12 min | Nav + card row with Flexbox |
| Recap & summary | 8 min | LO review + Grid preview |

---

## Session Opening (5 min)

**[Script:]** "Last session you built the skeleton — semantic HTML, forms, DevTools. Open that page in your browser. It looks plain. Good. That means your structure is ready for CSS. Today we make it look intentional: color, spacing, typography, and your first real layout tool — Flexbox."

**Problem hook:** Show two versions of the same HTML — unstyled vs styled. "Same bones. Different CSS. This is how products express brand and usability."

🎯 **Instructor Note:** Students should open their Session 5 `index.html` or a provided starter file. No one starts from scratch unless they missed last class.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "Would you trust a bank website that looked like plain Times New Roman on white?" Discuss trust and UX in 60 seconds.

**[Script:]** "Users judge credibility in seconds. CSS is not decoration for decoration's sake — it communicates hierarchy, guides the eye, and makes interfaces usable. Flexbox alone powers nav bars, toolbars, and card lists on almost every modern site."

**Real-world use:**
- Design systems and component libraries (spacing, colors, typography tokens)
- Responsive navigation and marketing landing pages
- Form layout and button alignment

**Pain if misunderstood:**
- Box model surprises — layouts 'break' by a few pixels
- Wrong selectors — styles silently do not apply
- Flexbox on wrong element — applying `display: flex` on child instead of parent
- Inline style soup — unmaintainable projects

---

## What Is the Concept?

### Linking CSS: Internal vs External

**Definition:** CSS rules consist of a selector and declaration block: `selector { property: value; }`

**Mental model:** External CSS is a shared theme file; internal is a one-page exception.

**Demo structure:**

```
project/
  index.html
  styles.css
```

```html
<link rel="stylesheet" href="styles.css">
```

🎯 **Instructor Note:** Intentionally break `href` path — show unstyled page — fix together. Common student error.

**Common mistakes:**
- `href="style.css"` when file is `styles.css`
- CSS inside `<body>` instead of `<head>` (still works but messy)
- Missing `rel="stylesheet"`

### Selectors: Element, Class, ID

| Selector | Example | Specificity (simple) |
|----------|---------|----------------------|
| Element | `p { }` | Low — broad |
| Class | `.btn { }` | Medium — reusable |
| ID | `#hero { }` | High — one element |

**[Script:]** "Prefer classes for components: `.card`, `.btn-primary`, `.nav-link`. IDs are for unique anchors and JS hooks — not for every style."

**Comparison:** Python has no direct equivalent — closest is naming conventions in templates.

**Common mistakes:**
- `.classname` in CSS but `class="classname "` with typo
- Using id for repeated buttons — invalid HTML pattern

### Typography, Color, Background, Border

**Walkthrough properties:**

```css
body {
  font-family: system-ui, sans-serif;
  font-size: 16px;
  line-height: 1.6;
  color: #222;
  background-color: #fafafa;
}

h1 {
  color: #1a56db;
  border-bottom: 3px solid #1a56db;
  padding-bottom: 8px;
}
```

🎯 **Instructor Note:** Introduce hex colors briefly. Mention named colors (`navy`) for demos only.

### The Box Model

**Definition:** Every element is a rectangular box: content → padding → border → margin.

**Mental model:** Mat and frame around a picture.

**Live DevTools demo:** Select element → Layout/Computed panel → hover box model diagram.

```css
.demo-box {
  width: 200px;
  padding: 20px;
  border: 4px solid #333;
  margin: 16px;
  background: #e8f4ff;
}
```

**[Script:]** "Total horizontal space = width + padding-left + padding-right + border-left + border-right + margin-left + margin-right. This is why beginners say 'CSS is broken' — the math is just new."

**Optional mention:**

```css
* { box-sizing: border-box; }
```

"Padding and border count inside the width you set — industry norm."

### Flexbox Basics

**Definition:** Flexbox is a one-dimensional layout method for distributing space along a row or column.

**Parent (flex container) properties today:**
- `display: flex`
- `flex-direction: row | column`
- `justify-content` — main axis
- `align-items` — cross axis
- `gap`

**Mental model:** Parent is the manager; children are team members in a line.

```css
.nav {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  padding: 12px 24px;
  background: #1a56db;
}
.nav a {
  color: white;
  text-decoration: none;
}
```

🎯 **Instructor Note:** Draw main axis and cross axis on board for `row` vs `column`.

**Common mistakes:**
- `display: flex` on `<a>` tags instead of `<nav>`
- Expecting `justify-content` to work without `display: flex` on parent
- Confusing `align-items` with `justify-content`

---

## How Do We Apply It?

### LO 1: Apply CSS using internal and external stylesheets

**Problem:** Style a page without mixing presentation into every HTML tag.

**Translate logic:** Create `styles.css` → link in `<head>` → move rules out of HTML.

**Write code:**

`index.html` head snippet:

```html
<link rel="stylesheet" href="styles.css">
```

`styles.css`:

```css
body { margin: 0; font-family: sans-serif; }
```

**Predict before running:** What changes compared to unstyled page?

**Explain result:** Zero default body margin, sans-serif font — global baseline.

**Second demo — internal for quick test only:**

```html
<style>
  h1 { color: crimson; }
</style>
```

**[Script:]** "Internal is fine for a 5-minute experiment. External is how we work on real projects."

---

### LO 2: Use element, class, and id selectors with core properties

**Problem:** Style headings globally, buttons as a reusable class, hero section uniquely.

**Write code:**

```css
h2 { font-size: 1.5rem; }

.btn {
  padding: 10px 20px;
  background: #1a56db;
  color: white;
  border: none;
  border-radius: 6px;
}

#hero {
  background: #f0f4ff;
  padding: 48px;
  text-align: center;
}
```

```html
<section id="hero">
  <h2>Welcome</h2>
  <button class="btn">Get Started</button>
</section>
```

**Predict:** Which rule styles the button — element, class, or id?

**Explain result:** `.btn` class styles the button; `#hero` styles the section wrapper.

🎯 **Instructor Note:** Add second `.btn` elsewhere — show class reusability.

---

### LO 3: Explain and apply the CSS box model

**Problem:** Three cards should have breathing room but sit in a row without overlapping.

**Translate logic:** Margin separates cards; padding separates text from card border; border defines edge.

**Write code:**

```css
.card {
  width: 180px;
  padding: 16px;
  margin: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
}
```

**Predict before running:** If three cards have `margin: 12px`, what creates space between them?

**Explain result:** Margins collapse/adjacent spacing — visual gap between boxes. Inspect in DevTools.

**Interactive:** Students change `padding` live — content area shrinks/grows inside border.

---

### LO 4: Build one simple row or column layout with Flexbox

**Problem:** Horizontal nav with logo left, links right.

**Write code:**

```html
<nav class="navbar">
  <span class="logo">Masai</span>
  <div class="links">
    <a href="#">Home</a>
    <a href="#">Courses</a>
  </div>
</nav>
```

```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 24px;
  background: #111;
  color: white;
}
.links {
  display: flex;
  gap: 16px;
}
```

**Predict:** Where do links sit — left or right?

**Explain result:** `space-between` pushes logo and link group to opposite ends.

**Column variant demo:**

```css
.stack {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
```

**Predict:** How does layout change vs row?

---

### LO 5: Debug basic CSS issues in DevTools

**Problem:** `.title` rule does not apply — find why.

**Scenario (intentional bug):**

```html
<h1 class="titel">Hello</h1>
```

```css
.title { color: green; }
```

**Walkthrough:**
1. Inspect `h1` — class is `titel`
2. Styles panel — `.title` rule not applied
3. Fix HTML class to `title`
4. Confirm green color

🎯 **Instructor Note:** Second bug — rule overridden by more specific selector. Show strikethrough in DevTools.

**[Script:]** "DevTools is not cheating. It is how every frontend developer works daily."

---

## Live Styling Lab (12 min)

**Task:** Take Session 5 HTML portfolio structure:

1. External `styles.css` linked
2. Global `body` typography and background
3. Flexbox `nav` with `gap` and horizontal layout
4. `.card` class with full box model on project articles
5. One intentional bug for partner to fix in DevTools

**[Script:]** "Polish, do not rebuild HTML. Structure stays; presentation arrives."

---

## Recap (8 min)

🎯 **Instructor Note:** Four questions:
1. Three ways to add CSS — which is best for projects?
2. Name box model layers outside-in
3. Which element gets `display: flex` — parent or child?
4. How do you see why a rule is not applied?

---

## Lecture Summary

- **External stylesheets** keep HTML clean and styles reusable across pages
- **Element, class, and id selectors** target elements at different levels of specificity
- **The box model** (content, padding, border, margin) explains spacing and sizing behavior
- **Flexbox** lays out children in a row or column with alignment and gap control
- **DevTools** reveals applied rules, box model, and typos in selectors or class names
- **Practical value:** CSS + Flexbox is how you turn wireframes into interfaces users trust and enjoy

**[Script:]** "Next session: CSS Grid and responsive design — full page layouts and mobile-friendly breakpoints. Keep today's files; we will extend them."
