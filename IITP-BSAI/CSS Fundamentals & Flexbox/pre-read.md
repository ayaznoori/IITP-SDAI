# Pre-Read: CSS Fundamentals & Flexbox

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **link CSS to HTML** using internal and external stylesheets
- How to **target elements** with element, class, and id selectors
- How the **box model** works — content, padding, border, and margin
- How to **style text, colors, and backgrounds** on a real page
- How to **build a simple row or column layout** with Flexbox
- How to **spot and fix basic CSS bugs** using browser DevTools

---

## 2. Detailed Explanation

### Why CSS Exists

**HTML** tells the browser *what* is on the page. **CSS** (Cascading Style Sheets — the design language of the web) tells the browser *how it should look*: colors, fonts, spacing, and layout.

**Analogy:** HTML is the floor plan of a room. CSS is the paint, furniture placement, and lighting.

Without CSS, every website would look like a plain document from 1995 — black text, blue links, no spacing. CSS is why modern products feel polished.

### Three Ways to Add CSS

**1. Inline styles** (quick test only — avoid in real projects):

```html
<p style="color: red;">Hello</p>
```

**2. Internal stylesheet** (inside `<head>`):

```html
<head>
  <style>
    p { color: navy; }
  </style>
</head>
```

**3. External stylesheet** (best practice — separate file):

```html
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

Create `styles.css` in the same folder as `index.html`. One CSS file can style many HTML pages.

### Selectors — Who Gets the Style?

A **selector** (the part of CSS that picks which elements to style) tells the browser which elements receive a rule.

| Selector type | Syntax | Targets |
|---------------|--------|---------|
| Element | `p { }` | All `<p>` tags |
| Class | `.card { }` | Elements with `class="card"` |
| ID | `#header { }` | The one element with `id="header"` |

```html
<p class="intro">Welcome</p>
<p>Regular paragraph</p>
```

```css
.intro {
  font-weight: bold;
  color: #2c3e50;
}
```

**Rule of thumb:** Use **classes** for reusable styles. Use **id** sparingly — only one per page. Use **element** selectors for global defaults (e.g. all `body` text).

### Core Properties You Will Use Today

**Color and background:**
```css
h1 {
  color: #1a73e8;
  background-color: #f0f4ff;
}
```

**Typography:**
```css
body {
  font-family: Arial, sans-serif;
  font-size: 16px;
  line-height: 1.5;
}
```

**Borders:**
```css
.card {
  border: 2px solid #ccc;
  border-radius: 8px;
}
```

### The Box Model — The Most Important CSS Idea

Every element on a page is a **box**. The **box model** (how width and height are calculated with spacing layers) has four layers from inside out:

1. **Content** — text or image
2. **Padding** — space inside the border
3. **Border** — edge line around padding
4. **Margin** — space outside the border (between boxes)

```css
.box {
  width: 200px;
  padding: 16px;
  border: 2px solid black;
  margin: 24px;
}
```

**Analogy:** Content is a photo. Padding is the mat inside a frame. Border is the frame. Margin is wall space between frames.

**Why It Matters:** Beginners set `width: 200px` and wonder why the box looks wider than 200px — padding and border add to total size unless you use `box-sizing: border-box` (we will mention this in class).

### Flexbox — One-Dimensional Layout

**Flexbox** (a CSS layout mode for arranging items in a row or column) solves: *"How do I line up these boxes in a row or column and control spacing?"*

Parent container needs:

```css
.container {
  display: flex;
  flex-direction: row;   /* or column */
  justify-content: center; /* main axis alignment */
  align-items: center;     /* cross axis alignment */
  gap: 16px;
}
```

| Property | What it controls |
|----------|------------------|
| `display: flex` | Turns on Flexbox on the parent |
| `flex-direction` | Row (horizontal) or column (vertical) |
| `justify-content` | Spacing along main axis |
| `align-items` | Alignment along cross axis |
| `gap` | Space between flex items |

**Mental model:** Parent is a conveyor belt. Children are packages. You control direction and how packages are spaced.

### Why It Matters

**Real-world hook:** Navigation bars, card rows, centered login forms, and mobile toolbars all use Flexbox daily.

**Benefits:**
- Replace fragile float hacks from old CSS
- Center content vertically and horizontally with few lines
- Build responsive component rows before learning Grid

### Debugging CSS in DevTools

1. Right-click an element → **Inspect**
2. Open **Styles** panel — see which rules apply
3. Toggle checkboxes to disable rules temporarily
4. Use the **box model diagram** in DevTools to see margin/padding

**Common beginner bugs:**
- Typo in class name — HTML says `classs` instead of `class`
- Wrong selector — styles never match
- Forgot to link `styles.css`
- Specificity — another rule overrides yours (mention lightly)

### Messy to Clear Walkthrough

**Messy:** Inline styles on every element, random `<br>` tags for spacing.

**Clear:** External CSS file, classes for repeated patterns, Flexbox for layout.

```css
/* styles.css */
.nav {
  display: flex;
  gap: 12px;
  justify-content: space-between;
}
```

```html
<nav class="nav">
  <a href="#">Home</a>
  <a href="#">About</a>
</nav>
```

### Building Blocks Checklist

- [ ] Link external CSS with `<link rel="stylesheet" href="...">`
- [ ] Write element, class, and id selectors
- [ ] Name the four parts of the box model
- [ ] Set `display: flex` on a parent and align children
- [ ] Open DevTools Styles panel and read applied rules

---

## 3. Practice Exercises

**Exercise 1 — External CSS**
Create `index.html` and `styles.css`. Make all `h1` elements dark blue and all paragraphs gray. Link the stylesheet correctly.

**Exercise 2 — Box model**
Create a `.card` class with padding 20px, border 1px solid #ddd, margin 16px, and a light background. Apply it to a `<div>`.

**Exercise 3 — Flexbox row**
Build a horizontal nav with three links inside a `<nav class="nav">`. Use Flexbox with `gap` and `justify-content: center`.

**Exercise 4 — DevTools**
Intentionally misspell a class in CSS. Find in DevTools why the style does not apply. Fix the typo and confirm.
