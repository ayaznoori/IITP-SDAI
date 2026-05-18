# Lecture Script: Introduction to Backend Development and FastAPI Fundamentals

**Duration:** 130 minutes
**Audience:** Absolute beginners
**Running Example:** BookShelf API — a backend service that manages a list of books

---

## Facilitator Overview

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 15 min |
| 2 | Backend Fundamentals | 30 min |
| 3 | FastAPI Basics | 35 min |
| 4 | API Testing with Postman | 25 min |
| 5 | Lecture Summary | 15 min |

**Running Example — BookShelf API**
A backend service for a bookshelf application. It stores a list of books and responds to requests for that data. Every demo, walkthrough, and code snippet uses this same project. Learners see one consistent codebase grow from an empty folder to a working, testable API.

---

## Block 1 — Why Does This Matter?

**Time: 15 minutes**

---

> 🎯 **Instructor Note — Opening Hook**
> Before speaking, open a browser. Go to any book retailer — Goodreads works well. Search for a book. The results appear almost instantly. Ask the room: "You typed a title. Results came back. Where did those results come from? There is no book data inside your browser. So where is it?"
> Pause. Let answers come. Do not confirm or correct yet. Hold the tension. Then begin the script.

---

**[Script:]**

"The data came from somewhere else entirely. A server. A program running on a machine you will never see, waiting for requests, finding data, and sending it back."

"That program is the backend. And today you start building one."

"Every application you use has two sides. The frontend is what you see and click. The backend is everything that actually stores, retrieves, and processes information."

"Think about logging into any account. You type your email and password. The frontend sends that to a backend. The backend checks it against a database. The backend decides: valid or not. The backend sends a response. The frontend shows you the result."

"None of that works without a backend. The frontend is just a screen without one."

"Here is the pain of not understanding this: you will build beautiful frontends that cannot do anything real. No login. No saved data. No communication between users. A website that forgets everything the moment you close the tab."

"Backend development is what makes software *persistent*, *shareable*, and *useful*."

---

> 🎯 **Instructor Note**
> Write this on the whiteboard now and keep it visible the entire session:
> `Frontend = what the user sees | Backend = where the work happens`
> Return to it whenever the distinction matters.

---

## Block 2 — Backend Fundamentals

**Time: 30 minutes**

---

### 2a — What Is Backend Development?

---

**[Script:]**

"Let's get precise. Backend development means building the part of an application that runs on a server, not in a browser."

"The backend has three jobs. It receives requests. It does some work — reading data, writing data, running logic. It sends back a response."

"That is it. Request in. Work done. Response out."

"The way a frontend and backend talk to each other is through an API — an Application Programming Interface. You will hear this word constantly. For now, think of an API as a set of agreed-upon addresses. The frontend knows: if I send a request to this address, I get back book data. The backend knows: if someone requests that address, I send book data."

"We are going to build that backend today. From scratch."

---

> 🎯 **Instructor Note — Mental Model**
> Use this analogy and say it clearly:
> "Think of a restaurant. The customer is the frontend. The kitchen is the backend. The menu is the API — it lists what you can ask for. The waiter carries requests and responses between them. You never walk into the kitchen. You use the menu."
> Draw this on the whiteboard if the room looks uncertain.

---

### 2b — Project Setup: Virtual Environment and pip

---

**[Script:]**

"Before we write a single line of FastAPI code, we need to set up our project properly. This is not optional. This is how professional Python projects are built."

"Here is the problem we are solving. Your computer may have Python installed. It may already have packages installed on it. If every project you build shares those same packages, things break. Version conflicts. One project needs version 1 of a library. Another needs version 2. They cannot both win."

"The solution is a **virtual environment** — shortened to `venv`."

"A virtual environment is an isolated Python installation just for one project. It has its own packages, its own version records. What happens inside it does not affect anything outside it."

---

> 🎯 **Instructor Note**
> Use this analogy: "Think of a virtual environment like a lunchbox. Everything your project needs goes in the lunchbox. You take the lunchbox to work. Your colleague has their own lunchbox. What is in yours does not mix with what is in theirs."

---

**[Script:]**

"Here is how we create one. In your terminal, inside your project folder:"

Write on whiteboard:

```bash
python -m venv venv
```

"This creates a folder called `venv` inside your project. That folder contains an isolated Python installation."

"Now activate it."

On Mac or Linux:

```bash
source venv/bin/activate
```

On Windows:

```bash
venv\Scripts\activate
```

"When it is active, you will see `(venv)` appear at the start of your terminal prompt. That is the signal. You are now working inside the environment."

"Everything you install now goes into that environment and nowhere else."

---

> 🎯 **Instructor Note — Common Mistake**
> Learners frequently forget to activate the environment before installing packages. They install FastAPI globally by accident, then wonder why it is missing inside their project. Make this explicit: "If you do not see `(venv)` in your prompt, stop. Activate first. Then install."

---

**[Script:]**

"Now let's talk about **pip**. pip is Python's package manager. It is how you install libraries that other people have written."

"We need FastAPI and one more package called `uvicorn`. Uvicorn is the server that actually runs our FastAPI application."

Write on whiteboard:

```bash
pip install fastapi uvicorn
```

"Run that with your environment active. pip downloads both packages and makes them available inside your project."

---

### 2c — Environment Variables

---

**[Script:]**

"One more concept before we write application code. **Environment variables**."

"An environment variable is a piece of configuration that lives outside your code. It is stored on the machine or in a special file, not inside your Python files."

"Why does this matter? Imagine your backend needs to connect to a database. That connection requires a password. If you write the password directly into your Python file and push that code to GitHub, the password is now public. Anyone can read it."

"Environment variables solve this. The sensitive value lives outside the code. The code reads it at runtime."

"The standard approach is a file called `.env`. You put key-value pairs in it."

Write on whiteboard:

```
APP_NAME=BookShelf
DEBUG=True
```

"Your Python code reads from this file using a library. For today, we will define one variable and read it, so you understand the pattern. The important habit is: configuration outside code."

---

> 🎯 **Instructor Note**
> Ask the room: "Why would it be dangerous to put a password directly in a Python file?" Let one or two learners answer. Guide toward: version control, public repositories, shared codebases. This question takes thirty seconds and makes the purpose of environment variables memorable.

---

> 🎯 **Instructor Note — Common Mistake**
> Learners often add `.env` to their Git repository by accident. Mention briefly: in a real project, `.env` goes in `.gitignore`. You are planting the habit early, not teaching Git today.

---

### Walkthrough — FreshCart Project Structure

**[Script:]**

"Here is what our BookShelf project looks like after setup. This is the structure we will build into."

Write on whiteboard:

```
bookshelf/
├── venv/
├── .env
└── main.py
```

"Three things. The virtual environment folder. The environment variables file. And `main.py` — where our application code lives."

"Simple. Clean. Every file has a clear purpose."

---

## Block 3 — FastAPI Basics

**Time: 35 minutes**

---

### 3a — What Is FastAPI?

---

**[Script:]**

"FastAPI is a Python framework for building APIs. A framework is a toolkit — a set of ready-made tools that handle common problems so you do not have to build everything from scratch."

"FastAPI is specifically designed for building backends that receive HTTP requests and send back responses. It is fast to write, fast to run, and it automatically generates documentation for your API."

"You are writing Python. FastAPI handles the web layer — receiving requests, routing them to the right function, sending responses back."

---

> 🎯 **Instructor Note — Mental Model**
> Say this clearly: "You will write normal Python functions. FastAPI's job is to connect those functions to web addresses. Someone requests that address — FastAPI runs your function and sends back the result. You focus on the logic. FastAPI handles the plumbing."

---

### 3b — Creating the First FastAPI Application

---

**[Script:]**

"Let's build the first version of BookShelf. Open `main.py`. We start with three lines."

Write on whiteboard:

```python
from fastapi import FastAPI

app = FastAPI()
```

"Line one: we import FastAPI from the fastapi package we installed."

"Line two: we create an instance of FastAPI and store it in a variable called `app`. This `app` object is our entire application. Every route, every endpoint — everything attaches to it."

"That is a working FastAPI application. It does not do anything yet. But it exists. Let's run it."

Write on whiteboard:

```bash
uvicorn main:app --reload
```

"Break this down. `uvicorn` is the server. `main` is the filename — `main.py`. `app` is the variable inside that file. `--reload` tells the server to restart automatically every time we save a change. Essential during development."

"Open your browser. Go to `http://127.0.0.1:8000`. You will see a response."

---

> 🎯 **Instructor Note — Pause Point**
> Ask: "What does `127.0.0.1` mean?" Let learners guess. Guide toward: it is your own machine. Localhost. The server is running on your computer, not on the internet. Port 8000 is the door it is listening on.

---

### 3c — Implementing GET Requests

---

**[Script:]**

"Now let's give our application something to do."

"When a browser or any client wants to *read* data from a backend, it sends a **GET request**. GET means: give me information. Do not change anything. Just retrieve and return."

"In FastAPI, you create a GET endpoint by writing a Python function and attaching a decorator to it."

"A **decorator** is a line that starts with `@`. It modifies the function below it. In FastAPI, the decorator tells the framework: this function responds to requests at this specific address."

---

> 🎯 **Instructor Note**
> If learners look uncertain about decorators — keep it practical. Say: "Think of the decorator as a label on a door. The label says what kind of requests can knock on this door. The function inside is what happens when someone knocks." Do not go deeper into Python decorator mechanics. Stay on task.

---

**[Script:]**

"Here is the simplest possible GET endpoint."

Write on whiteboard:

```python
@app.get("/")
def read_root():
    return {"message": "Welcome to BookShelf"}
```

"The decorator `@app.get("/")` says: when someone sends a GET request to the root address — the `/` — run this function."

"The function returns a Python dictionary. FastAPI automatically converts that dictionary to JSON and sends it as the response."

"Predict before running: if I open `http://127.0.0.1:8000` in a browser, what will I see?"

**Pause. Take predictions.**

"Run it. There is our JSON response in the browser."

---

### Demo 1 — BookShelf GET Endpoint

> 🎯 **Instructor Note**
> Build this live in `main.py`. Type it — do not paste. Run the server. Show the browser response. Then show the automatic documentation at `/docs`. Learners are frequently surprised and delighted by the auto-generated docs. Use that reaction. It demonstrates FastAPI's value immediately.

**[Script:]**

"Now let's add a real endpoint. BookShelf needs to return a list of books."

"Predict before running: what will this return when someone requests `/books`?"

```python
@app.get("/books")
def get_books():
    books = [
        {"title": "The Alchemist", "author": "Paulo Coelho"},
        {"title": "1984", "author": "George Orwell"},
    ]
    return books
```

**Pause. Take predictions.**

"Run it. Go to `http://127.0.0.1:8000/books`."

"A list of two books, returned as JSON. The function returned a Python list of dictionaries. FastAPI serialised it to JSON automatically. We wrote Python. FastAPI handled the web layer."

---

> 🎯 **Instructor Note**
> Now navigate to `http://127.0.0.1:8000/docs`. Say: "FastAPI read our code and built this interactive documentation automatically. Every endpoint we add appears here. This is one of the reasons FastAPI is widely used — documentation is not a separate task, it is generated from the code itself."

---

### Demo 2 — GET Endpoint with a Path Parameter

> 🎯 **Instructor Note**
> This is an extension of the GET concept. Keep it short. One endpoint. One parameter. Connect it clearly to the BookShelf example.

**[Script:]**

"What if we want to retrieve one specific book by its index? We use a **path parameter** — a variable inside the URL address itself."

"Predict before running: if I request `/books/0`, what do you think this returns?"

```python
@app.get("/books/{book_id}")
def get_book(book_id: int):
    books = [
        {"title": "The Alchemist", "author": "Paulo Coelho"},
        {"title": "1984", "author": "George Orwell"},
    ]
    return books[book_id]
```

**Pause. Take predictions.**

"Run it. Request `/books/0`. There is The Alchemist. Request `/books/1`. There is 1984."

"The curly braces in the decorator — `{book_id}` — tell FastAPI: this part of the URL is a variable. Pass it into the function. The `: int` tells FastAPI the value should be an integer. FastAPI validates that automatically."

---

> 🎯 **Instructor Note — Common Mistake**
> Learners sometimes write the parameter name in the decorator differently from the function argument. For example, `{bookId}` in the decorator and `book_id` in the function. They will not match. FastAPI will raise an error. The names must be identical.

---

## Block 4 — API Testing with Postman

**Time: 25 minutes**

---

### 4a — What Is Postman?

---

**[Script:]**

"We have been testing our API by typing URLs into a browser. That works for simple GET requests. But a browser has limits. It cannot easily send other types of requests. It cannot set custom headers. It cannot inspect the full response in detail."

"**Postman** is a tool built specifically for testing APIs. It lets you send any type of request to any address, inspect the full response, and organise your tests."

"Think of Postman as a remote control for your API. Instead of a browser, you have a panel with precise controls. You choose the method — GET. You type the address. You press send. You see exactly what came back — status code, headers, body."

---

> 🎯 **Instructor Note**
> If Postman is not already installed on learner machines, this should have been done beforehand. If not, the web version at `web.postman.co` works as a fallback. Do not spend time troubleshooting installations during the session.

---

### 4b — Request and Response Structure

---

**[Script:]**

"Before we use Postman, let's understand what an HTTP request and response actually look like. These are the messages going back and forth between client and server."

"Every **request** has four parts."

Write on whiteboard:

```
Method   — GET, POST, PUT, DELETE (what action)
URL      — the address (where to send it)
Headers  — metadata about the request (optional for now)
Body     — data sent with the request (not used in GET)
```

"Every **response** has three parts."

Write on whiteboard:

```
Status Code  — a number indicating success or failure
Headers      — metadata about the response
Body         — the actual data returned (our JSON)
```

"The **status code** is critical. Learn these three now."

Write on whiteboard:

```
200 — OK. Request succeeded.
404 — Not Found. The address does not exist.
500 — Internal Server Error. Something broke on the server.
```

"When you request `/books` and get back your list — the status code is 200. When you request an address that does not exist — 404. When your Python code throws an error — 500."

---

> 🎯 **Instructor Note**
> Ask the room: "Have you ever seen a 404 page on a website?" Most will say yes. Say: "That is HTTP. That is the backend telling the browser — I looked, that address does not exist. The browser showed you a friendly page, but the message underneath was 404." This connects the concept to something they already know.

---

### 4c — Testing BookShelf Endpoints in Postman

---

> 🎯 **Instructor Note**
> Open Postman live. Walk through each step on screen. Go slowly. Learners are navigating a new interface while following along. Name every click.

---

**[Script:]**

"Open Postman. Create a new request. Here is the process we follow every time."

"Step one: choose the method. Click the dropdown on the left. Select GET."

"Step two: type the URL. `http://127.0.0.1:8000/books`."

"Step three: press Send."

"Predict before running: what status code and body do you expect to see?"

**Pause. Take predictions.**

"Send it. Look at the bottom panel. Two things to find. The status code — 200 OK — in the top right of the response panel. The body — your JSON list of books — below it."

"That is a successful API test. Method correct. Address correct. Server running. Response returned. Status 200."

---

**[Script:]**

"Now let's test a request that will fail deliberately. Change the URL to an address that does not exist."

```
http://127.0.0.1:8000/shelves
```

"Predict before running: what status code do you expect?"

**Pause. Take predictions.**

"Send it. Status 422 or 404. FastAPI is telling you: I have no endpoint at that address. Nothing to return."

"This is how you debug. You do not guess. You send a request, read the status code, read the response body — FastAPI often includes a message explaining what went wrong — and you fix accordingly."

---

> 🎯 **Instructor Note — Pause Point**
> Ask: "Why is it useful to test your API in Postman rather than only in a browser?" Let two or three learners answer. Guide toward: Postman shows full response details, works for all request types, lets you save and reuse requests, and makes it easy to test specific scenarios. A browser is a user tool. Postman is a developer tool.

---

### Walkthrough — Reading a Full Response

**[Script:]**

"Let's slow down and read a Postman response completely. Test the `/books/0` endpoint."

```
GET http://127.0.0.1:8000/books/0
```

"In the response panel, look at three things."

"First — the status code. 200. Request succeeded."

"Second — the response time. Postman shows how long the server took to respond. For a local server this will be under ten milliseconds. Fast."

"Third — the body. Our JSON object. One book. Title and author."

"Now test `/books/5`. We only have two books — index 0 and 1. Index 5 does not exist."

"Predict before running: what will happen?"

**Pause. Take predictions.**

"Send it. You get a 500 error. Our Python code tried to access `books[5]` and threw an index error. The server caught it and returned a 500 — Internal Server Error."

"This is important. A 404 means the address does not exist. A 500 means the address exists, but the code broke. Different problem. Different fix."

---

> 🎯 **Instructor Note — Common Mistake**
> Learners often treat all errors as the same. 404 and 500 require completely different responses from a developer. 404 means you need to check your URL or your routes. 500 means you need to debug your Python code. Make this distinction memorable. Write both on the whiteboard with their meanings.

---

## Block 5 — Lecture Summary

**Time: 15 minutes**

---

> 🎯 **Instructor Note — Delivery**
> Do not read this as a list. Speak each bullet as a confident statement. After each one, pause and ask "Does that match what we built today?" Let the room confirm before moving to the next. The goal is consolidation, not new information.

---

**[Script:]**

"Let's close by naming exactly what we covered and built today."

- The backend is the part of an application that runs on a server — it receives requests, does work, and sends back responses; the frontend is what users see, but the backend is where data lives and logic runs

- A virtual environment created with `python -m venv venv` isolates a project's dependencies so that packages installed for one project do not interfere with another

- `pip install` is how you add packages to your project inside an active virtual environment; you must activate the environment first or packages install to the wrong place

- Environment variables store configuration — such as sensitive credentials or settings — outside your Python code so that the code can be shared safely without exposing private values

- A FastAPI application is created by importing FastAPI, instantiating it as `app`, and running it with `uvicorn main:app --reload`

- A GET endpoint is defined by writing a Python function and attaching `@app.get("/address")` above it; FastAPI routes incoming GET requests at that address to that function and converts the return value to JSON automatically

- Path parameters allow a URL to carry a variable value — defined with curly braces in the decorator and received as a function argument — so one endpoint can serve different data based on what is in the URL

- Postman is a tool for sending HTTP requests to an API and inspecting the full response — including the status code, headers, and body — without using a browser

- Every HTTP response carries a status code: 200 means success, 404 means the address was not found, and 500 means the server code encountered an error

- Understanding backend fundamentals — how a server receives requests, how a framework routes them, and how to test the results — is the foundation for building any application that stores data, handles users, or communicates between systems

---

> 🎯 **Instructor Note — Closing Question**
> Ask the room: "In one sentence — what does a backend do?" Cold call two or three learners. You are listening for: it receives requests, processes them, and sends back responses. Correct gently if needed. Then ask: "What is the difference between a 404 and a 500?" These two questions together confirm whether the core concepts landed. End on those answers.

---

**[Script — Close:]**

"You built a backend today. From an empty folder to a running server with working endpoints that you tested and verified. That is not a small thing. Every real application you use is built on exactly this foundation."

---

*End of Lecture Script*
