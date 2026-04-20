
**Instructions:**  
- **MCQ1–6:** Select the **one** best answer (single correct).  
- **MCQ 7–8:** Select **all** that apply (multiple correct).  
- **Subjective:** Write your solution in clear steps; you may use pseudocode where syntax is not graded, but prefer valid JavaScript.

---

## Part A — Multiple choice (single correct)

**1.** Where does JavaScript attached via `<script src="app.js"></script>` typically execute when you open the HTML page in a normal desktop browser?

- A) The Python interpreter  
- B) The operating system kernel  
- C) The browser’s JavaScript engine  
- D) The HTML parser only (scripts are not executed)  

---

**2.** Which declaration should you **prefer by default** for a binding that will never be reassigned to a new value?

- A) `var`  
- B) `let`  
- C) `const`  
- D) `function`  

---

**3.** What does `typeof []` evaluate to in JavaScript?

- A) `"array"`  
- B) `"list"`  
- C) `"object"`  
- D) `"undefined"`  

---

**4.** What is the result of `0 === false`?

- A) `true`  
- B) `false`  
- C) `undefined`  
- D) It throws a `TypeError`  

---

**5.** Consider:

```javascript
{
  let x = 1;
}
console.log(x);
```

What happens?

- A) Prints `1`  
- B) Prints `undefined`  
- C) Throws a `ReferenceError`  
- D) Prints `null`  

---

**6.** Which loop is most idiomatic for iterating over **values** of an array `items` in modern JavaScript (among the choices below)?

- A) `for (let i in items) { ... }`  
- B) `for (const x of items) { ... }`  
- C) `while (items) { ... }`  
- D) `do { ... } while (items.length)`  

---

## Part B — Multiple choice (multiple correct — select all that apply)

**7.** Which of the following are **falsy** in JavaScript? *(Select all that apply.)*

- A) `0`  
- B) `"0"`  
- C) `""`  
- D) `[]`  
- E) `null`  

---

**8.** Given:

```javascript
const user = { name: "Ada", scores: [10, 20] };
```

Which statements are **valid** and **do not** throw an error? *(Select all that apply.)*

- A) `user.name = "Grace";`  
- B) `user = { name: "Grace" };`  
- C) `user.scores.push(30);`  
- D) `user.scores = [1, 2, 3];`  

---

## Part C — Subjective question

### Problem: Order summary (strings, arrays, objects, loops, conditionals)

You are given an array of **line items** for an e-commerce cart. Each item is an object:

```javascript
// Example input (you can assume this shape)
const cart = [
  { name: "Notebook", unitPrice: 12.5, quantity: 2 },
  { name: "Pen", unitPrice: 1.25, quantity: 5 },
  { name: "Sticker", unitPrice: 0.75, quantity: 0 },
];
```

**Tasks:**

1. Write a function `summarizeCart(cart)` that returns a **new object** with:
   - `lineCount`: number of line items where `quantity > 0`  
   - `subtotal`: sum over all lines of `unitPrice * quantity` (include lines with `quantity === 0` as contributing `0`)  
   - `label`: a single string built with a **template literal**, exactly in this pattern:  
     `"Items: <lineCount>, Subtotal: $<subtotal>"`  
     where `<subtotal>` is shown with **two decimal places** (hint: you may use `toFixed(2)` on a number — be careful about types when concatenating).

2. Explain in **2–4 sentences** why mutating `cart` directly inside the function could be problematic for callers, and what you did to avoid that (if you avoided it).

**Constraints / expectations:**

- Use at least one **`for...of`** loop.  
- Use **`const` / `let`** appropriately.  
- Do **not** use external libraries.

---

### Answer key (for instructor — remove or separate when sharing with students)

**Single correct:** 1-C, 2-C, 3-C, 4-B, 5-C, 6-B  

**Multiple correct:** 7-A,C,E | 8-A,C  

**Subjective — sample solution sketch:**

```javascript
function summarizeCart(cart) {
  let subtotal = 0;
  let lineCount = 0;

  for (const item of cart) {
    subtotal += item.unitPrice * item.quantity;
    if (item.quantity > 0) lineCount++;
  }

  const label = `Items: ${lineCount}, Subtotal: $${subtotal.toFixed(2)}`;

  return { lineCount, subtotal, label };
}
```

**Explanation sample:** Mutating `cart` (e.g. pushing items or changing quantities) surprises callers who may reuse the same array elsewhere; returning a fresh summary object avoids side effects and keeps input data stable.

---

**Total MCQ:** 8 (6 single-select + 2 multi-select)  
**Subjective:** 1
