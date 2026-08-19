# Lecture Script: CSS Grid & Responsive Design
**Duration:** 110 minutes | **Tools:** VS Code, Browser, DevTools | **Languages:** HTML + CSS

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Flexbox limits, Grid wins |
| Why Does This Matter? | 12 min | Mobile traffic, layout systems |
| What Is the Concept? | 30 min | Grid tracks, areas, media queries, units |
| How Do We Apply It? (LOs) | 45 min | Full page layout + responsive reflow |
| Live responsive lab | 10 min | Resize browser, fix breakpoints |
| Recap & summary | 8 min | LO review + DOM preview |

---

## Session Opening (5 min)

**[Script:]** "Flexbox lines things up in one direction brilliantly. But a full webpage is two-dimensional — header across the top, sidebar beside main, footer below. That is Grid territory. Today we also answer: what happens when the screen is phone-sized instead of laptop-sized?"

**Problem hook:** Resize browser on a fixed-width layout — horizontal scrollbar appears. "Users leave. Responsive design keeps them."

🎯 **Instructor Note:** Open DevTools device toolbar — toggle iPhone view on a non-responsive page for instant motivation.

---

## Why Does This Matter?

**[Script:]** "Your portfolio on a recruiter's phone is your first interview. Grid gives you page-level control. Media queries let layout adapt. Together they are the baseline skill for any frontend role."

**Real-world use:**
- Dashboard layouts (header, sidebar, content, footer)
- Marketing sites with hero + feature grid
- Admin panels and e-commerce category pages

**Pain if misunderstood:**
- Fixed pixel widths break on mobile
- Desktop-first CSS becomes tangled `max-width` overrides
- Grid without `gap` — cramped layouts
- Forgetting `min-width` vs `max-width` in media queries

🎯 **Instructor Note:** Stat hook — majority of users browse on mobile; no need for exact percentage, emphasize trend.

---

## What Is the Concept?

### CSS Grid Overview

**Definition:** Grid is a two-dimensional layout system — rows and columns simultaneously.

**Mental model:** Spreadsheet with named regions.

```css
.page {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
  gap: 16px;
}
```

**Key terms:**
- **Grid container** — parent with `display: grid`
- **Grid items** — direct children
- **Tracks** — rows and columns
- **`fr`** — flexible fraction of free space

### Grid Template Areas

**Definition:** Named layout map readable in CSS.

```css
.page {
  display: grid;
  grid-template-areas:
    "header header"
    "main   aside"
    "footer footer";
  grid-template-columns: 2fr 1fr;
  gap: 12px;
}
header { grid-area: header; }
main   { grid-area: main; }
aside  { grid-area: aside; }
footer { grid-area: footer; }
```

🎯 **Instructor Note:** Draw ASCII grid on board matching `grid-template-areas` string.

**Common mistake:** Number of cells in each row string must match column count.

### Header / Main / Footer

**[Script:]** "Most pages fit this pattern. Grid row 1: header. Row 2: main grows (`1fr`). Row 3: footer."

Simplified three-row, one-column mobile base:

```css
.page {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}
```

### Media Queries

**Definition:** Conditional CSS based on device features — usually viewport width.

**Mobile-first pattern:**

```css
/* Base: phone */
.features {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}

@media (min-width: 768px) {
  .features {
    grid-template-columns: 1fr 1fr 1fr;
  }
}
```

**[Script:]** "`min-width: 768px` means: when viewport is **at least** 768px, apply these extra rules. Small screens never see the three-column rule — they keep one column."

🎯 **Instructor Note:** Demo resizing browser across breakpoint. Slow down — this is often confusing first time.

### Responsive Units: `%` and `rem`

| Unit | Behavior |
|------|----------|
| `%` | Relative to parent element |
| `rem` | Relative to root `html` font-size |
| `px` | Fixed — fine for borders |

```css
html { font-size: 16px; }
main {
  width: 90%;
  max-width: 1100px;
  margin: 0 auto;
  padding: 1.5rem;
}
```

**Why `max-width`:** Content does not stretch uncomfortably on ultra-wide monitors.

### Mobile-First Mindset

**Definition:** Write base styles for smallest screen; enhance for larger with `min-width` queries.

**Contrast desktop-first:**

```css
/* Avoid teaching as primary pattern */
@media (max-width: 767px) { ... }
```

**[Script:]** "Mobile-first matches how CSS grows: start simple, add complexity when space allows."

---

## How Do We Apply It?

### LO 1: Define grid rows, columns, and gaps on a container

**Problem:** Create a 3-column feature section on desktop.

**Write code:**

```css
.features {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
  padding: 24px;
}
```

**Predict:** Are columns equal width?

**Explain result:** Three `1fr` columns share space equally; `gap` adds gutters.

**Starter demo — 2×2 grid:**

```css
.grid-demo {
  display: grid;
  grid-template-columns: 100px 100px;
  grid-template-rows: 80px 80px;
  gap: 8px;
}
```

---

### LO 2: Place header, main, and footer on a simple grid

**Problem:** Page should fill viewport; main expands.

**Write code:**

```html
<div class="page">
  <header>Header</header>
  <main>Main content</main>
  <footer>Footer</footer>
</div>
```

```css
.page {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
}
header, footer { background: #eee; padding: 1rem; }
main { padding: 1.5rem; }
```

**Predict:** Which section grows when content is short?

**Explain result:** Middle `1fr` row absorbs extra vertical space.

🎯 **Instructor Note:** Add `grid-template-areas` variant for advanced pairs — optional 5 min extension.

---

### LO 3: Write one media query for phone vs desktop

**Problem:** Sidebar beside main on desktop; stacked on phone.

**Write code:**

```css
.layout {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}
@media (min-width: 768px) {
  .layout {
    grid-template-columns: 250px 1fr;
  }
}
```

**Predict:** At 400px width, how many columns?

**Explain result:** One column — base rule only. At 800px, two columns activate.

**DevTools demo:** Drag responsive width across 768px — layout flips.

---

### LO 4: Use basic responsive units in spacing or width

**Problem:** Comfortable reading width and scalable padding.

**Write code:**

```css
html { font-size: 16px; }
article {
  width: 92%;
  max-width: 720px;
  margin: 0 auto;
  padding: 2rem 1rem;
}
```

**Predict:** If user sets browser font to larger, how does `2rem` padding behave?

**Explain result:** `rem` scales with root font — accessibility win.

---

### LO 5: Apply a mobile-first approach at a beginner level

**Problem:** Refactor desktop-only three-column grid to mobile-first.

**Before (show):**

```css
.grid { grid-template-columns: 1fr 1fr 1fr; }
```

**After:**

```css
.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}
@media (min-width: 768px) {
  .grid { grid-template-columns: repeat(3, 1fr); }
}
```

**Predict:** Default experience on phone?

**Explain result:** Single column stack — readable without horizontal scroll.

🎯 **Instructor Note:** Pair refactor exercise — 10 minutes on paper then code.

---

## Live Responsive Lab (10 min)

**Task:** Extend portfolio from prior sessions:

1. `.page` grid with header, main, footer
2. Project cards in grid — 1 column mobile, 2 columns at 600px, 3 at 900px (two breakpoints OK if one is stretch goal; minimum one query required)
3. `main` uses `max-width` and `rem` padding
4. Test in DevTools device mode — screenshot checklist

---

## Recap (8 min)

🎯 **Instructor Note:** "Flexbox or Grid for a full page layout?" (Grid) "For a nav bar row?" (Flexbox) — quick discrimination.

---

## Lecture Summary

- **CSS Grid** defines rows, columns, and gaps for two-dimensional layouts
- **Grid template areas** map header, main, and footer to readable regions
- **Media queries** with `min-width` enable mobile-first responsive behavior
- **`%` and `rem`** create flexible widths and scalable spacing
- **Mobile-first** means base styles for small screens, enhancements for larger viewports
- **Practical value:** Responsive Grid layouts are how modern sites work on every device recruiters and users actually use

**[Script:]** "Next session: JavaScript meets your HTML — the DOM and events. Layout is solved; now we make pages react to users."
