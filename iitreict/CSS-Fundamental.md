# CSS Layout Fundamentals

## Session Overview

**Running Example**: Smart Cart Web App — an e-commerce shopping cart interface featuring a product list, cart summary, checkout button, and navigation header.

---

## Opening

[Script:]
Welcome to CSS Layout Fundamentals. Over the next two hours, you'll learn how to build the visual structures that hold every website together. We'll work with one project throughout this session: the Smart Cart Web App. By the end, you'll understand how to position elements, create flexible layouts that adapt to screen sizes, and build responsive grids — all skills you'll use in every web project you create.

---

## Why Does This Matter?

🎯 **Instructor Note**: Hold up a printed screenshot of a poorly-laid-out e-commerce page (misaligned products, overlapping text, broken mobile view). Ask the group:

[Script:]
Look at this screenshot. What feels wrong? Don't worry about the technical terms — just describe what your eyes see.

Allow 2 minutes for responses. Common answers: "The prices are overlapping," "The buttons are cut off," "It looks different on my phone."

[Script:]
Every issue you just described is a layout problem. CSS layout controls where elements sit, how they size, and how they respond when the browser changes. Without layout skills, you can't build interfaces that work. With them, you can create anything from a simple blog to a complex dashboard.

Here's what happens when layout is misunderstood:

- Elements overlap and become unreadable
- Buttons become unclickable
- Sites break on mobile devices
- Developers spend hours fixing what should take minutes

The good news? Layout is predictable once you understand the rules. Let's learn those rules.

---

## What Is the Concept?

### CSS Box Model

Every HTML element is a box. The CSS Box Model defines how that box is calculated:

1. **Content** — the actual text or image inside
2. **Padding** — space between content and the border
3. **Border** — the edge around the padding
4. **Margin** — space outside the border, separating boxes

🎯 **Instructor Note**: Draw a large rectangle on the whiteboard. Label each layer from inside out: content, padding, border, margin.

[Script:]
Think of a framed photograph. The photo is the content. The mat board around it is padding. The frame is the border. The space between this frame and the next frame on the wall is margin. Each layer adds to the total size of the element.

**Common Mistake**: The default box model adds padding and border *inside* the width you specify. A 200px element with 20px padding becomes 240px wide. This surprises beginners constantly.

### Positioning

CSS offers five positioning schemes:

1. **Static** — default, elements flow normally
2. **Relative** — positioned relative to its normal position
3. **Absolute** — positioned relative to the nearest positioned ancestor
4. **Fixed** — positioned relative to the viewport, doesn't scroll
5. **Sticky** — toggles between relative and fixed based on scroll position

[Script:]
Positioning is like choosing how to place furniture in a room. Static is like pieces that just sit where they naturally fit. Relative is like moving a chair slightly from where it would normally be. Absolute is like hanging a picture on a specific wall, measured from the wall's edges. Fixed is like a TV mounted on the wall that stays visible no matter where you walk in the room.

### Flexbox

Flexbox is a one-dimensional layout method for arranging items in rows or columns. It automatically distributes space and aligns items.

**Key Properties**:
- `display: flex` — activates flexbox on a container
- `justify-content` — alignment along the main axis (horizontal in a row)
- `align-items` — alignment along the cross axis (vertical in a row)
- `flex-direction` — row or column

[Script:]
Flexbox is like a flex team at work. You have a container (the team) and items (the team members). You can tell the items how to line up, who goes first, how they share space. It's designed for one-dimensional layouts — a single row or a single column.

### CSS Grid

CSS Grid is a two-dimensional layout system for rows and columns simultaneously. It excels at creating overall page structure.

**Key Properties**:
- `display: grid` — activates grid on a container
- `grid-template-columns` — defines column sizes
- `grid-template-rows` — defines row sizes
- `gap` — space between rows and columns

[Script:]
If Flexbox is a single team, Grid is the entire office floor plan. You can place items in specific rows and columns, create complex patterns, and define the whole structure at once. Grid is your go-to for page-level layouts.

---

## How Do We Apply It?

---

### Learning Objective 1: CSS Box Model and Positioning

#### Problem

[Script:]
In the Smart Cart Web App, the product cards have inconsistent spacing. Some products feel cramped. The checkout button gets hidden when the cart has many items. We need consistent sizing and a button that stays visible.

#### Translate Logic

1. Set a consistent box model with `box-sizing: border-box`
2. Use padding to create internal spacing within cards
3. Use margins to separate cards from each other
4. Use fixed positioning to keep the checkout button always visible

#### Write Code

**Demo 1: Box Model Basics**

```css
/* Reset box model for all elements */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.product-card {
  width: 250px;
  padding: 20px;
  border: 2px solid #333;
  margin: 15px;
}

.checkout-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  padding: 15px 30px;
  background: #28a745;
  color: white;
  border: none;
}
```

🎯 **Instructor Note**: Ask the group to predict before showing the result.

[Script:]
Predict before running: If the product card has width: 250px, padding: 20px, and border: 2px, what is the total rendered width? Remember the default box model.

Allow 30 seconds for responses. Answer: 294px (250 + 20×2 + 2×2) with default content-box. With border-box: 250px.

**Demo 2: Positioning in Action**

```css
.header {
  position: relative;
  height: 60px;
  background: #2c3e50;
}

.logo {
  position: absolute;
  left: 20px;
  top: 50%;
  transform: translateY(-50%);
}

.cart-badge {
  position: absolute;
  top: -5px;
  right: -5px;
  background: red;
  color: white;
  border-radius: 50%;
  padding: 2px 6px;
  font-size: 12px;
}
```

🎯 **Instructor Note**: Ask the group to predict.

[Script:]
Predict before running: The header is 60px tall. The logo uses top: 50% and transform: translateY(-50%). Where will the logo vertically sit?

Answer: Vertically centered in the header.

#### Explain Result

[Script:]
The `box-sizing: border-box` reset makes sizing predictable — the width you set is the width you get. Fixed positioning removed the checkout button from normal flow, so it stays at bottom-right regardless of other content. The relative header becomes the positioning context for the absolute logo and badge.

---

### Learning Objective 2: Flexbox Fundamentals

#### Problem

[Script:]
The Smart Cart product list displays products in a rigid grid that breaks on smaller screens. We need products to flow naturally, wrap to new lines when needed, and stay vertically aligned. The cart summary should stay centered at the bottom.

#### Translate Logic

1. Make the product container a flex container with `display: flex`
2. Allow wrapping with `flex-wrap: wrap`
3. Use `justify-content` to control horizontal alignment
4. Use `align-items` to control vertical alignment
5. Use `flex` property on items to allow them to grow/shrink

#### Write Code

**Demo 1: Flexbox Product List**

```css
.product-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  padding: 20px;
}

.product-card {
  flex: 0 1 280px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
}
```

🎯 **Instructor Note**: Ask the group to predict.

[Script:]
Predict before running: With `flex: 0 1 280px`, what happens when the screen is 400px wide and we have three product cards?

Answer: Two cards display (280px each = 560px, exceeds 400px so one wraps), each card can shrink below 280px if needed but prefers 280px.

**Demo 2: Flexbox Alignment**

```css
.cart-summary {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 30px;
  background: #f8f9fa;
  border-top: 2px solid #dee2e6;
}

.cart-total {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 15px;
}

.checkout-btn {
  padding: 12px 40px;
  font-size: 16px;
}
```

[Script:]
Predict before running: The cart summary uses flex-direction: column with align-items: center and justify-content: center. If there's only one line of text, where does everything sit?

Answer: Everything centers both horizontally (align-items) and vertically (justify-content) within the cart summary container.

#### Explain Result

[Script:]
Flexbox handles the complexity of responsive wrapping automatically. The `flex: 0 1 280px` shorthand means: don't grow (0), can shrink (1), prefer 280px width. When containers get smaller than 280px, cards shrink. When they can't shrink enough, they wrap. The gap property adds consistent spacing without calculating margins.

---

### Learning Objective 3: Grid Layouts for Responsive Design

#### Problem

[Script:]
The Smart Cart page needs a complete layout: header at top, product grid in the middle, cart summary sidebar on desktop but below products on mobile. We need a layout that fundamentally changes structure based on screen size.

#### Translate Logic

1. Define a grid container with `display: grid`
2. Create columns that adapt: 1 column on mobile, 2 on tablet, 3-4 on desktop
3. Use `grid-template-areas` for intuitive placement
4. Use media queries to change the grid structure
5. Use `gap` for consistent spacing

#### Write Code

**Demo 1: CSS Grid Page Layout**

```css
.page-container {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-areas:
    "header"
    "products"
    "summary";
  gap: 20px;
  min-height: 100vh;
}

.header { grid-area: header; }
.product-list { grid-area: products; }
.cart-summary { grid-area: summary; }

@media (min-width: 768px) {
  .page-container {
    grid-template-columns: 3fr 1fr;
    grid-template-areas:
      "header header"
      "products summary";
  }
}
```

🎯 **Instructor Note**: Ask the group to predict.

[Script:]
Predict before running: On a 375px mobile screen, how many columns does the grid have? Where does each section appear vertically?

Answer: 1 column. Header at top, products in middle, summary at bottom — stacked vertically.

**Demo 2: Responsive Product Grid**

```css
.product-grid {
  display: grid;
  gap: 20px;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
}

.product-card {
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
}
```

[Script:]
Predict before running: The product grid uses `repeat(auto-fill, minmax(250px, 1fr))`. On a 1200px screen, how many columns? On a 600px screen?

Answer: 1200px ÷ 250px = 4+ columns (auto-fill fits as many as possible). 600px ÷ 250px = 2 columns. At 300px, drops to 1 column.

#### Explain Result

[Script:]
Grid-template-areas makes layout readable in the CSS itself. You can see the visual structure just by reading the code. The media query switches from a single-column stacked layout to a two-column layout at 768px — typical tablet breakpoint. The `auto-fill` with `minmax` creates truly responsive columns without multiple media queries — the browser calculates how many columns fit automatically.

---

## Lecture Summary

[Script:]
Let's review what we covered today:

- **CSS Box Model**: Every element is a box with content, padding, border, and margin layers. Use `box-sizing: border-box` for predictable sizing. Positioning controls where boxes live — static, relative, absolute, fixed, and sticky each serve different purposes.

- **Flexbox Fundamentals**: One-dimensional layouts for rows or columns. Use `display: flex`, control alignment with `justify-content` and `align-items`, allow wrapping with `flex-wrap`. Perfect for component-level layouts like product lists and navigation.

- **Grid Layouts for Responsive Design**: Two-dimensional layouts for rows and columns simultaneously. Use `display: grid`, define columns with `grid-template-columns`, place items with `grid-template-areas`. The `auto-fill` with `minmax` creates responsive columns without media queries.

**Practical Value**: These three skills — box model, Flexbox, and Grid — form the foundation of every CSS layout you'll build. Master these, and you can create any interface. Struggle with them, and every project becomes a battle. The Smart Cart Web App you built today demonstrates the exact patterns used in real production applications. Practice these until they become second nature.
