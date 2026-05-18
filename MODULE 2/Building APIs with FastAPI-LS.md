# Lecture Script: Building APIs with FastAPI

**Duration:** 130 minutes
**Audience:** Beginner learners
**Running Example:** TaskFlow API — a backend service that manages a list of tasks

---

## Facilitator Overview

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | Request and Response Handling | 20 min |
| 3 | HTTP Methods: GET, POST, PUT, DELETE | 30 min |
| 4 | Path Parameters, Query Parameters, and Request Body | 25 min |
| 5 | Validation, Pydantic, and Response Models | 25 min |
| 6 | OpenAPI, Swagger UI, and Auto Documentation | 10 min |
| 7 | Lecture Summary | 10 min |

**Running Example — TaskFlow API**
A task management backend. It stores a list of tasks, each with an id, title, and completion status. Every demo, walkthrough, and code snippet operates on this same in-memory list. Learners watch one consistent application grow from a single GET endpoint to a fully documented, validated API with all four HTTP methods.

---

## Block 1 — Why Does This Matter?

**Time: 10 minutes**

---

> 🎯 **Instructor Note — Opening Hook**
> Before speaking, open any project management tool in the browser — Trello or a to-do app works well. Create a task. Check it off. Delete it. Ask the room: "Every one of those actions — create, update, delete — sent a different kind of request to a backend. They were not all the same request. Why does the type of request matter? What would break if everything used the same kind?"
> Pause. Let answers surface. Do not resolve the question yet. Begin the script.

---

**[Script:]**

"What you just watched was four different HTTP methods in action. Create a task — POST. Read the list — GET. Check it off — PUT. Delete it — DELETE."

"Each method signals a different *intention*. The server reads the method and knows immediately what kind of work to do before it reads anything else."

"This is not a convention someone invented arbitrarily. It is a contract. Frontend developers, mobile developers, and third-party integrators all rely on this contract to know how to communicate with your backend."

"If you ignore it — if you use GET for everything — your API becomes unpredictable. Caches behave incorrectly. Security tools misfire. Other developers cannot reason about what your endpoints do. You become the person who wrote the confusing API that nobody wants to work with."

"Beyond methods, there is another problem. Data arrives in different shapes. Sometimes it is in the URL. Sometimes it is in the request body. Sometimes it is malformed — missing a required field, wrong data type, invalid value. Your backend needs to handle all of that cleanly."

"Today you build a backend that does all of this correctly. Handles every method. Accepts data in every form. Validates it. Documents itself automatically."

---

> 🎯 **Instructor Note**
> Write on the whiteboard and keep it visible:
> `GET = read | POST = create | PUT = update | DELETE = remove`
> Return to this every time a new method is introduced.

---

## Block 2 — Request and Response Handling

**Time: 20 minutes**

---

### What Is a Request? What Is a Response?

---

**[Script:]**

"Every interaction with a backend follows the same pattern. A client sends a request. The server does work. The server sends back a response. That exchange is the foundation of every API you will ever build."

"Let's make both sides concrete."

"A **request** is a structured message sent to the server. It has four parts."

Write on whiteboard:

```
Method   — the action (GET, POST, PUT, DELETE)
URL      — the address to send it to
Headers  — metadata about the request
Body     — data included with the request (not used in GET)
```

"A **response** is the server's reply. It also has structure."

Write on whiteboard:

```
Status Code  — a number that signals success or failure
Headers      — metadata about the response
Body         — the data being returned (usually JSON)
```

"These two structures are not FastAPI-specific. They are part of HTTP — the protocol the entire web runs on. FastAPI helps you build the server side of this exchange cleanly."

---

> 🎯 **Instructor Note — Mental Model**
> Use this analogy: "Think of a formal letter exchange. You write a letter — that is the request. You address it — that is the URL. You say whether you are asking a question or submitting a form — that is the method. The reply comes back with a header saying whether it was received successfully — that is the status code. The content of the reply is the response body."

---

### Status Codes — The Signal System

---

**[Script:]**

"Status codes are the first thing you read in any response. Before you look at the data, the status code tells you whether the request succeeded."

"Learn four ranges."

Write on whiteboard:

```
2xx — Success       (200 OK, 201 Created)
3xx — Redirect      (not common in APIs, skip for now)
4xx — Client Error  (400 Bad Request, 404 Not Found, 422 Unprocessable)
5xx — Server Error  (500 Internal Server Error)
```

"4xx means the problem is in the request — wrong data, wrong address, missing field. The server received the request and rejected it."

"5xx means the problem is in the server — your code broke. The request was valid, but something went wrong on the backend."

"This distinction matters when debugging. 4xx — fix the request. 5xx — fix the code."

---

> 🎯 **Instructor Note — Pause Point**
> Ask: "If you send a request with a missing required field and the server returns 422, whose fault is it — the client or the server?" Take answers. Confirm: 422 is a client error. The request was malformed. The server did exactly the right thing by rejecting it.

---

### TaskFlow Setup

---

**[Script:]**

"Here is the starting point for TaskFlow. Open `main.py`. We set up the application and create a small in-memory data store."

Write on whiteboard:

```python
from fastapi import FastAPI

app = FastAPI()

tasks = [
    {"id": 1, "title": "Buy groceries", "done": False},
    {"id": 2, "title": "Read a book", "done": False},
]
```

"Two tasks. Each has an id, a title, and a done status. This list lives in memory — it resets when the server restarts. That is fine for today. We are focused on the API layer, not persistence."

"Run the server."

```bash
uvicorn main:app --reload
```

"The foundation is running. Now we build on it."

---

## Block 3 — HTTP Methods: GET, POST, PUT, DELETE

**Time: 30 minutes**

---

### 3a — GET: Reading Data

---

**[Script:]**

"GET is the simplest method. It reads data. It does not change anything on the server. That word — *idempotent* — means you can call it ten times and the result is the same every time. Nothing changes."

"GET requests have no body. All the information they need is in the URL."

---

**[Script:]**

"Two GET endpoints for TaskFlow. One returns all tasks. One returns a single task by id."

Write on whiteboard:

```python
@app.get("/tasks")
def get_tasks():
    return tasks

@app.get("/tasks/{task_id}")
def get_task(task_id: int):
    for task in tasks:
        if task["id"] == task_id:
            return task
```

"Predict before running: if I request `GET /tasks`, what comes back? If I request `GET /tasks/1`, what comes back?"

**Pause. Take predictions.**

"Run both. `/tasks` returns the full list. `/tasks/1` returns the first task only."

"Notice `{task_id}` in the decorator. That is a path parameter — a variable embedded in the URL. FastAPI extracts it and passes it into the function as the argument `task_id`. The `: int` tells FastAPI to expect an integer and validate it automatically."

---

> 🎯 **Instructor Note — Common Mistake**
> Learners often write the path parameter name differently in the decorator versus the function argument — `{taskId}` in the decorator and `task_id` in the function, for example. FastAPI will raise an error because it cannot match them. The names must be identical. Write this on the whiteboard: decorator name = function argument name.

---

### 3b — POST: Creating Data

---

**[Script:]**

"POST is how you create something new. The client sends data in the request body. The server uses that data to create a new resource."

"Unlike GET, POST carries a body. That body contains the information needed to create the new item."

"POST is not idempotent. Call it twice with the same data and you get two new tasks."

---

**[Script:]**

"For TaskFlow, a POST to `/tasks` should create a new task. The client sends a title. The server assigns an id, sets `done` to False, adds it to the list, and returns the new task."

Write on whiteboard:

```python
@app.post("/tasks", status_code=201)
def create_task(task: dict):
    new_task = {
        "id": len(tasks) + 1,
        "title": task["title"],
        "done": False,
    }
    tasks.append(new_task)
    return new_task
```

"Predict before running: if I POST to `/tasks` with the body `{"title": "Write tests"}`, what comes back?"

**Pause. Take predictions.**

"Run it. The server returns the new task with an assigned id and `done: False`. Status code 201 — Created. Not 200. 201 specifically signals that something new was created. That distinction matters."

---

> 🎯 **Instructor Note**
> Point to `status_code=201` in the decorator. Say: "FastAPI defaults to 200 for all responses. We override it here with 201 to be semantically accurate. Clients reading status codes will know exactly what happened. This is part of building an API others can trust."

---

> 🎯 **Instructor Note — Common Mistake**
> Using `task: dict` as the parameter type works but has no validation. Any dictionary passes through — even one with missing or wrong fields. We will fix this in Block 5 using Pydantic. Plant the problem here so the solution lands with purpose. Say: "This works today. But notice — there is nothing stopping someone from sending a body with no title at all. We will address that shortly."

---

### 3c — PUT: Updating Data

---

**[Script:]**

"PUT is used to update an existing resource. The client sends the updated data. The server finds the resource and replaces its values."

"PUT is idempotent. Sending the same update twice produces the same result — the resource ends up in the same final state."

---

**[Script:]**

"For TaskFlow, a PUT to `/tasks/{task_id}` should update an existing task's title or done status."

Write on whiteboard:

```python
@app.put("/tasks/{task_id}")
def update_task(task_id: int, updates: dict):
    for task in tasks:
        if task["id"] == task_id:
            task.update(updates)
            return task
```

"Predict before running: if I PUT to `/tasks/1` with body `{"done": true}`, what changes? What comes back?"

**Pause. Take predictions.**

"Run it. The first task now has `done: true`. The server found it by id, applied the update, and returned the modified task."

---

> 🎯 **Instructor Note — Pause Point**
> Ask: "What happens if I send a PUT request for a task id that does not exist?" Let learners think. The current code returns `None` silently — no error, no message. That is bad API design. Acknowledge it. Say: "We will handle missing resources properly as we improve this code. For now, see the problem. A real API should return a 404 and a clear message." Do not implement error handling now — stay on the method concepts.

---

### 3d — DELETE: Removing Data

---

**[Script:]**

"DELETE removes a resource. The client specifies which resource via the URL. The server removes it and confirms."

"DELETE is also idempotent in outcome — deleting something that is already gone leaves the system in the same state."

---

**[Script:]**

"For TaskFlow, a DELETE to `/tasks/{task_id}` removes the matching task."

Write on whiteboard:

```python
@app.delete("/tasks/{task_id}", status_code=200)
def delete_task(task_id: int):
    for i, task in enumerate(tasks):
        if task["id"] == task_id:
            tasks.pop(i)
            return {"message": "Task deleted"}
```

"Predict before running: if I DELETE `/tasks/2`, what happens to the list?"

**Pause. Take predictions.**

"Run it. Then call `GET /tasks`. Task 2 is gone. The server found it, removed it from the list, and returned a confirmation message."

---

> 🎯 **Instructor Note**
> Return to the whiteboard entry from the opening: `GET = read | POST = create | PUT = update | DELETE = remove`. Point to each one. Ask the room to name the endpoint we built for each. This takes sixty seconds and cements the full picture before moving on.

---

## Block 4 — Path Parameters, Query Parameters, and Request Body

**Time: 25 minutes**

---

### 4a — Path Parameters

---

**[Script:]**

"You have already used path parameters. Let's name them precisely."

"A **path parameter** is a variable embedded directly inside the URL path. It identifies a specific resource."

Write on whiteboard:

```
/tasks/1    →   task_id = 1
/tasks/5    →   task_id = 5
```

"The curly braces in the decorator tell FastAPI: this segment of the URL is a variable. Extract it. Validate its type. Pass it to the function."

"Path parameters are used when the value is essential to identify *which* resource you are working with. Deleting task 3 requires knowing it is task 3 — that goes in the path."

---

### 4b — Query Parameters

---

**[Script:]**

"A **query parameter** is appended to the URL after a question mark. It is optional additional information — a filter, a search term, a pagination value."

Write on whiteboard:

```
/tasks?done=false
/tasks?done=true
```

"In FastAPI, if you add a function argument that does not appear in the path decorator, FastAPI automatically treats it as a query parameter."

Write on whiteboard:

```python
@app.get("/tasks")
def get_tasks(done: bool = None):
    if done is None:
        return tasks
    return [t for t in tasks if t["done"] == done]
```

"Predict before running: what does `GET /tasks?done=false` return? What does `GET /tasks` return?"

**Pause. Take predictions.**

"Run both. Without the query parameter — all tasks. With `done=false` — only incomplete tasks."

"The `= None` default makes it optional. If the client does not include it, `done` is None and all tasks are returned. If the client includes it, the list is filtered."

---

> 🎯 **Instructor Note — Common Mistake**
> Learners sometimes confuse when to use a path parameter versus a query parameter. Use this rule of thumb and write it on the whiteboard:
> `Path parameter = identifies a resource | Query parameter = filters or modifies a collection`
> Fetching one specific task — path parameter. Filtering tasks by status — query parameter.

---

### 4c — Request Body

---

**[Script:]**

"A **request body** carries data from the client to the server inside the HTTP request. GET requests have no body. POST and PUT requests do."

"In FastAPI, you accept a request body by declaring a function parameter with a Pydantic model as its type. We will build the model properly in the next block. For now, understand the concept: the body is structured data, and FastAPI reads it from the incoming request and makes it available as a Python object."

"Right now we are using `dict` as a shortcut. That works but offers no validation. The improvement comes next."

---

> 🎯 **Instructor Note — Transition**
> This is a deliberate bridge moment. Say: "We have been accepting `dict` for our POST and PUT bodies. That is fragile. Nothing stops a client from sending an empty body or a body with wrong field names. Let's fix that now with proper validation."

---

## Block 5 — Validation, Pydantic, and Response Models

**Time: 25 minutes**

---

### 5a — Why Validation Matters

---

**[Script:]**

"Here is the problem. A client sends a POST request to create a task. They forget to include the title field. Your code tries to read `task["title"]` — it does not exist — and the server crashes with a 500 error."

"A 500 is a server error. But the problem was in the request. The client sent bad data. Your server should have caught that, rejected it with a 422, and returned a clear message explaining what was wrong."

"**Data validation** means checking that incoming data is the right shape, the right type, and contains the required fields — before your logic ever touches it."

"FastAPI has a validation system built in. It is powered by a library called **Pydantic**."

---

### 5b — Pydantic Models

---

**[Script:]**

"**Pydantic** is a Python library for defining data shapes using classes. You declare a class that inherits from `BaseModel`. You add attributes with type annotations. Pydantic enforces them automatically."

Write on whiteboard:

```python
from pydantic import BaseModel

class TaskCreate(BaseModel):
    title: str
    done: bool = False
```

"Read this: a `TaskCreate` must have a `title` that is a string. It may optionally have a `done` value — a boolean — that defaults to False if not provided."

"If a client sends a request body that is missing `title`, or sends `title` as an integer, Pydantic rejects it instantly and FastAPI returns a 422 with a detailed error message explaining exactly what failed. Your code never runs."

---

> 🎯 **Instructor Note**
> Say this clearly and repeat it: "Validation happens before your function body executes. If the data is wrong, your code does not run at all. FastAPI handles the rejection for you. You write the model. FastAPI and Pydantic do the checking."

---

### Demo 1 — Replacing dict with a Pydantic Model

> 🎯 **Instructor Note**
> Update `main.py` live. Replace the `task: dict` parameter in `create_task` with the Pydantic model. Show the difference in behaviour — valid request, then deliberately invalid request.

**[Script:]**

"Let's replace `dict` in our POST endpoint with the Pydantic model."

```python
@app.post("/tasks", status_code=201)
def create_task(task: TaskCreate):
    new_task = {
        "id": len(tasks) + 1,
        "title": task.title,
        "done": task.done,
    }
    tasks.append(new_task)
    return new_task
```

"Notice the change. `task: TaskCreate` instead of `task: dict`. And `task.title` instead of `task["title"]`. Pydantic models are accessed with dot notation, not dictionary brackets."

"Predict before running: what happens if I POST with a valid body `{"title": "Write tests"}`? What happens if I POST with an empty body `{}`?"

**Pause. Take predictions.**

"Run both. Valid body — 201, new task returned. Empty body — 422, detailed error message telling us exactly which field is missing and why."

"We did not write that error message. FastAPI and Pydantic generated it from our model definition."

---

> 🎯 **Instructor Note — Pause Point**
> Point to the 422 error body in the response. Read it aloud with the room. It will say something like: `field required` for `title`. Ask: "If you were a frontend developer receiving this error, would you know exactly what to fix?" Yes. That is the value of proper validation. The API is communicating clearly.

---

### 5c — Response Models

---

**[Script:]**

"So far we return whatever our function returns — the full task dictionary. But in a real API, you often want to control exactly what the response contains. Maybe some fields should not be returned. Maybe you want to guarantee the response shape."

"FastAPI lets you declare a **response model** — a Pydantic model that defines the shape of the response. FastAPI filters the return value through that model before sending it."

Write on whiteboard:

```python
class TaskResponse(BaseModel):
    id: int
    title: str
    done: bool

@app.get("/tasks/{task_id}", response_model=TaskResponse)
def get_task(task_id: int):
    for task in tasks:
        if task["id"] == task_id:
            return task
```

"Even if the internal task dictionary had extra fields — timestamps, internal flags — the response would only include `id`, `title`, and `done`. The response model acts as a filter and a guarantee."

---

> 🎯 **Instructor Note — Common Mistake**
> Learners sometimes define a response model and then try to return a Pydantic model object where FastAPI expects a dictionary, or vice versa. FastAPI is flexible here — it can serialise both. But mixing them inconsistently across an API creates confusion. Establish the pattern early: return dictionaries or Pydantic model instances, and let FastAPI handle serialisation.

---

### Demo 2 — Applying a Response Model

> 🎯 **Instructor Note**
> Add the `TaskResponse` model and apply it to the GET endpoints. Demonstrate that the response shape is now guaranteed. Then briefly show what happens in Swagger UI — the response schema is now visible in the documentation automatically.

**[Script:]**

"Predict before running: if I apply `response_model=TaskResponse` to `GET /tasks/{task_id}`, what does the response look like?"

**Pause. Take predictions.**

"Run it. Clean response. Only `id`, `title`, and `done`. Nothing extra. Shape guaranteed."

"Now open `http://127.0.0.1:8000/docs`. Find the GET `/tasks/{task_id}` endpoint. Look at the response schema section. FastAPI read our `TaskResponse` model and documented the exact shape of the response automatically."

"We declared the model once. We got validation, serialisation control, and documentation from that one declaration."

---

## Block 6 — OpenAPI, Swagger UI, and Automatic Documentation

**Time: 10 minutes**

---

### What Is OpenAPI?

---

**[Script:]**

"**OpenAPI** is a standard format for describing APIs. It defines a specification — a structured document that lists every endpoint, every parameter, every request body, every response shape, and every status code your API supports."

"This document is machine-readable. Tools can consume it to generate documentation, generate client code, run tests, or validate requests."

"FastAPI generates an OpenAPI specification automatically from your code. Every decorator, every Pydantic model, every type annotation — FastAPI reads all of it and builds the spec without you writing a single line of documentation manually."

---

### Swagger UI

---

**[Script:]**

"**Swagger UI** is a browser-based interface that reads an OpenAPI specification and renders it as interactive documentation. FastAPI includes it by default at `/docs`."

"Open `http://127.0.0.1:8000/docs`."

"Every endpoint is listed. Expand any one of them. You see the method, the path, the parameters, the expected request body shape, and the possible response codes. You can send a real request directly from this page using the Try it out button."

"This is not a separate documentation task. It is generated from the code you already wrote."

---

> 🎯 **Instructor Note — Live Walkthrough**
> Walk through the Swagger UI live. Expand the POST `/tasks` endpoint. Show the request body schema generated from `TaskCreate`. Click Try it out. Send a valid request. Send an invalid one. Show the 422 response. Do this slowly — many learners will be genuinely surprised that this comes for free.

---

**[Script:]**

"There is also a second documentation interface at `/redoc`. Same data, different layout. More readable for sharing with non-developers. Same source — the OpenAPI spec."

"Two documentation views, zero documentation effort. That is a direct payoff from writing typed, validated FastAPI code."

---

> 🎯 **Instructor Note — Common Mistake**
> Learners sometimes assume Swagger UI is a testing tool only and skip reading it carefully. It is also the clearest view of whether their API is structured correctly. If an endpoint looks wrong in Swagger UI — missing parameters, incorrect types — that is a signal the code has a bug. Treat the docs as a diagnostic tool, not just a convenience.

---

## Block 7 — Lecture Summary

**Time: 10 minutes**

---

> 🎯 **Instructor Note — Delivery**
> Speak each bullet as a confident statement. Pause after each one. Ask "Does that match what we built?" before moving to the next. The goal is consolidation, not new information. If a learner looks uncertain at any bullet, spend thirty seconds on it before continuing.

---

**[Script:]**

"Let's close by naming exactly what we covered and what we built."

- Every HTTP interaction follows the same structure — a request carrying a method, URL, optional headers, and optional body; a response carrying a status code, headers, and a body

- Status codes signal the outcome of every request: 2xx means success, 4xx means the client sent something wrong, and 5xx means the server code broke — the range tells you immediately where to look when something fails

- GET retrieves data without changing anything; POST creates a new resource; PUT updates an existing one; DELETE removes it — each method signals a clear and different intent to the server and to every client that reads the response

- Path parameters are embedded in the URL path and identify a specific resource; query parameters follow a question mark and filter or modify a collection; request body carries structured data for POST and PUT operations — choosing the right one depends on what role the data plays

- Pydantic models define the expected shape and types of incoming data; declaring a function parameter with a Pydantic type activates automatic validation so that malformed requests are rejected with a 422 before any application logic runs

- Response models declared in the `response_model` argument of a route decorator control exactly what fields are included in the response, filter out internal data, and guarantee the shape the client receives

- FastAPI generates an OpenAPI specification automatically from decorators, type annotations, and Pydantic models — no manual documentation is required

- Swagger UI at `/docs` renders that specification as interactive documentation where endpoints can be explored and tested directly in the browser

- Understanding how to structure requests and responses correctly, validate data at the boundary, and document an API automatically is the difference between a backend that is reliable and trustworthy and one that breaks unpredictably and requires constant explanation to the people trying to use it

---

> 🎯 **Instructor Note — Closing Questions**
> Ask two questions. First: "Name the four HTTP methods and one word for what each one does." Cold call four different learners — one per method. Second: "What happens in FastAPI when a POST request arrives with a missing required field?" Listen for: Pydantic rejects it, FastAPI returns 422, your function code never runs. These two questions together confirm that the core API structure and the validation model both landed. End on those answers.

---

**[Script — Close:]**

"You now have a TaskFlow API with all four HTTP methods, parameter handling in three forms, automatic validation, and self-generating documentation. Every pattern we used today is the same pattern professionals use in production. The scale changes. The concepts do not."

---

*End of Lecture Script*
