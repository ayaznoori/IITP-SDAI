# Lecture Script: FastAPI CRUD, CORS & React Integration
**Duration:** 110 minutes | **Tools:** VS Code, Uvicorn, Vite React, Browser DevTools | **Language:** Python / FastAPI / React

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | List is not a product |
| Why Does This Matter? | 12 min | CRUD jobs, CORS surprise |
| What Is the Concept? | 20 min | Status codes, CORS, fetch |
| How Do We Apply It? (LOs) | 55 min | Five LOs live |
| Live lab | 8 min | End-to-end create |
| Recap | 7 min | RAM dies on restart |

---

## Session Opening (8 min)

**[Script:]** "You can GET a list and validate a POST body. Today we **keep** created items in memory, add **PUT or DELETE**, return **201** and **404**, unlock the **browser** with **CORS**, and call list plus create from **React fetch**. One create must be traceable from button to JSON."

**Problem hook:** React on 5173, API on 8000. Network tab shows the request. Console shows CORS error. Students think FastAPI is down. It is not.

🎯 **Instructor Note:** Confirm two terminals: Uvicorn and Vite. Same machine. CORS origin must match the address bar.

---

## Why Does This Matter?

🎯 **Instructor Note:** Demo a POST in Swagger (works) then the same POST from React without CORS (fails). Flip middleware on. Cheer.

**[Script:]** "Full-stack means the UI and API agree on URLs, JSON, and codes. If you return 200 for a missing id, React cannot show 'not found.' If you skip CORS, only Swagger works and you will think React is broken."

> **In the Real World:** Local CORS is the first week of every intern's full-stack setup. Production uses real allowed origins, not `*` forever, but today we unlock localhost React.

**Pain if misunderstood:**

- POST 200 vs 201 — clients and tests disagree
- Silent CORS — empty UI
- Mutating the list without checking id — wrong item updates

| Client | Server |
|--------|--------|
| `fetch` + JSON body | Pydantic + list |
| Read `res.status` | `HTTPException` |
| Vite origin | `CORSMiddleware` |

---

## What Is the Concept?

**POST:** create, usually **201**.  
**PUT:** replace one item. **DELETE:** remove. At least one of PUT/DELETE required.  
**404:** id not in the list.  
**CORS:** browser permission for another origin.  
**Create flow:** UI event → fetch → validate → store → JSON → UI state.

🎯 **Instructor Note:** Pick PUT **or** DELETE for the live demo so time stays honest. Students may implement the other as bonus.

---

## How Do We Apply It?

### LO 1: Implement POST and at least PUT or DELETE on an in-memory resource

**Problem:** GET-only API cannot save a new campus item.

**Translate logic:** POST appends with next id. DELETE removes by id (demo choice).

**Write code:**

```python
@app.post("/items", status_code=201)
def create(item: Item):
    new = {"id": len(ITEMS) + 1, "name": item.name, "qty": item.qty}
    ITEMS.append(new)
    return new
```

**Predict before running: What will happen?**

**Predict:** Swagger POST → 201 and JSON including `id`.

**Explain result:** List grew in RAM. GET list shows the new row.

Add DELETE (or PUT) in the same block — unknown id later.

---

### LO 2: Return appropriate status codes and clear error messages

**Problem:** Unknown id must not look like success.

**Translate logic:** Find item. If missing, `HTTPException(404, detail=...)`.

**Write code:**

```python
@app.delete("/items/{item_id}")
def remove(item_id: int):
    for i, row in enumerate(ITEMS):
        if row["id"] == item_id:
            ITEMS.pop(i)
            return {"ok": True}
    raise HTTPException(status_code=404, detail="Item not found")
```

**Predict before running: What will happen?**

**Predict:** Bad id → 404 body `{"detail":"Item not found"}`.

**Explain result:** Clients can branch on status. Message is human-readable.

🎯 **Instructor Note:** Point at `status_code=201` on POST vs default 200.

---

### LO 3: Enable CORS for a local React client

**Problem:** Browser hides the 201 from JavaScript.

**Translate logic:** Allow `http://localhost:5173` (or 5174 if Vite shifted).

**Write code:**

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],
    allow_methods=["*"],
    allow_headers=["*"],
)
```

**Predict before running: What will happen?**

**Predict:** OPTIONS preflight succeeds. POST from React reaches JS.

**Explain result:** FastAPI sends CORS headers. Browser permits the frontend origin.

---

### LO 4: Call list and create endpoints from React with fetch

**Problem:** Swagger works; the product UI does not call the API yet.

**Translate logic:** `useEffect` or button for GET. Form `onSubmit` for POST with headers.

**Write code:**

```javascript
await fetch("http://127.0.0.1:8000/items");
await fetch("http://127.0.0.1:8000/items", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Lab", qty: 1 }),
});
```

**Predict before running: What will happen?**

**Predict:** Network tab: GET 200 array. POST 201 object.

**Explain result:** Same JSON as Swagger. React can `setState` from `res.json()`.

---

### LO 5: Trace one full create flow from UI to API response

**Problem:** Students cannot debug because they cannot narrate the path.

**Translate logic:** Click → fetch POST → Pydantic → append → 201 JSON → list re-render.

**Write code:** Live walk with DevTools open. Number the steps on the board.

**Predict before running: What will happen?**

**Predict:** One click produces one POST, one 201, one extra row in GET.

**Explain result:** If any step fails, name it: CORS, 422, or wrong URL.

🎯 **Instructor Note:** Cold-call one student to narrate. Correct gaps.

---

## Live Lab (8 min)

Create from React, confirm in Swagger GET, then DELETE or PUT once.

> **In the Real World:** This is the intern demo: form, network 201, list updates.

---

## Recap (7 min)

🎯 **Instructor Note:** "Restart Uvicorn. Is the posted item still there?" No. That pain introduces the database module.

**[Script:]** "You wrote POST plus a write path, returned 201/404, enabled CORS, fetched from React, and traced create. Memory is temporary. Next: PostgreSQL on Neon so data survives."

---

## Lecture Summary

- **POST + PUT or DELETE** on an in-memory resource
- **201 / 200 / 404** and `detail` messages
- **CORS** unlocks local Vite → FastAPI
- **React fetch** for list and create
- **Create flow** from UI click to API JSON
- **Practical value:** First true full-stack loop in this course
