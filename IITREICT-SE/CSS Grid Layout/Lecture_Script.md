# Lecture Script: CSS Grid Layout
**Duration:** 110 minutes | **Tools:** VS Code, Browser, DevTools | **Project:** Session 14–15 portfolio

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 5 min | Flexbox limits, Grid wins |
| Why Does This Matter? | 14 min | Dashboards, e-commerce |
| What Is the Concept? | 28 min | Tracks, areas, page patterns |
| How Do We Apply It? (LOs) | 50 min | Grid portfolio upgrade |
| Layout lab | 8 min | Sidebar + main pattern |
| Recap | 5 min | JavaScript preview |

---

## Session Opening (5 min)

**[Script:]** "Your portfolio has Flexbox nav and styled cards. But the **whole page** — header spanning full width, sidebar beside content, footer anchored — needs two-dimensional layout. **CSS Grid** is how **Amazon** product grids, **admin dashboards**, and **news sites** align complex sections."

**Real-world hook:** Open a news site — inspect grid on article + sidebar layout. "This is not magic — it is `display: grid`."

---

## Why Does This Matter?

> **In the Real World:** **Flipkart** category pages use grid-like product matrices. **Notion** page columns are grid thinking. Frontend job UI tasks often say: 'Build dashboard shell with sidebar.'

**Flexbox vs Grid decision tree (board):**
- One direction? → Flexbox
- Page shell or card matrix? → Grid

**Pain if misunderstood:**
- Grid on wrong element (child not parent)
- `grid-template-areas` row length mismatch
- Forgetting `min-height: 100vh` for sticky footer effect

---

## What Is the Concept?

### Grid Container Properties

```css
.dashboard {
  display: grid;
  grid-template-columns: 240px 1fr;
  grid-template-rows: 64px 1fr 48px;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  gap: 16px;
  min-height: 100vh;
}
```

### Placing Items

```css
header { grid-area: header; background: #1e293b; color: white; }
.sidebar { grid-area: sidebar; }
main { grid-area: main; }
footer { grid-area: footer; }
```

🎯 **Instructor Note:** Draw ASCII map matching `grid-template-areas` string — students copy into notebooks.

---

## How Do We Apply It?

### LO 1: Explain CSS Grid for 2D layout

**Case study:** Spotify desktop — left nav column + main content + optional right panel. Grid models this naturally.

**Predict:** Can Flexbox alone define 2×3 card matrix equally cleanly? (Possible but Grid is clearer.)

---

### LO 2: Define rows, columns, gaps

```css
.product-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
```

**Demo:** Change to `repeat(2, 1fr)` live — watch reflow.

---

### LO 3–5: Areas, multi-section layout, apply to portfolio

**Live upgrade** of student portfolio:

1. Wrap in `.page` grid container
2. Assign header/main/footer areas
3. Optional sidebar for "Skills" section
4. Compare before/after screenshots

> **In the Real World:** Design handoff from Figma often shows 12-column grid — `fr` and `repeat()` mirror that thinking.

---

## Layout Lab (8 min)

Build **admin-style shell:** header with logo, sidebar with 4 nav links, main with welcome text, footer with copyright — Grid only, no Flexbox on page shell (Flexbox inside components OK).

---

## Lecture Summary

- **Grid** controls rows and columns simultaneously for page-level layout
- **`grid-template-areas`** documents structure in CSS
- **Header/main/footer** pattern uses `auto 1fr auto` rows
- **Portfolio upgrade** applies Grid to real student work
- **Practical value:** Grid is the standard answer for dashboard and marketing page layouts in industry interviews and products
