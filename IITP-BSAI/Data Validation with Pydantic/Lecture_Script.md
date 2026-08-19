# Lecture Script: Data Validation with Pydantic
**Duration:** 110 minutes | **Tools:** VS Code, Uvicorn, Swagger UI | **Language:** Python / FastAPI / Pydantic

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Junk JSON at the gate |
| Why Does This Matter? | 12 min | Boundary vs UI-only checks |
| What Is the Concept? | 22 min | BaseModel, required/optional, 422 |
| How Do We Apply It? (LOs) | 53 min | Five LOs live |
| Live lab | 8 min | Item model + bad POST |
| Recap | 7 min | CRUD uses this model |

---

## Session Opening (8 min)

**[Script:]** "Yesterday GET returned a list you control. Today someone else **sends** JSON. They will omit fields. They will send a string where you wanted a number. **Pydantic BaseModel** is the shape of a valid body. FastAPI runs it at the **API boundary** — before your function body. POST is how we receive that body. We are not finishing CRUD today."

**Problem hook:** `price: "cheap"` lands in Python. Multiply later. Crash. Or worse, silent wrong data.

🎯 **Instructor Note:** Reuse last session's app. Keep GET. Add one POST with a model.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask: "If only React validates, what about Swagger? What about a mobile client?" Wait for "the server must check too."

**[Script:]** "UI checks are polite. Server checks are the law. Payment and signup APIs fail closed. A 422 with a field name is cheaper than a corrupt row next module."

> **In the Real World:** Public APIs publish schemas. Frontend codegen and Swagger both read them. Pydantic is how FastAPI publishes yours.

**Pain if misunderstood:**

- Treating 422 as a "FastAPI bug"
- Making every field optional "so it works"
- Validating only after you append to the in-memory list

| Layer | Role |
|-------|------|
| React form | UX hints |
| Pydantic | Trust boundary |
| Route function | Business steps |

---

## What Is the Concept?

**BaseModel:** class with type hints. Pydantic parses and checks.

**Required:** no default.

**Optional:** default value (or `None`).

**Validation error:** HTTP 422, `detail` array with `loc` and `msg`.

**Request body:** annotate the parameter with the model type.

🎯 **Instructor Note:** Show Swagger schema panel generated from the model. Pause for "this is why types matter."

---

## How Do We Apply It?

### LO 1: Create a Pydantic BaseModel with typed fields

**Problem:** We need a shared shape for a campus item.

**Translate logic:** Class inheriting `BaseModel`. `name: str`, `qty: int`.

**Write code:**

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    qty: int
```

**Predict before running: What will happen?**

**Predict:** `Item(name="Pen", qty=2)` prints those fields.

**Explain result:** Types are the contract. Instances hold parsed values.

---

### LO 2: Mark fields required or optional with defaults

**Problem:** `in_stock` is usually true. Callers should not always send it.

**Translate logic:** Required `name` and `qty`. `in_stock: bool = True`.

**Write code:**

```python
class Item(BaseModel):
    name: str
    qty: int
    in_stock: bool = True
```

**Predict before running: What will happen?**

**Predict:** Body `{"name":"Pen","qty":1}` → `in_stock` is `True`.

**Explain result:** Missing optional uses default. Missing `name` still fails.

🎯 **Instructor Note:** Demo omitting `qty` vs omitting `in_stock`.

---

### LO 3: Trigger and read a validation error with bad input

**Problem:** Students must not fear 422. They must read it.

**Translate logic:** POST `{"name":"Pen","qty":"many"}` in Swagger.

**Write code:** (use existing POST from next LO, or a 3-line route)

**Predict before running: What will happen?**

**Predict:** Status 422. `detail[0].loc` includes `qty`. `msg` mentions int.

**Explain result:** Pydantic rejected before `create` ran. The list did not change.

---

### LO 4: Attach a Pydantic model as a FastAPI request body

**Problem:** Route must receive parsed `Item`, not a raw dict.

**Translate logic:** `def create(item: Item)`.

**Write code:**

```python
@app.post("/items")
def create(item: Item):
    return item
```

**Predict before running: What will happen?**

**Predict:** Valid JSON → 200 and the same fields echoed.

**Explain result:** FastAPI built `Item` from the body. Return serializes back to JSON.

---

### LO 5: Explain why validation belongs at the API boundary

**Problem:** Temptation to "fix it in React only."

**Translate logic:** Many clients. One server. Boundary = HTTP in.

**Write code:** Whiteboard, then this contrast:

```python
# Boundary: item: Item
# Too late: items.append(raw) then hope keys exist
```

**Predict before running: What will happen?**

**Predict:** Unvalidated append can store missing `name`. Later GET looks broken.

**Explain result:** Check once at entry. Route code stays simple. Every client gets the same 422.

---

## Live Lab (8 min)

Students cause one 422 and paste `detail` into chat. Then one valid POST echo.

> **In the Real World:** On-call reads 422 `loc` to see which field the app sent wrong. That is a job skill.

---

## Recap (7 min)

🎯 **Instructor Note:** "Required vs default?" "Where do you read the error?" (`detail`). "Why not only frontend?"

**[Script:]** "You modelled fields, set defaults, read 422, attached the body, and placed validation at the gate. Next session: POST for real plus PUT or DELETE, CORS, and React fetch."

---

## Lecture Summary

- **BaseModel** declares typed fields
- **Required vs optional** is default vs no default
- **422 + detail** is how you read failures
- **Model as body** wires Pydantic to FastAPI
- **API boundary** is the right place to validate
- **Practical value:** Garbage never reaches your store or, later, the database
