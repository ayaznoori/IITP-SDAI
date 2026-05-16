# Lecture Script: JavaScript Fundamentals & DOM Basics

**Duration:** 130 minutes
**Audience:** Absolute beginners
**Running Example:** FreshCart — a simple grocery shopping webpage

---

## Facilitator Overview

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 15 min |
| 2 | What Is JavaScript in the Browser? | 25 min |
| 3 | What Is the DOM? | 25 min |
| 4 | Selecting Elements | 25 min |
| 5 | Traversing the DOM | 25 min |
| 6 | Lecture Summary | 15 min |

**Running Example — FreshCart**
A grocery shopping webpage. It has a heading, a list of items, and a few buttons. Every demo and walkthrough uses this same page. Learners see one consistent HTML file grow and respond throughout the session.

---

## Block 1 — Why Does This Matter?

**Time: 15 minutes**

---

> 🎯 **Instructor Note — Opening Hook**
> Before speaking, open a browser. Go to any major shopping site. Add an item to the cart. Point to the cart count updating in the top corner. Ask the room: "Nobody refreshed the page. So how did that number change?"
> Pause. Let answers come. Do not confirm or correct yet. Hold the tension. Then begin the script.

---

**[Script:]**

"That number changing — without a page refresh — that is JavaScript. Right there. Live."

"Every time a webpage does something — responds to a click, shows a message, updates a number — JavaScript is doing that work."

"HTML writes the content. CSS makes it look good. But neither of them can *react*. If a user clicks a button, HTML does nothing. CSS does nothing. JavaScript is the only one listening."

"This matters for everything you will build. Forms that validate before submitting. Menus that open and close. Pages that feel alive instead of flat."

"If you skip this and go straight to building — you will hit a wall. You will write HTML and wonder why nothing responds. You will copy code you do not understand and have no idea how to fix it when it breaks."

"So we start here. We start with how JavaScript actually connects to a page — and how it finds and moves through what is on screen."

---

> 🎯 **Instructor Note**
> Write this on the whiteboard now. Keep it visible the entire session:
> `HTML = structure | CSS = style | JavaScript = behaviour`
> Return to it every time a new concept is introduced.

---

## Block 2 — What Is JavaScript in the Browser?

**Time: 25 minutes**

---

### Explain the Concept

**[Script:]**

"Let's be precise about what we mean. JavaScript is a programming language. But right now we care about one specific version of it — JavaScript running *inside a browser*."

"The browser is not just a viewer. It is an engine. When it loads your HTML file, it also runs any JavaScript attached to it."

"JavaScript in the browser can read the page, change the page, and respond to what the user does. That is its job."

---

> 🎯 **Instructor Note — Comparison**
> If anyone in the room has seen Python before, use this comparison. If not, skip it.

---

**[Script — Optional Python Comparison:]**

"If you have seen Python, you ran it in a terminal. You typed a command, it printed something, it stopped."

"JavaScript in the browser is different. It does not stop. It sits and waits. When a user clicks, it wakes up and responds. When a user types, it can react in real time."

"Same language concept — completely different environment."

---

### How JavaScript Connects to HTML

**[Script:]**

"So how do you attach JavaScript to an HTML file? One tag. The `<script>` tag."

"You write your HTML as normal. Then, before the closing `</body>` tag, you add one line."

Write on whiteboard:

```html
<script src="app.js"></script>
```

"That tells the browser: after you have loaded the HTML, go and fetch this JavaScript file and run it."

"Why at the bottom? Because JavaScript runs the moment the browser hits it. If you put it at the top, it runs before your HTML elements exist. Then it tries to find a button that has not appeared yet. It fails."

"Bottom of body. Always. That is the rule for now."

---

> 🎯 **Instructor Note — Common Mistake**
> Learners often place the script tag in the `<head>`. If anyone asks why not the head — explain it simply: "The head loads before the body. Your elements do not exist yet. JavaScript cannot find what is not there."

---

### Walkthrough — FreshCart Setup

**[Script:]**

"Here is our FreshCart page. This is what we will use for everything today."

Write or display this HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>FreshCart</title>
  </head>
  <body>
    <h1 id="store-name">FreshCart</h1>
    <ul id="item-list">
      <li class="item">Apples</li>
      <li class="item">Bread</li>
      <li class="item">Milk</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```

"Simple page. A heading. A list. Three items. A script tag at the bottom linking to `app.js`."

"Every demo today runs against this exact file. When JavaScript does something, you will be able to see exactly what it is touching and why."

---

### Demo 1 — Confirming the Connection

> 🎯 **Instructor Note**
> Open a code editor live. Create the HTML file above. Create a blank `app.js` next to it. Add one line to `app.js`. Open the HTML in the browser. Show the console output.

**[Script:]**

"Let's confirm the connection is working. I am going to put one line in `app.js`."

```js
console.log("FreshCart is connected.");
```

"Before I run this — predict with me. Where will this output appear? On the page? In a popup?"

**Pause. Take answers.**

"Open DevTools. F12. Go to the Console tab. There it is. That message came from our JavaScript file, running inside the browser."

"That is the connection. HTML in one file. JavaScript in another. Linked by that script tag. Both running together."

---

> 🎯 **Instructor Note — Pause Point**
> Ask: "Any questions about the script tag or why we place it at the bottom?" This is a frequent confusion point. Give it a full minute before moving on.

---

## Block 3 — What Is the DOM?

**Time: 25 minutes**

---

### Define the Concept

**[Script:]**

"Now we need to talk about the DOM. This is one of the most important ideas in browser JavaScript."

"DOM stands for Document Object Model."

"Here is the plain-English version: when the browser reads your HTML file, it does not just display it. It builds a *map* of every element on the page. That map is the DOM."

"The DOM is not your HTML file. Your HTML file is text. The DOM is the browser's live, working model of that text — built in memory, ready to be read and changed."

---

### Mental Model — The Family Tree

**[Script:]**

"Think of a family tree. One ancestor at the top. Children below them. Grandchildren below those. Every person has exactly one parent, except the one at the very top."

"The DOM is the same structure. One root at the top. Elements branching below it. Every element sits somewhere in that tree."

Write on whiteboard:

```
document
└── html
    ├── head
    │   └── title
    └── body
        ├── h1
        └── ul
            ├── li
            ├── li
            └── li
```

"This is the DOM for our FreshCart page. `document` is the root. Everything else branches from it."

"Each box in that tree is called a **node**. The nodes you will work with most are **element nodes** — things like `<h1>`, `<ul>`, `<li>`."

---

> 🎯 **Instructor Note — Whiteboard Activity**
> Draw the tree above slowly, naming each element as you place it. Then ask a learner: "Where does the `<ul>` sit in this tree?" Ask another: "How many children does the `<ul>` have?" This takes two minutes and locks in the mental model before code appears.

---

### Why the DOM Matters

**[Script:]**

"Here is why this matters so much. JavaScript cannot touch your HTML file directly. It is just a text file sitting on a server."

"What JavaScript *can* touch is the DOM — the live map the browser built. When JavaScript changes the DOM, the browser immediately updates what is on screen."

"This is how cart counts update. This is how menus open. This is how error messages appear. JavaScript changes the DOM. The browser reflects that change. The user sees it."

---

> 🎯 **Instructor Note — Common Mistake**
> A very common early confusion: learners think editing JavaScript changes what is on screen because it changes the HTML file. It does not. JavaScript changes the DOM. The HTML file stays exactly the same. If you refresh, the original HTML loads again and the DOM is rebuilt from scratch. Make this explicit.

---

**[Script:]**

"One more thing to fix before we move on. The DOM and the HTML file are not the same thing."

"Your HTML file does not change when JavaScript runs. Open it in a text editor after running JavaScript — it looks identical. What changed is the browser's internal model. The DOM. Not the file."

---

### Quick Comprehension Check

> 🎯 **Instructor Note**
> Ask the room — no hands needed, just call-and-response:
> - "What does DOM stand for?" — Document Object Model
> - "Is the DOM the same as the HTML file?" — No
> - "When JavaScript changes the DOM, what does the user see?" — The page updates

---

## Block 4 — Selecting Elements

**Time: 25 minutes**

---

### The Core Problem

**[Script:]**

"Here is the problem we need to solve. JavaScript can change the DOM. But to change an element, it first has to *find* it."

"Imagine you are in a warehouse full of boxes. You cannot just say 'change a box.' You have to say which box. You have to locate it first."

"Selecting elements is how JavaScript locates a specific box in the DOM warehouse."

---

### Method 1 — getElementById

**[Script:]**

"The most direct method. If an element has an `id` attribute, you can find it instantly."

Write on whiteboard:

```js
document.getElementById("store-name");
```

"Read this left to right. Start at `document` — that is the root of the DOM, the top of the tree. Call `getElementById`. Pass in the id as a string. The browser searches and returns that element."

"One id, one element. Ids are unique. This method always returns exactly one thing."

---

> 🎯 **Instructor Note**
> Point back to the FreshCart HTML. Show `id="store-name"` on the `<h1>`. Then show `id="item-list"` on the `<ul>`. Make the connection visual before writing code.

---

**[Script:]**

"Let's store what we find. We use `const` to hold the result."

```js
const heading = document.getElementById("store-name");
```

"Now `heading` holds a reference to that `<h1>` element in the DOM. We have not changed anything yet. We have just found it and given it a name."

---

### Method 2 — querySelector

**[Script:]**

"The second method is more flexible. `querySelector` accepts any CSS selector."

```js
document.querySelector("h1");
document.querySelector(".item");
document.querySelector("#item-list");
```

"You already know CSS selectors. Tag name, class with a dot, id with a hash. They all work here exactly the same way."

"One important rule: `querySelector` returns only the *first* matching element. If there are three `<li>` elements with class `item`, you get back the first one. Just the first."

---

> 🎯 **Instructor Note — Common Mistake**
> Learners frequently expect `querySelector(".item")` to return all matching elements. It does not. It returns one — the first. Make this contrast explicit and dramatic. Repeat it. This mistake causes real bugs.

---

### Demo 2 — Selecting FreshCart Elements

> 🎯 **Instructor Note**
> Write this live in `app.js`. Show output in the browser console. Do not paste — type it so learners can read at your speed.

**[Script:]**

"Predict before running: if I write this in `app.js`, what will appear in the console?"

```js
const heading = document.getElementById("store-name");
const firstItem = document.querySelector(".item");

console.log(heading);
console.log(firstItem);
```

**Pause. Take predictions.**

"Run it. Look at the console. The browser prints the actual element — you can see its tag, its content. That is not just a string. That is a live DOM element. We can do things with it."

"Notice `firstItem` — it found the first `<li>` with class `item`. Not all three. Just the first."

---

> 🎯 **Instructor Note — Pause Point**
> Ask: "What is the difference between `getElementById` and `querySelector`?" Let learners answer. Guide toward: one takes an id only, one takes any CSS selector. One is more specific, one is more flexible. Both return a single element.

---

### Predict and Discuss

**[Script:]**

"One more prediction question. What does this return?"

```js
document.querySelector("#item-list");
```

"Take a moment. Think about what `#item-list` means as a CSS selector."

**Pause.**

"It returns the `<ul>`. The hash means id. Same as `getElementById("item-list")` — different syntax, same result."

---

## Block 5 — Traversing the DOM

**Time: 25 minutes**

---

### The Core Problem

**[Script:]**

"We can find elements by searching. But sometimes searching is not the best tool."

"Imagine you already have the `<ul>` — the shopping list. Now you want the first item in that list. You could search for it. Or — you could just step into it from where you already are."

"That is traversal. Moving through the DOM tree using relationships. Parent to child. Child to parent. Sibling to sibling."

---

### The Relationships

**[Script:]**

"Every element in the DOM tree has relationships. Let's name them."

Write on whiteboard:

```
ul  ← parentElement of li
 ├── li  ← firstElementChild of ul
 ├── li  ← nextElementSibling of first li
 └── li
```

"If I have a `<ul>`, I can step down to its children. If I have a `<li>`, I can step up to its parent. If I have one `<li>`, I can step sideways to the next one."

"These are the four properties you need right now."

| Property | What it returns |
|---|---|
| `element.parentElement` | The element directly above |
| `element.children` | All direct children |
| `element.firstElementChild` | The first child element |
| `element.nextElementSibling` | The next element at the same level |

---

> 🎯 **Instructor Note — Whiteboard Activity**
> Draw the FreshCart DOM tree again. Point to the `<ul>`. Ask: "What is its `firstElementChild`?" Point to the first `<li>`. Ask: "What is its `parentElement`?" Ask: "What is its `nextElementSibling`?" Do this verbally and visually before any code. The tree does the teaching here.

---

### Walkthrough — Logic Before Code

**[Script:]**

"Here is how to think through traversal before you write a single line."

"I want the first item in the FreshCart list."

"Step one: I know how to find the list. `getElementById("item-list")` gets me the `<ul>`."

"Step two: From the `<ul>`, I want to step into its first child. That is `firstElementChild`."

"Step three: That gives me the first `<li>`. Done."

"Now I write the code that matches that logic exactly."

---

### Demo 3 — Traversing the FreshCart List

> 🎯 **Instructor Note**
> Build this live, one line at a time. Log each variable so learners see each step in the console.

**[Script:]**

"Predict before running: what will each `console.log` print?"

```js
const list = document.getElementById("item-list");
const firstItem = list.firstElementChild;
const secondItem = firstItem.nextElementSibling;

console.log(list);
console.log(firstItem);
console.log(secondItem);
```

**Pause. Take predictions.**

"Run it."

"There is the `<ul>`. There is the first `<li>` — Apples. There is the second `<li>` — Bread. We never searched for Apples or Bread by name. We navigated to them using the tree."

---

> 🎯 **Instructor Note — Pause Point**
> Ask: "Why would you use traversal instead of just calling `querySelector` again?" Let learners think. Guide toward: traversal is faster and more reliable when you already have a nearby element. It also makes relationships in your code explicit and readable.

---

### Common Mistakes — Traversal

**[Script:]**

"Two mistakes to watch for here."

"First — `children` versus `firstElementChild`. `children` gives you a collection — like a list of all children. `firstElementChild` gives you one element. They are not the same thing."

"Second — there is also a property called `childNodes` and `nextSibling`. Those include text nodes — invisible gaps and line breaks in your HTML. They will give you strange results. Stick with `firstElementChild`, `nextElementSibling`, and `children` for now. These only return elements."

---

> 🎯 **Instructor Note**
> If a curious learner asks about `childNodes` — validate the question, confirm it exists, and explain that it includes non-element nodes like text. Then redirect: "For everything we are building right now, the element-specific versions are what you need."

---

### Predict and Discuss

**[Script:]**

"Final prediction before the summary."

"Given the FreshCart HTML — what does this code return at each step?"

```js
const list = document.getElementById("item-list");
const last = list.children[2];
const parent = last.parentElement;
```

"Talk through it with the person next to you. One minute."

**Pause. Let learners discuss.**

"Right. `list` is the `<ul>`. `list.children[2]` is the third `<li>` — remember, counting starts at zero — so that is Milk. `parent` steps back up to the `<ul>`. We went down and then back up."

---

## Block 6 — Lecture Summary

**Time: 15 minutes**

---

> 🎯 **Instructor Note — Delivery**
> Do not read this as a list. Speak each bullet as a statement. After each one, pause and ask "Does that match what we did today?" Let the room confirm before moving to the next.

---

**[Script:]**

"Let's close by naming exactly what we covered."

- JavaScript is a programming language that runs inside the browser and makes pages respond to users

- You connect JavaScript to an HTML page using a `<script>` tag placed before the closing `</body>` tag

- The browser reads your HTML and builds the DOM — a live tree of every element on the page — which JavaScript can read and change

- The DOM and the HTML file are not the same thing — JavaScript changes the DOM, and the browser updates what the user sees, but the original file is unchanged

- `document.getElementById()` finds a single element by its unique `id` attribute

- `document.querySelector()` finds the first element that matches any CSS selector

- Traversal lets you move through the DOM tree using relationships — `parentElement`, `children`, `firstElementChild`, and `nextElementSibling` — without searching from scratch

- Understanding how to select and traverse the DOM is the foundation for everything interactive on the web — nothing moves, updates, or responds without JavaScript first finding the right element

---

> 🎯 **Instructor Note — Closing Question**
> Ask the room: "In one sentence — what is the DOM?" Cold call two or three learners. You are listening for: the browser's live map or model of the HTML page. Correct gently if needed. End on that answer. It is the clearest signal of whether the core concept landed.

---

**[Script — Close:]**

"Everything we did today was about one skill: getting JavaScript to the right place in the page. Find the element. Navigate the tree. That is the foundation. Build it solid and the rest follows."

---

*End of Lecture Script*
