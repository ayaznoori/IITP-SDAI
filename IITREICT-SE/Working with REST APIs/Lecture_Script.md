# Lecture Script: Working with REST APIs
**Duration:** 110 minutes | **Tools:** VS Code, local static server, Chrome Network | **API:** https://jsonplaceholder.typicode.com

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 6 min | Empty ul vs live todos |
| Why Does This Matter? | 10 min | Every product dashboard |
| What Is the Concept? | 20 min | fetch, ok, json, render |
| How Do We Apply It? (LOs) | 62 min | Build the todos page |
| Recap | 12 min | React will wrap this |

---

## Session Opening (6 min)

**Problem:** Advanced DOM lists were typed by the user. Products load **shared** data from a server.

**[Script:]** "Yesterday the **contract**. Today the **call**. **`fetch` + async/await**. Parse **JSON**. Handle failure. Paint the **DOM**. **JSONPlaceholder `/todos`**. One complete **page**."

> **In the Real World:** **Product Hunt** feeds, **Hacker News** lists, **GitHub Issues** — GET then render. You are doing the junior version.

🎯 **Instructor Note:** Network tab → Fetch/XHR. After demo, students must identify their own request row.

---

## Why Does This Matter?

🎯 **Instructor Note:** Unplug wifi for one demo (or DevTools Offline). Show catch path. "This is production."

**[Script:]** "Interview live-coding often says: fetch this URL, show titles. If you forget **`res.ok`**, you will parse an error HTML as JSON and crash."

**Pain if misunderstood:**
- `fetch` without `await`
- Assuming 404 throws
- Rendering 200 items with no limit

---

## What Is the Concept?

### fetch GET

`fetch(url)` → Promise of **Response**. Not the JSON yet.

### json() + errors

`res.json()` is another Promise. Check **`res.ok`**. **try/catch** for network + thrown HTTP errors.

### Render

Same createElement skills. Data-driven.

### JSONPlaceholder

Public fake REST. GET works. POST may fake-succeed without persist — **today is GET only**.

**Python vs JS:** `requests.get(url).json()` vs `await (await fetch(url)).json()` — two awaits.

---

## How Do We Apply It?

### LO 1: GET with Fetch + async/await

**Problem:** Load five todos.

**Translate logic:** async function. await fetch. return json.

**Write code:**

```javascript
async function getTodos() {
  const res = await fetch(
    "https://jsonplaceholder.typicode.com/todos?_limit=5"
  );
  return res;
}
```

**Predict before running:** Is `getTodos()` already the array?

**Explain result:** No. It is a Promise of a **Response**. Next LO parses.

**Demo:** `console.log(await getTodos())` — show status, ok, url.

---

### LO 2: Parse JSON and handle fetch errors

**Write code:**

```javascript
async function getTodos() {
  try {
    const res = await fetch(
      "https://jsonplaceholder.typicode.com/todos?_limit=5"
    );
    if (!res.ok) {
      throw new Error("Request failed: " + res.status);
    }
    return await res.json();
  } catch (err) {
    throw err;
  }
}
```

**Predict before running:** Fetch `/todos/99999` — does `json()` throw?

**Explain result:** Usually **no**. You get `{}` and **ok false** (404). **You** must throw.

**Demo:** Typo host → catch. Typo path → ok false.

> **In the Real World:** **Sentry** fills with uncaught fetch errors. Handle them.

---

### LO 3: Render API data in the DOM

**Problem:** `ul#todos` empty.

**Write code:**

```javascript
function render(todos) {
  const ul = document.getElementById("todos");
  ul.innerHTML = "";
  todos.forEach((t) => {
    const li = document.createElement("li");
    li.textContent = t.title;
    if (t.completed) li.classList.add("done");
    ul.append(li);
  });
}
```

**Predict before running:** Why `textContent` not `innerHTML` for `title`?

**Explain result:** Titles are data. **textContent** avoids HTML injection habits.

Keep `innerHTML = ""` only to clear **your** list container.

---

### LO 4: Read JSONPlaceholder docs and call todos

**Docs walk (live):**
- Base URL
- `/todos`
- `/todos/1`
- `?userId=`
- `_limit`

**Task:** Pairs write three URLs, fetch one in console.

**Predict:** Does GET `/todos` need a body?

**Explain result:** No. GET has no required body.

> **In the Real World:** Same ritual on **PokeAPI** or **Open Notify** — docs first, URL second, code third.

---

### LO 5: Implement a page that fetches and displays jsonplaceholder data

**Live lab (25+ min):**

```html
<h1>Todos</h1>
<p id="status">Loading…</p>
<ul id="todos"></ul>
```

```javascript
async function init() {
  const status = document.getElementById("status");
  try {
    const res = await fetch(
      "https://jsonplaceholder.typicode.com/todos?_limit=10"
    );
    if (!res.ok) throw new Error("HTTP " + res.status);
    const data = await res.json();
    render(data);
    status.textContent = data.length + " items";
  } catch (err) {
    status.textContent = "Could not load todos";
  }
}
init();
```

**Predict before running:** What does the user see before await finishes?

**Explain result:** **Loading…** — that is why status exists.

🎯 **Instructor Note:** Stretch: show `completed` as checkbox disabled (read-only). Still GET only.

**Serve files** via VS Code Live Server / `python -m http.server` so origin is http, not file.

---

## Recap (12 min)

Show 2–3 student pages. Network 200. One forced error.

**[Script:]** "You wired **HTTP + async + DOM**. Next masterclass: why teams do not keep doing this by hand forever — **libraries and frameworks**. Then **React**."

---

## Lecture Summary

- **GET** with **`fetch`** and **`async`/`await`**
- **Parse JSON**; treat **`res.ok`** and **try/catch** as required
- **Render** titles (and state) into the DOM
- **JSONPlaceholder `/todos`** is the documented source
- A **full page** loads, explains failure, and lists data
- **Practical value:** This is the core frontend data path used in every SPA you will see
