# Lecture Script: FastAPI Setup & GET Endpoints
**Duration:** 110 minutes | **Tools:** VS Code, Terminal, Browser, venv | **Language:** Python / FastAPI

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | React fetch needs a home URL |
| Why Does This Matter? | 12 min | JSON APIs, Swagger, in-memory limits |
| What Is the Concept? | 22 min | FastAPI, Uvicorn, GET, path vs query |
| How Do We Apply It? (LOs) | 53 min | Five LOs live |
| Live lab | 8 min | Campus list GET |
| Recap | 7 min | POST and Pydantic next |

---

## Session Opening (8 min)

**[Script:]** "Last time you ran Python in a venv. Today that Python **listens on a port**. When React calls `fetch('http://127.0.0.1:8000/items')`, FastAPI answers with JSON. **Uvicorn** is the waiter at the door. **GET** is read-only. We store items in a Python list — RAM, not a database yet."

**Problem hook:** Frontend team blocked because there is no URL to hit. Swagger becomes the contract.

🎯 **Instructor Note:** venv must be active. `pip install fastapi uvicorn` once per machine/project. If port 8000 is busy, use `--port 8001`.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show a failed React fetch to a URL that does not exist. Then start Uvicorn and refresh.

**[Script:]** "If you only know React, you consume APIs. Jobs also need people who **create** them. Wrong path vs query and the frontend sends data the server never reads. Skipping Swagger means you debug in the dark."

> **In the Real World:** Product APIs at companies like **Stripe** and **GitHub** are documented so other apps can integrate. Your `/docs` page is the beginner version of that habit.

**Pain if misunderstood:**

- Browser GET vs "I thought I posted"
- Path `/items/1` vs query `?id=1` mixed up
- Restart wipes the in-memory list — surprise "data loss"

| Piece | Job |
|-------|-----|
| FastAPI `app` | Routes and JSON |
| Uvicorn | HTTP server |
| Swagger `/docs` | Click-to-test |
| List / dict | Temporary store |

---

## What Is the Concept?

**FastAPI:** Decorators like `@app.get("/path")` bind a function to HTTP GET.

**Uvicorn:** `uvicorn main:app --reload` — module `main`, object `app`.

**Path parameter:** `{item_id}` in the URL path.

**Query parameter:** function arguments that are not in the path.

**In-memory:** Python list lives only while Uvicorn runs.

🎯 **Instructor Note:** Draw: Browser → Uvicorn → function → dict → JSON.

---

## How Do We Apply It?

### LO 1: Create a FastAPI app and run it with Uvicorn

**Problem:** No process is listening, so fetch cannot succeed.

**Translate logic:** Build `app = FastAPI()`, run Uvicorn against `main:app`.

**Write code:**

```python
from fastapi import FastAPI
app = FastAPI()
```

```bash
uvicorn main:app --reload
```

**Predict before running: What will happen?**

**Predict output:** Terminal shows Uvicorn running on port 8000.

**Explain result:** Uvicorn loaded `app` and opened HTTP. FastAPI is ready for routes.

---

### LO 2: Implement a basic GET returning JSON

**Problem:** Need a health URL for "is the API up?"

**Translate logic:** GET `/` returns a dict. FastAPI encodes JSON.

**Write code:**

```python
@app.get("/")
def home():
    return {"ok": True}
```

**Predict before running: What will happen?**

**Predict output:** Browser shows `{"ok":true}` (JSON boolean).

**Explain result:** Python `True` becomes JSON `true`. GET is a read.

🎯 **Instructor Note:** Open Network tab later in React week — same JSON shape.

---

### LO 3: Use path and query parameters

**Problem:** One route needs an id. Another needs an optional filter.

**Translate logic:** `{item_id}` in path. `limit` as a function argument = query.

**Write code:**

```python
@app.get("/items/{item_id}")
def one(item_id: int):
    return {"id": item_id}

@app.get("/items")
def many(limit: int = 10):
    return {"limit": limit}
```

**Predict before running: What will happen?**

**Predict:** `/items/4` → `{"id":4}`. `/items?limit=2` → `{"limit":2}`.

**Explain result:** Path is required position in the URL. Query is optional with a default.

🎯 **Instructor Note:** Hit `/items/abc` if typed as `int` — FastAPI 422. Mention only; full validation next session.

---

### LO 4: Test endpoints in Swagger UI

**Problem:** Students guess URLs and mistype them.

**Translate logic:** Open `/docs`, click GET, Try it out, Execute.

**Write code:** (no new code — use `/docs`)

**Predict before running: What will happen?**

**Predict output:** Swagger shows request URL, status 200, JSON body.

**Explain result:** FastAPI generated docs from your routes. Same contract React will use.

---

### LO 5: Serve data from a simple in-memory store

**Problem:** Return real-looking campus items, not only echo parameters.

**Translate logic:** Module-level list. GET returns it. GET by id scans the list.

**Write code:**

```python
ITEMS = [{"id": 1, "name": "Lab"}, {"id": 2, "name": "Talk"}]

@app.get("/events")
def list_events():
    return ITEMS
```

**Predict before running: What will happen?**

**Predict output:** JSON array of two objects.

**Explain result:** The list is RAM. Stop Uvicorn, start again, same starter data unless you edit the file.

---

## Live Lab (8 min)

`GET /events` and `GET /events/{id}` from a 3-item list. Prove both in Swagger.

> **In the Real World:** First backend ticket is often "add a GET that lists X." You just did that ticket.

---

## Recap (7 min)

🎯 **Instructor Note:** "Where do I try routes without React?" (`/docs`). "Does restart keep POSTed data?" (No — in-memory. We are GET-only today.)

**[Script:]** "You created the app, ran Uvicorn, returned JSON, used path and query, tested in Swagger, and listed in-memory data. Next: Pydantic so bad JSON is rejected at the door."

---

## Lecture Summary

- **Create FastAPI + Uvicorn:** `app` and `uvicorn main:app --reload`
- **GET JSON:** return a dict or list
- **Path vs query:** `/items/1` vs `?limit=2`
- **Swagger UI:** `/docs` is the live contract
- **In-memory store:** list/dict in RAM until restart
- **Practical value:** React now has a real local API to call later
