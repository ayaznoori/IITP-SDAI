# Lecture Script: Pytest Fundamentals
**Duration:** 110 minutes | **Tools:** VS Code, Pytest, FastAPI TestClient | **Language:** Python

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Broken rename, no one noticed |
| Why Does This Matter? | 14 min | Jobs, CI, fearless change |
| What Is the Concept? | 22 min | Pyramid, assert, TestClient |
| How Do We Apply It? (LOs) | 52 min | GET test, POST test, run |
| Live lab | 8 min | Students run pytest |
| Recap | 6 min | Full-stack lab next |

---

## Session Opening (8 min)

**[Script:]** "Yesterday your CRUD worked in Swagger. Tonight someone renames `/notes` to `/memos` and forgets the list route. Nobody clicks every button. **Pytest** is how we catch that at 9 AM, not in a demo. Today: why tests, the **test pyramid** in human language, one **GET** test, one **POST** test, and reading the terminal."

**Real-world hook:** Show a red CI badge on a public repo. "That red X is a conversation. You will create a tiny version locally."

🎯 **Instructor Note:** Install `pytest` and `httpx` (TestClient dependency) in the venv before the demo. Keep tests on existing GET/POST only.

---

## Why Does This Matter?

🎯 **Instructor Note:** Break a working GET live (wrong path). Ask who would notice without clicking. Then show a failing test in 2 seconds.

**[Script:]** "Hiring managers at product companies ask juniors: how do you know it works? 'I tried it' is weak. 'I have Pytest for GET and POST' is a senior-sounding beginner answer. **Amazon** and **Razorpay** pipelines fail the build when tests fail. You are practising that gate on your laptop."

**Pain if misunderstood:**
- Tests that never fail — they test nothing
- Testing implementation trivia, not status and JSON
- Skipping POST body — create path silently rots

> **In the Real World:** **Spotify** squads treat failing tests as a stop-the-line signal. Two tests today teach the stop.

---

## What Is the Concept?

**Pytest:** finds functions named `test_*` and runs them.

**Assert:** the check.

**TestClient:** in-process HTTP to FastAPI.

**Pyramid (plain words):** lots of cheap checks at the bottom; almost no giant end-to-end tests at the top. We stay at the bottom.

**Python vs JS:** Jest `expect(status).toBe(200)` equals `assert response.status_code == 200`.

**Common mistake:** Using a live production URL in tests. Test the app object.

---

## How Do We Apply It?

### LO 1: Explain why automated tests matter

**Problem:** Manual Swagger does not scale past five endpoints.

**Discussion:** Cost of a silent 500 after a refactor.

**Predict:** Does a test replace thinking? (No — it locks behaviour you already decided.)

---

### LO 2: Describe the test pyramid in plain words

**Whiteboard:** wide base = many fast API tests; tiny tip = slow full-stack later.

**[Script:]** "If all your tests need Chrome, they will be slow and flaky. Start with GET and POST functions."

**Predict:** Where do today's two tests sit? (The base.)

---

### LO 3: Write a Pytest for one GET

```python
def test_get_notes_ok():
    response = client.get("/notes")
    assert response.status_code == 200
```

**Predict before running:** Empty DB — still 200? (Yes, empty list is success.)

**Explain result:** Contract is “list endpoint is alive,” not “has three rows.”

---

### LO 4: Write a Pytest for one POST

```python
def test_post_note():
    response = client.post("/notes", json={"title": "pytest"})
    assert response.status_code in (200, 201)
    assert "title" in response.json()
```

**Predict before running:** Missing `title` — should this test still pass? (No — use a valid body; invalid body is a different test, out of scope if not in LOs.)

🎯 **Instructor Note:** Match the class's actual status code. Do not fight 200 vs 201 as religion — be consistent.

> **In the Real World:** **Twilio** API tests assert status and a small JSON shape. Same discipline.

---

### LO 5: Run tests locally and interpret the result

```bash
pytest -q
```

**Walkthrough fail:** Change path to `/note` (wrong). Read assertion diff.

**Walkthrough pass:** Restore path. Green.

**[Script:]** "Red is information. Read expected versus actual. Then fix one thing."

---

## Live Lab (8 min)

Every student: two tests, one intentional fail, one fix, screenshot of green run.

---

## Recap (6 min)

**[Script:]** "Lab next: one schema, FastAPI CRUD, React CRUD, keep them in sync. Tests will save you there."

---

## Lecture Summary

- **Automated tests** catch API breaks without relying on memory and clicks
- **Test pyramid** means many fast small checks, few slow full-app checks
- **GET Pytest** locks the list/read contract
- **POST Pytest** locks the create contract
- **Running pytest** teaches you to read pass and fail, not fear red
- **Practical value:** You can change routes next week and know within seconds if GET or POST broke
