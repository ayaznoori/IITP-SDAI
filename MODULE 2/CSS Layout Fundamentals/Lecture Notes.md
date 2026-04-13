## Every Element Is a Box: The CSS Box Model

Every HTML element renders as a rectangular box with four layers:

```
┌─────────────────── MARGIN ───────────────────┐
│  ┌──────────────── BORDER ──────────────────┐ │
│  │  ┌────────────── PADDING ──────────────┐  │ │
│  │  │  ┌───────────── CONTENT ──────────┐ │  │ │
│  │  │  │                               │ │  │ │
│  │  │  │    Text / Image / Children    │ │  │ │
│  │  │  │                               │ │  │ │
│  │  │  └───────────────────────────────┘ │  │ │
│  │  └─────────────────────────────────────┘  │ │
│  └──────────────────────────────────────────┘ │
└───────────────────────────────────────────────┘
```

-   **Content**: The text, images, or child elements.
-   **Padding**: Space inside the element, between content and border. The background color fills this area.
-   **Border**: A visible outline around the padding and content.
-   **Margin**: Space outside the border, separating the element from its neighbors. It is always transparent.

### The `box-sizing` Property

By default (`content-box`), `width` only applies to the content area. Padding and border add to the total rendered size. This is unintuitive.

The modern standard is to use `border-box`, where `width` defines the total width including padding and border.

```css
* {
  box-sizing: border-box; /* Put this at the top of every project */
}

div {
  width: 200px;
  padding: 20px;
  border: 5px solid;
  /* Actual rendered width: still exactly 200px */
}
```

## Layout Prerequisites: `width`, `height`, and `border`

To build layouts, you must control the dimensions and boundaries of your boxes.

-   **`width`**: Controls horizontal size. Can be pixels (`px`), percentage (`%`), or constrained with `max-width` and `min-width`.
-   **`height`**: Controls vertical size. Its default is `auto`. Use `min-height` for content containers to prevent overflow.
-   **`border`**: Creates a visible line. Requires `border-width`, `border-style`, and `border-color`.
-   **`border-radius`**: Rounds the corners. A value of `50%` creates a circle if `width` and `height` are equal.

## Display Types: `block` vs `inline`

| Feature              | Block                                      | Inline                                     |
| -------------------- | ------------------------------------------ | ------------------------------------------ |
| Starts on new line   | Yes                                        | No                                         |
| Takes full width     | Yes                                        | No (only content width)                    |
| Respects width/height| Yes                                        | No                                         |
| Respects all margins | Yes                                        | Left/right only                            |
| Examples             | `div`, `p`, `h1`, `section`, `header`, `li` | `span`, `a`, `strong`, `em`, `img`, `button` |

## Spacing: `margin` vs `padding`

| Feature               | Padding                                        | Margin                                         |
| --------------------- | ---------------------------------------------- | ---------------------------------------------- |
| Location              | **Inside** the border                          | **Outside** the border                         |
| Background filled?    | **Yes** (shows element's background)           | **No** (always transparent)                    |
| Negative values?      | No                                             | Yes                                            |
| Vertical Collapse?    | No                                             | Yes (between adjacent block elements)          |
| Centers elements?     | No                                             | Yes (`margin: 0 auto`)                         |

Vertical margins between adjacent block elements collapse to the size of the larger margin.

## Responsive Web Design & Media Queries

Responsive design allows one website to adapt its layout to any screen size. This is achieved primarily with **media queries**.

A media query is a CSS at-rule that conditionally applies styles. The browser evaluates the condition (e.g., screen width), and if it's true, the styles inside the query are applied.

### Media Query Syntax

The basic structure is `@media (condition) { /* CSS rules */ }`.

The most common conditions are `min-width` and `max-width`.

-   `min-width` (Mobile-First): Styles apply when the viewport is **wider** than the specified value. This is the recommended approach.
-   `max-width` (Desktop-First): Styles apply when the viewport is **narrower** than the specified value.

```css
/* Mobile-first approach */
p { font-size: 14px; } /* Base mobile style */

@media (min-width: 768px) { 
  p { font-size: 16px; } /* Tablet override */
}

@media (min-width: 1024px) { 
  p { font-size: 18px; } /* Desktop override */
}
```

### The Viewport Meta Tag

Always include this tag in your HTML `<head>` for media queries to work correctly on mobile devices.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

## The Five CSS `position` Types

| Value      | In Flow?              | Reference Point                  | Use Case                               |
| ---------- | --------------------- | -------------------------------- | -------------------------------------- |
| `static`   | Yes                   | Normal document flow             | Default state; no special positioning  |
| `relative` | Yes                   | Its own original position        | Nudging elements; creating context for `absolute` children |
| `absolute` | No                    | Nearest non-static ancestor    | Overlays, tooltips, badges, dropdowns  |
| `fixed`    | No                    | Viewport (browser window)        | Sticky navbars, floating buttons, modals |
| `sticky`   | Yes (until threshold) | Viewport once threshold is met   | Sticky table headers, section titles   |

Elements with `position: absolute` or `fixed` are removed from the normal document flow, meaning other elements will not make space for them and they can overlap other content.

## Key Takeaways

- Every element is a box with content, padding, border, and margin.
- Always use `* { box-sizing: border-box; }` for intuitive layouts.
- `width`, `height`, and `border` are the foundational properties for defining layout components.
- `block` elements stack vertically; `inline` elements flow horizontally within text.
- Padding is space *inside* a box (background-filled); Margin is space *outside* (transparent).
- Media queries (`@media`) conditionally apply CSS to make websites responsive.
- Use a mobile-first approach with `min-width` for modern responsive design.
- The five `position` values (`static`, `relative`, `absolute`, `fixed`, `sticky`) give you precise control over element placement.