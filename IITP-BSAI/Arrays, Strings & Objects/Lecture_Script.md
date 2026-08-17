# Lecture Script: Arrays, Strings & Objects
**Duration:** 110 minutes | **Tool:** One Compiler | **Language:** JavaScript

---

## Session Opening (5 min)

**[Script:]** "So far you stored one value at a time. Real apps hold lists of products, user profiles, and messages. Today: three data structures that appear in every codebase on earth."

**Problem hook:** A playlist app needs song titles (array), the currently playing label (string), and artist metadata (object). One screen, three structures.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show a JSON API response screenshot (or whiteboard sketch) — point out arrays `[]` and objects `{}`.

**[Script:]** "APIs send JSON. JSON is arrays and objects. If you cannot read and shape this data, you cannot build frontend or backend. Strings are how users see your app."

- **Real-world use:** Cart items, search results, user settings, chat text
- **Pain if misunderstood:** Wrong index (off-by-one), mutating shared arrays, confusing string vs array methods

---

## What Is the Concept?

### Arrays

**Definition:** Ordered, index-based collection. Zero-indexed.

**Mental model:** A numbered shelf. `push`/`pop` at the end.

**Key methods today:** `push`, `pop`, `length`, `map`

**Common mistake:** `arr[arr.length]` is undefined — last index is `length - 1`.

### Strings

**Definition:** Immutable sequence of characters.

**Mental model:** A labeled ribbon — slice a piece without destroying the original.

**Methods:** `slice`, `split`, `join`, `toUpperCase`, `toLowerCase`, `trim`

### Objects

**Definition:** Collection of key-value properties.

**Mental model:** A contact card — fields with names.

**Access:** dot notation `user.name` or bracket `user["name"]`

### Choosing the Right Structure

🎯 **Instructor Note:** Rapid-fire — "Temperatures for the week?" (array) "User email?" (string) "Product with price and SKU?" (object)

---

## How Do We Apply It?

### LO 1: Create and manipulate arrays using indexing, push, pop, length, and map

**Problem:** Track scores and double each for a bonus round.

**Translate logic:** Array of scores → `map` to double → log.

**Write code:**
```javascript
const scores = [10, 20, 30];
scores.push(40);
const bonus = scores.map(s => s * 2);
console.log(scores.length, bonus);
```

**Predict before running:** `length`? `bonus`?

**Explain result:** `length` is 4 after push. `bonus` is `[20, 40, 60, 80]`. Original `scores` unchanged by `map`.

**Demo pop:**
```javascript
scores.pop();
console.log(scores);
```
**Predict:** `[10, 20, 30]` — last item removed.

---

### LO 2: Work with strings using indexing, slicing, and common string methods

**Problem:** Clean and format a username from messy input.

**Translate logic:** `trim` → `toLowerCase` → `slice` first 5 chars.

**Write code:**
```javascript
const raw = "  MASAI  ";
const clean = raw.trim().toLowerCase();
console.log(clean.slice(0, 3));
```

**Predict:** `"mas"`

**Split/join demo:**
```javascript
const tags = "js,html,css".split(",");
console.log(tags.join(" | "));
```
**Predict:** `"js | html | css"`

---

### LO 3: Create objects and read or update their properties

**Problem:** Store and update a student record.

**Write code:**
```javascript
const student = { name: "Priya", grade: "B" };
student.grade = "A";
console.log(student.name, student.grade);
```

**Predict:** `Priya A`

🎯 **Instructor Note:** Add a new property live: `student.city = "Pune"`

---

### LO 4: Choose array, string, or object for a simple data need

**Problem:** Design data for a todo item with text and done flag.

**Translate logic:** One item → object; many items → array of objects.

**Write code:**
```javascript
const todos = [
  { text: "Study JS", done: false },
  { text: "Walk", done: true }
];
console.log(todos[0].text);
```

**Predict:** `"Study JS"`

---

### LO 5: Solve short practice problems using these three structures

**Guided problem:** Given `const words = ["code", "learn", "build"]`, produce uppercase comma-separated string.

**Write code:**
```javascript
const words = ["code", "learn", "build"];
const line = words.map(w => w.toUpperCase()).join(", ");
console.log(line);
```

**Predict:** `"CODE, LEARN, BUILD"`

**Pair exercise (8 min):** Given a product object and a list of tags (strings), students write code to print `"Product: {name} | Tags: tag1-tag2"`.

---

## Live Demo Block (15 min)

Build a mini "class roster":
- Array of student objects
- `map` to list names in uppercase
- `filter` mention only — or manual loop if not in LO (stick to map)
- Update one student's property and show change

---

## Recap (10 min)

Three-structure quiz: instructor names a scenario, class shouts array/string/object.

---

## Lecture Summary

- **Arrays** store ordered lists — index, `push`, `pop`, `length`, `map`
- **Strings** hold text — slice, split, join, case and trim helpers
- **Objects** group named fields — read and update with dot notation
- **Choosing the right structure** keeps code clear and API-friendly
- **Combining all three** mirrors real app data models
- **Practical value:** Every fetch, form, and React component you build will use these shapes

**[Script:]** "Next session: functions — the tool that stops you from rewriting the same logic. You now have data; next you package behavior."
