# Lecture Script: Advanced FastAPI Features — Dependency Injection and Middleware
**Facilitator-Facing | 110 Minutes | Beginner Level**

---

## Running Example: Smart Cart Web App

Every concept in this script lives inside a single application: a **Smart Cart Web App** — an e-commerce backend where customers browse products, add items to a cart, and place orders. The API has multiple routes, multiple clients, and real cross-cutting concerns: every request needs to be authenticated, every response needs to be logged, every database connection needs to be managed.

Return to Smart Cart constantly. When learners see the same application getting cleaner and more capable as new tools are introduced, the concepts feel purposeful rather than academic.

---

## Session Overview

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 15 min |
| 2 | What Is Dependency Injection? | 20 min |
| 3 | Creating and Using Dependencies | 30 min |
| 4 | What Is Middleware? | 15 min |
| 5 | Request and Response Processing with Middleware | 30 min |
| 6 | Lecture Summary | 10 min |

---

## Block 1: Why Does This Matter?

### Instructor Note — Hook Activity

Before speaking, write these two code fragments on the board side by side. Do not explain them. Ask learners to look at them for 30 seconds and then say what they notice.

**Fragment A — without dependency injection or middleware:**

```python
@app.get("/cart")
def get_cart(token: str):
    user = verify_token(token)
    db = get_database_connection()
    log_request("GET", "/cart", user)
    cart = db.query(Cart).filter_by(user_id=user.id).all()
    log_response("GET", "/cart", 200)
    db.close()
    return cart

@app.post("/orders")
def place_order(token: str):
    user = verify_token(token)
    db = get_database_connection()
    log_request("POST", "/orders", user)
    order = db.create_order(user.id)
    log_response("POST", "/orders", 201)
    db.close()
    return order
```

**Fragment B — with dependency injection and middleware:**

```python
@app.get("/cart")
def get_cart(user: User = Depends(get_current_user), db: Session = Depends(get_db)):
    return db.query(Cart).filter_by(user_id=user.id).all()

@app.post("/orders")
def place_order(user: User = Depends(get_current_user), db: Session = Depends(get_db)):
    return db.create_order(user.id)
```

Give learners 60 seconds to look. Then ask: "What is different? What has disappeared from Fragment B? Where did it go?" Take three or four answers without correcting anyone. Then begin.

---

[Script:]
"Every line that disappeared from Fragment B is still running. The authentication check still happens. The database connection is still opened and closed. The logging still fires on every request. None of that work went away. It just moved. That is the entire point of what we are covering today."

"Look at Fragment A again. Every route is doing five things: authenticate the user, open a database connection, log the request, do the actual business logic, log the response, close the database. That is a five-to-one ratio of overhead to logic. And every time you add a new route, you write all five again. If the authentication logic changes, you update it in thirty places."

"In real production APIs, this is one of the most painful problems teams face. Not the business logic — that is usually the easy part. The hard part is all the code that has to run around every piece of business logic, consistently, without being forgotten on any route."

"Dependency injection and middleware are the two tools FastAPI gives you to solve this. Dependency injection handles the per-route shared logic — things a route needs injected into it, like a database connection or a verified user. Middleware handles the cross-cutting pipeline logic — things that run on every single request and response regardless of which route handles them."

"If you skip these tools and write Fragment A style code, you will spend more time maintaining scaffolding than building features. And eventually something will break — a route that forgot to close the database connection, or a token that slipped through without being checked — because humans forget things when they repeat them forty times."

---

## Block 2: What Is Dependency Injection?

### Clear Definition

[Script:]
"Dependency injection is a pattern where a function declares what it needs, and something else — the framework — is responsible for creating and providing those things. The function does not go and fetch its own dependencies. It asks for them, and they arrive."

"The phrase sounds intimidating. Break it down literally. A dependency is something your code needs to do its job — a database connection, a verified user, a configuration value. Injection means it is handed to you rather than you going to get it. Dependency injection: the things you need are handed to you."

---

### Mental Model

[Script:]
"Here is a mental model. Think of a restaurant kitchen. A chef does not go to the store to buy ingredients before cooking each dish. The kitchen has a system: a supplier delivers ingredients, a prep cook portions them, and the chef finds what they need ready at their station. The chef declares what they need for the recipe. The kitchen system provides it."

"Your route function is the chef. `Depends()` is how you tell FastAPI what your function needs. FastAPI is the kitchen system that resolves and delivers everything before your function runs."

---

### Python Comparison — Before and After

[Script:]
"Without dependency injection in plain Python, a function that needs a database connection just creates it:"

```python
def get_cart():
    db = DatabaseConnection()   # function creates its own dependency
    result = db.query("SELECT * FROM cart")
    db.close()
    return result
```

"The function is responsible for the full lifecycle of everything it needs. That means it is also coupled to the specific implementation. To test this function, you have to have a real database. You cannot swap in a fake one easily."

"With dependency injection:"

```python
def get_cart(db):             # dependency is handed in from outside
    return db.query("SELECT * FROM cart")
```

"Now the function only knows how to use a database connection. It does not know how to create one. That concern belongs elsewhere. This makes the function easier to test, easier to reuse, and easier to change."

---

### FastAPI's `Depends()` — The Mechanism

[Script:]
"In FastAPI, dependency injection uses a function called `Depends`. You put `Depends(some_function)` as the default value of a parameter in your route function. FastAPI sees this, calls `some_function` to get the value, and passes the result into your route as that parameter."

"The dependency itself is just a Python function. It can return anything. It can also declare its own dependencies using `Depends`, so you can chain them. FastAPI resolves the whole chain before your route runs."

---

### Common Mistakes — Surface Early

[Script:]
"Three mistakes I see beginners make here. First: thinking `Depends` is some kind of magic import. It is not. It is a plain function call that returns a special object FastAPI knows how to interpret. The dependency function you write is completely ordinary Python."

"Second: writing logic inside the route that should be in a dependency. If two routes need the same thing — the current user, an open database session — that thing belongs in a dependency, not copy-pasted into both routes."

"Third: forgetting that `Depends` functions run every time the route is called. They are not singletons. Every request gets a fresh execution of the dependency function unless you explicitly configure otherwise."

---

## Block 3: Creating and Using Dependencies — Learning Objectives 1 and 2

### 3A: A Simple Dependency

#### Problem

[Script:]
"In Smart Cart, most routes need to know who is making the request. Right now, imagine every route is extracting a token from the request header and calling `verify_token()` manually. We have six routes. That means six copies of token extraction code. When we add a seventh route, we will probably forget."

"The problem: how do we write the authentication logic once and make it available to every route that needs it, automatically?"

---

#### Translate Logic

[Script:]
"We pull the token extraction and verification into its own function. That function returns the verified user. Any route that needs the current user declares a dependency on that function using `Depends`. FastAPI calls the function, gets the user, and passes it in."

---

#### Write the Code

```python
from fastapi import FastAPI, Depends, HTTPException, Header

app = FastAPI()

def get_current_user(authorization: str = Header(...)):
    if authorization != "valid-token":
        raise HTTPException(status_code=401, detail="Invalid token")
    return {"user_id": 1, "name": "Alice"}

@app.get("/cart")
def get_cart(user: dict = Depends(get_current_user)):
    return {"cart": "Alice's items", "user": user["name"]}
```

[Script:]
"The dependency function `get_current_user` is just a regular Python function. It takes the `Authorization` header as a parameter — FastAPI injects that from the request automatically. If the token is invalid, it raises an `HTTPException`. If valid, it returns the user dictionary."

"In the route, `user: dict = Depends(get_current_user)` tells FastAPI: before running `get_cart`, call `get_current_user`, and pass the result in as `user`. The route function never sees the token. It only sees the clean, verified user object."

---

### Instructor Note — Prediction Pause

Write on the board before running:

```python
# Request with header: Authorization: valid-token
# Request with header: Authorization: bad-token
# Request with no Authorization header
```

Ask: "Predict before running: what does each of these return? Specifically — what happens when the header is missing entirely?"

Take three answers. Then run all three cases.

[Script:]
"When the header is missing, FastAPI returns a 422 error — not a 401. Why? Because `Header(...)` with the `...` default means the header is required. The validation failure happens before the dependency function even runs. This is Pydantic doing its job at the boundary. If you want to make the header optional, you would use `Header(None)` and handle the `None` case inside the function."

---

### Demo 1 — Dependency in Action

```python
from fastapi import FastAPI, Depends, HTTPException, Header

app = FastAPI()

def get_current_user(authorization: str = Header(...)):
    if authorization != "Bearer smart-cart-token":
        raise HTTPException(status_code=401, detail="Unauthorized")
    return {"user_id": 42, "name": "Alice"}

@app.get("/profile")
def get_profile(user: dict = Depends(get_current_user)):
    return {"message": f"Hello, {user['name']}", "id": user["user_id"]}
```

[Script:]
"Run this with a valid token and we get the profile. Run it with an invalid token and we get a 401. The route function `get_profile` contains zero authentication logic. All of it lives in `get_current_user`. If authentication requirements change tomorrow, we update one function."

---

### 3B: Dependencies with Their Own Dependencies — Chaining

[Script:]
"Here is where it gets powerful. A dependency function can itself declare dependencies using `Depends`. FastAPI resolves the whole chain automatically."

"In Smart Cart, consider this: getting the current user requires a valid token. Getting a database session is a separate concern. A route might need both. We can chain them."

```python
from fastapi import FastAPI, Depends, Header, HTTPException

app = FastAPI()

def get_db():
    db = {"connection": "open"}   # simulating a DB session
    try:
        yield db
    finally:
        db["connection"] = "closed"

def get_current_user(
    authorization: str = Header(...),
    db: dict = Depends(get_db)
):
    if authorization != "Bearer smart-cart-token":
        raise HTTPException(status_code=401, detail="Unauthorized")
    return {"user_id": 1, "name": "Alice", "db_status": db["connection"]}

@app.get("/orders")
def get_orders(user: dict = Depends(get_current_user)):
    return {"orders": [], "user": user["name"]}
```

[Script:]
"Notice `get_current_user` now depends on `get_db`. The route depends on `get_current_user`. FastAPI resolves the chain: it calls `get_db` first, passes the result to `get_current_user`, then passes that result to `get_orders`. The route declares one dependency. FastAPI handles the rest."

---

### Instructor Note — The `yield` Pattern

This will cause confusion. Address it directly before moving on.

[Script:]
"Notice `get_db` uses `yield` instead of `return`. This is how you write a dependency that needs cleanup code. Everything before `yield` runs before the route. The `yield` value is what gets injected. Everything after `yield` — in the `finally` block — runs after the route finishes, even if the route raises an exception."

"Think of it like a context manager. The dependency opens the database before the route, the route uses it, and then the dependency closes it after. This is the standard pattern for managing resources like database connections in FastAPI. You will see it constantly."

---

### Instructor Note — Pause Point: Yield vs Return

Ask the room: "Without `yield`, what would happen if an exception occurred in the route? Would the database connection still close?" Let learners think. The answer: with plain `return`, there is no cleanup code at all — the connection would leak. The `yield` pattern guarantees cleanup.

---

### 3C: Reusing Dependencies Across Multiple Routes

[Script:]
"The real payoff of dependencies is reuse. Let us add a second route to Smart Cart that also needs authentication."

```python
@app.get("/cart")
def get_cart(user: dict = Depends(get_current_user)):
    return {"cart": "items", "user": user["name"]}

@app.get("/orders")
def get_orders(user: dict = Depends(get_current_user)):
    return {"orders": [], "user": user["name"]}

@app.delete("/cart/{item_id}")
def remove_item(item_id: int, user: dict = Depends(get_current_user)):
    return {"removed": item_id, "user": user["name"]}
```

[Script:]
"Three routes. One authentication function. If we add ten more routes, they all use the same `Depends(get_current_user)`. If authentication logic changes — say, we move from a simple string check to a JWT library — we update one function and all routes are updated automatically. That is the value."

---

## Block 4: What Is Middleware?

### Clear Definition

[Script:]
"Middleware is code that runs on every request before it reaches a route, and on every response before it leaves the application. Every request passes through the middleware layer on the way in and on the way out."

"Dependencies are per-route — a route opts in by declaring a `Depends`. Middleware is global — it applies to every single request, to every route, automatically, with no opt-in required."

---

### Mental Model

[Script:]
"Here is a mental model. Think of middleware as a security checkpoint at an airport. Every passenger — every request — passes through the same checkpoint regardless of where they are flying. The checkpoint does not care about the destination. It runs its process, and if everything is fine, the passenger continues. On the return side, every departing passenger also passes through."

"Your route is the gate. The middleware is the checkpoint. The checkpoint runs before and after. The gate only handles the specific flight."

"A second mental model: middleware is like a pipeline of layers, one around the other, like an onion. Each layer wraps all the layers inside it. Request goes inward through the layers, hits the route, response comes outward through the same layers in reverse. This is sometimes called the middleware stack or middleware chain."

---

### Dependency Injection vs. Middleware — The Distinction

[Script:]
"This is important because beginners often ask: when do I use a dependency and when do I use middleware? Here is the rule of thumb."

"Use a dependency when: the logic is specific to certain routes, the logic needs to inject something into the route function, you want individual routes to opt in or opt out."

"Use middleware when: the logic must run on every single request without exception, you need to inspect or modify the raw request before any routing happens, you need to inspect or modify the response after routing is complete, you want to add headers or log timing across the whole application."

"Examples for Smart Cart: authentication for protected routes — dependency. Adding a response time header to every response — middleware. Logging every incoming request URL — middleware. Getting the current user for a specific route — dependency."

---

### Common Mistakes

[Script:]
"Two mistakes beginners make with middleware. First: using middleware for logic that only applies to some routes. That is what dependencies are for. Putting route-specific logic in middleware makes it run unnecessarily and makes it harder to maintain."

"Second: forgetting to call `await call_next(request)` inside the middleware. If you forget that line, the request never reaches the route. The middleware handles the request entirely and the route handler never runs. The application appears to do nothing."

---

## Block 5: Request and Response Processing with Middleware — Learning Objectives 3 and 4

### 5A: Writing Your First Middleware

#### Problem

[Script:]
"Smart Cart is getting production traffic. The operations team wants to know how long every API request takes to process. They want this information in the response headers so their monitoring tools can pick it up. Every response should include a header called `X-Process-Time` with the elapsed time in seconds."

"We cannot put this in every route function — that would be dozens of changes and it would still not catch error responses from failed validation. We need it to run at the application level."

---

#### Translate Logic

[Script:]
"We write a middleware function. Before passing the request to the route, we record the start time. After the route finishes and we have the response, we calculate elapsed time and add it as a header. We use FastAPI's `@app.middleware('http')` decorator to register it."

---

#### Write the Code

```python
import time
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def add_process_time_header(request: Request, call_next):
    start_time = time.time()
    response = await call_next(request)
    process_time = time.time() - start_time
    response.headers["X-Process-Time"] = str(process_time)
    return response

@app.get("/products")
def get_products():
    return {"products": ["Laptop", "Mouse", "Keyboard"]}
```

[Script:]
"Walk through this line by line. `@app.middleware('http')` tells FastAPI this function is middleware for HTTP requests. The function takes two parameters: `request`, the incoming request object, and `call_next`, a function that passes the request to the next layer — ultimately the route handler."

"`start_time` is recorded before calling `call_next`. Then we `await call_next(request)` — this is where the actual route runs. Control comes back to us with `response` — the route's response. We calculate elapsed time, add the header, and return the response."

"Everything before `await call_next` is request processing. Everything after is response processing. That line is the boundary."

---

### Instructor Note — Prediction Pause

Write on the board before running:

```python
# What headers will the /products response contain?
# What value will X-Process-Time have?
# What happens if we remove `await call_next(request)` entirely?
```

Ask all three questions. Get predictions for each. Then run and show the response headers. Then demonstrate what happens when `call_next` is removed.

[Script:]
"Without `await call_next(request)`, the route never runs. The middleware returns immediately — but in this case, it returns nothing because we removed the return too. The application hangs or returns an empty response. `call_next` is not optional. It is the handoff. Always include it."

---

### Demo 2 — Logging Middleware

```python
import time
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def log_requests(request: Request, call_next):
    print(f"Incoming: {request.method} {request.url.path}")
    response = await call_next(request)
    print(f"Completed: {response.status_code}")
    return response

@app.get("/cart")
def get_cart():
    return {"cart": ["Laptop", "Mouse"]}
```

### Instructor Note — Prediction Pause

Ask before running: "When we call `GET /cart`, how many print statements will fire? In what order? What will they say?"

Run it.

[Script:]
"Two print statements. First the incoming line fires — that is before `call_next`. The route runs. Then the completed line fires — that is after `call_next`. The order is guaranteed: pre-route code, then route, then post-route code. This is why middleware is perfect for logging. You see both sides of every request."

---

### 5B: Middleware for Request Inspection

[Script:]
"Middleware can also inspect and act on the incoming request itself. In Smart Cart, suppose we want to block all requests that do not include a custom `X-API-Version` header. We want this check to apply globally, not per route."

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse

app = FastAPI()

@app.middleware("http")
async def check_api_version(request: Request, call_next):
    api_version = request.headers.get("X-API-Version")
    if not api_version:
        return JSONResponse(
            status_code=400,
            content={"error": "X-API-Version header required"}
        )
    response = await call_next(request)
    return response
```

[Script:]
"If the header is missing, we return a `JSONResponse` directly from the middleware, and `call_next` is never called. The route never runs. This is how middleware acts as a gatekeeper — it can short-circuit the pipeline and return a response before the route is ever involved."

"If the header is present, we call `call_next` and let the request through normally."

---

### Instructor Note — Pause Point: Middleware Ordering

This is a confusion point for beginners. Address it explicitly.

[Script:]
"When you register multiple middleware functions, they run in the order you register them — but the wrapping is layered. The first middleware registered is the outermost layer. Its pre-route code runs first. Its post-route code runs last. The second middleware registered is the next layer in. Think of it like nesting: if middleware A wraps middleware B, A's pre-route code runs, then B's pre-route code runs, then the route runs, then B's post-route code runs, then A's post-route code runs."

"In practice: if you have logging middleware and version-checking middleware, register logging first so it captures the full picture — including requests that get rejected by version checking."

---

### 5C: Adding Response Headers Globally — CORS as a Familiar Example

[Script:]
"One of the most common real-world uses of middleware in web APIs is adding headers to every response. The most widely known example is CORS — Cross-Origin Resource Sharing — which tells browsers which other origins are allowed to call your API."

"FastAPI includes a built-in `CORSMiddleware` that handles this. Rather than adding CORS headers manually to every route response, you add the middleware once and it handles every response."

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://smartcart.com"],
    allow_methods=["GET", "POST", "DELETE"],
    allow_headers=["Authorization", "Content-Type"],
)

@app.get("/products")
def get_products():
    return {"products": ["Laptop", "Mouse"]}
```

[Script:]
"Notice the second way to add middleware: `app.add_middleware()`. This is used for class-based middleware — pre-built middleware components that accept configuration. `CORSMiddleware` is one. You pass configuration as keyword arguments."

"The `allow_origins` list tells the middleware which domains are allowed. `allow_methods` says which HTTP methods are permitted from those origins. `allow_headers` specifies which request headers are allowed."

"For Smart Cart, our front end lives at `smartcart.com`. Without this middleware, browser-based requests from the front end would be blocked. One configuration line protects the entire API."

---

### Instructor Note — Pause Point: Decorator vs add_middleware

Learners will ask which form to use. Address it directly.

[Script:]
"The `@app.middleware('http')` decorator is for custom middleware you write yourself as a function. `app.add_middleware()` is for class-based middleware, usually pre-built components you import — like `CORSMiddleware`. Both work. Use the decorator for your own logic, use `add_middleware` for third-party or built-in middleware components."

---

### 5D: Combining Dependencies and Middleware

[Script:]
"Now let us bring both tools together in Smart Cart as they would appear in a real application. Middleware handles global concerns — logging and CORS headers. Dependencies handle route-specific concerns — authentication and database access."

```python
import time
from fastapi import FastAPI, Depends, Request, Header, HTTPException
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://smartcart.com"],
    allow_methods=["GET", "POST"],
    allow_headers=["Authorization"],
)

@app.middleware("http")
async def log_and_time(request: Request, call_next):
    start = time.time()
    print(f"Request: {request.method} {request.url.path}")
    response = await call_next(request)
    response.headers["X-Process-Time"] = str(round(time.time() - start, 4))
    return response

def get_current_user(authorization: str = Header(...)):
    if authorization != "Bearer smart-cart-token":
        raise HTTPException(status_code=401, detail="Unauthorized")
    return {"name": "Alice"}

@app.get("/cart")
def get_cart(user: dict = Depends(get_current_user)):
    return {"cart": "Alice's items", "user": user["name"]}
```

[Script:]
"Read through this as a request lifecycle. A request arrives at Smart Cart. CORS middleware runs first — it checks the origin and may add CORS headers to the response. Then our logging and timing middleware runs — it records the start time and logs the path. Then the route is resolved and `get_cart` is about to run. Before it does, FastAPI calls `get_current_user` — the dependency — to authenticate. If authentication fails, a 401 goes back out through the middleware layers. If it passes, `get_cart` runs and returns the cart. The response travels back through the timing middleware, which adds `X-Process-Time`, and then back through CORS middleware, which adds the CORS headers."

"Every request in Smart Cart goes through this entire journey. None of that is duplicated in the route handler. The route handler has one job: get the cart for this user."

---

## Block 6: Lecture Summary

- **Dependency injection is a pattern where functions declare what they need and FastAPI provides it.** Instead of a route function creating its own database connections or running its own authentication checks, it declares dependencies and receives the results. The route function only handles business logic.

- **`Depends()` is FastAPI's mechanism for dependency injection.** You use it as the default value of a route parameter. FastAPI calls the dependency function, resolves any further dependencies it declares, and passes the final result into your route before it runs.

- **The `yield` pattern in dependency functions enables resource lifecycle management.** Code before `yield` runs before the route. The yielded value is injected. Code after `yield` — in a `finally` block — runs after the route completes, even on errors. This is the correct pattern for database connections and other managed resources.

- **Dependencies can be chained.** A dependency can itself declare dependencies using `Depends`. FastAPI resolves the full chain automatically. This lets you build composable, reusable pieces of shared logic that routes can combine as needed.

- **Middleware runs on every request and every response, regardless of which route handles them.** It is registered once and applies globally. It does not require opt-in from individual routes.

- **The `@app.middleware("http")` decorator registers a function as HTTP middleware.** The middleware function receives the `request` and a `call_next` function. Code before `call_next` is request processing. Code after is response processing. `call_next` must always be called or the route will never run.

- **`app.add_middleware()` registers class-based middleware**, typically pre-built components like `CORSMiddleware`, which accept configuration parameters. Both forms work; use the decorator for custom logic and `add_middleware` for configurable components.

- **The rule for choosing between dependencies and middleware:** use a dependency when the logic is route-specific or needs to inject a value into the route function. Use middleware when the logic must run on every request without exception, or when it needs to inspect or modify the raw request or final response at the application level.

- **The practical value:** both tools solve the same underlying problem — repeated code that grows with every new route. Dependency injection centralizes per-route shared logic so that a change to authentication, database access, or any shared concern updates every route at once. Middleware centralizes application-level concerns — logging, timing, security headers, CORS — so they are guaranteed to run on every request without any route needing to know about them. Together, they produce route handlers that contain only business logic, which makes the codebase smaller, more readable, and dramatically easier to change.

---

### Instructor Note — Closing Callback

Return to the Fragment A and Fragment B from the opening. Display or redraw them.

[Script:]
"Look at Fragment B again. You can now account for every line that disappeared from Fragment A. The authentication check lives in `get_current_user`, injected via `Depends`. The database lifecycle lives in `get_db`, also injected via `Depends`. The logging lives in middleware. The route functions contain exactly what they should: the logic that is specific to that route and nothing else. That is the goal. That is what these tools make possible."