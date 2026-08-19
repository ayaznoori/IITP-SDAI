# Lecture Script: React Fundamentals — CRUD App
**Duration:** 110 minutes | **Tools:** Vite React app from prior session | **API:** GET `https://jsonplaceholder.typicode.com/todos` only | **Forbidden:** FastAPI, extra hooks, routing libraries

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 5 min | Take-home spec on the board |
| Why Does This Matter? | 8 min | Intern projects, Tasks apps |
| What Is the Concept? | 15 min | State as source of truth |
| How Do We Apply It? (LOs) | 70 min | Build the full Todo |
| Recap | 12 min | Module 2 close |

---

## Session Opening (5 min)

**Problem:** Theory counters are not a product. Recruiters want **CRUD + persist + one fetch**.

**[Script:]** "Today we ship a **Todo**. **Add, edit, delete, complete.** **localStorage**. **useState** and **useEffect** only. **One GET** to JSONPlaceholder. **Loading** text. **No FastAPI.** The server is fake and public."

> **In the Real World:** **Linear** and **Jira** are CRUD on issues. You are building the smallest honest version.

🎯 **Instructor Note:** Write acceptance criteria. Demo against it at the end. No extra features (auth, drag-drop, FastAPI).

---

## Why Does This Matter?

🎯 **Instructor Note:** "If refresh wipes todos, would you trust this app?" Persistence is the bar.

**[Script:]** "Module 3 will replace JSONPlaceholder with **your** API. The React side stays: state, effects, loading. Learn the shape now."

**Pain if misunderstood:**
- Fetch always overwrites localStorage
- Missing `key` on lists
- `useEffect` save running before load → wiping data

---

## What Is the Concept?

### Source of truth

`todos` array in **useState**. UI is `todos.map`. Mutations copy arrays (`map`, `filter`, spread). Never mutate nested objects in place without copying.

### Persistence

Serialize JSON. Load once. Save on change.

### Seed fetch

If no local data, GET samples. `?_limit=8` to stay small. `setLoading(false)` in `finally`.

**Python vs JS:** Python `json.dump` to a file. Browser **localStorage** is the file.

---

## How Do We Apply It?

### LO 1: Todo with add, edit, delete, mark-complete

**Problem:** Empty `[]`.

**Translate logic:**
- Add: `setTodos([...todos, { id: Date.now(), title, completed: false }])`
- Delete: `filter`
- Toggle: `map` flip `completed`
- Edit: `map` replace `title` (inline input or `prompt` for speed)

**Write code (toggle):**

```jsx
function toggle(id) {
  setTodos(todos.map((t) =>
    t.id === id ? { ...t, completed: !t.completed } : t
  ));
}
```

**Predict before running:** Why spread `{ ...t }`?

**Explain result:** Copy the object. Do not mutate the old one.

**UI sketch:** input, Add, list with checkbox, Edit, Delete.

> **In the Real World:** **Microsoft To Do** checkbox is this toggle.

🎯 **Instructor Note:** 25 min timed build of this LO before persistence.

---

### LO 2: Persist in localStorage; load on reload

**Write code:**

```jsx
useEffect(() => {
  const raw = localStorage.getItem("se-todos");
  if (raw) setTodos(JSON.parse(raw));
}, []);
```

```jsx
useEffect(() => {
  if (!hydrated) return;
  localStorage.setItem("se-todos", JSON.stringify(todos));
}, [todos, hydrated]);
```

**Simpler classroom pattern:** `useState` lazy init:

```jsx
const [todos, setTodos] = useState(() => {
  const raw = localStorage.getItem("se-todos");
  return raw ? JSON.parse(raw) : [];
});
```

Then a save effect on `[todos]`. Combine carefully with fetch (next LOs): **if lazy load has items, skip API seed**.

**Predict before running:** Refresh — still there?

**Explain result:** Yes, if stringify/parse succeeded.

**Demo:** Application tab in DevTools → Local Storage.

---

### LO 3: useState + useEffect to manage todos and side effects

**Walkthrough:** Inventory all state: `todos`, `text` (input), `loading`, maybe `editingId`.

**Effects allowed:**
- persist
- fetch seed
- optional `document.title` count

**Predict:** Two `useEffect`s — is that OK?

**Explain result:** **Yes.** One purpose each. Still only **those two hook types**.

No `useReducer`. If students ask, "not today."

---

### LO 4: One GET to jsonplaceholder.typicode.com/todos

**Write code:**

```jsx
useEffect(() => {
  let cancelled = false;
  async function seed() {
    const existing = localStorage.getItem("se-todos");
    if (existing && JSON.parse(existing).length) {
      setLoading(false);
      return;
    }
    try {
      const res = await fetch(
        "https://jsonplaceholder.typicode.com/todos?_limit=5"
      );
      if (!res.ok) throw new Error("fail");
      const data = await res.json();
      if (!cancelled) {
        setTodos(
          data.map((t) => ({
            id: t.id,
            title: t.title,
            completed: t.completed,
          }))
        );
      }
    } catch (e) {
      if (!cancelled) setTodos([]);
    } finally {
      if (!cancelled) setLoading(false);
    }
  }
  seed();
  return () => {
    cancelled = true;
  };
}, []);
```

Keep cleanup optional if it overruns; **must** have one GET and `setLoading`.

**Predict before running:** Network tab — how many GETs on refresh if localStorage full?

**Explain result:** **Zero API calls** if you skip seed. If they always fetch, they will **wipe** edits — catch that bug live.

> **In the Real World:** **Offline-first** notes apps prefer local, then sync. You prefer local if present.

**No FastAPI** — do not proxy through Python.

---

### LO 5: Show loading while API fetch is in progress

**Write code:**

```jsx
if (loading) return <p>Loading todos…</p>;
```

Or overlay the list section only.

**Predict before running:** Slow 3G in DevTools — do we see loading?

**Explain result:** Yes, if `loading` starts `true` and flips in `finally`.

**Empty state:** After load, `todos.length === 0` → "No todos yet. Add one."

---

## Recap (12 min)

Acceptance test as a class:
- [ ] Add
- [ ] Edit title
- [ ] Delete
- [ ] Toggle complete
- [ ] Refresh keeps data
- [ ] First visit (cleared storage) fetches `/todos` and shows loading
- [ ] No FastAPI

**[Script:]** "Module 2 is complete: from **HTML** to a **React CRUD** that talks to a **public REST** API. Module 3 you will **own** the server."

---

## Lecture Summary

- A **React Todo** supports **add, edit, delete, complete**
- **localStorage** persists JSON across refresh
- **`useState` + `useEffect`** are enough for this app
- **One GET** to **JSONPlaceholder `/todos`** seeds empty storage
- **Loading** UI covers the wait
- **Practical value:** This matches a standard intern take-home and prepares you to swap in FastAPI later without new React hooks
