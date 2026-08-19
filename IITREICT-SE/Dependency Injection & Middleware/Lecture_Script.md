# Lecture Script: Dependency Injection & Middleware
**Duration:** 110 minutes | **Tools:** VS Code, Uvicorn, Swagger, browser Network tab | **Language:** FastAPI

**Agenda:** Opening 8 · Why 10 · Concepts 20 · LO walkthroughs 50 · Live demo 12 · Recap 10

---

## Session Opening (8 min)

**[Script:]** "Your routes are getting chubby — skip, limit, fake auth headers. Today we extract that into **dependencies**. Then we wrap the whole app with **middleware** so every request can be timed."

**Problem:** Five list endpoints copy the same `skip`/`limit` code. A sixth forgets `limit` and returns 50,000 rows.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show duplicated pagination in two files. Ask "what happens when we change default limit to 20?" Wait for "miss a file."

**[Script:]** "Production FastAPI codebases at startups look like this: `get_db`, `get_current_user` as dependencies. Middleware logs request ids. If you skip DI, copy-paste bugs. If you skip middleware, you cannot see slow routes. We will not do JWT today — only the mechanism."

- **Real-world use:** Shared pagination, feature flags, request timing headers
- **Pain if misunderstood:** Middleware that never calls `call_next` — API hangs; dependencies that do heavy work twice

---

## What Is the Concept?

**Dependency injection:** FastAPI calls a helper, then passes the result into the route (or just runs it).

**`Depends()`:** the hook.

**Middleware:** `async def foo(request, call_next)` around all HTTP.

**Mental model:** DI = reusable plug for selected sockets. Middleware = plastic wrap around the whole box.

**Common mistakes:** Forgetting `await call_next`; using middleware for one route only (use a dependency); mutating global state inside middleware without care.

**Python vs JS:** Express `app.use` is middleware. FastAPI `Depends` is closer to "per-route plugins." Both exist.

---

## How Do We Apply It?

### LO 1: Explain DI and why it is useful in FastAPI

**Problem:** Routes mixed with setup code.

**Translate logic:** Setup → dependency. Use → route.

**Walkthrough:** Whiteboard "pagination" used by `/books` and `/notices`.

**Predict:** Changing default `limit` in one function updates both docs.

**Explain result:** DRY plus OpenAPI still shows query params from the dependency signature.

---

### LO 2: Create and inject reusable dependencies

**Write code:**

```python
from fastapi import Depends, FastAPI

app = FastAPI()

def pagination(skip: int = 0, limit: int = 10):
    return (skip, limit)

@app.get("/items")
def items(page: tuple = Depends(pagination)):
    skip, limit = page
    return {"skip": skip, "limit": limit}
```

**Predict before running:** `GET /items?limit=3` → `{"skip":0,"limit":3}`.

**Explain result:** FastAPI merged query params from `pagination`.

---

### LO 3: Implement basic middleware

**Write code:**

```python
from fastapi import Request

@app.middleware("http")
async def add_header(request: Request, call_next):
    response = await call_next(request)
    response.headers["X-Campus"] = "IITREICT"
    return response
```

**Predict before running:** Response headers include `X-Campus`.

**Explain result:** After the route, we still can modify the response.

🎯 **Instructor Note:** Network tab → Response headers.

---

### LO 4: Shared dependency across endpoints

**Write code:**

```python
def require_token(x_token: str | None = Header(default=None)):
    if x_token != "dev":
        raise HTTPException(401, "Need X-Token: dev")
    return True

@app.get("/a", dependencies=[Depends(require_token)])
def a():
    return {"route": "a"}

@app.get("/b", dependencies=[Depends(require_token)])
def b():
    return {"route": "b"}
```

**Predict before running:** No header → 401 on both. `GET /items` still open.

**Explain result:** One dependency, many routes. Public routes omit it.

---

### LO 5: Middleware before and after handlers

**Write code:**

```python
@app.middleware("http")
async def wrap(request: Request, call_next):
    print("BEFORE", request.url.path)
    response = await call_next(request)
    print("AFTER", response.status_code)
    return response
```

**Predict before running:** Terminal prints BEFORE, then AFTER, for each request.

**Explain result:** `call_next` is the route (and inner middleware). Before = inbound. After = outbound.

**Demo:** Hit `/items` twice. Two BEFORE/AFTER pairs.

---

## Live Demo Block (12 min)

Add pagination dependency to two list routes. Add timing header middleware. Show 401 on protected pair. Show public `/health` untouched.

**[Script:]** "Notice `/docs` for `/items` still lists skip and limit. Dependencies are first-class."

---

## Recap (10 min)

🎯 **Instructor Note:** "DI or middleware: log every request? reuse skip/limit? protect two admin routes?"

---

## Lecture Summary

- **DI** extracts shared setup so FastAPI routes stay thin
- **Reusable dependencies** inject values or checks via `Depends`
- **Middleware** processes every request/response pair
- A **shared dependency** can cover many endpoints without copy-paste
- Middleware runs **before** `call_next` and **after** the handler returns
- **Practical value:** You can grow an API without duplicating gates — next, CORS and files use the same middleware idea
