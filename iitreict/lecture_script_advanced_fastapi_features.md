# Lecture Script: Advanced FastAPI Features
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | Dependency Injection | 25 min |
| 3 | Middleware Implementation | 15 min |
| 4 | CORS Configuration | 15 min |
| 5 | Background Tasks | 15 min |
| 6 | File Uploads and Downloads | 20 min |
| 7 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** Open with the growth problem — what happens to a FastAPI app once it works and starts getting bigger. Learners at this stage have built basic routes with path parameters, query parameters, request bodies, and Pydantic models. They know the core. The hook is: that core alone does not scale cleanly. Wait after the opening scenario — let the problem register before you offer the solution.

**[Script:]**

"You have a working FastAPI application. Routes, Pydantic models, request bodies, responses — all of it. Now requirements arrive in batches.

Every route that touches user data needs authentication — check the token, verify the user, raise a 401 if anything is wrong. You have twelve routes. Do you copy that logic into all twelve? What happens when the token format changes?

Every request coming into the API needs a timestamp logged. Every response needs a custom header added. You cannot put that inside individual route functions — there are too many of them and the logic does not belong there.

A frontend application is calling your API from a different port. The browser blocks every request with a CORS error. You need to allow it without opening the API to the entire internet.

An endpoint accepts a file upload and then runs a virus scan on it. The scan takes three seconds. Do you make the user wait?

These are not edge cases. They are the standard concerns of any real API: shared logic across routes, request and response processing, cross-origin access, deferred work, and file handling. FastAPI has a specific, clean answer for each one. Today we cover all five — dependency injection, middleware, CORS, background tasks, and file handling. Each one is a tool that solves a real problem you will hit."

---

## Block 2 — Dependency Injection

### 2A — What Is Dependency Injection?

**[Script:]**

"Dependency injection is a pattern where a function declares what it needs and the framework provides it automatically. Instead of the function creating its own dependencies — opening a database connection itself, verifying a token itself — it declares what it requires as parameters, and FastAPI resolves and injects those at request time.

You have already seen a version of this. When you declare a Pydantic model as a route parameter, FastAPI parses and validates the request body and hands you the model instance. You did not parse the JSON yourself. FastAPI did it because your function declared what it needed. Dependency injection is the same pattern, generalized to any logic."

> 🎯 **Instructor Note:** Write the core mental model on the board before any code appears.

```
Route function declares what it needs
         ↓
FastAPI resolves the dependency
         ↓
Dependency runs, returns a value
         ↓
Value is injected into the route function
```

**[Script:]**

"The dependency is just a function. It can do anything — read a header, query a database, validate a token. Its return value is what gets injected. You define it once and reuse it across as many routes as you need."

---

### 2B — Defining and Using a Dependency

**[Script:]**

"You declare a dependency in a route parameter using `Depends()`. `Depends` is imported from `fastapi`. You pass it the dependency function — not a call to the function, just the function itself. FastAPI calls it for you.

The dependency function is a regular Python function. It can have its own parameters — path parameters, query parameters, headers — and FastAPI resolves those too."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If a route has a `Depends(get_current_user)` parameter and `get_current_user` raises an `HTTPException`, what happens to the route?" Answer: the route never runs. The exception is raised before the route function is called. This is the key protection pattern — reject bad requests at the dependency layer, before any business logic executes.

**Demo 1 — Basic Dependency Injection (whiteboard-friendly)**

```python
from fastapi import FastAPI, Depends, HTTPException, Header

app = FastAPI()

def verify_token(x_token: str = Header(...)):
    if x_token != "secret-token":
        raise HTTPException(status_code=401, detail="Invalid token")
    return x_token

@app.get("/protected")
def protected_route(token: str = Depends(verify_token)):
    return {"message": "Access granted", "token": token}
```

**[Script:]**

"Call `/protected` without the `X-Token` header — FastAPI returns a 422 because the header is required. Call it with the wrong token — `verify_token` raises a 401. Call it with `secret-token` — the route runs and returns the message.

The route function receives the return value of `verify_token` as its `token` parameter. The dependency ran, returned the token string, and FastAPI injected it.

Now here is the power. Add this same `Depends(verify_token)` to ten routes — all ten are protected. Change the validation logic in one place — all ten update automatically. This is why dependency injection exists: shared logic, single definition, zero duplication."

---

### 2C — Dependencies with Return Values and Shared Logic

**[Script:]**

"Dependencies are not limited to validation. A common use is extracting common query parameters that appear across many routes — pagination, for example."

**Demo 2 — Dependency for shared parameters (whiteboard-friendly)**

```python
from fastapi import FastAPI, Depends

app = FastAPI()

def pagination(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}

@app.get("/items")
def list_items(page: dict = Depends(pagination)):
    return {"skip": page["skip"], "limit": page["limit"]}

@app.get("/users")
def list_users(page: dict = Depends(pagination)):
    return {"skip": page["skip"], "limit": page["limit"]}
```

**[Script:]**

"Both `/items` and `/users` accept `skip` and `limit` query parameters. Both validate and default them through `pagination`. If the business rule changes — maximum limit becomes 50 — you change it in one function. Both routes update.

You can also chain dependencies — a dependency can itself have dependencies declared with `Depends`. FastAPI resolves the chain automatically. Keep chains shallow for readability, but the capability is there when you need it."

> 🎯 **Instructor Note:** Pause here. Ask: "Without dependency injection, where would the pagination logic live?" Answer: copied into every route function — or in a helper function called manually in every route. Both are worse than dependency injection because they either duplicate code or require each route to remember to call the helper. `Depends` makes it automatic and declarative.

**Recap of Block 2 before moving on:**

- Dependency injection declares what a route needs; FastAPI resolves and provides it
- A dependency is a regular function whose return value is injected into the route
- Declare with `Depends(function)` in the route parameter list — pass the function, not a call to it
- If a dependency raises `HTTPException`, the route never executes
- Dependencies are reusable across any number of routes — define once, use everywhere

---

## Block 3 — Middleware Implementation

### 3A — What Is Middleware?

**[Script:]**

"Middleware is a layer that wraps your entire application. Every request passes through middleware before it reaches any route. Every response passes through middleware before it leaves the application. Middleware does not care which route is being called — it runs for all of them.

This is the right place for concerns that apply universally: logging every request, adding headers to every response, timing how long requests take, blocking certain IP addresses. These belong in middleware because they have nothing to do with individual route logic."

> 🎯 **Instructor Note:** Draw the request lifecycle on the board.

```
Client Request
      ↓
  Middleware (runs first)
      ↓
  Route Handler
      ↓
  Middleware (runs on response)
      ↓
Client Response
```

**[Script:]**

"You add middleware with `@app.middleware('http')`. The function receives the request and a `call_next` function. You do whatever you need before the route runs, call `call_next(request)` to actually run the route and get the response, then do whatever you need on the response before returning it."

---

### 3B — Writing a Middleware Function

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If middleware adds a header to every response, and I call any route on the application — `/items`, `/users`, `/health` — what do all their responses have in common?" Answer: all of them will carry the header the middleware added, regardless of what the individual route returns.

**Demo 3 — Middleware for logging and headers (whiteboard-friendly)**

```python
import time
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def add_timing_header(request: Request, call_next):
    start = time.time()
    response = await call_next(request)
    duration = time.time() - start
    response.headers["X-Process-Time"] = str(duration)
    return response

@app.get("/ping")
def ping():
    return {"status": "ok"}
```

**[Script:]**

"Call `/ping`. The response JSON is `{'status': 'ok'}`. But look at the response headers — `X-Process-Time` is there, showing how many seconds the request took to process. Every route on this application now returns that header without any route function knowing about it.

The `await call_next(request)` line is where the route actually runs. Code before it runs before the route. Code after it runs after the route has produced its response. This gives you a clean before-and-after hook around every request.

Middleware must be `async def`. The `call_next` function is awaitable — you must `await` it."

> 🎯 **Instructor Note:** Ask: "What is the difference between using middleware versus using a dependency for logging?" Guide learners to: middleware runs for every request unconditionally, including requests that fail routing entirely (404s, method not allowed). Dependencies run only for routes they are declared on. Middleware is for universal concerns, dependencies for route-specific shared logic.

**Recap of Block 3 before moving on:**

- Middleware wraps the entire application — it runs for every request and every response
- Use `@app.middleware("http")` to register a middleware function
- The function receives `request` and `call_next`; code before `await call_next(request)` runs pre-route, code after runs post-route
- Middleware is `async def`; `call_next` must be awaited
- Use middleware for universal concerns: logging, timing, response headers — not for route-specific logic

---

## Block 4 — CORS Configuration

### 4A — What Is CORS and Why Does It Trigger?

**[Script:]**

"CORS — Cross-Origin Resource Sharing — is a browser security mechanism. The Same-Origin Policy says a page loaded from one origin cannot make requests to a different origin unless that second origin explicitly permits it. Origin is the combination of scheme, domain, and port.

Your FastAPI backend runs on port 8000. Your React frontend runs on port 3000. Same machine, different port — different origins. The browser blocks the API response unless the backend sends the right headers back.

This is entirely a browser rule. curl and Postman do not enforce it — that is why your API works in Postman but breaks in the browser. The API is fine. The browser is doing its job. You need to tell the browser which origins your API trusts."

> 🎯 **Instructor Note:** Write the origin definition on the board.

```
Origin = scheme + domain + port

http://localhost:3000   ← frontend origin
http://localhost:8000   ← backend origin
Different port → different origin → browser blocks without CORS headers
```

**[Script:]**

"FastAPI provides `CORSMiddleware` — a built-in middleware that handles all the CORS header logic automatically. You add it once, configure which origins to trust, and every response gets the right headers."

---

### 4B — Configuring CORSMiddleware

**[Script:]**

"The key parameters: `allow_origins` is the list of trusted origin strings. `allow_credentials` allows cookies and auth headers. `allow_methods` specifies which HTTP methods are permitted. `allow_headers` specifies which request headers are allowed.

In development, you might use `['*']` for origins to allow everything. In production, always list specific origins."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I add CORS middleware with `allow_origins=['http://localhost:3000']` and a request comes from `http://localhost:5000`, what does the browser do?" Answer: blocks it. Port 5000 is not in the allow list. The API still receives the request — the browser blocks the response from being read by the page at 5000.

**Demo 4 — CORSMiddleware (whiteboard-friendly)**

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

@app.get("/data")
def get_data():
    return {"message": "hello from the API"}
```

**[Script:]**

"Call `/data` and inspect the response headers. You will see `Access-Control-Allow-Origin: http://localhost:3000`. That header is what the browser reads to decide whether to allow the frontend page to access the response.

`add_middleware` is called on the app object, not inside a route. It wraps the whole application. Call it once, near the top of your file. Every route automatically inherits the CORS headers.

For multiple trusted origins, add them all to the list: `allow_origins=['http://localhost:3000', 'https://myapp.com']`. Never use `['*']` in production — it allows any domain including hostile ones."

> 🎯 **Instructor Note:** Common confusion: learners think adding CORS middleware makes their API less secure. Correct this — the Same-Origin Policy protects users of your API's frontend, not the API itself. CORS middleware relaxes the browser restriction in a controlled, explicit way. You choose exactly which origins to trust.

**Recap of Block 4 before moving on:**

- CORS is a browser mechanism — curl and Postman are unaffected; only browser requests are blocked
- Same-Origin Policy blocks cross-origin requests; different port means different origin
- `CORSMiddleware` handles all CORS headers automatically — add it once with `app.add_middleware`
- `allow_origins` takes specific origin strings; use `['*']` only in development
- In production, list every trusted origin explicitly

---

## Block 5 — Background Tasks

### 5A — Why Background Tasks Exist

**[Script:]**

"When a request arrives, the client is waiting. Every millisecond your route spends doing work is a millisecond the client waits for a response. Some work genuinely must complete before you respond — saving a record, validating data. But other work does not: sending a confirmation email, writing an audit log, triggering a webhook. The client does not need to wait for those.

`BackgroundTasks` in FastAPI lets you queue functions to run after the response has been sent. The client gets an immediate response. The deferred work happens afterward in the same process."

> 🎯 **Instructor Note:** Use the restaurant analogy. "The waiter confirms your order immediately — they do not stand at the counter watching the kitchen cook. The confirmation is instant. The food preparation happens separately. Background tasks are the same separation."

---

### 5B — Using BackgroundTasks

**[Script:]**

"You declare `BackgroundTasks` as a route parameter. FastAPI injects it automatically — same pattern as `Depends`. You call `background_tasks.add_task(function, arg1, arg2)` — pass the function reference, not a call to it, then any arguments the function needs."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If a background task prints to the terminal, and the route returns a response — which happens first, the print or the response arriving at the client?" Answer: the response arrives first. The background task runs after the response is sent. The client never sees any output from the background task directly.

**Demo 5 — BackgroundTasks (whiteboard-friendly)**

```python
from fastapi import FastAPI, BackgroundTasks

app = FastAPI()

def send_email(address: str, message: str):
    print(f"Sending email to {address}: {message}")

@app.post("/register")
def register(email: str, background_tasks: BackgroundTasks):
    background_tasks.add_task(send_email, email, "Welcome!")
    return {"status": "registered"}
```

**[Script:]**

"Call `/register?email=alice@example.com`. You get `{'status': 'registered'}` immediately. Then — after the response — you see the print in the terminal.

The critical syntax: `add_task(send_email, email, 'Welcome!')` — the function first, then its arguments separately. A common mistake is writing `add_task(send_email(email, 'Welcome!'))` — that calls the function immediately during the request and passes its return value to `add_task`. The function must be passed as a reference.

You can add multiple background tasks in one route — call `add_task` multiple times. They run in order after the response.

Background tasks run in the same process after the response. They are not a job queue, not a separate worker. Use them for lightweight work: emails, logs, notifications. For heavy or long-running work, a dedicated task queue is the right tool — but background tasks cover the common case cleanly."

**Recap of Block 5 before moving on:**

- `BackgroundTasks` runs functions after the HTTP response has been sent
- Declare it as a route parameter — FastAPI injects it automatically
- `add_task(function, arg1, arg2)` — function reference first, then arguments; never call the function directly
- Multiple tasks can be added per route; they run in the order added
- Use for lightweight deferred work: emails, logs, notifications — not heavy computation

---

## Block 6 — File Uploads and Downloads

### 6A — What Is Different About Files?

**[Script:]**

"Everything we have built so far — request bodies, query parameters, path parameters — works with text and structured data. Files are binary, often large, and they travel over HTTP as `multipart/form-data`, not `application/json`. FastAPI handles this with two types: `File` and `UploadFile`.

`File(...)` with a `bytes` annotation reads the entire file into memory as a bytes object. Simple, but dangerous for large files — the whole thing sits in RAM. `UploadFile` wraps the file in an object with async methods and stores it as a temporary file rather than loading everything into memory. For anything beyond tiny files, use `UploadFile`."

> 🎯 **Instructor Note:** Write the comparison on the board.

```
bytes / File(...)   → whole file in memory     → simple, risky for large files
UploadFile          → file-like async object   → efficient, use this in practice
```

---

### 6B — Receiving File Uploads

**[Script:]**

"An `UploadFile` object gives you `filename`, `content_type`, and the async `read()` method. Because `read()` is async, your route must be `async def`. After calling `await file.read()`, the file cursor is at the end — if you need to read it again, call `await file.seek(0)` first."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I call `await file.read()` twice in a row without seeking, what does the second call return?" Answer: empty bytes — `b""`. The cursor is at the end after the first read. This is a real bug that appears in code reviews constantly.

**Demo 6 — File Upload (whiteboard-friendly)**

```python
from fastapi import FastAPI, File, UploadFile

app = FastAPI()

@app.post("/upload")
async def upload(file: UploadFile = File(...)):
    content = await file.read()
    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "size_bytes": len(content)
    }
```

**[Script:]**

"Test with curl: `curl -X POST 'http://localhost:8000/upload' -F 'file=@yourfile.txt'`. The response shows the filename, content type, and size.

`File(...)` in the default means the file is required. If no file is sent, FastAPI returns a 422 automatically — same validation behavior as required body fields in Pydantic.

To save the file to disk instead of reading it into memory, use `shutil.copyfileobj` — it copies in chunks, so the entire file is never in memory at once."

```python
import shutil
from fastapi import FastAPI, UploadFile, File

app = FastAPI()

@app.post("/save")
async def save(file: UploadFile = File(...)):
    with open(f"uploads/{file.filename}", "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)
    return {"saved": file.filename}
```

**[Script:]**

"`file.file` is the underlying file-like object. `shutil.copyfileobj` reads from it in chunks and writes to the destination. The `uploads/` directory must exist before this runs — otherwise you get a `FileNotFoundError`."

---

### 6C — File Downloads and Responses

**[Script:]**

"Sending files back to the client uses `FileResponse` for files on disk, and `StreamingResponse` for generated or in-memory content.

`FileResponse` takes a file path. FastAPI streams the file efficiently, sets the content type, and with the `filename` parameter adds a `Content-Disposition: attachment` header that triggers a browser download rather than inline display.

`StreamingResponse` takes any iterable — a generator, an `io.BytesIO` object — and streams it to the client. Use this when the content is generated dynamically or does not exist as a file."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I return a `FileResponse` without the `filename` parameter, will the browser download the file or try to display it?" Answer: depends on the content type. PDFs and images often display inline; other types may download. Adding `filename` sets the `Content-Disposition: attachment` header explicitly, which always triggers a download.

**Demo 7 — FileResponse and StreamingResponse (whiteboard-friendly)**

```python
from fastapi import FastAPI
from fastapi.responses import FileResponse, StreamingResponse
import io

app = FastAPI()

@app.get("/download")
def download():
    return FileResponse("report.pdf", media_type="application/pdf",
                        filename="report.pdf")

@app.get("/export")
def export():
    data = b"name,score\nAlice,95\nBob,87\n"
    return StreamingResponse(io.BytesIO(data), media_type="text/csv",
                             headers={"Content-Disposition":
                                      "attachment; filename=export.csv"})
```

**[Script:]**

"The `FileResponse` serves a PDF from disk. The `StreamingResponse` generates CSV content in memory and streams it. Both set content type and disposition so the browser handles them as downloads.

`io.BytesIO` wraps bytes in a file-like object so `StreamingResponse` can iterate over it. For large dynamic content, replace `io.BytesIO` with a generator that yields chunks — that way you never hold the full content in memory."

> 🎯 **Instructor Note:** Point out that background tasks and file uploads compose naturally — the common pattern is: receive the file, save it, return an immediate response, queue processing in a background task. Learners now have all the pieces for that pattern. Do not write a combined demo unless time permits — naming the composition is sufficient.

**Recap of Block 6 before moving on:**

- Files are sent as `multipart/form-data`; use `UploadFile` for efficient in-memory-safe handling
- `UploadFile` provides `filename`, `content_type`, and async `read()`; use `seek(0)` before re-reading
- Save to disk with `shutil.copyfileobj(file.file, buffer)` to avoid loading the entire file into memory
- `FileResponse` serves a file from disk; the `filename` parameter triggers a browser download
- `StreamingResponse` serves generated or in-memory content; wrap bytes in `io.BytesIO` or use a generator

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "Where does dependency injection code live relative to routes? What does middleware do that dependencies cannot? What makes CORS a browser problem rather than an API problem? What is the syntax mistake everyone makes with add_task? What is the difference between UploadFile and bytes?" Keep pace brisk — the goal is consolidation, not re-teaching.

**Dependency Injection**

- Dependency injection declares what a route needs; FastAPI resolves and injects the return value automatically
- Define a dependency as a regular function; use `Depends(function)` in the route parameter — pass the reference, not a call
- If a dependency raises `HTTPException`, the route never runs — this is the core protection pattern
- Dependencies are reusable across any number of routes; change the logic once, all routes that depend on it update
- Dependencies can have their own parameters; FastAPI resolves them exactly as it does route parameters

**Middleware Implementation**

- Middleware wraps the entire application — it processes every request and every response regardless of which route is called
- Register with `@app.middleware("http")`; the function receives `request` and `call_next`
- Code before `await call_next(request)` runs pre-route; code after runs post-route on the response object
- Use for universal concerns — logging, timing, adding response headers; use dependencies for route-specific shared logic

**CORS Configuration**

- CORS is enforced by the browser based on the Same-Origin Policy; different ports are different origins
- `CORSMiddleware` handles all CORS headers automatically when added with `app.add_middleware`
- `allow_origins` lists trusted origins; use `["*"]` only in development, always list specific origins in production
- curl and Postman are not affected by CORS — only browser-based requests

**Background Tasks**

- `BackgroundTasks` runs functions after the HTTP response is sent; the client does not wait
- Declare as a route parameter; FastAPI injects automatically
- `add_task(function, arg1, arg2)` — function reference first, then arguments; never call the function inline
- Best for lightweight deferred work: emails, logs, webhooks; not for heavy computation

**File Uploads and Downloads**

- `UploadFile` handles file uploads efficiently — it does not load the entire file into memory
- `await file.read()` reads content; call `await file.seek(0)` before reading again
- Save to disk with `shutil.copyfileobj(file.file, buffer)` for chunk-based, memory-safe writes
- `FileResponse` serves files from disk; the `filename` parameter triggers a browser download
- `StreamingResponse` serves generated content; wrap bytes in `io.BytesIO` or use a generator for large payloads

**Why All Five Features Matter Together**

- Dependency injection, middleware, CORS, background tasks, and file handling are the five features that separate a basic FastAPI prototype from an API that handles real production concerns — shared logic without duplication, universal request processing, cross-origin browser access, non-blocking deferred work, and binary file handling — every backend application you build beyond the simplest will need at least two of these, and knowing all five means you reach for the right tool immediately rather than working around the problem

---

*End of script.*
