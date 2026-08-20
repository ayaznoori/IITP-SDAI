# Pre-Read: Authentication with JWT

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 2: Web Fundamentals<br/><i>[HTTP · Cookies intro]</i><br/>Clients send credentials"]
        P2["<b>Previous Module</b><br/>Module 1: Developer Setup<br/><i>[Python · secrets mindset]</i><br/>Do not hardcode blindly"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 3: FastAPI Backend<br/><i>[ORM · Depends]</i><br/>Routes and reusable gates"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>Authentication with JWT<br/><i>Mental shift:</i> from <b>open APIs</b> to <b>proven identity</b><br/>bcrypt · login · expiring JWT"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>Token for protected routes next<br/>Capstone login flow"]
        RL["<b>Real-Life Use</b><br/>SPA login · mobile APIs · session-less auth"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 4: LLM & OpenAI APIs<br/><i>[API keys]</i><br/>Different secret; same care"]
        U2["<b>Upcoming Module</b><br/>Module 6: Shipping AI Apps<br/><i>[Secrets · Deploy]</i><br/>JWT secret in env"]
        U3["<b>Upcoming Module</b><br/>Module 7: Capstone<br/><i>[Auth]</i><br/>Login in the product"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Careful Secrets&nbsp;| CM
    CM ==>|&nbsp;Builds on&nbsp;| CS
    CS ==>|&nbsp;Course Path&nbsp;| CV
    CS ==>|&nbsp;Real-Life Use&nbsp;| RL
    CS ==>|&nbsp;Next Module&nbsp;| U1
    U1 -.-> U2
    U2 -.-> U3

    classDef previous fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px,color:#1a1a1a
    classDef current fill:#FFF3CD,stroke:#E6A817,stroke-width:3px,color:#1a1a1a
    classDef value fill:#D4EDDA,stroke:#28A745,stroke-width:2px,color:#1a1a1a
    classDef future fill:#F3E8FF,stroke:#9B59B6,stroke-width:2px,color:#1a1a1a

    class P1,P2,CM previous
    class CS current
    class CV,RL value
    class U1,U2,U3 future

    linkStyle default stroke-width:3px
```

## 1. What You'll Learn

In this pre-read, you'll discover:

- The difference between **authentication** (who you are) and **authorization** (what you may do)
- How to **hash and verify passwords with bcrypt** — never store plain passwords
- How to **create and verify JWT** tokens in FastAPI
- How a **login endpoint** returns a JWT after a successful password check
- Why **token expiration** makes stolen tokens less dangerous

---

## 2. Detailed Explanation

### Authn vs Authz

**Authentication** answers: "Are you Asha?"  
**Authorization** answers: "May Asha delete this club?" (next session)

Today is mostly authentication: prove identity, issue a token.

**Analogy:** College ID card. **Authentication** is the photo check at the gate. **Authorization** is whether that ID opens the faculty lounge.

> **In the Real World:** **Gmail** login is authentication. Only the mailbox owner can delete mail — authorization. **IRCTC** login vs "cancel this PNR" is the same split.

**Why It Matters**

- APIs on the public internet cannot trust the client
- Plain-text passwords in SQLite are a disaster if the file leaks
- JWTs let the server stay stateless: no "memory of login" required for each GET

### Messy to Clear

**Messy:** `if password == user.password` stored as `"secret123"`.

**Clear:** Store a **bcrypt hash**. Verify with `bcrypt.checkpw`.

```python
import bcrypt

hashed = bcrypt.hashpw(b"secret123", bcrypt.gensalt())
ok = bcrypt.checkpw(b"secret123", hashed)
```

Hashing is one-way. Even the server should not "see" the original password again.

### JWT — A Signed Hall Pass

A **JWT** (JSON Web Token) is three Base64 parts: header, payload, signature.

The **payload** might hold `sub` (user id) and `exp` (expiry). The **signature** proves the server issued it.

```python
from datetime import datetime, timedelta, timezone
import jwt  # PyJWT

SECRET = "dev-only-change-me"
token = jwt.encode(
    {"sub": "1", "exp": datetime.now(timezone.utc) + timedelta(minutes=30)},
    SECRET,
    algorithm="HS256",
)
payload = jwt.decode(token, SECRET, algorithms=["HS256"])
```

**Create** = `encode`. **Verify** = `decode` with the same secret. Wrong secret or bad token → error.

### Login Endpoint

1. Client POST `{"email", "password"}`  
2. Load user, `checkpw`  
3. If ok, `encode` JWT, return `{"access_token": token}`  
4. If not, 401

Protecting routes with that token is the next session's main event. Today, login **returns** the JWT.

### Expiration

`exp` is a Unix time. After it, `decode` fails. Short-lived tokens (e.g. 30 minutes) limit damage if someone copies the token from DevTools.

---

## 3. Practice Exercises

**Exercise 1 — Authn or authz (3 min)**  
"Show student ID." "Only admins may POST /clubs." Label each.

**Exercise 2 — Predict bcrypt (3 min)**  
Two hashes of the same password look different (salt). Can `checkpw` still return True? Why use salt at all, in one phrase?

**Exercise 3 — JWT parts (3 min)**  
A JWT has three segments separated by dots. Which part stops a student from forging `sub=admin`?

**Exercise 4 — Login flow (4 min)**  
Order these: verify hash, return 401, encode JWT, read email from body. What is missing if we skip expiration?

**Exercise 5 — Real-world (4 min)**  
You leave a **GitHub** session open on a lab PC. Why do sites still expire tokens/sessions? Connect to `exp`.
