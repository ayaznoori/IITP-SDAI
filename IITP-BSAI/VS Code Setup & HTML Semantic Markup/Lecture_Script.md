# Lecture Script: VS Code Setup & HTML Semantic Markup
**Duration:** 110 minutes | **Tools:** VS Code, Browser, DevTools | **Language:** HTML

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Why local dev + HTML structure |
| Why Does This Matter? | 12 min | Real products, accessibility, SEO |
| What Is the Concept? | 25 min | VS Code, document structure, semantics, forms |
| How Do We Apply It? (LOs) | 45 min | Hands-on build with prediction checks |
| Live build lab | 15 min | Full semantic page + form |
| Recap & summary | 8 min | LO review + preview CSS |

---

## Session Opening (5 min)

**[Script:]** "You have been writing JavaScript in the browser of One Compiler. Today we step into how real web developers work: files on your machine, opened in a browser, edited in VS Code. HTML is not a programming language in the same sense as JavaScript — it is the **structure layer** of every website you have ever visited."

**Problem hook:** Show a blank browser tab vs a simple rendered page. "Someone wrote HTML so the browser knew: here is a heading, here is a paragraph, here is a link. Without structure, the browser has nothing to display."

🎯 **Instructor Note:** Poll the room — "Who has VS Code installed?" Help anyone stuck before moving on. Pair students who are ready with those who need setup.

---

## Why Does This Matter?

🎯 **Instructor Note:** Open any popular site (e.g. a news homepage). Inspect the `<header>` and `<nav>`. Say: "This is not magic. It is HTML tags under the hood."

**[Script:]** "Every React component you will write later still renders to HTML in the browser. Every API response eventually becomes something a user **sees** — and that starts with structure. If your HTML is a soup of meaningless `<div>` tags, you hurt accessibility, SEO, and your own ability to debug."

**Real-world use:**
- Product landing pages, login screens, checkout forms
- Portfolio sites for job applications
- Email templates and admin dashboards (all start with HTML structure)

**Pain if misunderstood:**
- Forms without labels → users cannot click labels to focus fields; screen readers fail
- Missing `alt` on images → accessibility and broken-image UX suffer
- No semantic regions → CSS and JavaScript become harder to target and maintain
- Wrong document structure → browser quirks and invalid pages

**[Script:]** "Today you build the **bones**. Next session we add **skin** with CSS. The week after, JavaScript will make those bones **move**."

---

## What Is the Concept?

### VS Code as Your Development Environment

**Definition:** VS Code is a source-code editor optimized for writing and organizing project files locally.

**Mental model:** Workshop with labeled drawers (folders) and a workbench (editor). Live Server is a quick preview window.

**Walkthrough (live, narrated):**
1. File → Open Folder → `masai-web-week1`
2. New file `index.html`
3. Save early, save often — `Cmd+S` / `Ctrl+S`
4. Optional: Live Server → "Open with Live Server"

🎯 **Instructor Note:** Emphasize **folder per project**. Show bad example: random HTML files on Desktop with no project folder.

**Common mistakes:**
- Saving as `index.html.txt` (hidden extension on Windows)
- Not refreshing browser after save (without Live Server)
- Working outside an opened folder — files get lost

### HTML Document Structure

**Definition:** HTML uses **elements** (tags) to mark up content. Most tags come in pairs: opening and closing.

**Mental model:** Outline for a document — title page (head), main content (body).

**Required skeleton (put on board):**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>
</head>
<body>
  <!-- visible content -->
</body>
</html>
```

**[Script:]** "`lang` helps screen readers pronounce correctly. `viewport` prepares us for mobile — we will use it heavily when we do responsive design."

**Common mistakes:**
- Forgetting closing tags: `<p>text` without `</p>`
- Putting visible content inside `<head>`
- Multiple `<h1>` tags — hurts document outline

### Headings, Paragraphs, Links, Images

| Element | Purpose | Key attribute |
|---------|---------|---------------|
| `<h1>`–`<h6>` | Heading hierarchy | — |
| `<p>` | Paragraph | — |
| `<a>` | Hyperlink | `href` |
| `<img>` | Image | `src`, `alt` |

🎯 **Instructor Note:** Demo bad `alt=""` on informative image vs good descriptive alt. "Decorative only" images may use empty alt — mention briefly, do not deep-dive.

**Comparison note:** In Markdown you write `# Heading`. In HTML you write `<h1>Heading</h1>`. Same idea, different syntax.

### Semantic HTML

**Definition:** Semantic elements describe the **role** of content, not just a generic box.

**Core set for today:**

| Tag | Meaning |
|-----|---------|
| `<header>` | Introductory content, often logo + nav |
| `<nav>` | Navigation links |
| `<main>` | Primary unique content — one per page |
| `<section>` | Themed grouping, usually with a heading |
| `<article>` | Standalone content (blog post, card) |
| `<footer>` | Footer info, copyright, links |

**[Script:]** "A `<div>` is a cardboard box. A `<nav>` is a labeled drawer that says **navigation**. When you have a labeled drawer available, use it."

**Common mistakes:**
- Wrapping entire page in one `<div>`
- Skipping heading levels: `<h1>` then `<h4>` with no `<h2>`
- Using `<section>` without a heading inside

### Forms and Input Elements

**Definition:** A form groups controls that collect user input.

**Building blocks:**
- `<form>` — container; `action` and `method` come later with backends
- `<label for="id">` — accessible name for control
- `<input type="...">` — single-line controls
- `<button type="submit">` — submit the form

**Types to demo today:** `text`, `email`, `password`, `number`, `checkbox`, `radio`

🎯 **Instructor Note:** Show clicking the **label** focuses the input — students remember this when they build React forms later.

**Common mistakes:**
- `label for` does not match input `id`
- Missing `name` on inputs (matters when we submit to servers later)
- Using `<div onclick>` instead of `<button>` — accessibility anti-pattern

### Browser DevTools

**Definition:** Built-in browser tools to inspect and debug live pages.

**Mental model:** X-ray vision for the DOM tree (the browser's live representation of HTML).

**Quick tour:**
1. Right-click → Inspect
2. Elements panel — expand tree
3. Select element → see styles panel (preview for next session)
4. Edit HTML temporarily in DevTools (changes lost on refresh — explain why)

---

## How Do We Apply It?

### LO 1: Open and run HTML files locally in VS Code

**Problem:** You need to see your HTML file rendered, not as raw text.

**Translate logic:** Create file → valid HTML → open in browser → verify rendering.

**Write code:** Minimal `index.html` with `h1` and `p`.

**Predict before running:** Will the browser show the tags or the text inside them?

**Explain result:** Browser parses HTML and renders visible content; tags themselves are not shown.

🎯 **Instructor Note:** Show what happens if you double-click `.html` vs use Live Server — both work; Live Server helps with refresh workflow.

**Second mini-demo:**

```html
<!DOCTYPE html>
<html lang="en">
<head><title>Test</title></head>
<body><p>Local file works!</p></body>
</html>
```

**Predict:** Tab title? Body content?

---

### LO 2: Write well-structured pages using semantic tags

**Problem:** Build a "Course Landing" page that a new developer can read and understand without CSS.

**Translate logic:** Map page regions to semantic tags → heading hierarchy inside each region.

**Write code (starter — complete live with class):**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>IITP Web Foundations</title>
</head>
<body>
  <header>
    <h1>Web Foundations</h1>
    <nav>
      <a href="#syllabus">Syllabus</a>
      <a href="#contact">Contact</a>
    </nav>
  </header>
  <main>
    <section id="syllabus">
      <h2>Syllabus</h2>
      <p>HTML, CSS, JavaScript, and beyond.</p>
    </section>
    <article>
      <h2>Why This Course?</h2>
      <p>Build job-ready frontend skills.</p>
    </article>
  </main>
  <footer id="contact">
    <p>Contact: mentor@example.com</p>
  </footer>
</body>
</html>
```

**Predict before running:** How many `<h1>` tags should exist? Where does primary content live?

**Explain result:** One `h1`, one `main`, nav in `header`, supplementary info in `footer`.

🎯 **Instructor Note:** Ask students to add a second `article` card — peer check for valid nesting.

---

### LO 3: Implement forms with common input elements and labels

**Problem:** Create a registration form for a workshop.

**Translate logic:** Each field = label + input; group related options; submit button at end.

**Write code:**

```html
<form>
  <label for="fullName">Full Name</label>
  <input type="text" id="fullName" name="fullName">

  <label for="email">Email</label>
  <input type="email" id="email" name="email">

  <label for="age">Age</label>
  <input type="number" id="age" name="age" min="16">

  <p>Track:</p>
  <label><input type="radio" name="track" value="bsai"> BSAI</label>
  <label><input type="radio" name="track" value="other"> Other</label>

  <label><input type="checkbox" name="agree"> I agree to terms</label>

  <button type="submit">Register</button>
</form>
```

**Predict before running:** What happens when you click "Full Name" label text?

**Explain result:** Input focuses — `for`/`id` pairing works. Radio buttons share `name` so only one is selected.

**Common mistake callout:** Two radios with different `name` values — both can be selected. Fix live.

---

### LO 4: Apply semantic HTML for clearer page structure

**Problem:** Refactor a "div soup" page into semantic HTML.

**Translate logic:** Identify regions → assign semantic tags → preserve content.

**Before (show on board):**

```html
<div class="top">...</div>
<div class="content">...</div>
<div class="bottom">...</div>
```

**After:** `header`, `main`, `footer` with proper headings.

🎯 **Instructor Note:** Pair exercise — 8 minutes. Give printed "div soup" handout or slide; pairs refactor on paper first, then one volunteer types in VS Code.

**Predict:** Will the page look different without CSS? (No — structure only; appearance unchanged.)

**Explain result:** Semantics improve meaning and maintainability, not default visuals.

---

### LO 5: Inspect HTML using browser DevTools

**Problem:** A link does not work — is the `href` wrong or the tag broken?

**Translate logic:** Inspect element → verify tag name and attributes → edit in source file.

**Walkthrough (live debugging scenario):**

```html
<a href="syllabus.html">Syllabus</a>
<!-- Intentional bug: file is syllabus.htm -->
```

**Steps narrated:**
1. Inspect the `<a>` — see `href`
2. Compare with actual filename in VS Code
3. Fix and save
4. Refresh and verify

**Predict before fixing:** What will DevTools show for the broken link's `href`?

🎯 **Instructor Note:** Show **Console** tab briefly — mention JavaScript errors appear here next module.

---

## Live Build Lab (15 min)

**Task:** Build a "Personal Portfolio — Structure Only" page:

- Semantic layout: `header`, `nav`, `main`, `footer`
- `nav` with 3 anchor links to page sections (`#about`, `#projects`, `#contact`)
- `section` for About, `article` for at least one project
- Contact `section` with a form: name, email, message (`textarea` if time — optional mention), submit button
- Validate in DevTools: exactly one `main`, one `h1`

**[Script:]** "No CSS today — if it looks plain, you succeeded. Structure first, style second."

---

## Recap (8 min)

🎯 **Instructor Note:** Rapid fire:
1. What goes in `<head>` vs `<body>`?
2. Why use `<nav>` instead of `<div>`?
3. What does `alt` on an image do?
4. How do you connect a label to an input?

---

## Lecture Summary

- **VS Code + local files** is the standard workflow for web development
- **HTML document structure** (`DOCTYPE`, `html`, `head`, `body`) gives browsers a valid blueprint
- **Semantic tags** (`header`, `nav`, `main`, `article`, `section`, `footer`) add meaning and improve accessibility
- **Forms** with proper `label`, `input`, and `button` elements collect user data accessibly
- **DevTools** let you inspect and debug structure before CSS and JavaScript complicate the picture
- **Practical value:** Every frontend role, every React app, every product page starts with HTML structure — this is the vocabulary of the web

**[Script:]** "Next session: CSS. You built the skeleton today. Tomorrow we teach it to look professional with color, spacing, and Flexbox layout. Save your `index.html` — you will style it live."
