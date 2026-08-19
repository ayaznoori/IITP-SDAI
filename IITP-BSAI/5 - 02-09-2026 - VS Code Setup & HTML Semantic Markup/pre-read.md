# Pre-Read: VS Code Setup & HTML Semantic Markup

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **install and open VS Code** and create your first local web project folder
- How to **write a valid HTML document** with the correct structure from top to bottom
- How to **use semantic tags** so your page has meaning, not just appearance
- How to **build a form** with labels, inputs, and buttons that users can actually fill out
- How to **open your page in a browser** and **inspect elements** with DevTools

---

## 2. Detailed Explanation

### From One Compiler to Your Own Machine

So far, you ran JavaScript in One Compiler online. Web pages live on your computer as files. You open them in a browser. **VS Code** (Visual Studio Code — a free code editor) is where professional developers write those files.

**Analogy:** One Compiler is like a shared classroom whiteboard. VS Code is your personal notebook and workshop on your laptop.

### Setting Up VS Code (Quick Tour)

1. Download VS Code from the official site if you have not already.
2. Open VS Code.
3. Use **File → Open Folder** and pick (or create) a folder like `my-first-website`.
4. Create a file named `index.html`.
5. Install the **Live Server** extension (optional but helpful) — it refreshes the browser when you save.

**Tip:** Keep one folder per small project. Messy desktops become messy projects.

### What Is HTML?

**HTML** (HyperText Markup Language — the structure language of the web) tells the browser what content exists on a page. It does not style colors or layouts deeply — that is CSS later. HTML answers: *What is on this page?*

**Analogy:** HTML is the skeleton of a house. CSS is paint and furniture. JavaScript is electricity and switches.

### The HTML Document Structure

Every proper page follows a pattern:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My First Page</title>
</head>
<body>
  <h1>Hello, Web!</h1>
  <p>This is my first local page.</p>
</body>
</html>
```

| Part | Role |
|------|------|
| `<!DOCTYPE html>` | Tells browser: this is modern HTML5 |
| `<html>` | Root wrapper for the whole page |
| `<head>` | Metadata — title, charset, not visible as main content |
| `<body>` | Everything the user sees |
| `<title>` | Text on the browser tab |

### Headings, Paragraphs, Links, and Images

- **`<h1>` to `<h6>`** — headings; use **one** `<h1>` per page for the main title
- **`<p>`** — paragraph of text
- **`<a href="url">`** — hyperlink; `href` is where the link goes
- **`<img src="path" alt="description">`** — image; `alt` helps accessibility and when image fails to load

```html
<h1>Welcome to Masai</h1>
<p>Learn to build real products.</p>
<a href="https://masaischool.com">Visit Masai</a>
<img src="logo.png" alt="Masai logo">
```

### Semantic HTML — Tags with Meaning

A **semantic tag** (an HTML element whose name describes its purpose) helps browsers, screen readers, and other developers understand your page.

| Instead of… | Prefer… | Why |
|-------------|---------|-----|
| `<div>` for everything | `<header>`, `<nav>`, `<main>`, `<footer>` | Clear page regions |
| `<div class="article">` | `<article>` | Self-contained content block |
| `<div class="section">` | `<section>` | Themed group of content |

**Example skeleton:**

```html
<header>
  <h1>My Blog</h1>
  <nav>
    <a href="#home">Home</a>
    <a href="#about">About</a>
  </nav>
</header>
<main>
  <article>
    <h2>First Post</h2>
    <p>Content here...</p>
  </article>
</main>
<footer>
  <p>&copy; 2026 My Blog</p>
</footer>
```

### Why Semantic HTML Matters

**Real-world hook:** Google and assistive technologies read structure, not just pretty CSS. A blind user with a screen reader hears "navigation" when you use `<nav>`, not "generic div number 47."

**Benefits:**
- **Accessibility** — more people can use your site
- **SEO** — search engines understand content hierarchy
- **Maintainability** — your future self thanks you when the project grows

### Forms — Collecting User Input

A **form** (a section where users enter data and submit it) is built with `<form>`, inputs, labels, and buttons.

Common input types:
- `text` — name, city
- `email` — email address
- `password` — hidden characters
- `number` — numeric values
- `checkbox` — on/off options
- `radio` — pick one from a group

**Always pair `<label>` with inputs** — click the label to focus the field:

```html
<form>
  <label for="username">Username:</label>
  <input type="text" id="username" name="username">
  <button type="submit">Sign In</button>
</form>
```

The `for` on the label must match the `id` on the input.

### Browser DevTools — Your X-Ray Glasses

1. Open your HTML file in Chrome or Edge (right-click file → Open With).
2. Right-click any element → **Inspect**.
3. See the **Elements** panel — live HTML tree.
4. Hover nodes to highlight parts of the page.

**Why It Matters:** When something looks wrong, DevTools shows whether the problem is missing tags, wrong nesting, or CSS (next sessions).

### Messy to Clear — A Mini Walkthrough

**Messy (avoid):**

```html
<div>
  <div>My Site</div>
  <div>Click here</div>
</div>
```

**Clear (prefer):**

```html
<header>
  <h1>My Site</h1>
  <nav><a href="/">Home</a></nav>
</header>
```

Same visual potential with CSS later — but the clear version is understandable without CSS.

### Building Blocks Checklist

Before the live session, you should recognize:

- [ ] `<!DOCTYPE html>` and basic page skeleton
- [ ] Difference between `<head>` and `<body>`
- [ ] At least five semantic tags: `header`, `nav`, `main`, `article`, `footer`
- [ ] How to write a link and an image with required attributes
- [ ] Form with `label`, `input`, and `button`
- [ ] How to open DevTools and find an element

---

## 3. Practice Exercises

**Exercise 1 — Skeleton**
Create `about.html` in VS Code with full document structure, a page title "About Me", and one `<h1>` and two `<p>` tags in the body. Open it in your browser.

**Exercise 2 — Semantic layout**
Build a page with `<header>`, `<main>`, and `<footer>`. Put your name in the header, a short bio in main, and a copyright line in the footer.

**Exercise 3 — Form**
Create a signup form with fields for: full name (text), email (email), and a Submit button. Every input must have a `<label>`.

**Exercise 4 — DevTools**
Open your page, inspect the `<h1>`, and write down (on paper) its parent element tag name. Change the `<h1>` text in VS Code, save, refresh — confirm the change in the browser.
