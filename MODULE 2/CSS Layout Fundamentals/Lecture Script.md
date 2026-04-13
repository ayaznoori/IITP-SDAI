## LO–1.2.9: Explain the CSS box model and visualize websites as collections of boxes (20 minutes)

Every element on a web page is a rectangle. Understanding this system, the box model, is key to building any layout.

Four layers, from inside to outside:

```
┌─────────────────────────────────────┐  ← MARGIN (outside, transparent)
│                                     │
│   ┌─────────────────────────────┐   │  ← BORDER
│   │                             │   │
│   │   ┌─────────────────────┐   │   │  ← PADDING (inside, colored)
│   │   │                     │   │   │
│   │   │      CONTENT        │   │   │
│   │   │   (text, images)    │   │   │
│   │   │                     │   │   │
│   │   └─────────────────────┘   │   │
│   │                             │   │
│   └─────────────────────────────┘   │
│                                     │
└─────────────────────────────────────┘
```

1.  **Content** — The text or image. `width` and `height` control this area.
2.  **Padding** — Space between content and border. The background color fills this area.
3.  **Border** — A line around the padding.
4.  **Margin** — Space outside the border. Always transparent. Creates distance between elements.

An analogy is a framed painting: the painting is content, the mat board is padding, the frame is the border, and the gap to the next frame is the margin.

By default, `width` and `height` only control the content area. Padding and border add to the total size.

Formula: `Total rendered width = width + left padding + right padding + left border + right border`

This is unintuitive. The solution is `box-sizing: border-box`. This changes the calculation so `width` and `height` *include* padding and border.

```css
* {
  box-sizing: border-box; /* Applied to every element */
}

div {
  width: 200px;
  padding: 20px;
  border: 5px solid black;
  /* Total rendered width: still 200px */
}
```

Put `box-sizing: border-box` at the top of every CSS file.

Let's build a styled card and see this in DevTools.

```html
<div class="card">
  <h2>Product Name</h2>
  <p>This is a description of the product.</p>
  <button>Buy Now</button>
</div>
```

```css
* { box-sizing: border-box; }
body { font-family: Arial, sans-serif; background-color: #f0f4f8; padding: 40px; }
.card {
  width: 300px;
  padding: 24px;          /* space inside the card */
  border: 2px solid #cbd5e0; /* visible border */
  margin: 20px auto;      /* space outside, centered */
  background-color: #ffffff;
  border-radius: 8px;
}
button { padding: 10px 20px; }
```
In DevTools, select the `.card`, and the box model diagram will show you the exact dimensions of its content, padding, border, and margin.

## LO–1.2.10: Identify height, width, and border as prerequisites for building layouts (15 minutes)

An element with no content has zero height and is invisible. We need `width`, `height`, and `border` to define the size and boundaries of our layout components.

**width**: Sets the width of an element. Block elements like `div` are 100% wide by default.
```css
.sidebar { width: 250px; }       /* Fixed pixels */
.main { width: 70%; }            /* Percentage of parent */
.container { max-width: 1200px; } /* Maximum width */
```

**height**: Has no natural default. Its default is `auto`, meaning it grows to fit its content.
```css
.hero { height: 500px; }        /* Fixed */
.card { min-height: 200px; }    /* Grows with content */
.full-screen { height: 100vh; } /* 100% of viewport height */
```
Use `min-height` for content areas to avoid content overflowing a fixed-height box.

**border**: Draws a visible line with three values: width, style, and color.
```css
.box { border: 2px solid #333; } /* Shorthand */
.card { border-top: 4px solid #3182ce; } /* Individual side */
.rounded { border-radius: 8px; } /* Rounded corners */
.circle  { border-radius: 50%; } /* Circle (if width=height) */
```

Let's build a few common layout boxes.

```html
<div class="navbar">Navigation Bar</div>
<div class="hero">Hero Section</div>
<div class="card">Card Component</div>
<div class="circle-avatar"></div>
```
```css
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: Arial, sans-serif; }

.navbar { width: 100%; height: 60px; background: #2d3748; border-bottom: 3px solid #3182ce; }
.hero { width: 100%; height: 400px; background: #ebf8ff; }
.card { width: 300px; min-height: 150px; border: 1px solid #e2e8f0; border-radius: 12px; margin: 24px auto; }
.circle-avatar { width: 80px; height: 80px; background: #f6ad55; border-radius: 50%; margin: 20px auto; }
```
These three properties are the foundation. Every layout component is built upon them.

## LO–1.2.13: Differentiate between block and inline elements (10 minutes)

Why do some elements stack vertically and others flow side-by-side? The answer is their `display` type: block or inline.

**Block Elements** (`div`, `p`, `h1`)
- Take up the **full available width**.
- Start on a **new line**.
- Respect `width`, `height`, `margin`, and `padding` on all sides.

**Inline Elements** (`span`, `a`, `strong`)
- Take up only as much width as their **content**.
- Flow **within a line** of text.
- **Ignore** `width` and `height`.

| Feature              | Block | Inline             |
| -------------------- | ----- | ------------------ |
| New line?            | Yes   | No                 |
| Full width?          | Yes   | No (content width) |
| Respects width/height? | Yes   | No                 |

Let's see a demo.
```html
<div style="background: #ebf8ff;">I am a block div</div>
<div style="background: #fef3c7;">I am another block div</div>

<span style="background: #dcfce7;">I am inline</span>
<span style="background: #fce7f3;">also inline</span>
```
In the browser, the `div`s stack, and the `span`s flow. If you try to set `width: 200px` on an inline `span`, it will have no effect.

## LO–1.2.17: Differentiate between margin and padding (10 minutes)

Margin and padding both create space, but one is outside the box and one is inside.

-   **Padding** is the space *inside* an element, between the content and the border. The background color fills the padding.
-   **Margin** is the space *outside* an element, between its border and other elements. It is always transparent.

**Visual Test:** If the space has the element's background color, it's padding. If it's transparent, it's margin.

**Margin Collapse:** When two block elements are stacked vertically, their top/bottom margins collapse. The larger one wins. A `32px` bottom margin followed by a `24px` top margin results in `32px` of space, not `56px`.

Let's demo this.
```html
<div class="demo-container">
  <div class="box padding-demo">I have padding: 32px</div>
  <div class="box margin-demo">I have margin: 32px</div>
</div>
```
```css
.demo-container { background-color: #e2e8f0; border: 2px dashed #94a3b8; padding: 10px; }
.box { background-color: #3498db; color: white; border: 2px solid #2c81ba; }
.padding-demo { padding: 32px; } /* Space inside is blue */
.margin-demo { margin: 32px; }   /* Space outside shows gray container background */
```
When in doubt, open DevTools and look at the box model diagram. It color-codes padding (green) and margin (orange).

## LO–1.7.1: Explain the purpose of media queries in building responsive web designs (20 minutes)

Have you ever visited a website on your phone and had to zoom in to read it? That's non-responsive design. Users visit sites on phones, tablets, and large monitors; they all deserve a good experience.

Media queries solve this. They let one set of HTML and CSS adapt its appearance to the screen it's viewed on.

Responsive web design is the practice of building websites that respond to the user's device. It has three pillars:
1.  Fluid grid layouts (using percentages)
2.  Flexible images (that scale)
3.  Media queries (CSS rules that activate at certain screen sizes)

Let's see the problem media queries solve. Here's a three-column layout:
```html
<div class="container">
  <div class="card">Article One</div>
  <div class="card">Article Two</div>
  <div class="card">Article Three</div>
</div>
<style>
  .container { display: flex; gap: 16px; }
  .card { flex: 1; padding: 20px; background: #e0e0e0; }
</style>
```
On a narrow screen, the cards become unusably narrow. Now, let's add a media query:
```css
@media (max-width: 600px) {
  .container {
    flex-direction: column;
  }
}
```
When the screen is 600px or less, the cards stack vertically, making them perfectly readable. Media queries can change any CSS property—layout, fonts, colors, visibility—to create a tailored experience for every device.

## LO–1.7.2: Define media queries and describe how they conditionally apply CSS rules (15 minutes)

A media query is a CSS at-rule that tests a condition. If the condition is true, the CSS inside is applied. If false, it's ignored. It's like an `if` statement for CSS.

The syntax is `@media (condition) { /* styles */ }`.

Let's break down an example:
`@media (max-width: 768px) { .sidebar { display: none; } }`
-   `@media` starts the query.
-   `(max-width: 768px)` is the condition: "Is the viewport 768px or less?"
-   The CSS block applies if the condition is true.

At 1200px, the condition is false and the sidebar is visible. At 768px or below, the condition is true and the sidebar is hidden.

The browser evaluates media queries continuously. Every time you resize the window, it re-checks. Let's see this live:

```html
<!DOCTYPE html><html><head><style>
  body { background-color: white; text-align: center; padding: 40px; transition: background-color 0.3s; }
  h1::after { content: " (Desktop)"; color: blue; }
  @media (max-width: 768px) {
    body { background-color: lightyellow; }
    h1::after { content: " (Tablet)"; color: orange; }
  }
  @media (max-width: 480px) {
    body { background-color: lightgreen; }
    h1::after { content: " (Mobile)"; color: green; }
  }
</style></head><body><h1>Screen Size</h1></body></html>
```
As you resize the browser, you'll see the background color and label change instantly. Styles inside a media query follow normal CSS cascade rules, overriding base styles only when active.

## LO–1.7.5: Write media query syntax using @media with min-width and max-width (20 minutes)

The two most common conditions are `min-width` and `max-width`.

**`max-width`** is for a "desktop-first" approach. You write desktop styles by default, then add overrides for smaller screens.
```css
/* Default styles for desktop */
.hero { font-size: 48px; }

/* Override for screens 768px or NARROWER */
@media (max-width: 768px) {
  .hero { font-size: 32px; }
}
```

**`min-width`** is for a "mobile-first" approach. You write mobile styles by default, then add enhancements for larger screens. This is the modern best practice.
```css
/* Default styles for mobile */
.hero { font-size: 24px; }

/* Enhancement for screens 480px or WIDER */
@media (min-width: 480px) {
  .hero { font-size: 32px; }
}

/* Enhancement for screens 768px or WIDER */
@media (min-width: 768px) {
  .hero { font-size: 48px; }
}
```
Mobile-first is preferred because it prioritizes the most common browsing context and often leads to simpler, faster-loading CSS.

You can also combine them with `and` to target a specific range, like for a tablet.
```css
/* ONLY for screens between 600px and 1023px */
@media (min-width: 600px) and (max-width: 1023px) {
  .layout { grid-template-columns: 1fr 1fr; }
}
```

## LO–1.7.6: Apply different styles for large, medium, and small screens (10 minutes)

Now we'll build a complete three-tier responsive layout using the mobile-first approach. We'll have distinct visual experiences for mobile, tablet, and desktop from a single HTML file.

Here is the HTML:
```html
<!DOCTYPE html><html lang="en"><head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Three-Tier Layout</title>
  <link rel="stylesheet" href="style.css">
</head><body>
  <header class="site-header">
    <h1>My Website</h1>
    <nav> <a href="#">Home</a> <a href="#">About</a> <a href="#">Contact</a> </nav>
  </header>
  <main class="content-grid">
    <article class="main-content">
      <h2>Welcome</h2>
      <p>This layout adjusts for any screen size.</p>
    </article>
    <aside class="sidebar"> <h3>Quick Links</h3> </aside>
  </main>
</body></html>
```

First, the mobile styles. This is our base CSS, with no media query.
```css
/* --- MOBILE BASE STYLES --- */
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: sans-serif; }
.site-header { background: #2c3e50; color: white; padding: 16px; }
nav { display: flex; flex-direction: column; gap: 8px; }
.content-grid { padding: 16px; display: flex; flex-direction: column; gap: 16px; }
```
On mobile, navigation and content stack vertically.

Next, tablet styles. We add these inside a `min-width` query.
```css
/* --- TABLET STYLES (768px and wider) --- */
@media (min-width: 768px) {
  .site-header { display: flex; align-items: center; justify-content: space-between; }
  nav { flex-direction: row; }
  .content-grid { flex-direction: row; }
  .main-content { flex: 2; }
  .sidebar { flex: 1; }
}
```
At 768px, the header and content switch to a horizontal, multi-column layout.

Finally, desktop styles. We add more space and a max-width container.
```css
/* --- DESKTOP STYLES (1024px and wider) --- */
@media (min-width: 1024px) {
  .site-header, .content-grid { padding-left: 60px; padding-right: 60px; }
  .content-grid { max-width: 1200px; margin: 0 auto; gap: 32px; }
}
```
Resize the browser window to see the layout transform at each breakpoint.

## LO–1.7.13: Identify the five CSS position types static, relative, absolute, fixed, and sticky (15 minutes)

CSS has five `position` values that control how elements are placed.

-   `static`: The default. The element stays in the normal document flow.
-   `relative`: Stays in the flow, but you can shift it with `top`, `left`, etc. Its original space is preserved.
-   `absolute`: Removed from the flow. It's positioned relative to the nearest non-static ancestor.
-   `fixed`: Removed from the flow. It's positioned relative to the browser window (viewport) and doesn't scroll.
-   `sticky`: Stays in the flow until you scroll past a certain point, then it sticks in place like `fixed`.

Let's see some quick examples:
- **relative**: To nudge an icon up a few pixels: `.icon { position: relative; top: -3px; }`
- **absolute**: For a "NEW" badge on a product card: The parent card needs `position: relative`, and the badge gets `position: absolute; top: 8px; right: 8px;`.
- **fixed**: For a chat button that stays in the corner: `.chat-button { position: fixed; bottom: 20px; right: 20px; }`
- **sticky**: For a table header that stays visible: `thead th { position: sticky; top: 0; }`

A critical distinction is whether an element is in or out of the document flow.
- **In-flow**: `static`, `relative`, `sticky`. Other elements make space for them.
- **Out-of-flow**: `absolute`, `fixed`. Other elements ignore them, which allows for overlapping.