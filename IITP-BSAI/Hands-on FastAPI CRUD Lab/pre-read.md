# Pre-Read: Hands-on FastAPI CRUD Lab

## 1. What You'll Learn

In this pre-read, you'll discover:

- How to **implement or extend GET and POST** on your lab API
- How to **validate a request body with Pydantic**
- How to **test routes in Swagger UI**
- How to **return correct status codes** for success and not-found
- How to **debug one common FastAPI request mistake**

---

## 2. Detailed Explanation

### Lab Day, Not New Theory

You already met FastAPI, GET, Pydantic, POST, and status codes. This session is **practice with a mentor nearby**.

**One-line definition:** A lab is guided time to make GET and POST **work on your machine**, with Swagger proof.

**Analogy:** Driving lessons after the rulebook. Same pedals. More road.

> **In the Real World:** Intern week one is often "finish the list and create endpoints; send a screenshot of `/docs`." Interviewers ask you to debug a 422. That is today's muscle.

### Why It Matters

- Lectures fade; a working `main.py` stays
- Status codes are how React will branch later
- Most lab time is spent on **small request mistakes**, not big ideas

### GET and POST You Should Have

| Method | Path (example) | Success code | Body |
|--------|----------------|--------------|------|
| GET | `/items` | **200** | none |
| GET | `/items/{id}` | **200** or **404** | none |
| POST | `/items` | **201** | Pydantic model |

Not-found GET by id → **404** and a `detail` message. Do not return 200 with `null` and call it done.

### Pydantic on POST

The POST parameter type is your `BaseModel`. Swagger then shows a JSON example. Bad types → **422**. That is success of validation, not failure of FastAPI.

### Swagger Checklist

1. Open `/docs`  
2. Try GET list  
3. Try POST valid body — expect 201  
4. Try GET by id  
5. Try GET missing id — expect 404  
6. Try POST missing field — expect 422  

### Common Request Mistakes (Pick One to Debug)

Stay in this list. Do not invent new frameworks.

| Mistake | What you see | Fix idea |
|---------|--------------|----------|
| POST with no `Content-Type: application/json` | 422 or empty body | Send JSON in Swagger (it sets the header) |
| Path `/items/1` vs query `?id=1` mixed up | 404 or ignored id | Match the decorator path |
| Forgetting to restart / wrong port | Connection refused | Confirm Uvicorn URL |
| Body field name ≠ model field | 422 `loc` | Read `detail` |
| Using GET when you meant POST | List unchanged | Method dropdown in Swagger |

**Debug habit:** Read status, then `detail`, then the request URL. Do not rewrite the whole file first.

### Messy to Clear

**Messy:** Change five things at once. Still broken.

**Clear:** One POST in Swagger. One error. One fix. Re-test.

### Building Blocks Checklist

- [ ] GET list works
- [ ] POST validates and returns 201
- [ ] Missing id returns 404
- [ ] Swagger is my test tool
- [ ] I can explain one request bug I fixed

---

## 3. Practice Exercises

**Exercise 1 — GET list**  
Ensure `GET /items` returns JSON array. Screenshot 200.

**Exercise 2 — POST + Pydantic**  
POST a valid body. Confirm 201 and fields match the model.

**Exercise 3 — Swagger 422**  
Omit a required field. Write the `loc` value you got.

**Exercise 4 — 404**  
`GET /items/9999` (or similar). Confirm 404 and `detail`.

**Exercise 5 — Debug**  
Reproduce one mistake from the table. Write the symptom, cause, and fix in three lines.
