# Lecture Script: React Router
**Duration:** 110 minutes | **Tools:** VS Code, Vite React + Tailwind, Browser | **Library:** react-router-dom (setup only)

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Fake page state vs real URLs |
| Why Does This Matter? | 12 min | Bookmarks, Back, product nav |
| What Is the Concept? | 20 min | Provider, Routes, Link, pages folder |
| How Do We Apply It? (LOs) | 52 min | Setup through readable structure |
| Nav polish lab | 10 min | Two–three pages, Tailwind links |
| Recap | 8 min | Build lab teaser |

---

## Session Opening (8 min)

**Problem:** Show a React app that switches views with `useState("home")`. Click About. URL stays `/`. Hit Back — leave the site.

**[Script:]** "That trick does not match **Netflix**, **Airbnb**, or **GitHub**. Those products put the screen in the **URL**. React Router is how a Vite app gets `/`, `/about`, and `/posts` without three HTML files. Today: install, two or three routes, **Link**, page components, keep it readable. No nested dashboards. No login walls."

**Real-world hook:** Type `github.com/facebook/react` vs `github.com/facebook/react/issues`. Same chrome, different page. "Your campus app will feel like that."

🎯 **Instructor Note:** Use the **current** `react-router-dom` docs for `createBrowserRouter` vs `<BrowserRouter>`. Pick **one** pattern for the whole room. Do not mix.

---

## Why Does This Matter?

🎯 **Instructor Note:** Hook — “Can you bookmark a useState page?” Pause. “No. You can bookmark `/about`.”

**[Script:]** "Recruiters open your deployed app and click Around. If the URL never changes, it feels like a demo. **Notion** pages, **Figma** files, **Linear** issues — all URLs. If you wrap everything in mystery nested routes this week, nobody can help you debug. Simple list. Readable."

**Real-world use:**

| Product | Paths you can name |
|---------|-------------------|
| **LinkedIn** | Feed vs profile |
| **Amazon** | Home vs cart |
| **Spotify** | Search vs library |
| **Gmail** | Inbox vs sent |
| **Zomato** | Home vs search |

**Pain if misunderstood:**
- `<a href>` causing full reload — lose React state, flash white
- Forgetting to wrap the tree — `Link` crashes
- Ten routes in one file with copy-paste junk — unreadable

---

## What Is the Concept?

**Definition:** A **route** pairs a **path** with a **page component**. **`Link`** changes the path inside the SPA.

**Mental model:** Browser URL → Router → one matching `element` → that component renders inside `Routes`.

**Comparison:** Multi-page HTML = many `.html` files. SPA + Router = one app, many components.

**Common mistakes:**
- `to="about"` missing `/` inconsistently — pick `/about` always
- Putting `BrowserRouter` in the wrong file so `Link` is outside

**Flow:** Problem → Explain → Walkthrough → Demo → Recap each LO.

---

## How Do We Apply It?

### LO 1: Set up React Router in a Vite React project

**Problem:** `Link` and `Routes` need a router context, like **Vercel** docs sites wrapping the app.

**Translate logic:** Install package. Wrap root. Restart dev server.

**Write code:**

```jsx
import { BrowserRouter } from "react-router-dom";
import App from "./App.jsx";

createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

If class uses `createBrowserRouter` + `RouterProvider`, demo **that** 10-line `main.jsx` instead. One pattern only.

**Predict before running:** What will happen? App still shows. No error in console.

**Explain result:** Setup is plumbing. Pages come next.

🎯 **Instructor Note:** Version mismatch is the #1 fail. Read the error. Do not invent APIs.

**Recap:** Wrap once at the root.

---

### LO 2: Define routes for two or three pages

**Problem:** **Airbnb**-style Home and About (and optional Posts).

**Translate logic:** List paths. Each `element` is a component.

**Write code:**

```jsx
import { Routes, Route } from "react-router-dom";

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
      <Route path="/posts" element={<Posts />} />
    </Routes>
  );
}
```

Keep `Home`/`About`/`Posts` as one-liners in the same file for the first demo, then split files.

**Predict before running:** What will happen? Manual URL `/about` shows About. Unknown path: blank unless you add a catch-all (skip catch-all unless asked).

**Explain result:** URL drives which page mounts.

**Recap:** Two or three routes is enough. **Gmail** has more; you do not today.

---

### LO 3: Navigate between pages using Link

**Problem:** **YouTube** nav should not reboot the app.

**Translate logic:** `Link` with `to`. Place nav **outside** `Routes` so it stays visible.

**Write code:**

```jsx
import { Link, Routes, Route } from "react-router-dom";

function App() {
  return (
    <div>
      <nav className="flex gap-4 p-4">
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<p>Home</p>} />
        <Route path="/about" element={<p>About</p>} />
      </Routes>
    </div>
  );
}
```

**Predict before running:** What will happen? Clicks change URL and inner text. Network tab: no full document reload.

**Explain result:** `Link` is in-app travel. Contrast one `<a href="/about">` reload.

**Recap:** Users click Links like **Amazon** header.

---

### LO 4: Map each route to a page component

**Problem:** Inline `<p>Home</p>` does not scale to a **LinkedIn** page.

**Translate logic:** File per page. Import. Pass as `element={<Home />}`.

**Write code:**

```jsx
// src/pages/Home.jsx
export default function Home() {
  return <h1 className="p-4 text-2xl font-bold">Home</h1>;
}
```

`App.jsx`: `import Home from "./pages/Home";` then `element={<Home />}`.

**Predict before running:** What will happen? Same as inline, cleaner files.

**Explain result:** Pages are normal components. They may use state, Tailwind, fetch later.

**Recap:** Route → component map is the product sitemap.

---

### LO 5: Keep the route structure simple and readable

**Problem:** Intern dumps 40 routes and helpers into `App.jsx` like a junk drawer.

**Translate logic:** Nav. `Routes`. Three `Route` lines. Pages folder. Stop.

**Write code:** Show a **readable** `App.jsx` (nav + three routes). If it exceeds 10 lines, still keep it the only routing file — no extra abstractions.

**Predict before running:** What will happen? A teammate finds `/posts` in five seconds.

**Explain result:** Readability is an LO. **Stripe** dashboards are complex; your file should not pretend to be one.

🎯 **Instructor Note:** Ban `useNavigate` unless a student asks; stick to `Link`. Ban nested routes.

**Recap:** Simple structure is the professional beginner bar.

---

## Nav Polish Lab (10 min)

Three pages, Tailwind nav (`flex gap-4`), `Link`s, pages folder. Optional: `Posts` shows static titles only (fetch optional, not required).

> **In the Real World:** First PR on a **Freshworks** or **Zoho** React app is often “add a page and a nav link.” That is this lab.

---

## Lecture Summary

- **React Router** is set up once around a Vite React app
- **Routes** declare two or three paths
- **`Link`** navigates without a full reload
- **Each route maps** to a page component
- **Readable structure** means a short route list and a `pages` folder
- **Practical value:** Multi-page product UX like GitHub and Airbnb starts with this map

**[Script:]** "Next is a hands-on lab: components, state, Tailwind, mentor debugging. Bring this router app or a smaller screen — you will leave with something that works."
