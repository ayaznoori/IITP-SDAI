# Pre-Read: Fetch API — Vanilla JS & React

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **GET JSON** with `fetch` in plain JavaScript and parse it
- How to **show that data in the DOM** and handle a basic error
- How to **repeat the same GET in React** with `useEffect`
- How to show a **loading state** while the React request runs
- How the **vanilla and React versions** of the same call differ

---

## 2. Detailed Explanation

### The Browser Can Ask Other Computers

Your React screens still use fake strings in the file. Real **Swiggy** menus and **Twitter/X** feeds come from a server. The browser sends an HTTP **GET** and reads the response.

**`fetch`** is the built-in browser function for that request.

**One-line definition:** `fetch(url)` starts a network request and gives you a Promise; for JSON APIs you then call `.json()`.

**Analogy:** You text a friend “send the guest list.” They reply with a list. You copy names onto a whiteboard (the DOM or React state). `fetch` is the text. JSON is the list format.

> **In the Real World:** **JSONPlaceholder** is a free fake API used in tutorials worldwide. Production apps at **Netflix**, **Spotify**, and **Razorpay** use the same GET + JSON pattern against real servers.

### Why It Matters

**Real-world hook:** Without fetch, your UI is a brochure. With fetch, it is a client of a backend — the skill you will reuse with FastAPI later.

**Benefits:**
- **Live data** — titles and names not hardcoded
- **Same skill in two worlds** — script tag vs React
- **Honest UI** — loading and error, not a blank flash

### GET in Plain JavaScript

```javascript
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then((res) => {
    if (!res.ok) throw new Error("Bad response");
    return res.json();
  })
  .then((data) => {
    document.getElementById("title").textContent = data.title;
  })
  .catch(() => {
    document.getElementById("title").textContent = "Could not load";
  });
```

| Step | What it does |
|------|----------------|
| `fetch(url)` | GET by default |
| `res.ok` | HTTP success? |
| `res.json()` | Parse JSON body |
| `then` | Use the object |
| `catch` | Network or thrown error |

**Display in the DOM:** After parse, set `textContent` (or create elements). Same DOM skills as Module 2.

**Basic error:** Failed network or `!res.ok` → show a message. Keep it simple. No retry libraries.

### The Same GET in React with useEffect

If you call `fetch` directly in the component body, it runs **every render** — a loop of requests. **`useEffect`** runs after render, with a **dependency array**.

```jsx
useEffect(() => {
  fetch("https://jsonplaceholder.typicode.com/users/1")
    .then((res) => res.json())
    .then((data) => setUser(data));
}, []);
```

**Empty `[]`:** run once after the first paint (beginner mental model: “on mount”).

Store results with **`useState`**. Show `{user.name}` in JSX — not `document.getElementById`.

> **In the Real World:** **LinkedIn** profile headers and **GitHub** repo pages load user JSON, then render. React teams put that load in an effect (or a data library later — not today).

### Loading State

While the Promise is unresolved, show “Loading…”.

```jsx
const [loading, setLoading] = useState(true);
```

Set `loading` to `false` after JSON arrives (and in `catch`). Conditional render:

```jsx
{loading ? <p>Loading...</p> : <p>{user.name}</p>}
```

**Why It Matters:** **YouTube** and **Instagram** show skeletons or spinners so users wait instead of staring at emptiness.

### Vanilla vs React — Same Call, Different Home

| | Vanilla JS | React |
|--|------------|--------|
| When it runs | When the script runs | Inside `useEffect` so it is not every render |
| Where data lives | DOM nodes | `useState` |
| How UI updates | `textContent` / DOM APIs | Re-render from state |
| Loading | Manually toggle a `<p>` | `loading` state + ternary |
| Mental model | You paint the tree | You describe UI from data |

Both use **`fetch` + JSON**. React adds **when** (effect) and **where** (state).

### Messy to Clear

**Messy:** Fetch in the component body → infinite requests.

**Clear:** `useEffect(..., [])` + `setData` + `setLoading`.

### Building Blocks Checklist

- [ ] I can write a GET `fetch` and call `.json()`
- [ ] I can put a title or name into a DOM node
- [ ] I can show a basic error message
- [ ] I can fetch inside `useEffect` with `[]`
- [ ] I can show Loading… until data arrives
- [ ] I can say how vanilla and React versions differ

---

## 3. Practice Exercises

**Exercise 1 — Vanilla GET**  
Fetch `https://jsonplaceholder.typicode.com/posts/1`. `console.log` the parsed object.

**Exercise 2 — DOM display**  
Put `data.title` into `#output`. If fetch fails, set text to `"Error"`.

**Exercise 3 — React effect**  
In a component, `useEffect` + `useState` to store that post. Render `{post.title}`.

**Exercise 4 — Loading**  
Start `loading` true. Show `"Loading..."` until the request finishes.

**Exercise 5 — Compare**  
Write 4–6 sentences: same URL, different update path (DOM vs state/re-render), and why `useEffect` exists.
