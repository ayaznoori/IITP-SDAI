# Lecture Script: Hands-on FastAPI CRUD Lab
**Duration:** 110 minutes | **Tools:** VS Code, venv, Uvicorn, Swagger UI | **Language:** Python / FastAPI

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Lab goals, freeze scope |
| Why Does This Matter? | 10 min | Portfolio proof, debug habit |
| What Is the Concept? | 12 min | Recap GET/POST/Pydantic/codes |
| How Do We Apply It? (LOs) | 65 min | Build, test, debug |
| Live lab | 8 min | Mentor checkpoint |
| Recap | 7 min | What to commit |

---

## Session Opening (8 min)

**[Script:]** "This is a **lab**. No new framework. You **implement or extend GET and POST**, keep **Pydantic** on the body, **prove it in Swagger**, return **200 / 201 / 404**, and **debug one common request mistake** with a mentor. CORS and React can wait if they are already done. Neon is not required today. In-memory is fine."

**Problem hook:** Half the class has GET. Half has a 422 they cannot read. We finish both.

🎯 **Instructor Note:** TA-led. Circulate. Pair stuck students. Ban random AI dumps of full CRUD files — review small diffs.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask: "What evidence will you show a mentor?" Answer: `/docs` Try-it-out, not a speech.

**[Script:]** "Jobs do not grade your notes. They grade a running endpoint and whether you can read an error. 422 is a clue. Connection refused is a clue. Mixing path and query is a clue."

> **In the Real World:** Support tickets quote status codes. "It doesn't work" is not a ticket. "POST /items returns 422 on field qty" is.

**Pain if misunderstood:**

- Shipping GET-only and calling it CRUD
- Returning 200 on missing id
- Ignoring Swagger and only using the browser address bar (GET-only)

---

## What Is the Concept?

Quick board recap only:

- `@app.get` / `@app.post`
- `item: Item` Pydantic body
- `status_code=201`
- `HTTPException(404)`
- Swagger `/docs`

🎯 **Instructor Note:** 12 minutes max. Then laptops open.

---

## How Do We Apply It?

### LO 1: Implement or extend GET and POST endpoints

**Problem:** Starter app may only have `GET /`.

**Translate logic:** `GET /items` returns the list. `POST /items` appends.

**Write code:**

```python
@app.get("/items")
def list_items():
    return ITEMS

@app.post("/items", status_code=201)
def create_item(item: Item):
    row = {"id": len(ITEMS) + 1, "name": item.name, "qty": item.qty}
    ITEMS.append(row)
    return row
```

**Predict before running: What will happen?**

**Predict:** GET [] or seed rows. POST grows the list.

**Explain result:** Create and read on the same store.

🎯 **Instructor Note:** Students with working code extend GET-by-id instead of rewriting.

---

### LO 2: Validate a request body with Pydantic

**Problem:** POST accepts anything and stores junk.

**Translate logic:** `class Item(BaseModel)` required fields. Parameter type `Item`.

**Write code:**

```python
class Item(BaseModel):
    name: str
    qty: int
```

**Predict before running: What will happen?**

**Predict:** Missing `name` → 422. Valid body → 201.

**Explain result:** Validation at the boundary. Route only runs on success.

---

### LO 3: Test routes in Swagger UI

**Problem:** Guessing curl flags wastes lab time.

**Translate logic:** `/docs` → Try it out → Execute. Record status.

**Write code:** None. Live clicks.

**Predict before running: What will happen?**

**Predict:** Schema appears for POST. Responses visible.

**Explain result:** Swagger is the lab test runner for today.

---

### LO 4: Return correct status codes for success and not-found

**Problem:** GET missing id returns empty 200.

**Translate logic:** Find by id. Else `HTTPException(404, detail="Item not found")`. POST success 201.

**Write code:**

```python
@app.get("/items/{item_id}")
def one(item_id: int):
    for row in ITEMS:
        if row["id"] == item_id:
            return row
    raise HTTPException(status_code=404, detail="Item not found")
```

**Predict before running: What will happen?**

**Predict:** Known id 200. Unknown 404 JSON `detail`.

**Explain result:** Clients can trust codes. Mentors will check 404.

---

### LO 5: Debug one common FastAPI request mistake

**Problem:** "POST does nothing" — often wrong method, wrong path, or 422 unread.

**Translate logic:** Reproduce in Swagger. Read status + `detail` + URL.

**Write code:** Demo one bug — e.g. POST to `/item` (singular) while route is `/items`.

**Predict before running: What will happen?**

**Predict:** 404 on wrong path. Fix path → 201.

**Explain result:** The mistake was the request, not Pydantic. Write the trio: symptom, cause, fix.

🎯 **Instructor Note:** Each student names **one** bug. Stamp the lab complete only after that write-up.

---

## Live Lab (8 min)

Checkpoint: GET list 200, POST 201, missing id 404, one debug note.

> **In the Real World:** That checkpoint is a pull-request description.

---

## Recap (7 min)

🎯 **Instructor Note:** Share two anonymized 422 `detail` snippets. Celebrate reading errors.

**[Script:]** "You extended GET/POST, validated bodies, tested in Swagger, returned 201 and 404, and debugged one request mistake. Next lecture: SQL on Neon — SELECT, INSERT, UPDATE, DELETE, and a JOIN."

---

## Lecture Summary

- **GET and POST** implemented or extended
- **Pydantic** validates POST bodies
- **Swagger** proves the routes
- **200 / 201 / 404** used correctly
- **One request mistake** reproduced and fixed
- **Practical value:** Confidence before database-backed CRUD
