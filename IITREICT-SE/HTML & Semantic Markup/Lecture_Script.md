# Lecture Script: HTML & Semantic Markup
**Duration:** 110 minutes | **Tools:** VS Code, Browser, DevTools | **Language:** HTML

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Frontend in the stack |
| Why Does This Matter? | 15 min | User-facing layer, careers |
| What Is the Concept? | 28 min | Structure, semantics, forms |
| How Do We Apply It? (LOs) | 48 min | Build portfolio skeleton |
| Accessibility check | 8 min | Labels, headings, alt |
| Recap & summary | 6 min | CSS preview |

---

## Session Opening (5 min)

**[Script:]** "You spent Module 1 thinking like a Python developer — logic, files, Git. Module 2 asks: what does the **user** see? Frontend developers build the interface in the browser. HTML is the first layer. No HTML, no webpage — only a blank tab."

**Problem hook:** Open `about.html` in browser vs show raw HTML in editor. "Browser translates tags into rendered page. Your job is to write tags that mean something."

🎯 **Instructor Note:** Connect to their FastAPI future — "API returns JSON; frontend turns JSON into HTML users understand."

---

## Why Does This Matter?

🎯 **Instructor Note:** Draw three-box diagram: Browser (HTML/CSS/JS) ↔ API (FastAPI) ↔ Database.

**[Script:]** "Full-stack engineers must speak both languages — Python on the server and HTML in the browser. Even AI-generated UIs still output HTML. Semantic HTML is how you prove you understand the web, not just frameworks."

**Real-world use:**
- Portfolio and capstone landing pages
- Login and signup screens
- Marketing pages and documentation sites

**Pain if misunderstood:**
- Div soup — unmaintainable structure
- Forms without labels — accessibility failures
- Duplicate ids — invalid HTML, broken JS hooks later
- Missing `lang` or `charset` — encoding and screen reader issues

---

## What Is the Concept?

### Frontend Role in a Web Application

**Definition:** Frontend presents data and collects input; backend processes and persists.

| Concern | Frontend | Backend (later) |
|---------|----------|-----------------|
| Layout | HTML/CSS | — |
| User input | Forms → JS | Validation, auth |
| Data display | Render lists, cards | API JSON |

**Mental model:** Frontend is the restaurant dining room; backend is the kitchen.

### Document Structure

Required boilerplate (board + live type):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Portfolio — Your Name</title>
</head>
<body>
</body>
</html>
```

**[Script:]** "`viewport` meta is your future self thanking you when CSS responsive design arrives."

### Semantic Layout Tags

Walk through each with when-to-use:

- `header` — top branding, not every heading on page
- `nav` — link groups
- `main` — one per page, core content
- `section` — thematic group with heading
- `article` — standalone piece (project card, blog post)
- `footer` — meta links, copyright

🎯 **Instructor Note:** Compare screen reader announcement for `<nav>` vs `<div class="nav">` — conceptual, no screen reader required.

### Headings, Links, Images

**Heading hierarchy:** One `h1`, logical `h2`/`h3` nesting.

```html
<a href="https://github.com/you" target="_blank" rel="noopener">GitHub</a>
<img src="images/profile.jpg" alt="Portrait of student">
```

**Common mistakes:**
- `href` missing on `<a>`
- `alt` missing on informative images
- Using `h1` for styling only — use CSS later

### Forms

Full accessible pattern:

```html
<form action="#" method="post">
  <label for="contact-email">Email</label>
  <input type="email" id="contact-email" name="email" required>

  <label for="message">Message</label>
  <textarea id="message" name="message" rows="4"></textarea>

  <button type="submit">Send</button>
</form>
```

**`action="#"`** — placeholder until backend exists.

**Input types demo:** text, email, password, number, checkbox, radio.

### DevTools Inspection

Elements panel walkthrough — expand tree, highlight, edit text temporarily.

**Validation habits:**
- Unique ids
- Closed tags
- Labels paired correctly

---

## How Do We Apply It?

### LO 1: Explain how frontend developer role fits in web application

**Problem:** Student thinks Python alone is "full web dev."

**Discussion (facilitated):** Map user click on Submit → form data → (future) API → database → response → HTML update.

**Predict:** Can backend alone show a button? (No — needs HTML or native app UI.)

**Explain result:** Frontend and backend are partners; today's session owns structure layer.

🎯 **Instructor Note:** 5-minute table discussion — groups list 3 frontend tasks, 3 backend tasks.

---

### LO 2: Write well-structured pages using proper semantic tags

**Problem:** Build portfolio skeleton for GitHub Pages later.

**Live build:**

```html
<header>
  <h1>Your Name</h1>
  <p>Python Developer · Web Learner</p>
  <nav>
    <a href="#about">About</a>
    <a href="#projects">Projects</a>
    <a href="#contact">Contact</a>
  </nav>
</header>
<main>
  <section id="about">
    <h2>About</h2>
    <p>Short bio...</p>
  </section>
  <section id="projects">
    <h2>Projects</h2>
    <article>
      <h3>Text Utility</h3>
      <p>Python CLI for file processing.</p>
    </article>
  </section>
</main>
<footer id="contact">
  <p>Contact: you@email.com</p>
</footer>
```

**Predict:** How many `main` elements allowed?

**Explain result:** One — accessibility requirement.

---

### LO 3: Implement forms with labels, inputs, and buttons

**Problem:** Contact section needs real form, not just email text.

**Extend `#contact`:**

```html
<section id="contact">
  <h2>Contact</h2>
  <form>
    <label for="name">Name</label>
    <input type="text" id="name" name="name">

    <label for="email">Email</label>
    <input type="email" id="email" name="email">

    <button type="submit">Send Message</button>
  </form>
</section>
```

**Predict:** Click label text — focus moves to input?

**Demo:** Yes, when `for`/`id` match.

---

### LO 4: Apply semantic HTML for accessibility and readability

**Problem:** Refactor div-based snippet.

**Before/after exercise** — class refactors mentor-provided div soup in 10 minutes.

**Checklist:**
- [ ] Landmark regions identified
- [ ] Heading order logical
- [ ] Link text descriptive ("GitHub profile" not "click here")

**[Script:]** "Screen reader users skim by headings and landmarks — same way sighted users skim visually."

---

### LO 5: Inspect and validate HTML using DevTools

**Problem:** Link does not jump to section.

**Bug:** `href="#projects"` but section id is `project` (typo).

**Debug steps:**
1. Inspect link — check `href`
2. Inspect section — check `id`
3. Fix mismatch
4. Retest

**Predict:** After fix, does anchor scroll work?

🎯 **Instructor Note:** Show **Accessibility** tab in DevTools briefly if available — optional.

---

## Accessibility Check (8 min)

Pairs audit neighbor page:
- One `h1`?
- All inputs labeled?
- Informative images have `alt`?
- `main` present?

---

## Recap (6 min)

---

## Lecture Summary

- **Frontend developers** build the browser layer users interact with
- **HTML document structure** provides valid, portable page foundation
- **Semantic tags** communicate meaning to browsers, assistive tech, and teammates
- **Forms with labels** collect input accessibly
- **DevTools** help inspect, debug, and validate structure
- **Practical value:** HTML is the universal output format of the web — every framework and API-driven UI depends on it

**[Script:]** "Next session: CSS. Your portfolio skeleton is ready to become visually professional."
