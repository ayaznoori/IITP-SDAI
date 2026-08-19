# Lecture Script: How the Web Works
**Duration:** 110 minutes | **Tools:** Browser DevTools Network tab, whiteboard | **Demos:** wikipedia.org or student portfolio hosted, jsonplaceholder later preview only if needed for CORS talk

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 7 min | Restaurant ticket model |
| Why Does This Matter? | 12 min | Network tab as a superpower |
| What Is the Concept? | 26 min | HTTP anatomy |
| How Do We Apply It? (LOs) | 52 min | Trace a real navigation |
| Recap | 13 min | Bridge to API contracts |

---

## Session Opening (7 min)

**Problem:** Students think "the website is in the URL bar." They cannot explain a blank tab or a 404.

**[Script:]** "The browser is a **client**. Somewhere a **server** listens. They speak **HTTP**. Today you learn the **contract of the wire**: methods, codes, headers, cookies, sessions, **CORS**, and the full walk from **Enter** to **response**."

> **In the Real World:** **SRE and frontend** both open the **Network** tab first. **Cloudflare** status pages, **AWS** consoles, **IRCTC** — same verbs.

🎯 **Instructor Note:** Open Network, reload a news site. Filter Doc. Point at status 200. Students copy the habit.

---

## Why Does This Matter?

🎯 **Instructor Note:** Hook — "Your JS is perfect but the Network row is red. Whose bug is it?" Teach: could be server, CORS, or URL.

**[Script:]** "Module 3 **FastAPI** will **be** the server. Today you learn what that server must already speak. Misread **401** as **404** and you debug the wrong layer for hours."

**Pain if misunderstood:**
- Using GET for delete because "it worked in the address bar"
- Storing passwords in query strings
- Fighting CORS by "turning it off" without understanding origin

---

## What Is the Concept?

### Client-server

**Client** initiates. **Server** waits and answers. **Request-response** is one round trip (HTTP/1.1 mental model).

### Methods

Safe/read vs change. GET should not delete data.

### Status codes

2xx success, 4xx your request, 5xx server. Do not memorize all; memorize the families + 200, 201, 400, 401, 403, 404, 500.

### Headers, cookies, sessions

Headers = envelopes. Cookies = browser-held notes. Session = server map from cookie id to user.

### CORS

**Origin** = scheme + host + port. Browser **allows** JS to **read** the response only if the server says so. `curl` has no CORS. Chrome does.

### URL trace

DNS → TCP/TLS → HTTP request → server app → response.

**Python vs JS:** `requests.get` in Python is a client too. The **browser** adds CORS and cookies automatically.

---

## How Do We Apply It?

### LO 1: Client-server and HTTP request-response

**Problem:** Draw the loop for opening `example.com`.

**Translate logic:** Browser client → GET request → server → 200 + HTML.

**Walkthrough:** Pair whiteboard. Boxes: User, Browser, Server, Response body.

**Predict before running:** If the server is down, does the browser still send a request?

**Explain result:** It tries. Then you see connection error — **no HTTP status** from the app.

> **In the Real World:** **JioHotstar** app on phone is also a client. Same idea.

---

### LO 2: Identify common HTTP methods and status codes

**Problem:** Match product actions.

| Product action | Method | Happy code |
|----------------|--------|------------|
| Open homepage | GET | 200 |
| Create account | POST | 201 or 200 |
| Replace profile | PUT | 200 |
| Remove item | DELETE | 200 or 204 |

**Demo:** Network tab — navigate vs submit a form (if they have one). Identify method.

**Predict before running:** GET `/users/999` if user missing?

**Explain result:** Often **404**. **500** would mean the server crashed while looking.

**Case study:** **GitHub** REST uses these verbs on repos. Students will read this in API docs next session.

---

### LO 3: Headers, cookies, sessions

**Problem:** Why does "open in incognito" log you out of **LinkedIn**?

**Translate logic:** Cookies not copied. Session id gone. Server does not know you.

**Walkthrough:** Request headers `Cookie`. Response `Set-Cookie`. `Content-Type: text/html` vs `application/json`.

**Predict before running:** If we delete cookies for a site, does the HTML file on disk change?

**Explain result:** No. Only **browser storage** for that origin.

> **In the Real World:** **Banking sites** use short sessions. Steal a session cookie and you steal the session — hence HTTPS and later auth tokens.

Keep ethics: **explain**, do not teach theft.

---

### LO 4: CORS and why browsers need it for API calls

**Problem:** `file://` page fetch to `https://jsonplaceholder.typicode.com` may warn or behave oddly; `localhost:5500` vs `localhost:8000` is the classic classroom error.

**Translate logic:** JS from Origin A reading Origin B needs **CORS headers** from B.

**Demo:** Show a blocked request in Network (red). Read the console CORS message. Contrast opening the API URL directly in a new tab (works — **navigation** is not a JS read).

**Predict before running:** Does CORS block the **server** from receiving the request?

**Explain result:** Often the server **does** receive it. The **browser hides** the body from JS. That confuses everyone.

> **In the Real World:** **Stripe** and **Google Maps** document allowed origins. FastAPI later adds CORS middleware — you will know **why**.

---

### LO 5: Trace URL entry until server response

**Problem:** Narrate `https://www.wikipedia.org/wiki/HTTP` as a slow-motion film.

**Walkthrough (board, timed 8 min):**
1. Parse URL (scheme, host, path)
2. DNS lookup
3. TLS handshake (HTTPS)
4. HTTP **GET** with path and headers (`Host`, `User-Agent`, `Accept`)
5. Server routes `/wiki/HTTP`
6. **200** + HTML bytes
7. Browser starts render (Evolution masterclass)

**Predict before running:** Is CSS in that first response always?

**Explain result:** Sometimes inlined, usually **more GETs**. First response is often HTML.

🎯 **Instructor Note:** Students write the seven steps in their own words. Swap papers. Peer-correct.

---

## Recap (13 min)

Quiz: shout methods and codes. One volunteer traces a URL. One explains CORS in one analogy (stamp / bouncer).

**[Script:]** "Next session APIs as **contracts** — same HTTP, but JSON and resources. You now speak the dialect of the Network tab."

---

## Lecture Summary

- **Clients** send **HTTP requests**; **servers** return **responses**
- **GET/POST/PUT/DELETE** and **status families** describe intent and outcome
- **Headers, cookies, and sessions** carry metadata and login memory
- **CORS** is a **browser** rule so a page cannot silently read another origin
- **URL → DNS → request → response** is the path every click takes
- **Practical value:** You can debug frontend vs backend vs browser policy instead of guessing
