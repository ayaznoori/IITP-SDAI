**Instructions:**  
- **MCQ1–6:** Select the **one** best answer (single correct).  
- **MCQ 7–8:** Select **all** that apply (multiple correct).  
- **Subjective:** Write your solution in valid JavaScript; test your code with provided test cases.

---

## Part A — Multiple choice (single correct)

**1.** Where does JavaScript attached via `<script src="app.js"></script>` typically execute when you open the HTML page in a normal desktop browser?

- A) The Python interpreter  
- B) The operating system kernel  
- C) The browser's JavaScript engine  
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

## Part C — Practical Assignments

### Assignment 1: Order Summary (Strings, Arrays, Objects, Loops, Conditionals)

**Objective:** Process a shopping cart and generate a formatted order summary.

**Requirements:**

1. Write a function `summarizeCart(cart)` that accepts an array of line items
2. Return an object with:
   - `lineCount`: number of items where `quantity > 0`
   - `subtotal`: total cost (includes zero-quantity items as `0`)
   - `label`: formatted string `"Items: <lineCount>, Subtotal: $<subtotal>"` (subtotal with 2 decimal places)
3. Use `for...of` loop
4. Use `const`/`let` appropriately
5. Do not mutate the input array

**Test Cases:**

```javascript
// Test 1: Normal cart
const cart1 = [
  { name: "Notebook", unitPrice: 12.5, quantity: 2 },
  { name: "Pen", unitPrice: 1.25, quantity: 5 },
  { name: "Sticker", unitPrice: 0.75, quantity: 0 },
];
// Expected: { lineCount: 2, subtotal: 28.75, label: "Items: 2, Subtotal: $28.75" }

// Test 2: Empty cart
const cart2 = [];
// Expected: { lineCount: 0, subtotal: 0, label: "Items: 0, Subtotal: $0.00" }

// Test 3: All zero quantities
const cart3 = [
  { name: "Book", unitPrice: 9.99, quantity: 0 },
  { name: "Pen", unitPrice: 2.50, quantity: 0 },
];
// Expected: { lineCount: 0, subtotal: 0, label: "Items: 0, Subtotal: $0.00" }
```

**Solution:**

```javascript
function summarizeCart(cart) {
  let subtotal = 0;
  let lineCount = 0;

  for (const item of cart) {
    subtotal += item.unitPrice * item.quantity;
    if (item.quantity > 0) {
      lineCount++;
    }
  }

  const label = `Items: ${lineCount}, Subtotal: $${subtotal.toFixed(2)}`;

  return { lineCount, subtotal, label };
}

// Test execution
console.log("Test 1:", summarizeCart(cart1));
console.log("Test 2:", summarizeCart(cart2));
console.log("Test 3:", summarizeCart(cart3));
```

**Why no mutation:** Mutating `cart` inside the function surprises callers who may reuse the same array elsewhere. This function returns a fresh summary object and leaves the input array untouched, keeping data stable and preventing unintended side effects.

---

### Assignment 2: Student Grade Report Generator

**Objective:** Process student records and generate grade analytics.

**Requirements:**

1. Write a function `generateGradeReport(students)` that:
   - Calculates average score for each student
   - Determines grade letter (A: ≥90, B: ≥80, C: ≥70, D: ≥60, F: <60)
   - Returns array of report objects with `name`, `average`, `grade`
   - Returns summary object with `classAverage` and `passedCount` (D or above)
2. Use `for...of` loop for outer iteration
3. Use proper variable declarations
4. Do not mutate input data

**Test Cases:**

```javascript
const students = [
  { name: "Alice", scores: [95, 92, 88] },
  { name: "Bob", scores: [85, 78, 82] },
  { name: "Charlie", scores: [72, 68, 75] },
  { name: "Diana", scores: [55, 60, 58] },
];
// Expected:
// reports: [
//   { name: "Alice", average: 91.67, grade: "A" },
//   { name: "Bob", average: 81.67, grade: "B" },
//   { name: "Charlie", average: 71.67, grade: "C" },
//   { name: "Diana", average: 57.67, grade: "F" }
// ]
// summary: { classAverage: 75.67, passedCount: 3 }
```

**Solution:**

```javascript
function generateGradeReport(students) {
  const reports = [];
  let totalAverage = 0;
  let passedCount = 0;

  for (const student of students) {
    let scoreSum = 0;
    for (const score of student.scores) {
      scoreSum += score;
    }
    const average = scoreSum / student.scores.length;
    
    let grade;
    if (average >= 90) {
      grade = "A";
    } else if (average >= 80) {
      grade = "B";
    } else if (average >= 70) {
      grade = "C";
    } else if (average >= 60) {
      grade = "D";
    } else {
      grade = "F";
    }

    reports.push({
      name: student.name,
      average: parseFloat(average.toFixed(2)),
      grade: grade,
    });

    totalAverage += average;
    if (grade !== "F") {
      passedCount++;
    }
  }

  const classAverage = parseFloat((totalAverage / students.length).toFixed(2));

  return {
    reports: reports,
    summary: {
      classAverage: classAverage,
      passedCount: passedCount,
    },
  };
}

// Test execution
const result = generateGradeReport(students);
console.log("Reports:", result.reports);
console.log("Summary:", result.summary);
```

---

### Assignment 3: Product Filter and Transform

**Objective:** Filter and transform product data based on price range criteria.

**Requirements:**

1. Write a function `filterAndTransform(products, minPrice, maxPrice)` that:
   - Filters products within price range (inclusive)
   - Creates new objects with `name`, `price`, `discounted` (price × 0.9), `category`
   - Returns array sorted by price (ascending)
2. Use `for...of` loop for filtering
3. Return new array without mutating input
4. Use bubble sort or similar for sorting (no built-in sort method)

**Test Cases:**

```javascript
const products = [
  { name: "Laptop", price: 1200, category: "Electronics" },
  { name: "Mouse", price: 25, category: "Electronics" },
  { name: "Desk", price: 300, category: "Furniture" },
  { name: "Chair", price: 150, category: "Furniture" },
  { name: "Monitor", price: 400, category: "Electronics" },
];

// Filter: price between 50 and 500
// Expected output (sorted by price ascending):
// [
//   { name: "Chair", price: 150, discounted: 135, category: "Furniture" },
//   { name: "Desk", price: 300, discounted: 270, category: "Furniture" },
//   { name: "Monitor", price: 400, discounted: 360, category: "Electronics" }
// ]
```

**Solution:**

```javascript
function filterAndTransform(products, minPrice, maxPrice) {
  const filtered = [];

  for (const product of products) {
    if (product.price >= minPrice && product.price <= maxPrice) {
      filtered.push({
        name: product.name,
        price: product.price,
        discounted: parseFloat((product.price * 0.9).toFixed(2)),
        category: product.category,
      });
    }
  }

  // Bubble sort by price (ascending)
  for (let i = 0; i < filtered.length; i++) {
    for (let j = i + 1; j < filtered.length; j++) {
      if (filtered[i].price > filtered[j].price) {
        const temp = filtered[i];
        filtered[i] = filtered[j];
        filtered[j] = temp;
      }
    }
  }

  return filtered;
}

// Test execution
const result = filterAndTransform(products, 50, 500);
console.log(result);
```

---

## Answer Key

**Single correct:** 1-C, 2-C, 3-C, 4-B, 5-C, 6-B  

**Multiple correct:** 7-A,C,E | 8-A,C,D
