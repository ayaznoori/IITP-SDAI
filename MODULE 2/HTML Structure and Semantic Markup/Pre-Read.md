## The Three Languages of the Web

Every website you visit is built using three core technologies: HTML, CSS, and JavaScript. Each has a distinct role.

Think of building a house:
- **HTML is the structure** — the walls, floors, and doors. It defines what exists.
- **CSS is the interior design** — the paint color, furniture, and lighting. It controls how everything looks.
- **JavaScript is the plumbing and electricity** — the systems that make things work, like a light switch.

On any modern website, HTML provides the content skeleton, CSS dresses it up visually, and JavaScript makes it interactive.

## The HTML Document Structure

Every HTML file follows a standard format that ensures browsers render it correctly. A complete, valid HTML document looks like this:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>My First Web Page</title>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <p>This is my first web page.</p>
  </body>
</html>
```

- **`<!DOCTYPE html>`**: The first line, which tells the browser the file is HTML5.
- **`<html>`**: Wraps the entire document.
- **`<head>`**: Contains metadata *about* the page. The `<title>` appears in the browser tab.
- **`<body>`**: Contains all visible content that users see in the browser window.

## Semantic Headings

Heading tags (`<h1>` to `<h6>`) are more than just big text; they have **semantic meaning**. `<h1>` doesn't just say "make this text big"—it says "this is the most important heading in this document."

**The Golden Rule**: Choose heading levels based on the **outline of your content**, not on how big you want the text to look. Use CSS to change the visual appearance. Search engines and screen readers rely on a correct heading structure to understand and navigate your page.

## User Input in Web Applications

Early websites were **static**—every visitor saw the same content. Modern web applications are **dynamic**—content changes based on user input. Without user input, applications like Google Search, Amazon, and Gmail would be impossible.

HTML's `<input>` element is the primary way to collect user data:

```html
<!-- Collect a name -->
<input type="text" placeholder="Your name">
<!-- Collect an email address -->
<input type="email" placeholder="your@email.com">
```
These elements are the gateway to all user-driven functionality in web applications.

## Introduction to CSS

In the early web, styling was mixed into HTML, making it bloated and hard to maintain. CSS (Cascading Style Sheets) was created to solve this by enabling **separation of concerns**: HTML handles structure, and CSS handles presentation.

This separation makes websites more consistent, easier to maintain, and faster to load. Every modern website uses CSS to control its visual appearance.

## The `text-align` Property

The `text-align` property controls the horizontal alignment of text within a block element. It has four main values:
- `left`: Aligns text to the left (default).
- `right`: Aligns text to the right.
- `center`: Centers text horizontally.
- `justify`: Stretches text to fill the container's full width.

```css
h1 {
  text-align: center; /* centered page title */
}
.price {
  text-align: right; /* right-aligned price */
}
p {
  text-align: left; /* left-aligned paragraph */
}
```

## CSS Selectors: The Foundation of CSS

A CSS selector is a pattern used to identify which HTML elements a set of CSS rules should apply to. Using selectors with an external stylesheet is the professional standard, as it avoids repeating code and makes updates simple.

### The Three Main Selector Types

1.  **Tag Selector**: Targets all elements of a specific tag.
    ```css
    p {
      color: gray;
    }
    ```

2.  **ID Selector**: Targets a single, unique element with a specific `id`. Uses a `#` prefix.
    ```css
    #main-title {
      font-size: 48px;
    }
    ```
    The `id` attribute in HTML must be unique on the page.

3.  **Class Selector**: Targets all elements that share a `class` name. Uses a `.` prefix.
    ```css
    .highlight {
      background-color: yellow;
    }
    ```
    Classes are reusable and can be applied to many elements.

## Key Takeaways

- Websites use three complementary technologies: HTML (structure), CSS (style), and JavaScript (behavior).
- Every valid HTML page has a standard structure with `DOCTYPE`, `html`, `head`, and `body`.
- Use heading tags (`h1`-`h6`) to create a logical outline for your content. Their meaning is more important than their default look.
- User input is what enables dynamic and personalized web experiences.
- CSS was invented to separate styling from HTML, making code cleaner and easier to manage.
- CSS selectors are patterns that target HTML elements. The most common types are tag, ID, and class selectors.
- For styling, prefer reusable classes (`.card`) over unique IDs (`#header`), and use IDs only for major, one-off page sections.