# Lecture Script: Request Validation with Pydantic
**Duration:** 110 minutes | **Tools:** VS Code, Uvicorn, Swagger `/docs` | **Language:** Python + FastAPI + Pydantic

**Agenda:** Opening 7 · Why 12 · Concepts 20 · LO walkthroughs 48 · Live demo 13 · Recap 10

---

## Session Opening (7 min)

**[Script:]** "Last week POST took a dict. I can send `{year: 'banana'}`. Today Pydantic is the bouncer at the door. Wrong types never enter the function."

**Problem:** Campus bookstore API stored a book with empty title. The React list showed a blank card. Users thought the app was broken.

---

## Why Does This Matter?

🎯 **Instructor Note:** In Swagger, POST last session's dict route with `{}`. Show the silent bad object. Then contrast after models.

**[Script:]** "**Razorpay** and **AWS** APIs fail fast with field-level errors. If you skip validation, you get 500s, corrupt lists, and security holes — extra keys you did not expect. Response models also stop you from returning passwords by accident later."

- **Real-world use:** Signup forms, order payloads, admin updates
- **Pain if misunderstood:** Catching 422 in Python, or disabling validation to "make it work"

---

## What Is the Concept?

**Pydantic model:** a `BaseModel` subclass. Fields + types = schema.

**Request body validation:** FastAPI instantiates the model from JSON. Failure → 422, function skipped.

**Response model:** `response_model=` filters and documents output.

**Constraints:** `Field(min_length=..., max_length=...)`. Required = no default.

**Mental model:** Airport security tray. Wrong item, you do not board. Receipt (`detail` array) says which tray.

**Common mistakes:** Using `dict` still; putting path params on the model; confusing 400 vs 422; optional vs required.

**Python vs JS:** `JSON.parse` does not check types. Pydantic does.

---

## How Do We Apply It?

### LO 1: Create Pydantic models with types and type hints

**Problem:** Describe a book create payload.

**Write code:**

```python
from pydantic import BaseModel

class BookCreate(BaseModel):
    title: str
    year: int
    in_stock: bool = True
```

**Predict before running:** `in_stock` can be omitted. `title` cannot.

**Explain result:** Defaults make a field optional. Types are the first gate.

---

### LO 2: Apply request body validation on endpoints

**Problem:** POST must reject bad JSON.

**Write code:**

```python
from fastapi import FastAPI
app = FastAPI()

@app.post("/books")
def create_book(book: BookCreate):
    return {"saved": book.title}
```

**Predict before running:** Body `{"title": "SQL", "year": 2020}` → 200. Body `{"title": "SQL"}` → 422 (`year` missing).

**Explain result:** Validation is automatic. No `if` in the handler.

🎯 **Instructor Note:** Pause. Send `year` as string `"2020"`. Discuss coercion vs strictness at a beginner level — FastAPI may coerce; still show a true fail (`year: "nope"`).

---

### LO 3: Define response models

**Problem:** Internal dict has `internal_note`. Client must not see it.

**Write code:**

```python
class BookOut(BaseModel):
    title: str
    year: int

@app.post("/books", response_model=BookOut)
def create_book(book: BookCreate):
    return {"title": book.title, "year": book.year, "internal_note": "staff"}
```

**Predict before running:** JSON has `title` and `year` only.

**Explain result:** Response model is a filter and a contract.

---

### LO 4: Field constraints — min/max length, required

**Problem:** Titles of `""` or 5000 characters wreck the UI.

**Write code:**

```python
from pydantic import BaseModel, Field

class BookCreate(BaseModel):
    title: str = Field(min_length=1, max_length=80)
    year: int
```

**Predict before running:** `"title": ""` → 422 `string_too_short`.

**Explain result:** Constraints are extra rules on top of types. Required stays: `year` has no default.

---

### LO 5: Read and handle validation error responses

**Problem:** Frontend engineer asks why signup failed.

**Walkthrough:** Trigger 422. Read `detail[0].loc` and `.msg`. Fix the JSON. Re-send.

**"Handle"** for this session means: understand the body, correct the client, do not swallow errors in the route.

**Predict before running:** After fixing `title`, status becomes 200.

**Demo (≤10 lines):** Swagger POST with empty title, read red error panel.

---

## Live Demo Block (13 min)

Upgrade `/books` POST/PUT from `dict` to models. Show `/docs` schema boxes. Two failing payloads, one success. Point at `loc`.

**[Script:]** "The docs got smarter because the model is the source of truth."

---

## Recap (10 min)

🎯 **Instructor Note:** "422 vs 404 vs 200 — when?" Three sticky notes on the board.

---

## Lecture Summary

- **Pydantic models** declare field types with type hints
- **Request body validation** blocks bad JSON before the route runs
- **Response models** keep output consistent and trimmed
- **Field constraints** enforce length and required data
- **Validation errors** are 422 JSON you read via `loc` and `msg`
- **Practical value:** Your API is a contract, not a hope — ready for dependencies, files, and a real database next
