# Lecture Script: Fetch API — Vanilla JS & React
**Duration:** 110 minutes | **Tools:** VS Code, Browser, Vite React, Network tab | **APIs:** `fetch`, JSONPlaceholder

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Hardcoded UI vs live feeds |
| Why Does This Matter? | 12 min | Every app talks to a server |
| What Is the Concept? | 22 min | GET, JSON, DOM, useEffect, loading |
| How Do We Apply It? (LOs) | 50 min | Vanilla then React, then compare |
| Dual-tab lab | 10 min | Same URL, two implementations |
| Recap | 8 min | Router teaser |

---

## Session Opening (8 min)

**Problem:** Tailwind card with `"Priya"` hardcoded. Then open **JSONPlaceholder** `/users/1` in the browser.

**[Script:]** "Zomato does not bake every restaurant into the app binary. The app **GET**s JSON. **Twitter/X**, **Gmail**, **Spotify**, **IRCTC** UIs — same idea. Today we use the browser **Fetch API** twice: once like your DOM labs, once inside React with **`useEffect`**. Same GET. Different home for the result."

**Real-world hook:** DevTools → Network on **news.ycombinator.com** or a public site. Point at XHR/fetch. "That is your future FastAPI calls."

🎯 **Instructor Note:** Confirm internet. JSONPlaceholder can be slow; have the URL on the board. CORS is fine on this host.

---

## Why Does This Matter?

🎯 **Instructor Note:** Hook — “If the API is down, should the page look empty or say error?” Vote. Then teach both paths.

**[Script:]** "Juniors fail interviews when they `fetch` in the component body and freeze the tab with a request storm. Or they ignore errors and ship a blank **Amazon**-style grid. Loading and a basic error are professional habits, even on a fake API."

**Real-world use:**

| Product | Typical GET |
|---------|-------------|
| **Instagram** | Feed items JSON |
| **LinkedIn** | Profile / connections |
| **YouTube** | Video metadata |
| **GitHub** | Repo and user JSON |
| **Swiggy** | Restaurant list |

**Pain if misunderstood:**
- Fetch in render body — extra requests every state change
- No `res.ok` check — treating an error HTML page as JSON
- No loading — users think the app is broken

---

## What Is the Concept?

**Definition:** **GET with `fetch`** retrieves a resource. **JSON** is text that `res.json()` turns into objects/arrays.

**Mental model:** Request in flight → wait → parse → paint. React stores parse result in state so paint is a re-render.

**Comparison:** Vanilla paints with DOM APIs. React paints with JSX from state. Python `requests.get` is the same conversation on the server later — not today.

**Common mistakes:**
- Forgetting `return res.json()`
- Using `useEffect` without importing it
- Missing `[]` and re-fetching every render

**Flow:** Problem → Explain → Walkthrough → Demo → Recap each LO.

---

## How Do We Apply It?

### LO 1: Make a GET request with fetch in plain JavaScript and parse JSON

**Problem:** Read one post from JSONPlaceholder like a **Medium** article stub.

**Translate logic:** `fetch` URL → check ok → `json()` → log `title`.

**Write code:**

```javascript
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then((res) => {
    if (!res.ok) throw new Error("fail");
    return res.json();
  })
  .then((data) => console.log(data.title));
```

**Predict before running:** What will happen? Console shows a long title string after a short wait.

**Explain result:** Default method is GET. Body is JSON. `data` is a plain object.

🎯 **Instructor Note:** Network tab: status 200, type fetch. Compare to opening the URL in a tab.

**Recap:** `fetch` + `json()` is the core.

---

### LO 2: Display fetched data in the DOM and handle a basic error

**Problem:** Show the title on the page, like a **BBC** headline slot. If it fails, say so.

**Translate logic:** Success → `textContent`. `catch` → error string. Break the URL once to demo error.

**Write code:**

```javascript
const el = document.getElementById("out");
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then((res) => {
    if (!res.ok) throw new Error("fail");
    return res.json();
  })
  .then((data) => { el.textContent = data.title; })
  .catch(() => { el.textContent = "Could not load"; });
```

HTML: `<p id="out">Loading</p>` plus script at end of body.

**Predict before running:** What will happen? Title appears. With a bad URL: “Could not load”.

**Explain result:** Vanilla still uses the DOM. Error path is required for **IRCTC**-style honesty.

**Recap:** Parse, paint, catch.

---

### LO 3: Repeat the same GET inside React using useEffect

**Problem:** Same post, **React** card — no `getElementById`.

**Translate logic:** State for post. Effect with `[]` runs fetch once. `setPost` causes re-render.

**Write code:**

```jsx
function PostTitle() {
  const [post, setPost] = useState(null);
  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/posts/1")
      .then((res) => res.json())
      .then(setPost);
  }, []);
  return <p>{post ? post.title : ""}</p>;
}
```

**Predict before running:** What will happen? First render empty-ish, then title after fetch.

**Explain result:** Same GET as vanilla. Result lives in state. **GitHub** profile pages follow this shape.

🎯 **Instructor Note:** If they fetch in the function body, show the Network tab exploding — then move into `useEffect`.

**Recap:** Effect = when. State = where.

---

### LO 4: Show a loading state while the React fetch runs

**Problem:** **YouTube** does not flash a blank player; it waits visibly.

**Translate logic:** `loading` starts true. After then/catch, false. Ternary in JSX.

**Write code:**

```jsx
function UserName() {
  const [name, setName] = useState("");
  const [loading, setLoading] = useState(true);
  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users/1")
      .then((res) => res.json())
      .then((u) => { setName(u.name); setLoading(false); });
  }, []);
  return <p>{loading ? "Loading..." : name}</p>;
}
```

**Predict before running:** What will happen? “Loading...” then a name (Leanne Graham on JSONPlaceholder).

**Explain result:** Loading is just boolean state. Same conditional rendering as the state session.

**Recap:** Users need a wait signal. **Instagram** and **Gmail** do this.

---

### LO 5: Explain how the vanilla and React versions of the same call differ

**Problem:** Students think React invented HTTP.

**Translate logic:** Same URL, same JSON. Different **orchestration**.

**Write code:** Side-by-side on the board (not a 20-line file). Vanilla: `textContent`. React: `setState` in `useEffect`.

**Predict before running:** What will happen? Both can show the same title. React will not keep working if you mix in `document.querySelector` and then re-render.

**Explain result:**

| Question | Vanilla | React |
|----------|---------|--------|
| Who updates UI? | You, via DOM | React, via state |
| When does fetch run? | Script execution | After render, effect `[]` |
| Loading | Manual node | `loading` state |

**[Script:]** "When we build FastAPI, the React side stays this pattern. The URL changes from JSONPlaceholder to your API."

**Recap:** One network skill, two UI homes.

---

## Dual-Tab Lab (10 min)

Left: `index.html` + vanilla fetch. Right: React `UserName`. Same endpoint. Mentors check `useEffect` deps and error text.

> **In the Real World:** **Slack** loads workspaces with GET; the React app still handles loading and errors the same way.

---

## Lecture Summary

- **`fetch` GET** plus **`.json()`** loads remote data in the browser
- **Vanilla** writes results into the **DOM** and should **catch** failures
- **React** repeats the GET inside **`useEffect`**
- **Loading state** keeps the UI honest while the Promise runs
- **Vanilla vs React** share fetch; they differ in when it runs and how UI updates
- **Practical value:** This is how product UIs from Swiggy to GitHub fill themselves

**[Script:]** "Next session: multiple pages with React Router — Home, Posts, About — still the same fetch skill."
