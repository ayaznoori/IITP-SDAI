# Lecture Script: Masterclass — Evolution of the Web
**Duration:** 110 minutes | **Tools:** Browser, DevTools, whiteboard | **Tone:** Professor masterclass — stories, mental models, few short demos | **Audience:** Beginners who already wrote HTML, CSS, and JS syntax

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Newspaper vs product |
| Why Does This Matter? | 14 min | Career and product history |
| What Is the Concept? | 28 min | Eras + browser pipeline + JS runtime |
| How Do We Apply It? (LOs) | 48 min | Five LOs, stories + short demos |
| Recap | 12 min | Map to Async, DOM, APIs, React |

---

## Session Opening (8 min)

**Problem:** Students can write a pretty static portfolio. They cannot yet explain *why* industry moved past "save HTML and upload."

**[Script:]** "You already built pages. Today is not a new tag. Today is the **story of the platform** you are joining. Why did the web refuse to stay a stack of documents? Why does a **JavaScript runtime** sit inside Chrome? If you cannot answer that, React will feel like fashion. If you can, React will feel like a tool."

> **In the Real World:** **Yahoo** directories were Web 1.0 maps of the internet. **Google Search** made the web a query engine. **Facebook** made it a social operating system. **Netflix** made it a TV in the tab.

🎯 **Instructor Note:** Show three screenshots: a 1990s university homepage, Gmail, and a React-looking dashboard. Do not name extra frameworks. Ask: "Which is a document? Which is a product?"

---

## Why Does This Matter?

🎯 **Instructor Note:** Hook question — "If HTML can show a form, why do we need JavaScript at all?" Collect answers. Steer: HTML displays; JS **changes** the page after load.

**[Script:]** "Full-stack engineers at **Razorpay**, **Zoho**, and **Swiggy** live in this history. Backend later stores truth. Frontend is where the **user** believes the product exists. Static-only sites cannot hold a session, a cart, or a live score. Misunderstand this and you ship brochures when the market wants apps."

**Pain if misunderstood:**
- Treating every page as a one-time download
- Blaming CSS when the real issue is "no runtime for behavior"
- Jumping to React without knowing the browser already runs JS

---

## What Is the Concept?

### Web 1.0 — static documents

**Definition:** **Web 1.0** means mostly read-only pages. The server sends HTML. The user reads. Little two-way product behavior.

**Mental model:** A PDF you open in a browser.

**Comparison (Python vs JS):** Python scripts you ran locally **compute**. Static HTML **displays**. JS in the browser **computes in the user's machine after paint**.

### Demerit of static-only

Same file for every visitor. Content updates are file edits. Interaction is a full reload or a dead form.

### Web 2.0 — interactive platforms

**Definition:** **Web 2.0** means users contribute, pages respond, and products remember context (login, cart, feed).

**Mental model:** A restaurant that takes your order, not a menu poster on the wall.

### Browser load and render

**Definition:** The browser **loads** resources (HTML, CSS, images, scripts) and **renders** them into pixels.

**Pipeline (board):** URL → request → HTML bytes → parse DOM → CSS → layout → paint → JS may mutate DOM → paint again.

### JavaScript runtime

**Definition:** The **JS runtime** is the engine (how JS executes) plus **browser APIs** that let JS touch the page and the clock.

**Common mistakes:**
- "The server paints the page" — the **browser** paints
- "HTML is the app" — HTML is the first snapshot
- "JS is optional decoration" — for Web 2.0 products, JS **is** the behavior layer

---

## How Do We Apply It?

### LO 1: Explain why the web needed to evolve beyond static Web 1.0

**Problem:** A bookstore puts every title in a separate HTML file. Inventory changes daily.

**Translate logic:** Static files cannot be a live catalog. The web needed pages that **update** and later **pull data**.

**Walkthrough:** Timeline on board — CERN/HTML docs → commercial sites → search → social → SaaS in the browser.

**Demo (concept, no new APIs):** Open student portfolio vs open **Wikipedia** edit flow conceptually. "One is a snapshot. The other is a living product."

**Predict before running:** Will a static `prices.html` stay correct if the warehouse changes stock tonight?

**Explain result:** No. Evolution was driven by **fresh, personal, interactive** needs.

> **In the Real World:** **IRCTC** seat availability cannot be a printed HTML table from last week.

---

### LO 2: Explain demerits of static-only websites

**Problem:** A NGO wants donations, volunteer signup, and a "thank you" that uses the donor's name.

**Translate logic:** Static site can show a PDF of the mission. It cannot personalize or store the donor.

**Write code (contrast):**

```html
<!-- Static: same greeting for everyone -->
<h1>Welcome, visitor</h1>
```

```html
<!-- After JS runtime (preview of later sessions) -->
<h1 id="greet">Welcome, visitor</h1>
<script>
  document.getElementById("greet").textContent = "Welcome, Priya";
</script>
```

**Predict before running:** Does the first snippet ever say "Priya"?

**Explain result:** Static-only HTML cannot personalize without a new file per user. That is the demerit.

**Case study:** **Craigslist**-style listings vs **Airbnb** search with maps and dates. Static HTML cannot filter 10,000 stays.

---

### LO 3: Explain how Web 2.0 changed websites into interactive platforms

**Problem:** Students think "interactive" means CSS hover.

**Translate logic:** Hover is presentation. **Platform** means users **write** data and see **updated UI**.

**Walkthrough:** Map actions:

| Action | Web 1.0 | Web 2.0 |
|--------|---------|---------|
| Read article | Yes | Yes |
| Post a review | Email the webmaster | Form + live list |
| See your cart | Phone the shop | Page updates |
| Collaborate | Mail Word files | **Google Docs** in tab |

**Demo:** Click a button that toggles text. "Tiny Web 2.0. The platform idea is this loop at product scale."

```html
<button id="like">Like</button>
<p id="count">0</p>
<script>
  let n = 0;
  document.getElementById("like").onclick = () => {
    n += 1;
    document.getElementById("count").textContent = n;
  };
</script>
```

**Predict before running:** After three clicks, what does `#count` show?

**Explain result:** `3` — the page **remembered** in memory. That is interactive platform behavior on one machine. Later, servers remember for everyone.

> **In the Real World:** **YouTube** comments, **Reddit** threads, **LinkedIn** posts — user-generated platforms, not brochures.

---

### LO 4: Explain how a browser loads and renders a page

**Problem:** "I opened the file, so the page exists." Students skip the pipeline.

**Translate logic:** Even `file://` still **parses HTML**, applies CSS, paints, then runs scripts.

**Walkthrough (board, numbered):**

1. Enter URL or open file
2. Receive HTML
3. Parse tags into a tree
4. Fetch CSS and images
5. Layout boxes
6. Paint
7. Execute JS if present

**Demo:** DevTools **Network** tab on their portfolio. Reload. Point at HTML first, then CSS, then maybe JS.

🎯 **Instructor Note:** Pause. "If CSS arrives late, you may see unstyled content. That is render order, not a broken file."

**Predict before running:** If you comment out the CSS link, does HTML still appear?

**Explain result:** Yes. HTML can render without CSS. CSS styles. JS is optional for a document, required for a dynamic app.

> **In the Real World:** **BBC News** still works if JS is slow for reading the article. **Figma in the browser** is unusable without the JS runtime.

---

### LO 5: Explain the role of the JS runtime inside the browser

**Problem:** Students think JS is "the language we typed in the console." They miss **where** it runs.

**Translate logic:** Python ran on their laptop via an interpreter. **Browser JS** runs in a **runtime** shipped with Chrome/Firefox/Safari. It can use **DOM** and timers.

**Write code:**

```javascript
console.log("runtime is awake");
setTimeout(() => console.log("later"), 500);
console.log("still on the first turn");
```

**Predict before running:** Order of the three logs?

**Explain result:** `runtime is awake` → `still on the first turn` → `later`. The runtime **schedules** work. That is why Web 2.0 UIs feel live. Next session: Async JavaScript.

**Common mistake:** Believing the **server** runs `onclick`. It does not. The **browser runtime** does.

> **In the Real World:** **Spotify Web Player** is a JS-heavy app in a tab. Pause, seek, and playlist updates are runtime work, not new HTML files per click.

---

## Recap (12 min)

Map forward (do not teach those sessions):

- Async JS — runtime scheduling
- DOM — the tree the runtime edits
- HTTP/APIs — how platforms share data
- React — a library to structure UI updates

**[Script:]** "You are not learning random tools. You are learning the layers the evolved web actually uses."

---

## Lecture Summary

- **Web 1.0** was static, read-mostly documents; products outgrew that model
- **Static-only sites** cannot personalize, scale catalogs, or feel like apps
- **Web 2.0** made websites **interactive platforms** where users create and UI updates
- The **browser** loads HTML/CSS, **renders** pixels, then may run scripts
- The **JavaScript runtime** executes behavior in the tab and mutates the page
- **Practical value:** Every later frontend session is a consequence of this evolution — you now know *why* those sessions exist
