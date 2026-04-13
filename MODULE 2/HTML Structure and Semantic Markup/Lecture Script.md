## LO–1.1.3: Differentiate between HTML, CSS, and JavaScript using real-world analogies (15 minutes)
Modern websites are built with three distinct languages. Understanding these layers is the first and most important mental model in web development. This helps you debug problems faster and organize your code better.

### The Three Web Technologies

| Technology | Role | Real-World Analogy |
|---|---|---|
| HTML | Structure and content | The skeleton / building frame |
| CSS | Visual presentation | Clothing / interior design |
| JavaScript | Behavior and interactivity | The nervous system / electrical wiring |

- **HTML (HyperText Markup Language)** defines *what* is on the page: a heading, a paragraph, an image, a button.
- **CSS (Cascading Style Sheets)** defines *how* things look: colors, fonts, spacing, and layout.
- **JavaScript** defines *how* things behave: what happens when a user clicks a button, opens a menu, or submits a form.

### Step 1: HTML Alone
Start with just HTML — structure and content, no styling:

```html
<h1>My Portfolio</h1>
<p>I am a web developer based in Mumbai.</p>
<button>Contact Me</button>
```
This displays a plain heading, paragraph, and button.

### Step 2: Add CSS
Now add styling. The content has not changed, but it looks dramatically different.

```html
<h1 style="color: navy; font-size: 36px;">My Portfolio</h1>
<p style="color: gray; font-size: 16px;">I am a web developer based in Mumbai.</p>
<button style="background-color: orange; color: white; padding: 10px 20px; border: none;">
  Contact Me
</button>
```

### Step 3: JavaScript Adds Behavior
JavaScript handles interactivity. For example, when a user clicks 'Contact Me,' JavaScript could show a popup or scroll to a contact form.

### Summary
- HTML provides structure and content — the "what".
- CSS provides visual styling and layout — the "how it looks".
- JavaScript provides interactivity and behavior — the "what happens".
- These three technologies work together on every modern website.

## LO–1.1.7: Create a valid HTML document with doctype, html, head, and body tags (15 minutes)
Browsers will try to render poorly structured HTML, but a valid document ensures consistent rendering, accessibility, and search engine indexing. Every professional HTML file starts with the same foundational structure.

### What Makes an HTML Document "Valid"?
A valid HTML5 document must have a DOCTYPE declaration, an `<html>` root element, a `<head>` element, and a `<body>` element.

| Component | Purpose |
|---|---|
| `<!DOCTYPE html>` | Tells the browser to use HTML5 rendering mode |
| `<html>` | Root element — wraps the entire document |
| `<head>` | Metadata — information about the page (not displayed) |
| `<body>` | Content — everything the user sees |

### The Complete Boilerplate

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Portfolio</title>
  </head>
  <body>
    <h1>Welcome to My Portfolio</h1>
    <p>I am a web developer learning HTML and CSS.</p>
    <a href="about.html">Learn more about me</a>
  </body>
</html>
```

1.  **`<!DOCTYPE html>`**: Always the first line. It prevents browsers from entering "quirks mode."
2.  **`<html lang="en">`**: The root element. `lang="en"` helps screen readers and search engines.
3.  **`<head>`**: Contains metadata. `<meta charset="UTF-8">` ensures special characters display correctly. `<meta name="viewport"...>` is for mobile rendering. `<title>` sets the text in the browser tab.
4.  **`<body>`**: Contains all visible content like headings, paragraphs, images, and links.

### Summary
- Every valid HTML5 document starts with `<!DOCTYPE html>`.
- The `<html>` element is the root that wraps all other elements.
- The `<head>` element contains non-visible metadata.
- The `<body>` element contains all visible page content.
- The `<title>` in `<head>` appears in the browser tab.

## LO–1.1.10: Compare heading levels and explain their semantic meaning (10 minutes)
Heading tags directly affect Search Engine Optimization (SEO) and are critical for accessibility. A person using a screen reader can navigate a page using its heading structure, like a table of contents.

### Semantic HTML
"Semantic" means "relating to meaning." A semantic HTML element's tag name communicates the meaning of its content. `<h1>` means "the most important heading."

There are six heading levels, from `<h1>` (most important) to `<h6>` (least important).

### Heading Hierarchy Example
Map your content to an outline first, then write the corresponding HTML.

**Outline:**
```
I. Guide to Traveling India (h1)
  A. Northern India (h2)
    1. Delhi (h3)
  B. Southern India (h2)
    1. Chennai (h3)
```

**HTML:**
```html
<h1>Guide to Traveling India</h1>

<h2>Northern India</h2>
<h3>Delhi</h3>
<p>Delhi has many historical monuments.</p>

<h2>Southern India</h2>
<h3>Chennai</h3>
<p>Chennai is a major cultural and economic hub.</p>
```

### Visual Size Does Not Matter for Heading Choice
Choose heading levels based on semantic correctness, not appearance. If a heading needs to be a different size, use CSS. Never choose a heading tag just for its default size.

```html
<!-- Wrong: choosing h4 because it looks the right size -->
<h4>Northern India</h4>

<!-- Right: use h2 for semantic correctness, control size with CSS -->
<h2 style="font-size: 20px;">Northern India</h2>
```

### Summary
- HTML has six heading levels (`<h1>` to `<h6>`) representing a hierarchy of importance.
- "Semantic" means the tag communicates meaning about the content.
- Search engines and screen readers use headings to understand page structure.
- Choose heading levels based on content hierarchy, not visual appearance. Use CSS to control styling.

## LO–1.1.15: Explain why user input is required in web applications (10 minutes)
Without user input, interactive applications like Google Maps, Amazon, or WhatsApp would be useless. User input is the foundation of all interactive software. A website without user input is a static brochure; a web application with user input is a dynamic product.

### Static vs. Dynamic Web Content
- **Static content**: The same HTML is served to every user (e.g., an "About Us" page).
- **Dynamic content**: The server generates different HTML based on user input (e.g., your personalized Amazon home page).

User input enables authentication, search, data creation, personalization, communication, and transactions.

### User Input Examples
A login form collects user credentials to verify identity and load personalized content.
```html
<h2>Sign In</h2>
<input type="email" placeholder="Email address">
<input type="password" placeholder="Password">
<button type="submit">Sign In</button>
```
A search form allows an application to return results specific to a user's query.
```html
<form action="/search" method="GET">
  <input type="search" name="query" placeholder="Search for anything...">
  <button type="submit">Search</button>
</form>
```

### Summary
- Static websites show the same content to all users.
- Dynamic web applications change content based on user input.
- User input enables authentication, search, personalization, and more.
- HTML provides input elements (`<input>`, `<textarea>`) to collect user data, which is then processed to create dynamic experiences.

## LO–1.2.1: Explain the history of CSS and its role in improving website appearance (15 minutes)
Before CSS, styling was done with HTML tags like `<font>`. To change a color across a 50-page website, you had to edit all 50 files manually. CSS was invented to solve this problem of unmanageable, repetitive styling inside HTML.

### The Timeline of CSS
- **1991**: HTML is invented for structure only.
- **1993-1994**: Browsers add presentational tags (`<font>`, `<center>`), making HTML messy.
- **1996**: CSS Level 1 is published, standardizing basic styling.
- **1998**: CSS Level 2 adds positioning and other advanced features.
- **2001–present**: CSS3 is released in modules, adding modern features like flexbox, grid, and animations.

### The Core Principle: Separation of Concerns
This principle states that each language should have one job. It keeps code organized and allows teams to work independently.
- **HTML** → Structure (what is on the page)
- **CSS** → Presentation (how it looks)
- **JS** → Behavior (what it does)

### Linking CSS to HTML
There are three ways to add CSS, but External CSS is the professional standard because one file can control an entire website.

1.  **Inline CSS**: Style on a single element.
    ```html
    <h1 style="color: blue;">Hello</h1>
    ```
2.  **Internal CSS**: Inside a `<style>` tag in the `<head>`.
    ```html
    <style>
      h1 { color: blue; }
    </style>
    ```
3.  **External CSS**: In a separate `.css` file linked via `<link>`.
    ```html
    <link rel="stylesheet" href="styles.css">
    ```
    `styles.css`:
    ```css
    h1 {
      color: blue;
      font-size: 32px;
    }
    ```

### Summary
- CSS was created to separate visual styling from content structure.
- This separation makes websites easier to maintain, more consistent, and faster to load.
- External CSS is the professional approach, allowing one file to control the appearance of an entire website.

## LO–1.2.7: Apply text-align properties including left, right, center, and justify (15 minutes)
Text alignment is one of the most visible design decisions on a webpage. It signals structure and affects readability. The CSS property that controls this is `text-align`.

### The text-align Property
`text-align` controls how inline content is positioned horizontally within its container element.
- `text-align: left` — aligns text to the left edge (default for English).
- `text-align: right` — aligns text to the right edge.
- `text-align: center` — centers text horizontally.
- `text-align: justify` — stretches each line to fill the full width.

| Value | Common Use |
|---|---|
| `left` | Body text, articles, lists |
| `right` | Prices, dates, table numbers |
| `center` | Hero headings, logos, banners |
| `justify` | Newspaper columns, formal documents |

### Live Coding Demo

**HTML:**
```html
<section class="hero">
  <h1>Build Amazing Websites</h1>
</section>
<article>
  <p class="body-text">This course teaches you HTML and CSS from the ground up.</p>
</article>
<div class="price">$19.99</div>
```

**CSS:**
```css
/* HERO — centered */
.hero {
  text-align: center;
}

/* ARTICLE — justified */
.body-text {
  text-align: justify;
}

/* PRICE — right-aligned */
.price {
  text-align: right;
  font-weight: bold;
}
```

**Important**: `text-align` is applied to the container element, not the text itself. It tells the container how to position its inline content. It is also inherited, so children elements will adopt the parent's text alignment unless overridden.

### Summary
- Four values: `left`, `right`, `center`, `justify`.
- Use `center` for headings, `left` for body text, `right` for prices, and `justify` sparingly.
- The `text-align` property is set on the parent container to align its child inline content.

## LO–1.3.1: Explain what CSS selectors are and why they are used instead of inline styles (15 minutes)
Imagine a website with 100 product cards, each with a price in red. If the client asks to make the price green, using inline styles would require changing 100 HTML elements manually. CSS selectors solve this problem by allowing you to write a rule once and apply it everywhere.

### What are CSS Selectors?
A CSS selector is a pattern that matches one or more HTML elements. The CSS properties defined in the rule are then applied to those elements.

Anatomy of a CSS rule:
```css
selector {
  property: value;
}
```

### The Inline Problem vs. A Selector Solution
**Inline (Repetitive and Hard to Maintain):**
```html
<p style="color: red; font-size: 18px;">Paragraph 1</p>
<p style="color: red; font-size: 18px;">Paragraph 2</p>
```

**Selector (Efficient and Scalable):**
In an external `styles.css` file:
```css
/* styles.css */
p {
  color: red;
  font-size: 18px;
}
```
In the HTML file:
```html
<link rel="stylesheet" href="styles.css">
<p>Paragraph 1</p>
<p>Paragraph 2</p>
```
With the selector, changing the color from red to green requires editing only one line in the CSS file.

### Summary
- CSS selectors target HTML elements to apply styles.
- They eliminate the need for repetitive inline styles.
- They enable the separation of structure (HTML) and presentation (CSS), making code scalable and easy to update.

## LO–1.3.3: Apply tag selectors to style HTML elements (15 minutes)
Tag selectors are the most straightforward type of selector. They target every element of a given HTML tag. They are used to set a consistent baseline for an element type across an entire page.

### What is a Tag Selector?
A tag selector is written as the HTML element name without angle brackets. This rule matches every `<p>` element in the document.

```css
p {
  color: red;
}
```

### Styling a Navigation Bar
Three tag selectors (`ul`, `li`, `a`) can work together to build a styled component.

```html
<style>
  ul {
    background-color: #222;
    padding: 10px;
    list-style: none;
  }
  li {
    display: inline;
    margin-right: 16px;
  }
  a {
    color: white;
    text-decoration: none;
    font-weight: bold;
  }
</style>
<ul>
  <li><a href="#">Home</a></li>
  <li><a href="#">About</a></li>
  <li><a href="#">Contact</a></li>
</ul>
```
This example shows how base styles for lists, list items, and links can be defined site-wide.

### Summary
- Tag selectors match all HTML elements of a given type.
- The syntax is `tagname { property: value; }`.
- They are best used for setting baseline or default styles for common HTML elements like `p`, `h1`, and `a`.

## LO–1.3.5: Use ID selectors to uniquely target specific elements (15 minutes)
Tag selectors are too broad. Sometimes you need to target just one specific element on a page, like a main header or footer. An ID selector finds exactly one element on the page, like a unique student ID number.

### What is an ID Selector?
An ID selector uses a `#` prefix with the ID value from the HTML `id` attribute.

**CSS:**
```css
#page-title {
  color: darkblue;
}
```
**HTML:**
```html
<h1 id="page-title">My Portfolio</h1>
```
**Rules of IDs:**
- The `id` value must be unique on the page.
- It must start with a letter.
- The CSS selector uses a `#` prefix.

### Page Layout with ID Selectors
IDs are commonly used to style major sections of a page layout.

```html
<header id="site-header">...</header>
<main id="content">...</main>
<footer id="site-footer">...</footer>
```
```css
#site-header {
  background-color: #1a1a2e;
  color: white;
  padding: 20px;
}
#content {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
}
#site-footer {
  background-color: #0f3460;
  color: white;
  text-align: center;
  padding: 20px;
}
```
**What to avoid**: Never use the same ID on two elements. This is invalid HTML. If multiple elements need the same style, use a class.

### Summary
- ID selectors target a single unique element using the `#` prefix.
- The `id` attribute on the HTML element must be unique per page.
- IDs are ideal for styling major page landmarks like headers, footers, and main content areas.

## LO–1.3.7: Use class selectors to target groups of elements (10 minutes)
Tag selectors are too broad, and ID selectors are too narrow. Class selectors provide the middle ground, allowing you to style a specific group of elements, even if they are different tag types.

### What is a Class Selector?
Class selectors use a `.` (dot) prefix in CSS and are applied to elements using the `class` attribute in HTML.

**CSS:**
```css
.class-name {
  property: value;
}
```
**HTML:**
```html
<element class="class-name">...</element>
```
**Key differences from IDs:**
- Multiple elements can share the same class.
- One element can have multiple classes (separated by spaces).
- They are highly reusable.

### Product Cards Example
Classes are perfect for styling reusable components like cards.

```html
<style>
  .card {
    border: 1px solid #ccc;
    padding: 16px;
    border-radius: 6px;
    margin-bottom: 12px;
  }
</style>

<div class="card">Laptop - $999</div>
<div class="card">Mouse - $29</div>
<div class="card">Keyboard - $79</div>
```
Here, one `.card` rule styles all three product containers. This is efficient and easy to update.

A class can also be applied to different tag types to create a shared style.
```html
<style>
  .warning {
    color: red;
    font-weight: bold;
  }
</style>

<p class="warning">This paragraph is a warning.</p>
<li class="warning">This list item is a warning.</li>
```

### Summary
- Class selectors use the `.` prefix in CSS and the `class` attribute in HTML.
- Any number of elements can share a class, and they work across different tag types.
- They are the most commonly used selectors for styling components in real-world CSS.