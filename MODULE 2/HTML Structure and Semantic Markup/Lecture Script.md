## LO–1.1.3: Differentiate between HTML, CSS, and JavaScript using real-world analogies (15 minutes)

# Lecture Script: HTML, CSS, and JavaScript

## Why — Real-World Importance

Ask the class: "When you look at a website like Amazon or Netflix, what do you see?"

Take a few responses. Students will mention: text, images, buttons, colors, layouts, animations, search boxes.

Now ask: "Do you think all of that was built with one tool?"

The answer is no. Modern websites are built with three distinct languages, each responsible for a different layer of the experience. Understanding these layers is the first and most important mental model in web development. Before writing a single line of code, a developer should be able to look at any website and think: "That is HTML. That styling is CSS. That behavior is JavaScript."

This mental model helps you debug problems faster, organize your code better, and collaborate with other developers who specialize in one of these layers.

---

## What — Concept Definition

### The Three Web Technologies

| Technology | Role | Real-World Analogy |
|---|---|---|
| HTML | Structure and content | The skeleton / building frame |
| CSS | Visual presentation | Clothing / interior design |
| JavaScript | Behavior and interactivity | The nervous system / electrical wiring |

### HTML — HyperText Markup Language
HTML defines *what* is on the page. It answers: Is this a heading? A paragraph? An image? A form? A button? HTML gives content meaning and structure. Without HTML, the browser has nothing to display.

### CSS — Cascading Style Sheets
CSS defines *how* things look. It answers: What color is that heading? How big is that button? How much spacing is between paragraphs? How does the layout change on a mobile phone? CSS is entirely about visual presentation and layout.

### JavaScript
JavaScript defines *how* things behave. It answers: What happens when a user clicks a button? How does the dropdown menu open? How does the page update without reloading when you add an item to a cart? JavaScript adds interactivity, animation, and dynamic behavior.

---

## How — Step by Step with Code

### Step 1: HTML Alone

Start with just HTML — structure and content, no styling:

```html
<h1>My Portfolio</h1>
<p>I am a web developer based in Mumbai.</p>
<button>Contact Me</button>
```

This displays a heading, a paragraph, and a button. They look plain — browser default fonts, no special colors.

### Step 2: Add CSS

Now add styling. With CSS, you can change how everything looks:

```html
<h1 style="color: navy; font-size: 36px;">My Portfolio</h1>
<p style="color: gray; font-size: 16px;">I am a web developer based in Mumbai.</p>
<button style="background-color: orange; color: white; padding: 10px 20px; border: none;">
  Contact Me
</button>
```

The content has not changed. The same heading, paragraph, and button now look dramatically different.

### Step 3: JavaScript Adds Behavior

JavaScript would handle the button click. Without getting into JavaScript syntax, describe the concept:

"When a user clicks 'Contact Me,' JavaScript could show a popup, scroll to a contact form, or send the user to a new page — all dynamically, without a page reload."

### Demo Suggestion

1. Open any website (e.g., a news site). In browser DevTools, go to the Elements panel and delete a few HTML elements — show how content disappears.
2. In the same DevTools, select an element and change its CSS color — show how visual appearance changes instantly without changing content.
3. Describe the JavaScript behavior students see every day: dropdown menus, image sliders, live search, notifications.

---

## The Human Body Analogy

An even more vivid analogy for some students:

- **HTML = skeleton** — gives the body its shape and structure
- **CSS = skin, hair, clothing** — determines how the body looks
- **JavaScript = muscles and nervous system** — makes the body move and respond

You cannot have a body with just skin (CSS without HTML). And a skeleton without muscles cannot do anything (HTML + CSS without JavaScript on an interactive site).

---

## Summary

- HTML provides structure and content — the "what"
- CSS provides visual styling and layout — the "how it looks"
- JavaScript provides interactivity and behavior — the "what happens"
- These three technologies work together on every modern website
- Each has a distinct, irreplaceable role


---

## LO–1.1.7: Create a valid HTML document with doctype, html, head, and body tags (15 minutes)

# Lecture Script: Creating a Valid HTML Document

## Why — Real-World Importance

"What happens when you bake a cake without following the recipe in the right order? You might add ingredients too late, or in the wrong proportion. The result might still look like a cake, but it probably will not taste right."

An HTML document works similarly. Browsers are tolerant — they will try to render even badly structured HTML. But when you skip the required boilerplate or structure it incorrectly:
- The page may render differently in different browsers
- Accessibility tools may not work correctly
- Search engines may not index the page properly
- CSS and JavaScript may behave unexpectedly

Writing a valid HTML document from scratch is a skill every web developer must master. Every professional HTML file starts with the same foundational structure we cover in this lesson.

---

## What — Concept Definition

### What Makes an HTML Document "Valid"?

A valid HTML document follows the rules defined by the HTML standard. For HTML5, a valid document must have:

1. A DOCTYPE declaration
2. An `<html>` root element
3. A `<head>` element inside `<html>`
4. A `<body>` element inside `<html>`

These four components are non-negotiable for a properly structured HTML5 document.

### The Purpose of Each Component

| Component | Purpose |
|---|---|
| `<!DOCTYPE html>` | Tells the browser to use HTML5 rendering mode |
| `<html>` | Root element — wraps the entire document |
| `<head>` | Metadata — information about the page (not displayed) |
| `<body>` | Content — everything the user sees |

---

## How — Step by Step with Code

### Step 1: The DOCTYPE Declaration

```html
<!DOCTYPE html>
```

This is always the very first line. It is not an HTML element — it is a processing instruction. Without it, browsers may enter "quirks mode," an older compatibility mode that renders pages differently.

In older HTML versions, DOCTYPE declarations were long and complex. HTML5 simplified it to just `<!DOCTYPE html>`.

### Step 2: The `<html>` Element

```html
<!DOCTYPE html>
<html lang="en">

</html>
```

The `<html>` element is the root — everything else goes inside it. The `lang="en"` attribute tells the browser the page is in English, which helps screen readers use the correct pronunciation and helps search engines categorize the content.

### Step 3: The `<head>` Element

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Web Page</title>
  </head>
</html>
```

Three things in `<head>`:
- `<meta charset="UTF-8">` — Ensures special characters (é, ñ, ₹, etc.) display correctly
- `<meta name="viewport" ...>` — Makes the page render correctly on mobile devices
- `<title>` — The text shown in the browser tab and bookmarks

None of this content appears on the page. It is metadata — information about the page.

### Step 4: The `<body>` Element

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

Everything inside `<body>` is what the user sees. This is where all your headings, paragraphs, images, links, forms, and other content goes.

### Demo Suggestion

1. Open a plain text editor (VS Code, Notepad, or similar).
2. Type the complete boilerplate live, explaining each line as you go.
3. Save the file as `index.html`.
4. Open it in a browser — show the tab title matching the `<title>` tag.
5. Open DevTools and show the full structure in the Elements panel.
6. Change the `<title>` content and refresh — show the tab title updating.

Then demonstrate what happens without DOCTYPE: paste the same content into a new file without `<!DOCTYPE html>` and show in DevTools that the browser may add `<html>`, `<head>`, and `<body>` automatically — but in quirks mode.

---

## Summary

- Every valid HTML5 document starts with `<!DOCTYPE html>`
- The `<html>` element is the root that wraps all other elements
- The `<head>` element contains metadata — not displayed on the page
- The `<body>` element contains all visible page content
- The `<title>` in `<head>` appears in the browser tab
- `<meta charset="UTF-8">` ensures special characters display correctly
- Browsers can render invalid HTML, but valid HTML ensures consistent behavior


---

## LO–1.1.10: Compare heading levels and explain their semantic meaning (10 minutes)

# Lecture Script: Heading Levels and Semantic Meaning

## Why — Real-World Importance

Ask students: "Have you ever searched something on Google and noticed that the result shows a bold title and a short description below it? That title comes from the `<h1>` or page `<title>` of the website. The description often comes from the first `<h2>` or `<p>` on the page."

This is called **SEO — Search Engine Optimization**. The heading tags you choose directly affect where your website ranks in search results. A page with a well-structured heading hierarchy gets ranked more relevantly than a page where all text is in `<p>` tags with no headings.

Beyond SEO, heading tags are critical for **accessibility**. A person using a screen reader can ask their device "show me all the headings on this page" — this gives them a table of contents they can use to jump directly to the section they want. If your headings are wrong or missing, this navigation breaks entirely.

Good use of heading levels is not optional best practice — it is a professional responsibility.

---

## What — Concept Definition

### Semantic HTML

"Semantic" means "relating to meaning." A semantic HTML element is one whose tag name communicates something meaningful about the content it wraps.

- `<h1>` says: "This is the most important heading — the top-level title."
- `<h2>` says: "This is an important section heading, one level below the title."
- `<p>` says: "This is a paragraph of body text."

Compare to non-semantic: `<div>` and `<span>` say nothing about meaning. They are generic containers.

### The Six Heading Levels

```html
<h1>Largest / Most Important</h1>
<h2>Second Level</h2>
<h3>Third Level</h3>
<h4>Fourth Level</h4>
<h5>Fifth Level</h5>
<h6>Smallest / Least Important</h6>
```

In practice:
- `h1`, `h2`, `h3` are used frequently
- `h4` occasionally
- `h5` and `h6` rarely — most content does not need six levels of hierarchy

---

## How — Step by Step with Code

### Step 1: Visualizing a Heading Hierarchy

Map content to an outline first, then write headings:

**Outline:**
```
I. Guide to Traveling India (h1)
  A. Northern India (h2)
    1. Delhi (h3)
      a. Historical Sites (h4)
    2. Agra (h3)
  B. Southern India (h2)
    1. Chennai (h3)
    2. Bangalore (h3)
```

**HTML:**
```html
<h1>Guide to Traveling India</h1>

<h2>Northern India</h2>
<h3>Delhi</h3>
<h4>Historical Sites</h4>
<p>Delhi has many historical monuments including the Red Fort and Qutub Minar.</p>
<h3>Agra</h3>
<p>Agra is home to the Taj Mahal, one of the Seven Wonders of the World.</p>

<h2>Southern India</h2>
<h3>Chennai</h3>
<p>Chennai is a major cultural and economic hub with beautiful beaches.</p>
<h3>Bangalore</h3>
<p>Bangalore is India's technology capital, home to many global tech companies.</p>
```

### Step 2: Why Visual Size Does Not Matter for Heading Choice

Demonstrate this concept:

"What if a designer wants the 'Northern India' section title to be smaller than the article title but slightly larger than default? Should they use `<h4>` instead of `<h2>`? No! They should use `<h2>` and apply CSS to control the size."

```html
<!-- Wrong: choosing h4 because it looks the right size -->
<h4>Northern India</h4>

<!-- Right: use h2 for semantic correctness, control size with CSS -->
<h2 style="font-size: 20px;">Northern India</h2>
```

### Step 3: What Screen Readers Announce

Walk through a simulated screen reader experience verbally:

"A screen reader on a Wikipedia article about India would announce: Heading level 1: India. Heading level 2: History. Heading level 3: Ancient period. Heading level 3: Medieval period. Heading level 2: Geography. And so on."

The user can say "next heading" and jump directly to the next section — just like a table of contents. This only works if heading levels are used correctly.

### Demo Suggestion

1. Write a page with correct heading hierarchy. Use a browser accessibility tool (Chrome's Lighthouse or the "Accessibility Tree" in DevTools) to show how headings are exposed to assistive technologies.
2. Alternatively, show this using a free screen reader (NVDA on Windows or the built-in VoiceOver on Mac) navigating by headings.
3. Show a Google search result and trace: "this bold title came from the `<h1>` or `<title>` of this page."

---

## Summary

- HTML has six heading levels (`<h1>` to `<h6>`) representing a hierarchy of importance
- "Semantic" means the tag communicates meaning about the content
- `<h1>` signals the most important heading; `<h6>` signals the least
- Search engines use headings to understand page content (SEO)
- Screen readers use headings to enable navigation for users with visual impairments
- Choose heading levels based on content hierarchy, not visual appearance
- Use CSS to control how headings look — never change heading level just for size


---

## LO–1.1.15: Explain why user input is required in web applications (10 minutes)

# Lecture Script: Why User Input is Required

## Why — Real-World Importance

Open your phone and pick any app — maps, social media, banking, shopping.

"Ask yourself: what would this app be without user input? Google Maps without the ability to type a destination. Swiggy without the ability to type your address. WhatsApp without the ability to type a message."

The answer is: these apps would be completely useless. User input is not a feature — it is the foundation of all interactive software.

In web development terms: a website without user input is a **brochure**. A web application with user input is a **product**. The difference is everything.

---

## What — Concept Definition

### Static vs. Dynamic Web Content

**Static content**: The same HTML is served to every user. A company's "About Us" page is the same regardless of who visits it.

**Dynamic content**: The server generates different HTML based on user input. Your Amazon home page shows different products than your friend's — because Amazon uses your search history, location, and preferences (all forms of user input) to personalize your experience.

### What User Input Enables

1. **Authentication**: Login forms verify who you are
2. **Search**: Query boxes find content relevant to you
3. **Data creation**: Forms let users create posts, orders, messages
4. **Personalization**: Preferences, settings, saved items
5. **Communication**: Contact forms, chat, email composition
6. **Transactions**: Payment forms, booking systems

Without user input, none of these are possible.

---

## How — Step by Step with Code

### Step 1: The Problem — Static Content

```html
<!-- Static: same for everyone -->
<h1>Welcome to Our Store!</h1>
<p>Browse our products below.</p>
```

This is fine for a home page — but it cannot show personalized recommendations, process orders, or let users log in.

### Step 2: Introducing User Input

The `<input>` element collects data from users:

```html
<!-- Login form: user inputs their credentials -->
<h2>Sign In</h2>
<input type="email" placeholder="Email address">
<input type="password" placeholder="Password">
<button type="submit">Sign In</button>
```

Once the user enters their email and password, the application can:
- Verify their identity
- Load their saved preferences
- Show their order history
- Display personalized content

### Step 3: A Search Example

```html
<!-- Search: the most fundamental user input -->
<form action="/search" method="GET">
  <input type="search" name="query" placeholder="Search for anything...">
  <button type="submit">Search</button>
</form>
```

Every search on the web works this way. The user types a query, and the application returns results specific to that query.

### Step 4: A Contact Form

```html
<h2>Send Us a Message</h2>
<input type="text" placeholder="Your name" required>
<input type="email" placeholder="Your email" required>
<textarea placeholder="Your message..." rows="5"></textarea>
<button type="submit">Send Message</button>
```

Without this form, users have no way to communicate with the business. With it, the application can receive messages, store them in a database, and send automated responses.

### Demo Suggestion

1. Open a major website (Amazon, Google, or LinkedIn).
2. Ask: "What inputs can you see on this page?" Guide students to notice search bars, login forms, filter controls.
3. Disable JavaScript in the browser and reload — show how many interactive features break (because they rely on user input being processed by JavaScript).
4. Point out that the HTML input elements themselves work without JavaScript — HTML provides the collection mechanism.

---

## Summary

- Static websites show the same content to all users — no input needed
- Dynamic web applications change content based on user input
- User input enables: authentication, search, personalization, communication, transactions
- HTML provides input elements (`<input>`, `<textarea>`, `<select>`) to collect user data
- The data collected via HTML inputs is processed by JavaScript (front-end) and server code (back-end) to create dynamic experiences


---

## LO–1.2.1: Explain the history of CSS and its role in improving website appearance (15 minutes)

# Lecture Script: The History of CSS and Its Role in Web Design

## Why (5 minutes)

Open the lecture by pulling up a web browser and navigating to a modern website like Wikipedia or a news site. Ask students: "How do you think all this color, layout, and typography is controlled?"

Explain the problem that existed before CSS:

"Imagine being a developer in 1995. You have a website with 50 pages. Your boss says, 'Change all the text from black to navy blue.' Without CSS, you would have to open all 50 HTML files and change every single `<font color>` tag manually. That could be thousands of changes, and one missed file would make the site inconsistent."

This is the *why* behind CSS: **CSS was invented to solve the problem of unmanageable, repetitive styling inside HTML.**

Ask students: "What would make your life easier if you had to make a design change across a hundred pages?" Let them answer. Then reveal that CSS is exactly that solution: one file that controls the appearance of an entire website.

---

## What (10 minutes)

### The Timeline of CSS

Walk students through the history:

- **1991**: Tim Berners-Lee invents HTML and the web. HTML is purely structural.
- **1993-1994**: Browsers start adding presentational tags (`<font>`, `<center>`, `bgcolor`). HTML gets messy.
- **1994**: Håkon Wium Lie proposes CSS. Bert Bos joins the effort.
- **1996**: CSS Level 1 (CSS1) published as a W3C recommendation. Covers basic font, color, and spacing.
- **1998**: CSS Level 2 (CSS2) published. Adds positioning, z-index, media types.
- **2001–present**: CSS3 released in modules. Adds animations, gradients, flexbox, grid, custom properties.

Write these dates on the board or in a slide. Ask: "Why do you think CSS was split into modules for CSS3 instead of one big release?" (Answer: different features were ready at different times, and this allowed browsers to implement features incrementally.)

### The Core Principle: Separation of Concerns

Draw a simple diagram:

```
HTML ──► Structure (what is on the page)
CSS  ──► Presentation (how it looks)
JS   ──► Behavior (what it does)
```

Explain: "This is called separation of concerns. Each language has one job. When concerns are separated, teams can work independently. A designer edits the CSS without touching the developer's HTML. A developer adds new content to the HTML without breaking the designer's styles."

### What CSS Actually Does

Show a simple HTML page with no CSS:

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Page</title>
</head>
<body>
  <h1>Hello World</h1>
  <p>This is a paragraph.</p>
  <a href="#">Click here</a>
</body>
</html>
```

Then show the same page after adding a `<link>` to a CSS file. Demonstrate how dramatically the appearance changes.

---

## How (8 minutes)

### Linking CSS to HTML

Demonstrate the three ways to add CSS:

**Inline CSS** — style on one element:
```html
<h1 style="color: blue; font-size: 32px;">Hello</h1>
```

**Internal CSS** — inside a `<style>` tag in `<head>`:
```html
<head>
  <style>
    h1 {
      color: blue;
      font-size: 32px;
    }
  </style>
</head>
```

**External CSS** — separate `.css` file linked via `<link>`:
```html
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

In `styles.css`:
```css
h1 {
  color: blue;
  font-size: 32px;
}
```

Explain: "External CSS is the professional approach. It lets you control all pages from one file. Inline CSS is the least maintainable — avoid it for anything beyond quick tests."

### Demo Suggestion

**Live Demo**: Start with a plain HTML page in the browser. Open VS Code, create a `styles.css` file, link it, and add styles step by step:
1. Change the `body` background color.
2. Change the `h1` color and font size.
3. Change the `p` font family.

Each step, save and refresh the browser to show the change. This reinforces the real-time feedback loop of CSS development.

---

## Wrap-Up (2 minutes)

Summarize: "CSS was created to separate visual styling from content structure. This separation makes websites easier to maintain, more consistent, and faster to load. Every website today uses CSS — it is not optional for web development."

Preview the next topic: "Now that we understand what CSS is and where it came from, we will learn how to actually write CSS rules and apply different styling properties."


---

## LO–1.2.7: Apply text-align properties including left, right, center, and justify (15 minutes)

# Lecture Script: Applying text-align Properties

## Why (4 minutes)

Ask students to pull up any major website — Google, Amazon, LinkedIn. Point to the page:

"Look at the hero section at the top of this page. The heading is centered. Look at the navigation — those links might be right-aligned. Look at the main content — the text is left-aligned."

"Text alignment is one of the most visible and impactful design decisions on a webpage. It signals structure. Centered text feels balanced and formal — great for headlines. Left alignment feels natural and readable for long text. Right alignment draws the eye to the right — useful for prices, dates, and secondary information."

"Today we learn the CSS property that controls this: `text-align`."

---

## What (8 minutes)

### The text-align Property

"`text-align` controls how inline content (usually text) is positioned horizontally within its containing element."

Write the four values on the board:
- `text-align: left` — aligns text to the left edge (default for English)
- `text-align: right` — aligns text to the right edge
- `text-align: center` — centers text horizontally
- `text-align: justify` — stretches each line to fill the full width

### Demonstrating Each Value

Show visually (draw on board or use slides):

```
left:     Hello World
right:                Hello World
center:       Hello World
justify:  Hello    World  (fills full width with spacing)
```

For a multi-line paragraph:
```
justify:  The quick brown fox jumps over the lazy
          dog. This text is stretched to fill the
          full width of its container on each line.
          The last line is left-aligned.
```

### Where Each Value Is Used

"Let me show you real-world use cases for each:"

| Value | Common Use |
|-------|-----------|
| `left` | Body text, articles, menus, lists |
| `right` | Prices, dates, secondary nav items, table numbers |
| `center` | Hero headings, logos, centered navigation, banners |
| `justify` | Newspaper columns, formal documents, some article text |

---

## How (10 minutes)

### Live Coding Demo

Build a landing page section demonstrating all four values:

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Text Alignment Demo</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>

  <nav>
    <span class="logo">MyBrand</span>
    <span class="nav-links">Home  About  Contact</span>
  </nav>

  <section class="hero">
    <h1>Build Amazing Websites</h1>
    <p>Start your web development journey today.</p>
  </section>

  <article class="content">
    <h2>About This Course</h2>
    <p class="body-text">This course teaches you HTML and CSS from the ground up.
       You will learn how to create professional-looking web pages with clean,
       maintainable code that follows modern best practices.</p>
  </article>

  <table class="pricing">
    <tr>
      <td class="item">Basic Plan</td>
      <td class="price">$9.99</td>
    </tr>
    <tr>
      <td class="item">Pro Plan</td>
      <td class="price">$19.99</td>
    </tr>
  </table>

  <footer>
    <p>© 2025 MyBrand. All rights reserved.</p>
  </footer>

</body>
</html>
```

**CSS:**
```css
body {
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

/* NAV */
nav {
  display: flex;
  justify-content: space-between;
  padding: 16px 0;
  border-bottom: 1px solid #eee;
}

/* HERO — centered */
.hero {
  text-align: center;
  padding: 60px 0;
}

.hero h1 {
  font-size: 2.5rem;
  color: #1a202c;
}

/* ARTICLE — left + justify */
.content h2 {
  text-align: left;
}

.body-text {
  text-align: justify;
  color: #555;
  line-height: 1.7;
}

/* TABLE — right-aligned prices */
.pricing {
  width: 100%;
}

.item {
  text-align: left;
}

.price {
  text-align: right;
  font-weight: bold;
  color: #2d8a4e;
}

/* FOOTER — center */
footer {
  text-align: center;
  color: #999;
  margin-top: 40px;
  border-top: 1px solid #eee;
  padding-top: 20px;
}
```

Walk through each section as you write the CSS. After each section, save and show the browser.

### Important Point: text-align Applies to the Container

"Notice something: for the hero section, I put `text-align: center` on the `.hero` section element, not on the `<h1>` itself. `text-align` tells the container how to position its inline content."

"This also means `text-align` is inherited. When I set `text-align: center` on `.hero`, both the `<h1>` and the `<p>` inside it become centered — unless I override them."

---

## Wrap-Up (3 minutes)

"Four values: `left`, `right`, `center`, `justify`. Each has its place. Use `center` for headings and hero sections. Use `left` for body text. Use `right` for prices and numbers. Use `justify` sparingly — it can create uneven word spacing on short lines."

Preview: "In the next session, we'll inspect real websites to identify where these alignment values are being applied."


---

## LO–1.3.1: Explain what CSS selectors are and why they are used instead of inline styles (15 minutes)

# Lecture Script: CSS Selectors vs Inline Styles

## Why (5 min)

Open with a question to the class: "Imagine you're building a website with 100 product cards, and each card has a price displayed in red. What happens when your client calls and says 'make the price green instead'?"

With inline styles, a developer has to open every single HTML element and change the `style` attribute manually. That is error-prone, time-consuming, and very frustrating.

This is exactly the problem CSS selectors solve. They give us a way to write a rule once and apply it everywhere it's needed — and change it in a single location when requirements change.

**Key point to emphasize:** CSS is not just about making things look pretty. It's about writing styles in a way that scales. Selectors are the mechanism that makes CSS scalable.

## What (5 min)

A CSS selector is a pattern that matches one or more HTML elements. Once matched, the CSS properties defined in the rule are applied to those elements.

The anatomy of a CSS rule:

```css
selector {
  property: value;
}
```

For example:

```css
p {
  color: blue;
}
```

Here `p` is the selector. It tells the browser: "find every `<p>` element in the document and make its text blue."

CSS can be delivered to the browser in three ways:
- Inline: `<p style="color: blue;">` — styles live on the element
- Internal: `<style>` tag in `<head>` — styles live in the HTML file
- External: A `.css` file linked with `<link>` — styles live in their own file

Selectors are the key feature that makes internal and external CSS powerful.

## How (10 min)

### Demo 1: The Inline Problem

Write this on the board or in a code editor:

```html
<p style="color: red; font-size: 18px; font-weight: bold;">Paragraph 1</p>
<p style="color: red; font-size: 18px; font-weight: bold;">Paragraph 2</p>
<p style="color: red; font-size: 18px; font-weight: bold;">Paragraph 3</p>
```

Ask: "What's wrong with this?" Let students answer. Guide them to see: repetition, difficult to update, mixes structure with presentation.

### Demo 2: Refactoring with a Selector

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    p {
      color: red;
      font-size: 18px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
  <p>Paragraph 3</p>
</body>
</html>
```

Show how changing `color: red` to `color: green` in one place instantly updates all three paragraphs.

### Demo 3: External CSS (conceptual)

Explain that in a real project, the `<style>` block moves to a file called `styles.css`, and the HTML links to it:

```html
<link rel="stylesheet" href="styles.css">
```

```css
/* styles.css */
p {
  color: red;
  font-size: 18px;
  font-weight: bold;
}
```

This way, multiple HTML pages can share the same stylesheet.

## Summary

- CSS selectors target HTML elements and apply styles to them.
- Selectors eliminate the need to write the same style repeatedly.
- They enable separation of structure (HTML) and presentation (CSS).
- Changing a selector's rule updates every matched element instantly.


---

## LO–1.3.3: Apply tag selectors to style HTML elements (15 minutes)

# Lecture Script: Applying Tag Selectors

## Why (3 min)

Recall from the previous lesson: selectors solve the repetition problem. Tag selectors are the most straightforward type of selector — they target every element of a given HTML tag type.

Analogy: Imagine a school cafeteria rule that says "all students wearing red shirts get a free dessert." The rule doesn't care who the individual student is — if the shirt is red, the rule applies. A tag selector works the same way: "all `<p>` elements get this style."

Tag selectors are what you reach for when you want to set a consistent baseline for an element type across an entire page.

## What (5 min)

A tag selector is written as the HTML element name without angle brackets:

```css
p {
  color: red;
}
```

This matches every `<p>` in the document.

**Key properties of tag selectors:**
- Target all elements of the named tag type
- Apply to elements anywhere in the document (regardless of nesting)
- One rule, unlimited targets
- Very low specificity (more on this later)

## How (12 min)

### Demo 1: Basic Tag Selector

Open a new HTML file and build up from scratch live:

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    h1 {
      color: darkblue;
      text-align: center;
    }
  </style>
</head>
<body>
  <h1>My First Website</h1>
  <h1>Another Heading</h1>
</body>
</html>
```

Show in the browser: both `<h1>` elements are dark blue and centered. One rule did it.

### Demo 2: Multiple Tag Selectors

Expand the stylesheet:

```css
h1 {
  color: darkblue;
  text-align: center;
}

p {
  font-size: 16px;
  color: #444;
  line-height: 1.7;
}

a {
  color: crimson;
  text-decoration: none;
}
```

Add matching HTML elements and show the result. Emphasize: each rule targets a different tag, but the mechanism is identical.

### Demo 3: Styling a Navigation Bar

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

Show students how three tag selectors (`ul`, `li`, `a`) work together to build a styled navigation bar.

### Live Exercise (Students)

Give students 3 minutes to write CSS rules that:
1. Make all `<h2>` elements have `color: teal`
2. Make all `<p>` elements have `font-family: Georgia` and `line-height: 1.8`
3. Make all `<button>` elements have `background-color: orange` and `color: white`

Review answers as a group.

## Summary

- Tag selectors match all HTML elements of a given type.
- Syntax: `tagname { property: value; }`
- Multiple tag selectors can coexist in the same stylesheet.
- Best used for setting baseline/default styles for element types.


---

## LO–1.3.5: Use ID selectors to uniquely target specific elements (15 minutes)

# Lecture Script: ID Selectors

## Why (3 min)

Recap from last lesson: tag selectors are too broad — they style ALL elements of a type. Sometimes you need to target just ONE specific element on the page.

Think of a physical example: every student in a school has a student ID number. When the library system looks up "student 10042," it finds exactly one person. An ID selector works the same way — it finds exactly one element on the page.

This is the problem ID selectors solve: targeting a single, specific, unique element.

## What (5 min)

An ID selector uses a `#` prefix with the ID value:

```css
#element-id {
  property: value;
}
```

The matching HTML element uses the `id` attribute:

```html
<div id="element-id">Content</div>
```

**Rules of ID selectors:**
- The `id` value must be unique on the page
- Starts with a letter (not a number)
- Can contain letters, numbers, hyphens, and underscores
- Case-sensitive: `#MainTitle` and `#maintitle` are different
- CSS selector uses `#` prefix

## How (12 min)

### Demo 1: A Simple ID Selector

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    #page-title {
      color: darkblue;
      text-align: center;
      font-size: 40px;
      border-bottom: 3px solid darkblue;
      padding-bottom: 10px;
    }
  </style>
</head>
<body>
  <h1 id="page-title">My Portfolio</h1>
  <h1>Regular Heading</h1>
</body>
</html>
```

Show in the browser: only the `<h1 id="page-title">` is styled with the dark blue color and border. The second `<h1>` is unstyled (uses browser defaults). This demonstrates the precision of ID selectors.

### Demo 2: Multiple ID Selectors on a Page

Show a realistic page layout:

```html
<header id="site-header">...</header>
<nav id="main-nav">...</nav>
<main id="content">...</main>
<footer id="site-footer">...</footer>
```

```css
#site-header {
  background-color: #1a1a2e;
  color: white;
  padding: 20px;
}

#main-nav {
  background-color: #16213e;
  padding: 10px;
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

Point out: each section of the page has a unique role and a unique ID, and each is styled independently.

### Demo 3: What NOT to Do

Show the invalid use:

```html
<!-- WRONG: Same ID on two elements -->
<p id="intro">Paragraph one</p>
<p id="intro">Paragraph two</p>
```

Explain: this is invalid HTML. The browser might style both, but behavior is unpredictable and the HTML fails validation. If two elements need the same style, use a class instead.

### Live Exercise

Have students build a simple profile card:

```html
<div id="profile-card">
  <h2 id="profile-name">Jane Doe</h2>
  <p id="profile-bio">Front-end developer. Coffee lover.</p>
</div>
```

And style each ID independently. Walk through together.

## Summary

- ID selectors target a single unique element using the `#` prefix.
- The `id` attribute on the HTML element must match the selector.
- Each `id` should only be used once per page.
- ID selectors have high specificity — they override tag selectors.


---

## LO–1.3.7: Use class selectors to target groups of elements (15 minutes)

# Lecture Script: Class Selectors

## Why (3 min)

We've established two problems:
1. Tag selectors are too broad — they affect all elements of a type.
2. ID selectors are too narrow — they target exactly one element.

What about the middle ground? What if you need to style a specific group of elements — but not ALL elements of that tag type?

Enter class selectors. A class is a label you put on elements. Any element that carries that label gets the associated styles. Multiple elements can share a class. One element can even carry multiple classes.

Analogy: In a school, students can belong to clubs. The "Chess Club" label applies to all members, regardless of their year or grade. When the school orders Chess Club t-shirts, every member gets one. An ID selector is like a unique student ID; a class is like a club membership badge.

## What (5 min)

Class selectors use the `.` (dot) prefix in CSS:

```css
.class-name {
  property: value;
}
```

The matching HTML element uses the `class` attribute:

```html
<element class="class-name">...</element>
```

**Key differences from IDs:**
- Multiple elements can share the same class — no uniqueness requirement
- One element can have multiple classes (separated by spaces)
- Highly reusable — define once, use everywhere

## How (12 min)

### Demo 1: Basic Class Selector

```html
<style>
  .featured {
    background-color: gold;
    font-weight: bold;
    border-left: 4px solid darkorange;
    padding-left: 10px;
  }
</style>

<p>Regular paragraph.</p>
<p class="featured">This paragraph is featured.</p>
<p>Another regular paragraph.</p>
<p class="featured">This one is also featured.</p>
```

Show: only the two paragraphs with `class="featured"` are styled. The other two are unaffected.

### Demo 2: Cross-Tag Classes

Demonstrate that a class can apply to different tag types:

```html
<style>
  .warning {
    color: red;
    font-weight: bold;
  }
</style>

<p class="warning">This paragraph is a warning.</p>
<li class="warning">This list item is a warning.</li>
<span class="warning">And this span too.</span>
```

The same visual style appears on `<p>`, `<li>`, and `<span>` — different tags, same class.

### Demo 3: Button Variants

```html
<style>
  .btn {
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
  }

  .btn-success {
    background-color: #28a745;
    color: white;
  }

  .btn-danger {
    background-color: #dc3545;
    color: white;
  }
</style>

<button class="btn btn-success">Save</button>
<button class="btn btn-danger">Delete</button>
```

Preview of multiple classes on one element — the `btn` class handles shared layout, `btn-success` / `btn-danger` handles the color. Full details in a later lesson.

### Demo 4: Product Cards

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

Three cards, one rule. When the design changes, update once.

### Live Exercise

Ask students to create a "highlight" class that gives text a yellow background and bold weight, then apply it to one `<h2>`, one `<p>`, and one `<li>` on the same page.

## Summary

- Class selectors use the `.` prefix in CSS and the `class` attribute in HTML.
- Any number of elements can share a class.
- Classes work across different tag types.
- They are the most commonly used selectors in real-world CSS.
