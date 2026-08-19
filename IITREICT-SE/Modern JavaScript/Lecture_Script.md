# Lecture Script: Modern JavaScript
**Duration:** 110 minutes | **Tools:** Browser Console, VS Code | **Data:** Mock product API array

---

## Opening (8 min)

**[Script:]** "You can loop arrays with `for`. So can every developer since 1995. Modern teams write **`products.filter().map()`** because it is readable, chainable, and standard in **React** next module. Today is the syntax recruiters expect on JS screens."

> **In the Real World:** **Spotify** transforms playlist metadata with map/filter before UI render. **Flipkart** filters out-of-stock SKUs client-side for snappy UX.

---

## Concept Highlights

### Template literals + destructuring

```javascript
const order = { id: 42, user: { name: "Ravi" } };
const { id, user: { name } } = order;
console.log(`Order #${id} for ${name}`);
```

### map / filter / reduce pipeline

```javascript
const products = [
  { name: "Mouse", price: 799, inStock: true },
  { name: "Keyboard", price: 2499, inStock: false },
];

const available = products
  .filter((p) => p.inStock)
  .map((p) => ({ ...p, priceWithTax: p.price * 1.18 }));

const total = available.reduce((sum, p) => sum + p.price, 0);
```

**Predict:** How many items in `available`?

---

### Optional chaining

```javascript
const discount = cart?.promo?.percent ?? 0;
```

**Case study:** API user missing `address` field — app must not crash.

---

## Live Lab — "Mini Catalog"

Given 10 products JSON:
1. Filter in stock
2. Map to display strings with template literals
3. Reduce to inventory value
4. Sort by price (mention sort — optional stretch)

---

## Lecture Summary

- ES6+ syntax is industry default
- Array methods express data transforms clearly
- `?.` and `??` prevent null crashes
- Chaining replaces nested loops
- **Practical value:** This is the JavaScript you will write in React every day
