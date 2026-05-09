# Lecture Script: HTML Structure and Semantic Markup

---

## Introduction: Building the Smart Cart Web App

**[Script:]** Welcome everyone! Over the next two hours, we're building the foundation of a real web application called the Smart Cart. Think of it as an online shopping cart with a modern interface. By the end of this session, you'll have written the actual HTML and CSS that makes up the structure and style of a product listing page—something you could actually see in a browser.

Here's what we're working with today: a product page showing items a user can add to their cart. This isn't a toy example—it's the exact kind of markup you'll use in real projects.

**[Script:]** We have three learning goals today:

1. How to structure HTML properly using semantic tags that describe their meaning
2. How to build forms and input elements so users can interact with your page
3. How to apply CSS selectors and properties to make it visually complete

Let's start with why any of this matters.

---

## Why Does This Matter?

**[Script:]** Here's a question for everyone: When you search for something on Google, how does Google know what your page is about? It can't read your mind. It reads your HTML.

🎯 **Instructor Note:** Display two screenshots side-by-side—one using only `<div>` tags for everything, one using semantic tags like `<header>`, `<nav>`, `<article>`, `<footer>`. Ask: "Which one would Google understand better?"

**[Script:]** The difference is enormous. Google, screen readers used by visually impaired users, and browser tools all rely on your HTML structure to understand your page. If everything is just `<div>` with no meaning, your page is invisible to the web.

Let me tell you about a real pain point. I once worked with a team that built an entire e-commerce site using only `<div>` tags. When they wanted to improve their SEO, they had to rewrite everything. When they wanted to make the site accessible, they had to start over. It took three months to fix what could have been done correctly in three days.

**[Script:]** The practical reality: semantic HTML is not optional. It's the difference between a page that works and a page that thrives. It's the difference between a site that everyone can use and one that excludes millions of users.

Now let's learn how to do it right.

---

## What Is Semantic HTML?

**[Script:]** Semantic HTML means using HTML elements that describe their meaning—not just how they look, but what they are.

Think of it like this: imagine you're organizing a physical office. You could put everything in identical white boxes and label them all "container." That's what non-semantic HTML is like. Or you could use labeled filing cabinets, bookcases, and drawers that describe what's inside. That's semantic HTML.

Here's a useful comparison: in JavaScript, we declare variables with meaningful names like `const userCart = []` instead of `const x = []`. Semantic HTML is the same principle applied to your markup. You're naming your content containers so developers, browsers, and tools understand them.

**[Script:]** Common mistakes beginners make:

- Using `<div>` for everything because it's "easier"
- Confusing `<header>` with `<head>`—remember, `<head>` is invisible metadata, `<header>` is visible content
- Using `<b>` for bold instead of `<strong>`—`<b>` is just visual, `<strong>` carries meaning
- Nesting semantic tags incorrectly, like putting a `<nav>` inside an `<article>` when it should be in a `<header>`

Let's now build this into our Smart Cart app.

---

## How Do We Apply It? — Part 1: HTML Structure and Semantic Tags

### Problem: Our Smart Cart needs a meaningful structure

**[Script:]** Here's our problem: we need to build a product listing page for the Smart Cart. It needs a header with navigation, a main section showing products, a sidebar for filters, and a footer. Without semantic tags, we'd just have a soup of `<div>` elements that no one can understand.

### Translate the logic

**[Script:]** Let's map out what our page needs conceptually before we write code:

- The top section with logo and navigation links = `<header>`
- The main content area showing products = `<main>`
- Each individual product = `<article>`
- The sidebar with category filters = `<aside>`
- The bottom section with company info = `<footer>`
- Navigation links = `<nav>`

This is exactly how a screen reader would navigate our page. Let's write the code.

### Write the code

**[Script:]** Open your code editor and create `index.html`. Type along with me.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Smart Cart - Products</title>
</head>
<body>
    <header>
        <nav>
            <a href="#">Home</a>
            <a href="#">Products</a>
            <a href="#">Cart</a>
        </nav>
    </header>

    <main>
        <aside>
            <h2>Filters</h2>
            <p>Categories</p>
        </aside>
        
        <section>
            <article>
                <h3>Wireless Headphones</h3>
                <p>$99.99</p>
            </article>
            <article>
                <h3>Smart Watch</h3>
                <p>$149.99</p>
            </article>
        </section>
    </main>

    <footer>
        <p>&copy; 2024 Smart Cart</p>
    </footer>
</body>
</html>
```

🎯 **Instructor Note:** Watch for learners typing `href` inside `<a>` without the angle bracket—this is a common typo. Pause and correct if you see it.

**[Script:]** Notice what we did: every tag describes content type, not appearance. `<header>` doesn't mean "put this at the top"—it means "this is introductory content." The browser and assistive technologies understand that automatically.

I intentionally made a mistake in that code. Who can spot it?

🎯 **Instructor Note:** Pause for 10 seconds. Let learners look for the typo.

**[Script:]** The `<a>` tags have `href` instead of `href` is correct—wait, let me check... actually I wrote it correctly. Good catch that you're looking carefully! That's the kind of attention to detail that matters.

### Demo: Semantic tags in action

**[Script:]** Let's see this in a browser. I'll create a simple version and we can visualize how a screen reader would navigate it.

```html
<!-- Demo for visualization - max 10 lines -->
<!DOCTYPE html>
<html lang="en">
<head><meta charset="UTF-8"><title>Demo</title></head>
<body>
    <header><h1>Smart Cart</h1></header>
    <main>
        <article><h2>Product: Headphones</h2></article>
        <article><h2>Product: Watch</h2></article>
    </main>
    <footer>Copyright 2024</footer>
</body>
</html>
```

🎯 **Instructor Note:** Ask prediction question before running or showing output.

**Predict before running:** If a blind user opens this page with a screen reader, what will it announce first?

**[Script:]** The screen reader will announce "Heading level 1, Smart Cart" from the `<header>`. Then it will say "Heading level 2, Product: Headphones" from the first `<article>`. This navigation makes sense because each `<article>` is a self-contained piece of content—exactly what we want for products in a cart.

### Explain result

**[Script:]** The key insight: semantic tags provide built-in meaning. When you use `<article>`, you're telling every tool that reads your page "this is a self-contained composition, like a product card." When you use `<aside>`, you're saying "this is tangentially related content, like filters."

This matters for three practical reasons: SEO (search engines understand your structure), accessibility (screen readers navigate by semantic regions), and maintainability (developers can read your code instantly).

---

## How Do We Apply It? — Part 2: Forms and Input Elements

### Problem: Users need to interact with the Smart Cart

**[Script:]** Our product listing is working, but users can't do anything yet. They need to add products to their cart, filter by category, and search for items. For all of this, we need forms.

Forms are the primary way users send data to servers. Every time you log in, search, or submit an order—you're using a form.

### Translate the logic

**[Script:]** Let's break down what our Smart Cart form needs:

- A search input to find products
- A dropdown to filter by category
- Checkboxes for product options
- A submit button to add items to cart

The HTML elements for this are: `<form>` as the container, `<input>` for text entry, `<select>` for dropdowns, `<option>` for choices, `<input type="checkbox">` for multiple selections, and `<button>` or `<input type="submit">` to submit.

### Write the code

**[Script:]** Let's add a product search form to our Smart Cart page. Type this after the opening `<body>` tag:

```html
<form action="/search" method="GET">
    <label for="search">Search products:</label>
    <input type="text" id="search" name="q" placeholder="Headphones, Watch...">
    
    <label for="category">Category:</label>
    <select id="category" name="category">
        <option value="">All Categories</option>
        <option value="electronics">Electronics</option>
        <option value="accessories">Accessories</option>
    </select>
    
    <fieldset>
        <legend>Options:</legend>
        <label>
            <input type="checkbox" name="in_stock" value="1">
            In Stock Only
        </label>
    </fieldset>
    
    <button type="submit">Search</button>
</form>
```

🎯 **Instructor Note:** Pause here. Ask: "Why do we need the `for` attribute on labels and `id` on inputs?" Wait for responses.

**[Script:]** The `for` attribute connects the label to the input. Clicking the label focuses the input. This is crucial for accessibility—users with motor difficulties can click the larger text label instead of the small input box. It also helps screen readers know which label belongs to which input.

### Demo: Form behavior visualization

**[Script:]** Let's create a minimal demo to see how form data moves.

```html
<!-- Simple form demo - max 10 lines -->
<!DOCTYPE html>
<html lang="en">
<head><meta charset="UTF-8"><title>Form Demo</title></head>
<body>
    <form action="/search" method="GET">
        <label>Name: <input name="name" value="Alex"></label>
        <label>City: <select name="city"><option>NYC</option><option>LA</option></select></label>
        <button>Submit</button>
    </form>
</body>
</html>
```

**Predict before running:** If a user selects "LA" and clicks Submit, what URL will the browser request?

**[Script:]** The URL will be `/search?name=Alex&city=LA`. That's the GET method in action—form data becomes query parameters in the URL. This is how search engines work, how filters work, and how users share specific views of a page.

### Explain result

**[Script:]** Forms are the bridge between your HTML and server-side logic. The `name` attribute on each input is critical—without it, nothing gets sent. I cannot stress this enough: every input that should send data needs a `name` attribute.

Common mistakes to avoid:

- Forgetting `name` attributes—your data won't send
- Using `<div>` instead of `<label>`—breaks accessibility
- Not wrapping radio buttons in `<fieldset>`—loses the group context
- Using `type="text"` when you need `type="email"` or `type="number"`—you lose built-in validation

---

## How Do We Apply It? — Part 3: Intro to CSS Selectors and Properties

### Problem: The Smart Cart looks plain and unusable

**[Script:]** Our structure is perfect and our forms work, but honestly? It looks like a document from 1995. Gray Times New Roman, no spacing, no visual hierarchy. We need CSS to make this look like a modern e-commerce site.

CSS stands for Cascading Style Sheets. It controls the visual presentation—colors, fonts, spacing, layouts—without touching your HTML structure.

### Translate the logic

**[Script:]** Here's the mental model: CSS has two parts—selectors that target HTML elements, and properties that define what to change.

Think of it like a blueprint: selectors are the addresses ("paint the house at 123 Main Street"), properties are the instructions ("make it blue with white trim").

The cascade part of "Cascading Style Sheets" means styles can override each other. More specific selectors win over less specific ones. We'll talk about specificity in a moment.

### Write the code

**[Script:]** Create a new file called `styles.css`. We'll connect it to our HTML by adding this line inside `<head>`:

```html
<link rel="stylesheet" href="styles.css">
```

Now let's write the CSS:

```css
/* Basic reset */
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
    background-color: #f5f5f5;
}

/* Header styling */
header {
    background-color: #2c3e50;
    color: white;
    padding: 20px;
}

/* Product cards */
article {
    background-color: white;
    padding: 20px;
    margin-bottom: 15px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* Form styling */
input, select {
    padding: 10px;
    margin: 5px 0;
    border: 1px solid #ddd;
    border-radius: 4px;
}

button {
    background-color: #27ae60;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

button:hover {
    background-color: #219150;
}
```

🎯 **Instructor Note:** Walk through each rule. Ask: "What does `margin: 0` on body do?" and "What does `border-radius` do?"

**[Script:]** Important concepts here:

- We used element selectors (`body`, `header`, `article`) to target tags directly
- We used comma-separated selectors (`input, select`) to target multiple elements
- We used the `:hover` pseudo-class to change styling on interaction
- We used specific properties: `background-color`, `padding`, `margin`, `border-radius`, `box-shadow`

The cascade in action: `button:hover` is more specific than just `button`, so when you hover, the hover styles apply.

### Demo: CSS selector specificity

```html
<!-- CSS demo - max 10 lines -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CSS Demo</title>
    <style>
        p { color: blue; }
        .highlight { color: red; }
        #special { color: green; }
    </style>
</head>
<body>
    <p>Blue text</p>
    <p class="highlight">What color?</p>
    <p id="special">What color?</p>
</body>
</html>
```

**Predict before running:** What color will each paragraph be?

**[Script:]** First paragraph: blue (element selector). Second: red (class selector beats element). Third: green (id selector beats class).

That's specificity in action: IDs > Classes > Elements. More specific selectors always win. This is one of the most important concepts in CSS—it explains why your styles sometimes don't apply.

### Explain result

**[Script:]** CSS transforms your semantic HTML into a visual experience. Notice we didn't change our HTML at all—we just added a stylesheet. That's the power of separation: structure in HTML, presentation in CSS.

Key properties we used:

- `font-family` controls text appearance
- `margin` and `padding` control spacing (margin is outside, padding is inside)
- `background-color` and `color` control colors
- `border-radius` makes corners rounded
- `box-shadow` adds depth

The practical value: with just these properties, you can make any page look professional. Everything else in CSS builds on these fundamentals.

---

## Lecture Summary

**[Script:]** Let's wrap up what we built today in the Smart Cart:

- **HTML Structure and Semantic Tags**: We used `<header>`, `<nav>`, `<main>`, `<article>`, `<aside>`, and `<footer>` to give our page meaningful structure. Semantic tags make your page understandable to search engines, screen readers, and developers. Every tag describes content type, not appearance.

- **Forms and Input Elements**: We built a complete search form with `<form>`, `<input>`, `<select>`, `<label>`, `<fieldset>`, and `<button>`. The critical `name` attribute sends data to servers. Labels with `for` attributes connect to inputs for accessibility.

- **Intro to CSS Selectors and Properties**: We used element selectors, class concepts, and pseudo-classes like `:hover`. Properties like `padding`, `margin`, `background-color`, and `border-radius` control visual presentation. Specificity determines which styles apply: IDs beat classes, classes beat elements.

🎯 **Instructor Note:** Point to the final product files on screen.

**[Script:]** The practical value of everything we did today: you now have the foundational skills to build any website. These exact patterns—semantic HTML, forms, CSS styling—are used on every professional website you visit. Amazon, Google, Netflix—they all start with this same structure.

What you've built today is real. It has meaningful structure, working forms, and clean styling. This is the foundation everything else builds on.

Great work today. Let's keep building.
