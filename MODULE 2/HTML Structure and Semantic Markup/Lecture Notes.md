## The Three Core Web Technologies

| Technology | Role | Question It Answers |
|---|---|---|
| HTML | Structure & content | What is on the page? |
| CSS | Visual styling & layout | How does it look? |
| JavaScript | Behavior & interactivity | What does it do? |

### House Analogy
- **HTML** = Walls, floors, rooms — the physical structure
- **CSS** = Paint, furniture, lighting — the interior design
- **JavaScript** = Plumbing, electricity — the working systems

HTML defines what content exists, CSS controls how it looks, and JavaScript defines what happens in response to user actions. These three technologies are **complementary, not interchangeable**.

## Valid HTML Document Structure

Every HTML5 document must start with this boilerplate structure:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title Here</title>
  </head>
  <body>
    <!-- All visible content goes here -->
  </body>
</html>
```

### Breakdown of Each Part
- **`<!DOCTYPE html>`**: The very first line. Tells the browser to use HTML5 rendering mode.
- **`<html lang="en">`**: The root element that wraps everything. The `lang` attribute helps with accessibility and SEO.
- **`<head>`**: Contains metadata not visible on the page, such as `<title>` (for the browser tab), `<meta charset="UTF-8">` (for character encoding), and links to stylesheets.
- **`<body>`**: Contains all visible content the user sees, such as headings, paragraphs, images, and links.

## Heading Levels and Semantic Meaning

A semantic HTML element communicates meaning about its content through its tag name. There are six heading levels, `<h1>` (most important) to `<h6>` (least important).

**Rules for Headings:**
1.  Use only one `<h1>` per page for the main title.
2.  Never skip heading levels (e.g., don't jump from `<h1>` to `<h3>`).
3.  Choose levels for hierarchy and structure, not for visual appearance. Use CSS to control size and styling.

Proper heading semantics are crucial for SEO, as search engines use headings to understand page topics, and for accessibility, as screen reader users navigate pages via headings.

## Why User Input is Required

| Type | Description | Example |
|---|---|---|
| Static | Same content for every user | Company homepage, Wikipedia article |
| Dynamic | Content changes based on user input | Amazon, Gmail, Google Search |

User input is what transforms a static page into an interactive, dynamic application. It enables features like authentication (login), search, personalization, communication, and e-commerce transactions.

HTML provides elements to collect this data, such as `<input>`, `<textarea>`, and `<select>`.

## The History and Role of CSS

Before CSS, styling was mixed with HTML content using tags like `<font>`, which made websites difficult to maintain. CSS was created to solve this by separating presentation from structure.

### The Core Principle: Separation of Concerns
- **HTML** → Structure (content)
- **CSS** → Presentation (appearance)
- **JS** → Behavior (interactivity)

This separation makes code easier to maintain, more scalable, and allows teams to work independently.

### Three Ways to Add CSS
1.  **Inline**: `<h1 style="color: blue;">` (Avoid in production)
2.  **Internal**: Inside a `<style>` tag in the `<head>` (For single-page demos)
3.  **External**: In a separate `.css` file linked via `<link>` (Professional standard)

External CSS is best because one file can control an entire website and browsers can cache the file, making pages load faster.

## Applying text-align Properties

The `text-align` property controls the horizontal alignment of inline content (text) within a block element.

| Value | Best Used For |
|---|---|
| `left` | Body text, articles, long-form content (default) |
| `right` | Prices, numbers, dates |
| `center` | Hero headings, banners, logos |
| `justify` | Newspaper/magazine style articles |

**Important Behaviors**:
- `text-align` is set on the **parent container**, not the text itself.
- It is an **inherited** property, meaning children will adopt the parent's alignment unless overridden.
- It does not center block elements; for that, use `margin: 0 auto`.

## CSS Selectors

A CSS selector is a pattern that identifies which HTML elements a CSS rule applies to. Using selectors with external stylesheets is far superior to inline styles because it avoids repetition, makes updates easy, and separates concerns.

**Anatomy of a CSS rule:**
```css
selector {
  property: value;
}
```

### Comparison: Tag vs ID vs Class Selector

| Feature | Tag | ID | Class |
|---|---|---|---|
| CSS prefix | none | `#` | `.` |
| HTML attribute | none | `id` | `class` |
| Targets | All of a tag type | Exactly one element | Selected groups of elements |
| Reusable | Yes (forced) | No (unique) | Yes (by choice) |

- **Tag Selector** (`p`): Targets all HTML elements with a specific tag name. Best for setting baseline styles.
- **ID Selector** (`#main-header`): Targets a single, unique element. The `id` attribute must be unique per page. Best for major page landmarks.
- **Class Selector** (`.card`): Targets all elements with a matching `class` attribute. Classes are reusable and can be applied to any element, any number of times. They are the most common type of selector.

## Key Takeaways

- **Three Layers**: Websites are built with HTML for structure, CSS for presentation, and JavaScript for interactivity.
- **Valid HTML**: A valid document requires `<!DOCTYPE html>`, `<html>`, `<head>`, and `<body>` tags.
- **Semantic HTML**: Use tags like `<h1>`-`<h6>` for their meaning and to create a logical document structure, not just for their default appearance.
- **Dynamic Applications**: User input is the foundation of dynamic web applications, enabling features like search, login, and personalization.
- **Separation of Concerns**: CSS separates style from HTML structure, making websites easier to maintain.
- **External CSS**: Always use external stylesheets (`.css` files) for professional web development.
- **Selectors**: CSS selectors are patterns that target HTML elements. They are the key to writing scalable, maintainable styles.
- **Selector Types**:
    - **Tag selectors** (`p`) apply styles to all elements of a certain type.
    - **ID selectors** (`#unique-element`) apply styles to one specific element.
    - **Class selectors** (`.reusable-component`) are the most common, applying styles to groups of elements.