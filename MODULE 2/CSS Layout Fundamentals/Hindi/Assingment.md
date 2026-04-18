# CSS Basics: Selectors & Box Model — Module Assessment

**Section A:** 8 multiple-choice questions — pick the **best** answer.  
**Section B:** 1 **practical** task — you will write or fix **real HTML and CSS** (submit code, not long essays).

---

## Section A — Multiple choice

**1.** You have many HTML pages that should all use the **same** colors and fonts. What is usually the **best** way to write CSS?

- A) Put `style="..."` on every element  
- B) Put CSS only inside one page’s `<style>` tag  
- C) Use **one external** `.css` file and link it from each page  
- D) Do not use CSS files  

---

**2.** Which CSS selector selects **every** `<h2>` on the page?

- A) `#h2`  
- B) `.h2`  
- C) `h2`  
- D) `*h2`  

---

**3.** You have **five** product cards that should look the same. What do you put in the HTML?

- A) The same `id` on all five (e.g. `id="card"`)  
- B) The same **`class`** on all five (e.g. `class="card"`)  
- C) Only inline styles on the first card  
- D) Five different tag names  

---

**4.** Which HTML matches this CSS: `.sale { color: red; }`?

- A) `<p id="sale">Text</p>`  
- B) `<p class="sale">Text</p>`  
- C) `<p sale>Text</p>`  
- D) `<sale>Text</sale>`  

---

**5.** A `<p>` has `style="color: red;"` **and** your `style.css` has `p { color: blue; }`. What color is that paragraph? *(Use the simple rule from class: inline vs external.)*

- A) Blue  
- B) Red  
- C) Both at once  
- D) Black only  

---

**6.** In the box model, the space **between the content and the border** is called:

- A) Margin  
- B) Padding  
- C) Width  
- D) Border  

---

**7.** **Margin** is best described as:

- A) Space **inside** the box, around the text  
- B) Space **outside** the box, between this element and others  
- C) The line around the box  
- D) The text inside the box  

---

**8.** **Padding** is best described as:

- A) Space **outside** the border  
- B) Space **inside** the border, around the content  
- C) The same as margin  
- D) Only the text itself  

---

## Section B — Practical task (write code)

### Task: refactor messy cards + build `styles.css`

You are given **broken** HTML below. It uses **inline styles** and **wrong** use of `id`. Your job is to **fix it** and supply a **separate** stylesheet.

**Requirements (checklist — your solution must satisfy all):**

1. Remove **all** `style="..."` attributes from the three boxes.  
2. Use **one class** named **`card`** on all three `<div>`s (same class, not the same `id`).  
3. Only **one** `<div>` may keep a **unique** `id`: change the middle box to `id="featured-card"` (the other two have **no** `id`).  
4. Create **`styles.css`** linked with `<link rel="stylesheet" href="styles.css">` inside `<head>`.  
5. In **`styles.css`**, style **`.card`** so every card has:
   - `background-color: #e3f2fd;`  
   - `padding: 18px;`  
   - `margin-bottom: 14px;`  
   - `border: 2px solid #1565c0;`  
   - `max-width: 320px;`  
6. Style **`#featured-card`** so **only the middle** card gets **`border: 4px solid #ff6f00;`** (keep other `.card` rules still applying where they don’t conflict — you may override only the border width/color on the featured one).

**Starting HTML (copy and fix this):**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Cards</title>
  <!-- TODO: link styles.css here -->
</head>
<body>
  <h2>Our services</h2>

  <div id="card" style="background: yellow; padding: 10px;">
    <p>Design</p>
  </div>
  <div id="card" style="background: yellow; padding: 10px;">
    <p>Development</p>
  </div>
  <div id="card" style="background: yellow; padding: 10px;">
    <p>Support</p>
  </div>
</body>
</html>
```

**What to submit:**

- Your **full fixed `index.html`** (or paste into the answer box).  
- The **full `styles.css`** file content.

**Grading hint (students):** Open the page in a browser — you should see three blue-tinted cards, stacked, with the **middle** card’s border thicker/orange.

---

### Answer key (instructor / self-check)

**MCQ:** 1-C, 2-C, 3-B, 4-B, 5-B, 6-B, 7-B, 8-B  

**Practical task — sample solution:**

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Cards</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <h2>Our services</h2>

  <div class="card">
    <p>Design</p>
  </div>
  <div class="card" id="featured-card">
    <p>Development</p>
  </div>
  <div class="card">
    <p>Support</p>
  </div>
</body>
</html>
```

**styles.css**

```css
.card {
  background-color: #e3f2fd;
  padding: 18px;
  margin-bottom: 14px;
  border: 2px solid #1565c0;
  max-width: 320px;
}

#featured-card {
  border: 4px solid #ff6f00;
}
```

---

**End of assessment.**
