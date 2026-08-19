# Lecture Script: Authorization & API Security
**Duration:** 110 minutes | **Tools:** VS Code, Uvicorn, Swagger Authorize, terminal env | **Language:** FastAPI + JWT

**Agenda:** Opening 7 · Why 12 · Concepts 18 · LO walkthroughs 50 · Live demo 13 · Recap 10

---

## Session Opening (7 min)

**[Script:]** "You can mint a token. Until routes check it, it is a souvenir. Today **Depends** becomes a bouncer. Then **roles**. Then we stop committing secrets. Then we list mistakes that fail security reviews."

**Problem:** GET /me works without header. Swagger shows it. "We forgot the gate."

---

## Why Does This Matter?

🎯 **Instructor Note:** Call an admin DELETE with a student token after you add RBAC. Show 403. Contrast 401 with empty Authorize box.

**[Script:]** "**HDFC** APIs do not trust a hidden URL. **Google Cloud** IAM is RBAC at scale. If JWT_SECRET is in Git, anyone clones god-mode. If you skip validation, auth is not enough — bad JSON still hurts. Common mistakes are how real incidents start: tokens in logs, extra fields leaked, copy-paste auth that one intern forgets on a new route."

- **Real-world use:** Admin dashboards, student vs faculty portals
- **Pain if misunderstood:** 401/403 mix-up in frontend; `role` in JWT unsigned (must still verify signature)

---

## What Is the Concept?

**Auth dependency** = decode JWT or 401.

**RBAC** = role → allowed actions.

**Env vars** = config the process, not the repo.

**Secure defaults** = validation on, least data out, least routes public.

**Mental model:** ID check (401) then room key (403).

**Common mistakes:** 403 when missing token (should 401); checking role without verifying JWT; `os.environ.get("JWT_SECRET", "secret")` shipping the default.

---

## How Do We Apply It?

### LO 1: Protect routes with auth dependencies

**Write code:** sketch `get_current_user` + `@app.get("/me", ...)`.

**Predict before running:** No header → 401. Valid token → user id JSON.

**Explain result:** Same DI pattern as `require_token` earlier, now JWT.

---

### LO 2: RBAC basics

**Walkthrough:** Table of roles vs endpoints. Role stored in DB and copied into JWT claims `{"sub", "role", "exp"}` **or** loaded from DB after `sub`. Pick one for class: claim `role` after verify.

**Predict:** Changing role in DB without new token — discuss "stale role in JWT" in one sentence; keep simple.

---

### LO 3: Simple role check on selected routes

**Write code:**

```python
def require_admin(user: dict = Depends(get_user)):
    if user["role"] != "admin":
        raise HTTPException(403, "Admins only")
    return user
```

**Predict before running:** Student JWT on POST /clubs → 403. Admin → 200.

**Explain result:** Only selected routes list the dependency.

---

### LO 4: Secrets in environment variables

**Write code:**

```python
import os
SECRET = os.environ["JWT_SECRET"]
```

**Demo:** Run Uvicorn without env → crash. Then `export JWT_SECRET=...` and run.

**Predict before running:** Missing key raises `KeyError` (good — fail fast).

**Explain result:** Source code has no secret string.

---

### LO 5: Secure defaults and common API mistakes

**Checklist live:**

- Pydantic still on POST bodies
- No password_hash in response model
- No token in query
- `/health` public, `/me` protected
- Do not use HTTPException 200 for errors

**Predict:** Returning ORM user by accident includes hash — show response_model stripping it.

🎯 **Instructor Note:** This is not OWASP full catalog. Stay on these defaults.

---

## Live Demo Block (13 min)

Swagger: Authorize with Bearer. Hit /me. Hit admin route as student. Rotate SECRET via env, old token fails. Show `.env` in `.gitignore` mention only.

**[Script:]** "Security is mostly consistency: every sensitive route uses the same Depends, every secret comes from the environment."

---

## Recap (10 min)

🎯 **Instructor Note:** Rapid 401 vs 403. Where is SECRET?

---

## Lecture Summary

- **Auth dependencies** protect routes by verifying JWT
- **RBAC** maps roles to allowed endpoints
- A **simple role check** can lock selected routes (e.g. admin POST)
- **Environment variables** hold JWT secrets
- **Validation and tight responses** avoid common API mistakes
- **Practical value:** You can ship a small API that is not wide open — next we assemble CRUD + ORM + auth in one mini app
