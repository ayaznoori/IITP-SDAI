# Lecture Script: APIs as Contracts
**Duration:** 110 minutes | **Tools:** Browser, JSONPlaceholder docs, Network tab (optional GET in address bar) | **Note:** No Fetch implementation required; next session codes it

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 6 min | Menu as contract |
| Why Does This Matter? | 12 min | Team handoff, Stripe docs |
| What Is the Concept? | 24 min | REST + CRUD + errors + query |
| How Do We Apply It? (LOs) | 56 min | Docs lab on JSONPlaceholder |
| Recap | 12 min | Ready to Fetch |

---

## Session Opening (6 min)

**Problem:** Students hard-code fake arrays forever. They do not know how two apps agree.

**[Script:]** "An **API** is not a vibe. It is a **contract**: paths, verbs, JSON shape, errors. **REST** is the style you will see most. Today we **read** contracts. Tomorrow we **call** them."

> **In the Real World:** **OpenAPI/Swagger** at companies is this contract generated. **JSONPlaceholder** is the classroom stand-in. **Stripe** docs are the gold standard of clarity.

🎯 **Instructor Note:** Show two columns: Frontend expectation vs Backend promise. If they disagree, production breaks.

---

## Why Does This Matter?

🎯 **Instructor Note:** "Have you ever used a vending machine that listed no prices?" That is an API without docs.

**[Script:]** "In internships, you will be told 'hit the users endpoint.' If you cannot **find it in docs**, you stall. Contracts also include **failure**. **Paytm** does not only document 200."

**Pain if misunderstood:**
- Guessing fields (`name` vs `full_name`)
- Ignoring pagination — UI shows 5000 rows
- Treating every error as "network down"

---

## What Is the Concept?

### REST

**Resources** (nouns). **Endpoints** (URLs). **Verbs** (GET POST PUT DELETE). Stateless requests: each call carries what it needs.

### CRUD mapping

Create POST, Read GET, Update PUT, Delete DELETE. (PATCH as "partial" — mention, do not require.)

### Error contract

Status + body. Body might be `{ "detail": "..." }` in FastAPI later. Today: generic JSON error.

### Query params

Filter, sort, page. They do not change the **path resource**; they **qualify** the list.

### Docs literacy

Base URL, authentication section (JSONPlaceholder: none), examples, rate limits.

**Common mistakes:** Mixing `/todo/1` vs `/todos/1`. Putting JSON in GET query. Assuming list endpoints return one object.

---

## How Do We Apply It?

### LO 1: REST resources, endpoints, HTTP verbs

**Problem:** Design a "courses" API on the board. Do not implement.

**Translate logic:** Resource `courses`. Collection `/courses`. Item `/courses/12`.

**Walkthrough:** Students shout verbs for enroll vs list vs delete.

**Predict:** Is `/getCourses` REST style?

**Explain result:** Prefer `/courses` + GET. Verbs belong in the **method**, not the path.

> **In the Real World:** **GitHub** `/repos/{owner}/{repo}` is resource thinking.

---

### LO 2: Request/response structure for CRUD

**Problem:** Sketch four envelopes for a todo.

**Create POST** `/todos` body `{ "title": "Read docs", "completed": false }`  
**Response 201** `{ "id": 201, "title": "...", ... }` (JSONPlaceholder fakes the id).

**Read GET** `/todos/1` → 200 object.

**Update PUT** `/todos/1` full object.

**Delete DELETE** `/todos/1` → 200 `{}` on JSONPlaceholder.

**Predict before running:** Open `/todos/1` in the browser. What type is `completed`?

**Explain result:** Boolean. Contracts include **types**, not only names.

**Demo:** Paste URL in tab. Pretty-print JSON. This is a **GET** you can see without Fetch.

---

### LO 3: Interpret error responses

**Problem:** GET `/todos/999999` or a made-up path.

**Walkthrough:** Status **404**. Empty `{}` on JSONPlaceholder for missing todo — **docs matter**; not every API uses the same error body.

**Predict before running:** Does 404 always include `{ "error": "..." }`?

**Explain result:** **No.** The **status** is the portable part. Body is extra contract.

**Case study:** **400** validation vs **401** login vs **403** role vs **500** outage — different owner of the fix.

> **In the Real World:** **Razorpay** payment failures return coded errors so the UI can show "retry" vs "call bank."

---

### LO 4: Pagination, filtering, query params

**Problem:** `/todos` returns 200 items. A phone screen cannot show that.

**Translate logic:** `?_limit=10&_page=2` (JSONPlaceholder convention). Filter `?userId=1`.

**Write (URL only):**

```
https://jsonplaceholder.typicode.com/todos?userId=1
https://jsonplaceholder.typicode.com/posts?_limit=5
```

**Predict before running:** Does the path stay `/todos` when we add `?userId=1`?

**Explain result:** Yes. Query **qualifies**. Path still names the resource.

🎯 **Instructor Note:** Live open both URLs. Count objects. Compare.

---

### LO 5: Read third-party docs and identify required endpoints

**Problem:** "Show comments for post 3."

**Docs lab (15 min, pairs):**
1. Open JSONPlaceholder guide
2. Find **comments** resource
3. Identify filter `postId`
4. Write the full URL
5. Identify **users** list endpoint
6. Note: no API key required — contrast with real **OpenWeather** style keys (mention only; do not integrate)

**Predict:** Which method for "list comments"?

**Explain result:** **GET**. Creating a comment would be POST — fake API may not persist.

> **In the Real World:** First day on a job: **bookmark the API docs**, not random Medium posts.

---

## Recap (12 min)

Each pair submits: one resource map, one error story, one filtered URL.

**[Script:]** "You can **read** the menu. Next session you **order** with **Fetch** and put dishes on the **DOM**."

---

## Lecture Summary

- **REST** organizes **resources**, **endpoints**, and **HTTP verbs**
- **CRUD** has expected request bodies and response shapes
- **Errors** are status codes plus a documented body
- **Query params** paginate and filter without changing the resource name
- **Docs** tell you the required endpoints before you write code
- **Practical value:** You can integrate a third-party API by reading, not guessing
