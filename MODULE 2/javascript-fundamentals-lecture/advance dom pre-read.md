# Pre-Read: Advanced DOM Manipulation and Asynchronous Programming

## What You'll Learn

In this pre-read, you'll discover:

- How to find, create, and change elements on a webpage using JavaScript
- Ways to add and remove styles and CSS classes from elements dynamically
- The difference between code that runs right away and code that runs later
- How to make things happen after a delay using `setTimeout` and `setInterval`
- Simple ways to combine timing with updating the page

---

## Detailed Explanation

### Part 1: DOM Manipulation

**What is the DOM?**

The **DOM** (Document Object Model) is a map of your webpage. JavaScript reads this map to find and change any element on the page.

Think of the DOM like a family tree for HTML. Every tag (`<div>`, `<p>`, `<button>`) is a person in that tree. You can travel up, down, and across the tree to find any element you need.

**Why It Matters**

Imagine building a to-do list app. When someone clicks "Add Item," you need to create a new HTML element and put it on the screen. Without DOM manipulation, your page would stay frozen forever. You could never update prices, show alerts, or build interactive features.

Benefits of DOM manipulation:
- Build interactive websites users can click and play with
- Update content without reloading the page
- Create dynamic user experiences

**Finding Elements**

You can search the DOM to find elements:

```javascript
// Find one element by its ID
let header = document.getElementById("main-title");

// Find all elements with a class name
let buttons = document.getElementsByClassName("btn");
```

**Creating and Adding Elements**

You can make brand new HTML elements and put them on the page:

```javascript
// Create a new paragraph
let newParagraph = document.createElement("p");
newParagraph.innerText = "Hello, world!";

// Add it to the page
document.body.appendChild(newParagraph);
```

**Changing Styles**

You can change how elements look using JavaScript:

```javascript
let box = document.getElementById("my-box");
box.style.backgroundColor = "blue";
box.style.width = "200px";
```

**Adding and Removing CSS Classes**

Classes are powerful. You can add or remove them to style elements:

```javascript
let element = document.getElementById("card");

// Add a class
element.classList.add("highlighted");

// Remove a class
element.classList.remove("hidden");

// Check if element has a class
if (element.classList.contains("active")) {
  // do something
}
```

---

### Part 2: Asynchronous JavaScript

**What does asynchronous mean?**

**Synchronous** code runs line by line, one after another. Each line waits for the previous one to finish.

**Asynchronous** code can run while other code continues. It doesn't block the rest of your program.

**Analogy**

Imagine ordering coffee at a shop. Synchronous: you stand at the counter, wait for your coffee, then leave. Asynchronous: you order, get a number, sit down, and do other things while your coffee is made. When it's ready, they call your number.

**Why It Matters**

Web pages need to do many things at once: fetch data, run animations, wait for user clicks. If everything ran synchronously, your page would freeze while waiting for data to load.

Benefits:
- Pages stay responsive while loading
- Animations and timers work smoothly
- Users see updates without waiting

**setTimeout – Running Code Later**

`setTimeout` runs code after a delay:

```javascript
// Run this after 2 seconds
setTimeout(function() {
  console.log("Two seconds passed!");
}, 2000);
```

**setInterval – Running Code Over and Over**

`setInterval` runs code repeatedly:

```javascript
// Run every 1 second
let count = 0;
let timer = setInterval(function() {
  count++;
  console.log("Count:", count);
}, 1000);

// Stop after 5 seconds
setTimeout(function() {
  clearInterval(timer);
}, 5000);
```

**Combining DOM and Asynchronous Code**

You can update the page after a delay:

```javascript
let message = document.getElementById("status");

setTimeout(function() {
  message.innerText = "Loading complete!";
  message.classList.add("success");
}, 3000);
```

---

## Practice Exercises

1. Create a `<div>` with the ID "container", then use JavaScript to change its background color to red.

2. Write code that creates three new `<p>` elements and adds them to the body of your page.

3. Add the class "active" to a button element, then remove it after 3 seconds using `setTimeout`.

4. Use `setInterval` to count from 1 to 5, printing each number to the console. Stop the interval when you reach 5.

5. Create a button. When clicked, use `setTimeout` to change the button's text to "Wait..." after 1 second, then to "Done!" after another 2 seconds.

6. Write a function that takes an element ID and a color, then changes that element's background to the given color. Call it with two different elements and colors.