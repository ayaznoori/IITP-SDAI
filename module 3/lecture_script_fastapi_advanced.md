# Lecture Script: Advanced FastAPI Features — CORS, Background Tasks, and File Handling
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | CORS — What It Is and How to Configure It | 25 min |
| 3 | Background Tasks | 20 min |
| 4 | File Upload, Download, and Response Handling | 35 min |
| 5 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** Open with a real failure story, not a definition. Learners remember pain before they remember syntax. Give them the hook before anything else. If the group is quiet, ask the question below out loud and wait 10 full seconds for a response.

**[Script:]**

"Let me describe a situation that happens to almost every backend developer at least once.

You have built an API. It works perfectly — you have tested it with curl, Postman, everything looks great. Then a frontend developer on your team opens the browser, calls your API, and gets a red error in the console that says: 'blocked by CORS policy'. Your API is running. Their code is correct. But the browser is refusing to connect.

That is a CORS problem. We are going to understand exactly why it happens today, and how to fix it with three lines of code in FastAPI.

Second situation: your API receives a file upload — a PDF, an image, a CSV — and it needs to do something time-consuming with it. Maybe send a confirmation email, or run a scan. If you do that work inside the request handler, the user sits and waits. Sometimes for ten or twenty seconds. That is a terrible experience. FastAPI gives us background tasks to solve this cleanly.

Third: how do you let users upload and download files through your API? Not just receive a JSON body, but actual binary files. That requires a different set of tools.

These three topics — CORS, background tasks, and file handling — are what separate a toy API from a real one that works in production. By the end of today, all three will be clear and usable."

---

## Block 2 — CORS: Fundamentals and Configuration in FastAPI

### 2A — What Is CORS?

**[Script:]**

"Let us start with the problem CORS is solving, because the solution makes no sense without it.

Browsers enforce something called the Same-Origin Policy. This is a security rule built into every browser. It says: a page loaded from one origin — meaning one combination of scheme, domain, and port — is not allowed to make requests to a different origin unless that other origin explicitly says it is okay.

Origin is the key word. Let me make it concrete."

> 🎯 **Instructor Note:** Write this on the whiteboard as you say it.

```
Frontend:   http://localhost:3000    ← origin A
Backend:    http://localhost:8000    ← origin B
```

**[Script:]**

"These two origins are different. They share the same machine, but the port numbers are different. As far as the browser is concerned, that makes them completely separate origins. So when the React app on port 3000 calls the FastAPI backend on port 8000, the browser blocks the response unless the backend has told the browser: yes, I accept requests from port 3000.

That communication between backend and browser happens through HTTP headers. The browser sends an `Origin` header on the request. The backend must respond with `Access-Control-Allow-Origin`. That exchange is CORS — Cross-Origin Resource Sharing.

Notice that this is entirely a browser mechanism. If you test your API with curl or Postman, there is no browser enforcing this rule. That is why your API works in Postman but breaks in the browser. The API itself is fine. The browser is doing its job."

**Common Mistakes — Say These Out Loud**

> 🎯 **Instructor Note:** Learners make these mistakes consistently. Name them early so learners can pattern-match when they hit them.

**[Script:]**

"Three mistakes that trip up almost everyone the first time.

First: thinking CORS is an API bug. It is not. It is a browser policy. The API is working correctly. You need to add the right headers.

Second: using a wildcard `*` in production to make the error go away. That disables all cross-origin protection. We will talk about how to allow specific origins safely.

Third: forgetting that CORS has a preflight. For some request types — especially anything with a custom header or a non-GET method — the browser sends an OPTIONS request first to ask for permission before sending the real request. If your backend does not handle that OPTIONS request correctly, the whole thing fails even if your main endpoint is fine. FastAPI's middleware handles this automatically, which is one reason to use it properly."

---

### 2B — How to Configure CORS in FastAPI

**[Script:]**

"FastAPI handles CORS through middleware. Middleware in FastAPI is a layer that processes every request before it reaches your route handler and every response before it goes back to the client. Think of it as a wrapper around your whole application.

The CORS middleware is called `CORSMiddleware` and it lives in `fastapi.middleware.cors`. You add it once, and it takes care of all the headers on every response."

> 🎯 **Instructor Note:** Write the import and `add_middleware` call on the board before showing the full demo. Let learners copy the shape before they see all the parameters.

**The key parameters — explain each one before the demo:**

- `allow_origins`: A list of origin strings that are allowed. This is where you put your frontend URL.
- `allow_credentials`: Set to `True` if your frontend sends cookies or auth headers.
- `allow_methods`: Which HTTP methods are allowed. `["*"]` means all of them.
- `allow_headers`: Which request headers the client can send.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before running the demo below, ask the room: "If I add this middleware with `allow_origins=["http://localhost:3000"]` and then make a request from `http://localhost:5000`, what will the browser do?" Wait for answers. Correct answer: the browser will block it, because port 5000 is not in the allow list.

**Demo 1 — CORS Middleware Setup (whiteboard-friendly)**

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

"Run this and call the `/data` endpoint. Look at the response headers in the browser dev tools or in curl with `-v`. You will see `Access-Control-Allow-Origin: http://localhost:3000` in the response. That is the backend telling the browser: requests from this origin are welcome.

Now here is what to notice: `add_middleware` is called on the app object, not inside a route. It wraps the whole application. You call it once, near the top of your file, and every route automatically gets the CORS headers.

If you need to allow multiple origins — say, your local dev environment and your production frontend — just add both to the list."

```python
allow_origins=[
    "http://localhost:3000",
    "https://myapp.com"
]
```

**[Script:]**

"For development only, if you want to allow every origin without restriction, you can write `allow_origins=['*']`. Do not do this in production. In production, always list the specific origins you trust."

> 🎯 **Instructor Note:** Pause here. Ask: "What is the difference between `allow_origins=['*']` and `allow_origins=['http://localhost:3000']` in terms of security?" Let learners reason through it. The answer is specificity — wildcard allows any domain, including potentially hostile ones.

**Recap of CORS before moving on:**

- Same-Origin Policy is a browser security rule, not an API limitation
- CORS headers tell the browser which origins are trusted
- `CORSMiddleware` in FastAPI handles the headers automatically
- List specific origins in production, never use wildcard

---

## Block 3 — Background Tasks

### 3A — What Are Background Tasks and Why Do They Exist?

**[Script:]**

"Imagine a user submits a registration form through your API. Your route handler needs to do two things: save the user to the database, and send a welcome email. Saving to the database is fast — a few milliseconds. Sending an email involves talking to an external email service. That might take one or two seconds, sometimes more.

If you do both inside the route handler, the user waits for the email to finish before they get a response. From their perspective, the signup just felt slow. They have no idea why. That is a bad experience.

The solution is to do the fast thing — save the user — and immediately send the response back to the client. Then, after the response has been sent, do the slow thing — send the email — in the background.

FastAPI gives us `BackgroundTasks` for exactly this. You hand it a function and some arguments, and FastAPI runs that function after the response is returned. The client gets a fast response. The slow work still happens. Nobody waits."

**Mental model:**

> 🎯 **Instructor Note:** Use this analogy out loud. "Think of a restaurant. The waiter takes your order, confirms it immediately, and walks away. Then the kitchen prepares the food. You are not waiting at the counter watching the kitchen work. The confirmation is instant. The actual work happens separately."

---

### 3B — How Background Tasks Work in FastAPI

**[Script:]**

"The API is simple. You add a `BackgroundTasks` parameter to your route function. FastAPI injects it automatically — you do not create it yourself, same way you do not create the `Request` object. Then you call `background_tasks.add_task()` and pass it a function and whatever arguments that function needs.

The function you pass runs after the HTTP response has been sent. It runs in the same process, but after the response. This is important — it is not a separate worker or a queue. It is just deferred execution within the same request lifecycle."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I add a background task that prints to the terminal, and then my route returns a response — in what order will the print and the response happen?" Let learners think. Correct answer: the response goes to the client first, then the print runs. The client never sees the print output.

**Demo 2 — Background Tasks (whiteboard-friendly)**

```python
from fastapi import FastAPI, BackgroundTasks

app = FastAPI()

def write_log(message: str):
    with open("log.txt", "a") as f:
        f.write(message + "\n")

@app.post("/submit")
def submit(background_tasks: BackgroundTasks):
    background_tasks.add_task(write_log, "New submission received")
    return {"status": "submitted"}
```

**[Script:]**

"Run this and call `/submit`. You get `{'status': 'submitted'}` back immediately. After that response is sent, FastAPI runs `write_log` and appends a line to `log.txt`. Open the file — the line is there.

Notice what `add_task` receives: first the function itself, without calling it — so `write_log`, not `write_log()`. Then the arguments to pass to that function. You can pass as many arguments as the function needs.

You can add multiple background tasks in one route — just call `add_task` more than once. They run in the order you added them."

> 🎯 **Instructor Note:** Common confusion point — learners often write `add_task(write_log("some message"))` which calls the function immediately and passes the return value. It should be `add_task(write_log, "some message")`. Emphasize: pass the function, then its arguments separately.

**[Script:]**

"One more thing to be clear about: background tasks in FastAPI are not for heavy computation or things that take minutes. They are for lightweight deferred work — sending a notification, writing a log, updating a counter. For truly heavy workloads, you would use a dedicated task queue like Celery. But for the common case — sending an email, writing a file, calling a webhook — `BackgroundTasks` is exactly the right tool."

**Recap of Background Tasks before moving on:**

- `BackgroundTasks` lets you run functions after the response is sent
- FastAPI injects it automatically as a route parameter
- Call `add_task(function, arg1, arg2)` — pass the function, then its arguments
- Runs in the same process, after the response — not a separate worker
- Best for lightweight deferred work: emails, logs, notifications

---

## Block 4 — File Upload, Download, and Response Handling

### 4A — What Is Different About File Handling?

**[Script:]**

"Everything we have done so far — JSON bodies, path parameters, query strings — works with text data. Files are different. They are binary data, often large, and they travel over HTTP differently from JSON.

When a client uploads a file, it sends it as `multipart/form-data`, not as `application/json`. This is a different encoding that lets the HTTP request contain both text fields and file data at the same time. FastAPI handles this with two special types: `File` and `UploadFile`."

**[Script:]**

"There are two ways to receive a file in FastAPI. The first is `File(...)` with `bytes` — the whole file is read into memory as a Python bytes object. This is simple but dangerous for large files, because the entire file sits in RAM.

The second is `UploadFile` — this is an object that wraps the file and gives you a file-like interface with async methods. It stores the file temporarily rather than loading everything into memory at once. For anything beyond tiny toy files, you should use `UploadFile`."

> 🎯 **Instructor Note:** Write this comparison on the board:

```
bytes / File(...)    → whole file in memory → simple, dangerous for large files
UploadFile           → file-like object     → efficient, use this in practice
```

---

### 4B — Receiving File Uploads

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask the room: "If I upload a file called `report.pdf` through this endpoint, what information will I be able to access on the `UploadFile` object?" Let learners guess. Then show them the attributes.

**[Script:]**

"An `UploadFile` object gives you three useful attributes without reading any data: `filename` is the original name of the file as the client sent it. `content_type` is the MIME type — `image/jpeg`, `application/pdf`, and so on. And `file` is the actual file-like object if you need low-level access.

To actually read the file content, you call `await file.read()`. This is an async operation, which is why it uses `await`. That means your route function must be defined with `async def`."

**Demo 3 — File Upload (whiteboard-friendly)**

```python
from fastapi import FastAPI, File, UploadFile

app = FastAPI()

@app.post("/upload")
async def upload_file(file: UploadFile = File(...)):
    content = await file.read()
    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "size": len(content)
    }
```

**[Script:]**

"Call this endpoint with a file. You can use curl:

```
curl -X POST 'http://localhost:8000/upload' -F 'file=@yourfile.txt'
```

The response tells you the filename, the content type FastAPI detected, and the size in bytes. Notice `File(...)` in the parameter default — the `...` means the file is required. If no file is sent, FastAPI returns a 422 validation error automatically.

Now, one important thing: after you call `await file.read()`, if you call it again you get an empty bytes object — the file cursor is at the end. If you need to read the file more than once, call `await file.seek(0)` first to reset the cursor back to the beginning."

> 🎯 **Instructor Note:** Pause here — this is a real confusion point. Ask: "If I call `await file.read()` twice without seeking, what will the second call return?" Answer: empty bytes, `b""`. The file has already been consumed.

---

### 4C — Saving Uploaded Files to Disk

**[Script:]**

"Reading the content into memory is fine for small files or when you need to process the bytes directly. But if you want to save the file to disk, write it in chunks rather than reading it all into memory first. Here is the pattern."

```python
import shutil
from fastapi import FastAPI, UploadFile, File

app = FastAPI()

@app.post("/save")
async def save_file(file: UploadFile = File(...)):
    with open(f"uploads/{file.filename}", "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)
    return {"saved": file.filename}
```

**[Script:]**

"`shutil.copyfileobj` copies from the file object to the buffer in chunks. It does not load the whole file into memory at once. This is the correct approach for files of unknown or potentially large size. The `file.file` attribute is the underlying SpooledTemporaryFile — a standard Python file-like object — which is why we can use `shutil.copyfileobj` on it directly."

> 🎯 **Instructor Note:** Make sure learners understand the directory `uploads/` must exist before running this. They will get a `FileNotFoundError` otherwise. Mention this explicitly.

---

### 4D — Sending Files Back: File Download and Response Handling

**[Script:]**

"So far we have received files. Now let us send them. There are two main response types for returning files from FastAPI: `FileResponse` and `StreamingResponse`.

`FileResponse` is for sending a file that exists on disk. You give it a file path and FastAPI streams the file back to the client efficiently. It handles content type detection, content length headers, and range requests.

`StreamingResponse` is for when the content does not exist as a file on disk — maybe you are generating it on the fly, or reading from an in-memory bytes object. You give it an iterable or a generator and it streams the data."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I return a `FileResponse` pointing to a file called `report.pdf`, what will a browser do with it — display it or download it?" Answer: it depends on the `media_type` and whether you add a `Content-Disposition` header. Without disposition, browsers often try to display PDFs inline. With `Content-Disposition: attachment`, the browser triggers a download. Show them how to force a download.

**Demo 4 — FileResponse and StreamingResponse (whiteboard-friendly)**

```python
from fastapi import FastAPI
from fastapi.responses import FileResponse, StreamingResponse
import io

app = FastAPI()

@app.get("/download")
def download_file():
    return FileResponse(
        path="uploads/report.pdf",
        media_type="application/pdf",
        filename="report.pdf"
    )

@app.get("/stream")
def stream_data():
    data = b"col1,col2\nval1,val2\n"
    return StreamingResponse(io.BytesIO(data), media_type="text/csv")
```

**[Script:]**

"In the `FileResponse`, the `filename` parameter sets the `Content-Disposition` header, which tells the browser to download the file with that name rather than display it inline. The `media_type` tells the browser what kind of file it is.

In the `StreamingResponse`, we wrap a `bytes` object in `io.BytesIO` to make it a file-like iterable. FastAPI streams it to the client. You could replace `io.BytesIO(data)` with a generator that yields chunks, which is the right approach for truly large dynamic content.

The key mental model: `FileResponse` for files on disk, `StreamingResponse` for generated or in-memory content."

---

### 4E — Combining Background Tasks and File Handling

**[Script:]**

"These features compose naturally. A common real-world pattern is: accept a file upload, save it to disk, return an immediate response to the client, and then kick off a background task to process the file — scan it, extract data from it, send a notification.

You already have all the pieces. `UploadFile` to receive the file, `shutil.copyfileobj` to save it, `BackgroundTasks.add_task` to queue the processing."

```python
from fastapi import FastAPI, UploadFile, File, BackgroundTasks
import shutil

app = FastAPI()

def process_file(filename: str):
    print(f"Processing {filename}")

@app.post("/upload-and-process")
async def upload_and_process(
    file: UploadFile = File(...),
    background_tasks: BackgroundTasks = BackgroundTasks()
):
    path = f"uploads/{file.filename}"
    with open(path, "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)
    background_tasks.add_task(process_file, file.filename)
    return {"queued": file.filename}
```

> 🎯 **Instructor Note:** Do not dwell on the combined demo too long. The goal is for learners to see that the features are composable, not to introduce new syntax. Point out that `BackgroundTasks` here is injected as a parameter with a default — FastAPI handles injection when it is declared as a route parameter.

---

## Block 5 — Lecture Summary

> 🎯 **Instructor Note:** Deliver these bullets conversationally, not as a reading exercise. Ask learners to supply the answers before you say them. "What does CORS stand for? What triggers a preflight request?" This is recall practice, not just review.

**CORS — Fundamentals and Configuration**

- The Same-Origin Policy is a browser security rule that blocks cross-origin HTTP requests by default
- CORS is the mechanism that allows a backend to grant permission to specific origins
- An origin is defined by scheme, domain, and port — two URLs with different ports are different origins
- `CORSMiddleware` is added once to the FastAPI app with `app.add_middleware` and wraps every route
- `allow_origins` takes a list of trusted origin strings; use `["*"]` only in development
- The browser sends a preflight `OPTIONS` request for complex requests — FastAPI's middleware handles this automatically
- In production, always list specific origins, never use a wildcard

**Background Tasks**

- `BackgroundTasks` lets you run a function after the HTTP response has been sent to the client
- FastAPI injects it automatically when declared as a route parameter
- Call `background_tasks.add_task(function, arg1, arg2)` — pass the function reference, not a function call
- Tasks run in the same process after the response, in the order they were added
- Use for lightweight deferred work: logging, notifications, emails — not for heavy computation

**File Upload, Download, and Response Handling**

- Files are sent as `multipart/form-data`, not JSON — use `File` and `UploadFile` to receive them
- `UploadFile` is preferred over raw `bytes`: it does not load the entire file into memory
- `await file.read()` reads the content; call `await file.seek(0)` before reading again
- Use `shutil.copyfileobj(file.file, buffer)` to save files to disk without loading everything into memory
- `FileResponse` returns a file that exists on disk; use the `filename` parameter to trigger a browser download
- `StreamingResponse` returns generated or in-memory content as a stream; wrap bytes in `io.BytesIO` or use a generator

**Why These Three Features Work Together**

- Real-world APIs do not exist in isolation — they serve browser frontends, handle binary data, and perform work that should not block the client
- CORS lets your frontend talk to your backend across origins without security errors
- Background tasks let you respond immediately while slow work finishes after the response
- File handling extends your API beyond JSON to receive and serve documents, images, and data files
- Together, these three capabilities account for the majority of the practical requirements in a production-facing FastAPI backend

---

*End of script.*
