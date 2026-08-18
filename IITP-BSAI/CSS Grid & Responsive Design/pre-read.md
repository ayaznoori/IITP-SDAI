# Pre-Read: CSS Grid & Responsive Design

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **build page layouts with CSS Grid** using rows, columns, and gaps
- How to **place header, main, and footer** on a simple grid template
- How to **write one media query** so your layout changes on phone vs desktop
- How to **use responsive units** like `%` and `rem` for spacing and width
- How to **think mobile-first** — start small, then enhance for larger screens

---

## 2. Detailed Explanation

### Flexbox Got You a Row — Grid Builds the Whole Page

In the last session you used **Flexbox** to align items in one direction — a nav bar, a stack of cards. That is perfect for **components**. But a full page needs regions: header on top, main content in the middle, footer at the bottom. Sometimes side-by-side columns too.

**CSS Grid** (a two-dimensional layout system for rows and columns) lets you define a grid on a **container** and place children into **cells**.

**Analogy:** Flexbox is arranging books on one shelf left-to-right or top-to-bottom. Grid is designing the whole bookcase with fixed shelves and columns, then placing each book in a slot.

**One-line definition:** Grid divides a container into rows and columns so you can place items into specific areas.

### Why Grid and Responsive Design Matter

**Real-world hook:** Open any website on your phone, then on a laptop. The layout reflows — menu collapses, columns stack, text stays readable. That is **responsive design** (building pages that adapt to screen size). Grid plus **media queries** (CSS rules that apply only at certain screen widths) make this possible.

**Benefits:**
- **Full-page structure** without messy nested `<div>` hacks
- **Cleaner layouts** for dashboards, portfolios, and landing pages
- **Better mobile experience** — users on phones are the majority on many products

### CSS Grid Building Blocks

#### The Grid Container

Set on the parent:

```css
.page {
  display: grid;
  grid-template-rows: auto 1fr auto;
  grid-template-columns: 1fr;
  gap: 16px;
  min-height: 100vh;
}
```

| Property | Meaning |
|----------|---------|
| **`display: grid`** | Turns element into a grid container |
| **`grid-template-rows`** | Defines row sizes |
| **`grid-template-columns`** | Defines column sizes |
| **`gap`** | Space between rows and columns |

**Common row pattern for pages:**

```css
grid-template-rows: auto 1fr auto;
```

- Row 1 (`auto`) — header, as tall as its content
- Row 2 (`1fr`) — main, takes remaining space
- Row 3 (`auto`) — footer, as tall as its content

**`fr`** (fraction unit — a share of leftover space) means "take a fraction of free space." `1fr` = use what's left.

#### Placing Header, Main, and Footer

**HTML:**

```html
<div class="page">
  <header>Site Header</header>
  <main>Main content here</main>
  <footer>Footer info</footer>
</div>
```

**CSS:**

```css
.page {
  display: grid;
  grid-template-rows: auto 1fr auto;
  gap: 12px;
  min-height: 100vh;
}
header { background: #2d3748; color: white; padding: 1rem; }
main   { padding: 1rem; }
footer { background: #edf2f7; padding: 1rem; }
```

Grid auto-places children in order: first child row 1, second row 2, third row 3.

#### Simple Grid Areas (Optional Stretch)

You can name regions for clarity:

```css
.page {
  display: grid;
  grid-template-areas:
    "header"
    "main"
    "footer";
  grid-template-rows: auto 1fr auto;
  gap: 12px;
}
header { grid-area: header; }
main   { grid-area: main; }
footer { grid-area: footer; }
```

**Why It Matters:** Named areas read like a map. Teams use this on landing pages and admin layouts.

### Responsive Units — % and rem

Fixed `px` values do not always scale well. Two beginner-friendly units:

| Unit | Meaning | Good for |
|------|---------|----------|
| **`%`** | Percent of parent element | Width relative to container |
| **`rem`** | Relative to root font size (usually 16px) | Padding, font-size, gap |

Examples:

```css
main {
  width: 90%;
  max-width: 960px;
  margin: 0 auto;
  padding: 1.5rem;
}
h1 {
  font-size: 2rem;
}
```

**Analogy:** `%` is "fill this much of the room." `rem` is "size based on the house's default ruler," not each furniture piece's ruler.

**Why `rem` helps:** Change root font size once — spacing and headings scale together. Tailwind and modern design systems love `rem`.

### Media Queries — One Breakpoint

A **media query** wraps CSS rules that apply only when a condition is true — usually screen width.

```css
/* Mobile-first base styles (default) */
.main-nav {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

/* Desktop enhancement */
@media (min-width: 768px) {
  .main-nav {
    flex-direction: row;
  }
}
```

**Read it aloud:** "When the viewport is **at least** 768px wide, make the nav a row."

**Common mistake:** Writing desktop styles first, then trying to shrink for mobile — harder for beginners. **Mobile-first** flips that.

### Mobile-First — Start Small, Add for Big Screens

**Mobile-first** (designing default styles for small screens, then adding rules for larger screens) means:

1. Write CSS for phone-sized layout **first** (single column, stacked nav)
2. Add `@media (min-width: ...)` for tablet/desktop (side-by-side columns, wider spacing)

**Why It Matters:**
- Most users browse on phones
- Default CSS stays simpler
- You **add** enhancements instead of fighting to strip desktop styles away

**Messy to clear walkthrough:**

**Messy (desktop-only thinking):**

```css
.sidebar { width: 300px; float: left; }
.content { margin-left: 320px; }
/* breaks on phone — sidebar squishes content */
```

**Clear (mobile-first Grid):**

```css
.page {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}
@media (min-width: 768px) {
  .page {
    grid-template-columns: 240px 1fr;
  }
}
```

On phone: one column. On desktop: sidebar + main. Same HTML.

### Flexbox vs Grid — When to Use Which

| Tool | Best for |
|------|----------|
| **Flexbox** | One row or one column — nav links, button groups, card innards |
| **Grid** | Whole page or two-dimensional sections — header/main/footer, dashboard panels |

They work together: Grid for page shell, Flexbox inside header for logo + links.

### Building Blocks Checklist

Before the live session, you should recognize:

- [ ] `display: grid` on a container
- [ ] `grid-template-rows` and `grid-template-columns`
- [ ] `gap` for row/column spacing
- [ ] Header / main / footer row pattern (`auto 1fr auto`)
- [ ] One `@media (min-width: 768px)` block
- [ ] At least one use of `%` or `rem` in spacing or width
- [ ] Mobile-first idea: base = small screen, media query = larger

### Small Code Example

```css
.page {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
  gap: 1rem;
  padding: 1rem;
}
@media (min-width: 768px) {
  .page { padding: 2rem; }
  main { font-size: 1.125rem; }
}
```

---

## 3. Practice Exercises

**Exercise 1 — Basic page grid**
Wrap your styled page from Session 6 in `<div class="page">`. Apply `display: grid`, `grid-template-rows: auto 1fr auto`, and `min-height: 100vh`. Confirm header stays top, footer bottom, main fills middle.

**Exercise 2 — Gap and rem**
Add `gap: 1rem` to the grid and change `main` padding to `1.5rem`. Increase browser font size (browser settings) and notice `rem` spacing grows slightly.

**Exercise 3 — One media query**
Make `.main-nav` a **column** by default. Inside `@media (min-width: 768px)`, switch it to `flex-direction: row`. Resize browser window narrow vs wide — predict before testing.

**Exercise 4 — Two-column desktop**
On `.page`, keep one column by default. In a media query at `768px`, set `grid-template-columns: 1fr 1fr` and add a `<aside>` sidebar between header and footer in HTML. Place main content and sidebar side by side on desktop only.
