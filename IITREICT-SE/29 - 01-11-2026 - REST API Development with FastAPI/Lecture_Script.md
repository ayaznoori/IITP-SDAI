# Lecture Script: REST API Development with FastAPI
**Duration:** 110 minutes | **Tools:** VS Code, Uvicorn, browser Swagger, optional Postman | **Data:** In-memory list

**Agenda:** Opening 7 · Why 10 · Concepts 18 · LO walkthroughs 52 · Live demo 13 · Recap 10

---

## Session Opening (7 min)

**[Script:]** "Yesterday we served GET. Real products create, change, and delete. Today: four verbs, path and query, a JSON body, and `/docs` so you stop guessing URLs."

**Problem:** Club inventory lives in a Python list. We need HTTP CRUD so a React form can talk to us.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show GitHub REST docs screenshot. "This is the same shape you will publish at `/docs`."

**[Script:]** "If you mash all actions into GET, browsers cache deletes. If you skip pagination, `GET /users` dumps ten thousand rows and the UI dies. If you never open Swagger, you debug with print statements instead of a contract."

- **Real-world use:** **Shopify** product APIs, **Razorpay** payment objects, internal admin tools
- **Pain if misunderstood:** Wrong verb, body ignored, "it works in my browser" but POST never ran

---

## What Is the Concept?

**CRUD** maps to HTTP. Resource URL is a noun. Method is the verb.

| Verb | Memory list action |
|------|-------------------|
| GET collection | return list (maybe filtered) |
| GET one | find by id or 404 |
| POST | append new dict, return it |
| PUT | replace fields by id |
| DELETE | remove by id |

**Path** = identity. **Query** = options. **Body** = payload for write.

**Mental model:** Spreadsheet. Path is row number. Query is filter/sort. Body is the new row values.

**Common mistakes:** PUT without id in the path; POST with query instead of body; returning the whole list after every write with no status thought.

**Python vs JS:** `fetch(url, { method: 'POST', body: JSON.stringify(...) })` matches `@app.post` + body parameter.

---

## How Do We Apply It?

Keep one list in `main.py` (reset on restart — say this out loud).

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()
books = [{"id": 1, "title": "SQL", "year": 2020}]
```

### LO 1: Implement GET POST PUT DELETE

**Problem:** Full CRUD on `books`.

**Translate logic:** Four functions, four decorators.

**Write code:**

```python
@app.get("/books")
def list_books():
    return books

@app.post("/books")
def create_book(book: dict):
    books.append(book)
    return book
```

(Complete PUT/DELETE in live demo; sketch signatures now.)

**Predict before running:** POST then GET shows two items. Restart Uvicorn — extra item gone.

**Explain result:** Memory is not a database. Contract still real.

---

### LO 2: Path, query, and JSON body

**Problem:** Fetch one book; search by title fragment.

**Write code:**

```python
@app.get("/books/{book_id}")
def get_book(book_id: int):
    for b in books:
        if b["id"] == book_id:
            return b
    raise HTTPException(status_code=404, detail="Not found")
```

**Predict before running:** `/books/1` works. `/books/99` → 404 JSON.

🎯 **Instructor Note:** Show query `title` on list route next to path `book_id`. Ask which belongs where.

**Body:** POST function parameter `book: dict` reads JSON. (Pydantic models next session.)

---

### LO 3: Filtering, sorting, pagination

**Problem:** 100 books; UI wants page 2 of cheap titles.

**Write code:**

```python
@app.get("/books")
def list_books(q: str | None = None, skip: int = 0, limit: int = 10, sort: str = "id"):
    data = books
    if q:
        data = [b for b in data if q.lower() in b["title"].lower()]
    data = sorted(data, key=lambda b: b.get(sort, 0))
    return data[skip : skip + limit]
```

**Predict before running:** `?limit=1` returns one object. `?q=sql` filters.

**Explain result:** Query params are optional with defaults. Slice is pagination.

---

### LO 4: Explore Swagger at `/docs`

**Problem:** Teammate does not read `main.py`.

**Walkthrough:** Open `/docs`. Expand POST. Schema from type hints. Try it out.

**Predict before running:** Execute GET `/books` → 200 and JSON in the panel.

**Explain result:** OpenAPI is generated. `/redoc` exists too — mention, do not deep-dive.

---

### LO 5: Test CRUD with Postman or Swagger

**Problem:** Prove PUT and DELETE without a frontend.

**Demo in Swagger:** POST a book, PUT change title, GET id, DELETE, GET 404.

Optional Postman: same URL, method dropdown, Body → raw JSON.

**Predict before running:** After DELETE, list no longer contains that id.

---

## Live Demo Block (13 min)

Build `/books` CRUD end to end. Use Swagger only. Show `skip`/`limit`. Break a PUT with wrong id.

**[Script:]** "If Swagger succeeds, Fetch will succeed. The contract is the product."

---

## Recap (10 min)

🎯 **Instructor Note:** "Path or query: book 5? sort by year?" Rapid fire.

---

## Lecture Summary

- **GET POST PUT DELETE** implement CRUD on a resource
- **Path, query, JSON body** carry identity, options, and payload
- **Filter, sort, paginate** keep list endpoints usable
- **`/docs` Swagger** is the live contract and tester
- **Postman or Swagger** proves CRUD without React
- **Practical value:** You can ship an API teammates can call — next we stop using raw `dict` and validate with Pydantic
