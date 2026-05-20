## Pre-Read: Introduction to Backend Development and FastAPI Fundamentals

### What You'll Learn

In this pre-read, you'll discover:

- What backend development means and why it powers every app you use
- How to set up a Python project using virtual environments and manage dependencies
- How to create your first FastAPI application and handle GET requests
- How to test your API using Postman

---

### Detailed Explanation

**What is Backend Development?**

**Backend development** means building the invisible parts of software that users don't see.

When you open an app or visit a website, what you see—the buttons, images, text—is the **front end**. The backend lives on a server somewhere. It stores data, processes requests, and runs business logic.

**Analogy**: Think of a restaurant. The menu and dining room are the front end. The kitchen and recipes are the backend. You interact with the menu, but the real work happens behind the scenes.

**Why It Matters**

Backend development powers nearly everything digital:

- User accounts and data storage
- Processing payments securely
- Scaling apps to serve millions

Understanding backend fundamentals opens doors to building full-stack applications and working with cloud services.

---

**Setting Up Your Project**

Before building, you need a clean workspace.

A **virtual environment** (venv) isolates your project dependencies. It keeps your project separate from other Python projects on your computer.

Create one with:

```
python -m venv myenv
```

Activate it:

- Windows: `myenv\Scripts\activate`
- Mac/Linux: `source myenv/bin/activate`

**pip** installs packages from the Python Package Index. Use it to add libraries to your project:

```
pip install fastapi
```

**Environment variables** store sensitive values like API keys and passwords. Create a `.env` file to keep them separate from your code.

---

**Meet FastAPI**

**FastAPI** is a modern Python framework for building APIs. It's fast, easy to learn, and automatically generates documentation for you.

An **API** (Application Programming Interface) is a way for two programs to talk to each other. Think of it as a waiter taking your order to the kitchen.

Here's a simple FastAPI app:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, World!"}
```

Run it with:

```
uvicorn main:app --reload
```

The `@app.get("/")` decorator creates a **GET request** handler. This is how the server responds when someone visits that URL.

---

**Testing with Postman**

**Postman** is a tool for testing APIs. You can send requests and see responses without writing code.

A **request** has:

- A method (GET, POST, etc.)
- A URL (where to send it)

A **response** includes:

- A status code (200 means success)
- Data (usually JSON)

---

### Practice Exercises

1. Create a virtual environment called `practice_env` and activate it
2. Install FastAPI and uvicorn using pip
3. Write a FastAPI app that returns `{"welcome": "your name"}` at the root path
4. Start your server and visit `http://localhost:8000` in your browser
5. Open Postman and send a GET request to your endpoint
6. Add a second endpoint `/hello` that returns a greeting message
