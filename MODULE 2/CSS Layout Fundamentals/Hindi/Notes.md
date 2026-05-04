# CSS Basics: Applying Styles, Selectors & the Box Model — Student Study Notes

*Build confidence with where CSS lives, how you target elements, and how every element occupies space on the page.*

---

## Why Learn This?

CSS is how you turn **structure** (HTML) into a **readable, branded, usable** interface. This module helps you:

- **Choose the right place** to put styles (inline vs internal vs external) so your code stays maintainable.  
- **Target elements precisely** with selectors instead of duplicating markup hacks.  
- **Predict layout** using the **box model** so spacing and size behave the way you expect.  
- **Work faster with others** — teams expect shared stylesheets, class-based patterns, and clear naming on containers like `<div>`.

Whether you ship **marketing pages**, **internal tools**, or **product UIs**, these fundamentals prevent “mystery spacing” bugs and endless trial-and-error in the browser.

---

## Core Concepts

### Three ways to apply CSS

| Method | Where it lives | Typical use |
|--------|----------------|-------------|
| **Inline** | `style` attribute on a single element | Quick experiments, rare one-offs, some email HTML |
| **Internal** | `<style>` block inside `<head>` | Single-page prototypes, small demos |
| **External** | `.css` file linked with `<link>` | **Best practice** for real sites — reuse, caching, teamwork |

**Inline example:**

```html
<p style="color: red;">This is inline CSS</p>
```

**Internal example:**

```html
<head>
  <style>
    p { color: blue; }
  </style>
</head>
```

**External example:**

```html
<head>
  <link rel="stylesheet" href="style.css">
</head>
```

```css
/* style.css */
p {
  color: brown;
  font-size: 10px;
}
```

**Why external wins for projects:** one file can style **many** HTML pages; browsers can **cache** the file; designers and developers can collaborate without touching every tag.

---

### CSS selectors: tag, ID, and class

**1. Tag (type) selector** — every element of that HTML type.

```css
p {
  color: blue;
}
```

Affects **all** `<p>` elements (unless a more specific rule overrides).

**2. ID selector** — one element per page (by convention and HTML rules). Use **`#`**.

```css
#id1 {
  color: black;
  font-size: 20px;
}
```

```html
<p id="id1">Unique paragraph</p>
```

**3. Class selector** — one or many elements sharing a label. Use **`.`**.

```css
.featured {
  color: green;
  font-size: 100px;
}
```

```html
<p class="featured">Big green text</p>
```

**When to use what:**

- **Class** — reusable styling (cards, buttons, “featured” rows).  
- **ID** — unique hooks (often for labels, deep links, or JS targets); **don’t** rely on IDs for every cosmetic style.  
- **Tag** — base typography and resets for all headings/body text.

---

### Which styles “win”? (Introductory rule from class)

When teaching **where** styles come from, a simple ordering to remember is:

**Inline → Internal → External** (inline tends to override linked rules when conflicts are naïve).

> **Note for deeper study:** In professional CSS, **specificity** (IDs vs classes vs tags) and **source order** matter too. You’ll refine this when you learn the cascade in full. For now, connect “inline on the element is very strong” with “external file is the maintainable default.”

---

### The box model

Every element is drawn as a **rectangle** built from the inside out:

```text
Margin  →  Border  →  Padding  →  Content
```

| Layer | Role |
|--------|------|
| **Content** | Text, images, or nested elements. |
| **Padding** | Space **inside** the border, around the content. |
| **Border** | Visible edge around padding + content. |
| **Margin** | Space **outside** the border; separates this box from neighbors. |

**Properties you’ll use constantly:** `width`, `height`, `padding`, `border`, `margin`.

**Example:**

```css
.card {
  width: 300px;
  padding: 24px;
  border: 5px solid black;
  margin: 10px;
}
```

**Sizing intuition (default `box-sizing: content-box`):**  
The **`width`** sets the **content** width; **padding** and **border** add to the **visible** width you see on screen. **Margin** adds space **outside** (and can collapse with adjacent margins in some cases — a topic for later).

---

### HTML structural elements (quick map)

| Element | Role |
|---------|------|
| **`<div>`** | Generic **container** — groups sections for layout and styling. |
| **`<p>`** | Paragraph text. |
| **`<h1>` … `<h2>`** | Headings — hierarchy and accessibility. |
| **`<button>`** | Clickable control — style with classes for primary/secondary variants. |

Assign **classes** for repeated patterns; use **`id`** sparingly for unique identification.

---

## Case Study — In Action

### Banking: promotional rate banner + reusable cards

A bank launches a **savings rate promotion**. Marketing needs:

- A **full-width banner** (one unique design) → sensible use of an **`#promo-banner`** ID or a single **class** on a wrapper `<div>`.  
- **Three product cards** with the same look (icon, title, APR text) → **one `.rate-card` class** on each `<div>`, not three separate inline styles.  
- **Global paragraph** readability → **tag selector** `p { line-height: 1.5; }` in **`styles.css`** so every page stays consistent.

**Box model:** each card uses **padding** inside the border so text doesn’t touch the edge; **margin** separates cards; **border** subtly separates regions without heavy lines.

**Outcome:** Faster updates (change `.rate-card` once), fewer mistakes than inline color on every `<p>`, and a layout that **scales** when a fourth card is added.

---

## Comparison Tables

### Inline vs internal vs external

| | **Inline** | **Internal** | **External** |
|--|------------|--------------|--------------|
| **Reuse across pages** | No | No (per file) | **Yes** |
| **Maintainability** | Low | Medium | **High** |
| **Good for learning demos** | Quick test | Single HTML file | **Production-style** |

### Tag vs class vs ID selector

| | **Tag** | **Class** | **ID** |
|--|---------|-----------|--------|
| **Symbol** | `p`, `h1` | `.name` | `#name` |
| **How many elements?** | All of that type | Many | **One per page** (rule of thumb) |
| **Typical use** | Base styles | Components, themes | Unique section, JS hook |

### Box model layers (inside → outside)

| Layer | Inside or outside? | Affects “total width” next to content? |
|--------|-------------------|----------------------------------------|
| **Content** | — | Sets base box for `width` |
| **Padding** | Inside border | Yes (adds to painted size under `content-box`) |
| **Border** | Edge | Yes |
| **Margin** | Outside border | Separates boxes (special collapse rules later) |

---

## Study habits that pay off

- Keep a **single external** stylesheet for exercises when possible.  
- Name **classes by purpose** (`card`, `btn-primary`) not by color alone (`blue-box`).  
- In DevTools, hover elements to **see the box model** overlay.  
- **Revise** selectors and the box model until you can sketch **margin → border → padding → content** from memory.

---

*Revision and deliberate practice turn CSS from memorization into a reliable skill.*
