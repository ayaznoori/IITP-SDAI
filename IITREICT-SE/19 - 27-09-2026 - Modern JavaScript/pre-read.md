# Pre-Read: Modern JavaScript

## 1. What You'll Learn

In this pre-read, you'll discover:

- How **template literals** make string building cleaner than concatenation
- How **destructuring** unpack arrays and objects into variables in one line
- How **spread and rest operators** (`...`) copy, merge, and collect values
- How **default parameters** and **object shorthand** reduce boilerplate
- How **`map`, `filter`, and `reduce`** transform data without manual loops
- How **optional chaining (`?.`)** and **nullish coalescing (`??`)** prevent crashes on missing data

---

## 2. Detailed Explanation

### Why Modern JavaScript (ES6+)

JavaScript evolved. Code written in 2010 used `var`, string concatenation, and manual loops for every transformation. **ES6+** (ECMAScript 2015 and later) added syntax that matches how developers think — especially coming from Python.

You are learning the syntax used in **React**, **Node.js**, and every 2024+ tutorial. Old syntax still appears in legacy codebases, but new code uses modern patterns.

> **In the Real World:** **Vercel**, **Netlify**, and **Shopify** codebases overwhelmingly use ES6+ — template literals, arrow functions, destructuring, and array methods. Interviewers expect fluency with `map` and `filter`.

### Template Literals

**Old way:**

```javascript
const name = "Sara";
const msg = "Hello, " + name + "! You have " + count + " messages.";
```

**Modern way:**

```javascript
const msg = `Hello, ${name}! You have ${count} messages.`;
```

Backticks `` ` `` allow `${expression}` interpolation and multiline strings:

```javascript
const html = `
  <div class="card">
    <h2>${title}</h2>
  </div>
`;
```

### Destructuring

**Array destructuring:**

```javascript
const colors = ["red", "green", "blue"];
const [primary, secondary] = colors;
console.log(primary);  // "red"
```

**Object destructuring:**

```javascript
const user = { name: "Dev", role: "student", city: "Pune" };
const { name, role } = user;
console.log(name, role);  // "Dev" student
```

**Python comparison:** Similar to `name, role = user["name"], user["role"]` but cleaner.

### Spread and Rest

**Spread — expand into new array or object:**

```javascript
const a = [1, 2, 3];
const b = [...a, 4, 5];  // [1, 2, 3, 4, 5]

const defaults = { theme: "light", lang: "en" };
const settings = { ...defaults, theme: "dark" };  // override one key
```

**Rest — collect remaining items:**

```javascript
function sum(first, ...rest) {
  return rest.reduce((total, n) => total + n, first);
}
console.log(sum(10, 5, 3, 2));  // 20
```

### Default Parameters and Shorthand

```javascript
function createUser(name, role = "viewer", active = true) {
  return { name, role, active };  // shorthand: name: name → name
}

createUser("Ali");  // role defaults to "viewer"
```

### map, filter, reduce

**map** — transform each item:

```javascript
const prices = [100, 200, 300];
const withTax = prices.map((price) => price * 1.18);
// [118, 236, 354]
```

**filter** — keep items that pass a test:

```javascript
const scores = [45, 78, 92, 33, 88];
const passed = scores.filter((s) => s >= 40);
// [45, 78, 92, 88]
```

**reduce** — combine into one value:

```javascript
const total = prices.reduce((sum, price) => sum + price, 0);
// 600
```

**Mental model:** Assembly line. `map` modifies each item. `filter` removes unwanted items. `reduce` packs everything into one box.

### Optional Chaining and Nullish Coalescing

**Problem:** `user.address.city` crashes if `address` is null.

```javascript
const city = user?.address?.city;  // undefined instead of crash
const display = user.nickname ?? user.name;  // use name if nickname is null/undefined
```

| Operator | Meaning |
|----------|---------|
| `?.` | Safe access — stop if null/undefined |
| `??` | Default if null or undefined (not if `0` or `""`) |

> **In the Real World:** API responses from **JSONPlaceholder**, **Stripe**, or your future FastAPI backend often have optional nested fields. Optional chaining prevents production crashes.

### Refactoring Loops to Array Methods

**Loop version:**

```javascript
const names = ["amy", "bob", "carl"];
const upper = [];
for (const n of names) {
  upper.push(n.toUpperCase());
}
```

**Modern version:**

```javascript
const upper = names.map((n) => n.toUpperCase());
```

Both work. Array methods are preferred when transforming collections — especially in React `.map()` for lists.

### Why It Matters

Modern syntax appears in every React component, fetch handler, and npm package you will use. Reading open-source code requires ES6+ fluency. Writing it makes you faster and less error-prone.

**Benefits:**
- Less boilerplate, fewer bugs
- Readable data transformations
- Direct path to React list rendering and API data shaping

### Messy to Clear

**Messy:**
- String concatenation with `+` everywhere
- Manual loops when `filter` would be clearer
- `||` for defaults (treats `0` as falsy — wrong for counts)
- Ignoring optional API fields — runtime errors

**Clean:**
- Template literals for strings
- `map`/`filter`/`reduce` for collections
- `??` for null defaults
- `?.` for nested API data

### Building Blocks Checklist

- [ ] I can write template literals with `${}`
- [ ] I can destructure arrays and objects
- [ ] I can use spread to copy arrays/objects
- [ ] I can chain map and filter on an array
- [ ] I can use `?.` and `??` for safe access

---

## 3. Practice Exercises

**Exercise 1 — Template literals**
Create variables `product` and `price`. Build string: `Product: {product} costs ₹{price}` using template literals.

**Exercise 2 — Destructure**
Given `const course = { title: "Modern JS", weeks: 10, mentor: "Industry" }`, destructure `title` and `mentor` in one line. Log both.

**Exercise 3 — map and filter**
Start with `[12, 45, 67, 23, 89, 34]`. Use `filter` to keep scores >= 40. Use `map` to add 5 bonus points to each. Log final array.

**Exercise 4 — reduce**
Use `reduce` to sum an array of item prices `[299, 1499, 99, 2499]`. Log total.

**Exercise 5 — Safe access**
Given `const apiUser = { name: "Test" }` (no `profile` key), safely log `apiUser?.profile?.avatar ?? "default.png"`. Confirm no error in console.
