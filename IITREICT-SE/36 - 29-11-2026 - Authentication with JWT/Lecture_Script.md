# Lecture Script: Authentication with JWT
**Duration:** 110 minutes | **Tools:** VS Code, venv, Swagger | **Packages:** bcrypt, PyJWT | **Language:** FastAPI

**Agenda:** Opening 8 · Why 12 · Concepts 18 · LO walkthroughs 50 · Live demo 12 · Recap 10

---

## Session Opening (8 min)

**[Script:]** "Anyone can GET /health. Anyone must **not** GET /grades. Today we prove **who** the caller is: hash passwords, mint a **JWT**, expire it. Tomorrow we **use** the token on routes."

**Problem:** Campus API has POST /students open. A stranger adds fake students. We need login.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show a leaked `.db` with column `password` in plain text. Class reaction. Then hashes.

**[Script:]** "**WhatsApp Web**, **LinkedIn**, **SBI net banking** — authentication first. If you store plain passwords, one backup leak is game over. If you skip `exp`, a token in a screenshot works forever. JWT is how FastAPI APIs usually stay stateless for SPAs."

- **Real-world use:** Mobile login, SPA `Authorization` header later
- **Pain if misunderstood:** JWT in URL query (logs leak); HS256 secret committed to Git (next session env vars)

---

## What Is the Concept?

**Authentication** = identity. **Authorization** = permissions (preview only).

**bcrypt** = slow hash + salt for passwords.

**JWT** = signed payload. Server verifies with secret.

**Mental model:** Stamp a wristband (`exp` is when it peels off). Gate checks stamp, not your password, on every ride.

**Common mistakes:** Encrypt vs hash (passwords are hashed); putting password in JWT payload; using `none` algorithm.

**Python vs JS:** `localStorage` stores the token on the client. Server only issues and verifies. Do not deep-dive XSS.

---

## How Do We Apply It?

### LO 1: Authentication vs authorization

**Walkthrough:** Login vs "delete club." Two questions.

**Predict:** A valid JWT might still be forbidden from admin routes later. Today we only issue identity.

---

### LO 2: Hash and verify with bcrypt

**Write code:**

```python
import bcrypt
h = bcrypt.hashpw(b"passw0rd", bcrypt.gensalt())
print(bcrypt.checkpw(b"passw0rd", h), bcrypt.checkpw(b"nope", h))
```

**Predict before running:** `True` then `False`.

**Explain result:** Store `h` in DB, never `passw0rd`.

---

### LO 3: Create and verify JWT in FastAPI

**Write code:**

```python
import jwt
from datetime import datetime, timedelta, timezone
SECRET = "dev"
tok = jwt.encode({"sub": "42", "exp": datetime.now(timezone.utc) + timedelta(minutes=15)}, SECRET, algorithm="HS256")
print(jwt.decode(tok, SECRET, algorithms=["HS256"]))
```

**Predict before running:** Printed dict includes `sub` and `exp`.

**Explain result:** Same secret both ways. Tampered token fails decode.

---

### LO 4: Login endpoint returning JWT

**Write code:**

```python
@app.post("/login")
def login(body: dict):
    # demo user: email asha@campus.edu, hash precomputed
    if body.get("email") != "asha@campus.edu":
        raise HTTPException(401, "Bad credentials")
    if not bcrypt.checkpw(body["password"].encode(), stored_hash):
        raise HTTPException(401, "Bad credentials")
    token = jwt.encode({"sub": "1", "exp": datetime.now(timezone.utc) + timedelta(minutes=30)}, SECRET, algorithm="HS256")
    return {"access_token": token}
```

**Predict before running:** Wrong password → 401. Right → token string.

**Explain result:** Same error message for bad email or password (no user enumeration lecture — one line: same 401).

---

### LO 5: Token expiration

**Demo:** Encode with `exp` in the past. Decode → exception.

**Predict before running:** Expired token does not verify.

**Explain result:** Safer API access; client must login again.

🎯 **Instructor Note:** Swagger Authorize is next session. Today copy token from login JSON only.

---

## Live Demo Block (12 min)

Seed one hashed user. POST /login in `/docs`. Paste token into jwt.io **payload view only** (warn: secrets). Show `exp`. Fail login. Fail expired token in a tiny `/debug-decode` instructor-only function, then delete it.

**[Script:]** "Never put the password in the JWT. The token **replaces** sending the password on every request."

---

## Recap (10 min)

🎯 **Instructor Note:** "Authn or authz: bcrypt? JWT exp? Admin-only DELETE?"

---

## Lecture Summary

- **Authentication** identifies the user; **authorization** is access rules (next)
- **bcrypt** hashes and verifies passwords
- **JWT encode/decode** creates and verifies tokens in FastAPI
- **POST /login** returns a JWT after a successful check
- **Expiration** limits stolen-token lifetime
- **Practical value:** You can issue identity — next we lock routes and add roles
