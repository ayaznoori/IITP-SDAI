## The CSS Box Model

In CSS, every element is a rectangular box. This box is composed of four layers:

1.  **Content**: The text, image, or other elements inside.
2.  **Padding**: Transparent space inside the element, between the content and the border.
3.  **Border**: A line that goes around the padding and content.
4.  **Margin**: Transparent space outside the element, pushing other elements away.

By default, an element's `width` and `height` properties only define the size of the content area. To make layout math more intuitive, it is standard practice to add the following rule to your CSS. This makes `width` and `height` include padding and border.

```css
* {
  box-sizing: border-box;
}
```

## Core Layout Properties

-   **`width`**: Controls the horizontal size of an element.
-   **`height`**: Controls the vertical size. `min-height` is often preferred for content boxes, as it allows the box to grow if the content is long.
-   **`border`**: Defines the visible boundary of a box.
-   **`border-radius`**: Rounds the corners of a box. `border-radius: 50%` will create a circle if the element's width and height are equal.

## Block vs. Inline Elements

-   **Block elements** (`<div>`, `<p>`, `<h1>`) take up the full available width and start on a new line. They respect `width` and `height`.
-   **Inline elements** (`<span>`, `<a>`, `<strong>`) take up only as much space as their content needs and flow within text. They ignore `width` and `height`.

## Margin vs. Padding

-   **Padding** is space **inside** an element. The element's background color will fill the padded area.
-   **Margin** is space **outside** an element. It is always transparent.

## Responsive Design and Media Queries

Websites today must work on a wide range of devices. **Responsive Web Design** is the practice of creating a single website that adapts its layout to the screen size it's being viewed on.

The primary tool for this is the **media query**. A media query is a CSS feature that applies a block of styles only when a certain condition is met.

```css
/* Base styles (for mobile) */
.grid {
  grid-template-columns: 1fr; /* Single column */
}

/* Styles for larger screens (tablets and up) */
@media (min-width: 768px) {
  .grid {
    grid-template-columns: 1fr 1fr; /* Two columns */
  }
}
```

This example uses a "mobile-first" approach with `min-width`. The default style is a single column, which is suitable for phones. When the screen is 768px or wider, the media query activates and changes the layout to a two-column grid.

## The Five CSS `position` Types

CSS provides five values for the `position` property to control element placement.

| Value      | Description                                                          |
| ---------- | -------------------------------------------------------------------- |
| `static`   | Default. Element follows normal page flow.                           |
| `relative` | Element can be shifted, but its original space is preserved.         |
| `absolute` | Element is removed from flow and positioned relative to an ancestor. |
| `fixed`    | Element is removed from flow and positioned relative to the window.  |
| `sticky`   | Behaves like `relative` until a scroll point, then acts like `fixed`.  |