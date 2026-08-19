# Lecture Script: React Fundamentals — Theory, Components, JSX & Hooks
**Duration:** 110 minutes | **Tools:** Node.js, VS Code, Chrome | **Stack:** Vite React only | **Hooks allowed:** `useState`, `useEffect` only

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 6 min | Vanilla list vs component |
| Why Does This Matter? | 10 min | Industry default UI |
| What Is the Concept? | 22 min | VDOM, JSX, props, hooks |
| How Do We Apply It? (LOs) | 60 min | Vite app live |
| Recap | 12 min | Tomorrow CRUD |

---

## Session Opening (6 min)

**Problem:** Fetch page mixed HTML, CSS, JS in one file. Five widgets will fight.

**[Script:]** "**React** splits the screen into **components**. **JSX** describes UI. **props** pass data. **useState** for values that change. **useEffect** for mount and update side effects. No other hooks today. **Vite** is the starter."

> **In the Real World:** **Meta**, **Shopify** admin, **Cloudflare** dashboards — component trees. Job posts say React because teams share this model.

🎯 **Instructor Note:** Keep `createElement` memory alive: "React will call the real DOM; you stop micromanaging it."

---

## Why Does This Matter?

🎯 **Instructor Note:** "Who wants to re-bind Delete on every new `li`?" Hands. "State + list render fixes that."

**[Script:]** "Tomorrow's **Todo CRUD** is this session plus `localStorage` and one GET. If JSX and `setState` are shaky, CRUD will collapse."

**Pain if misunderstood:**
- Mutating state directly
- Infinite effects (`useEffect` without deps setting state)
- Using `class` in JSX

---

## What Is the Concept?

### Component architecture + virtual DOM (beginner)

UI = tree of functions. Virtual DOM = description used to **diff** (keep lightweight; no fiber internals).

### Vite + JSX + functions

`npm create vite@latest` template `react`. `App.jsx` exports a function.

### Props

Inputs to the function. Immutable from the child's point of view.

### useState

Triggers re-render. New snapshot of JSX.

### useEffect(mount/update)

Sync with **outside**: `document.title`, `fetch` (show pattern; full API todo is tomorrow), `console.log`.

**Python vs JS:** Python functions return numbers. React functions return **UI descriptions**.

**Forbidden today:** `useRef`, `useContext`, `useReducer`, `useMemo`, `useCallback`, class components, Redux, FastAPI.

---

## How Do We Apply It?

### LO 1: Component architecture and virtual DOM (beginner)

**Problem:** Header, list, footer as one soup.

**Translate logic:** `Header`, `TodoList`, `Footer` functions. `App` composes them.

**Walkthrough:** Boxes on board. Arrows = props down.

**Predict:** If `App` state changes, who re-renders?

**Explain result:** `App` and children that receive new props. React **reconciles**. Beginner line: "React updates the parts that changed."

> **In the Real World:** **YouTube** player vs comments are separate components.

---

### LO 2: Vite React setup; functional components; JSX

**Live (students follow):**

```bash
npm create vite@latest react-basics -- --template react
cd react-basics
npm install
npm run dev
```

**Write code:**

```jsx
function App() {
  const hour = 10;
  return (
    <main>
      <h1 className="title">React Basics</h1>
      <p>Hour: {hour}</p>
    </main>
  );
}
```

**Predict before running:** `class` vs `className` — which works in JSX?

**Explain result:** **`className`**. `class` is a JS reserved word.

**Demo:** `{2 + 2}` in JSX. Predict `4`.

🎯 **Instructor Note:** Self-close `<img />`. One parent element (or fragment `<>...</>` if you want — mention fragment as optional).

---

### LO 3: Pass data with props

**Write code:**

```jsx
function Student({ name, batch }) {
  return <p>{name} — {batch}</p>;
}

function App() {
  return <Student name="Asha" batch="SE" />;
}
```

**Predict before running:** Can `Student` do `name = "Raj"`?

**Explain result:** Should not. Lift state to parent if it must change. Props in, events out (show `onSelect` callback prop if time).

```jsx
function Button({ label, onGo }) {
  return <button onClick={onGo}>{label}</button>;
}
```

> **In the Real World:** **Design systems** are props: `variant`, `size`, `label`.

---

### LO 4: useState

**Problem:** Counter and a name field.

**Write code:**

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>
      Clicks {count}
    </button>
  );
}
```

**Predict before running:** Two clicks — display?

**Explain result:** `2`. Each `setCount` re-renders.

**Common bug demo:** `count++` — show it failing to update reliably. Fix with setter.

**Input:**

```jsx
const [name, setName] = useState("");
<input value={name} onChange={(e) => setName(e.target.value)} />
```

---

### LO 5: useEffect on mount and update

**Mount:**

```jsx
useEffect(() => {
  console.log("App mounted");
}, []);
```

**Update:**

```jsx
useEffect(() => {
  document.title = `Hello ${name || "guest"}`;
}, [name]);
```

**Predict before running:** Type five letters — how many title updates?

**Explain result:** After each `name` change (and first mount). Dep array controls it.

**Anti-pattern (show, then fix):** `useEffect(() => setCount(count + 1))` with **no array** — infinite loop risk. Always discuss deps.

**Optional fetch preview (pattern only):**

```jsx
useEffect(() => {
  // fetch tomorrow in CRUD session
  console.log("would fetch on mount");
}, []);
```

Do **not** require JSONPlaceholder here if time is tight; the **hook shape** is the LO.

> **In the Real World:** **Analytics** "page viewed" fires on mount. Search-as-you-type effects depend on `query`.

---

## Recap (12 min)

Checklist: Vite runs, two components, props, one `useState`, two `useEffect`s (`[]` and `[dep]`).

**[Script:]** "Tomorrow: **Todo CRUD**, **localStorage**, **one GET** to JSONPlaceholder, **loading** state. Same two hooks. No FastAPI."

---

## Lecture Summary

- **Components** compose UI; **virtual DOM** is a beginner model for efficient updates
- **Vite** scaffolds React; **JSX** describes UI in functional components
- **Props** pass data downward
- **`useState`** holds interactive values
- **`useEffect`** runs on **mount** and **update** via the dependency array
- **Practical value:** This is the minimum React literacy every frontend intern is assumed to have
