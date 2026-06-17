# Lecture Script: Authentication Fundamentals
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Authentication vs Authorization Concepts | 15 min |
| 3 | Password Hashing with bcrypt | 25 min |
| 4 | JWT Tokens Introduction | 15 min |
| 5 | Creating and Verifying JWT Tokens | 25 min |
| 6 | Token Expiration and Refresh Tokens | 15 min |
| 7 | Lecture Summary and Recap | 7 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** Open with a real and well-known security failure, not an abstract description of "login systems." The goal is to make the cost of getting this wrong feel real before any code appears. Wait after the opening question — this topic deserves a moment of seriousness, not a rush into syntax.

**[Script:]**

"In 2012, LinkedIn had a database breach. Millions of passwords were exposed. Many of those passwords were stored either with no protection at all, or with a weak hashing method that was already known to be crackable. Attackers recovered a huge fraction of the actual plaintext passwords within days. People who used the same password elsewhere — which is most people — had other accounts compromised too.

This was not a complicated attack. It was a consequence of password storage built the wrong way. Today we are building it the right way.

Two separate but related problems sit at the center of almost every backend application. First: how do you confirm someone is who they claim to be — authentication. Second: once you know who they are, how do you decide what they are allowed to do — authorization. These are different problems and conflating them leads to real security bugs, not just confusing code.

Once you have authenticated someone, you need a way for them to stay logged in across multiple requests without sending their password every single time. That is what tokens are for — specifically JSON Web Tokens, JWTs, which are now the standard mechanism behind most modern API authentication.

And tokens that never expire are a liability — if one is ever stolen, it grants access forever. Tokens that expire too aggressively annoy users by logging them out constantly. The balance between security and usability is solved with short-lived access tokens and longer-lived refresh tokens working together.

Today: authentication versus authorization, hashing passwords correctly with bcrypt, what a JWT actually is, how to create and verify one, and how expiration and refresh tokens fit together into a real authentication system."

---

## Block 2 — Authentication vs Authorization Concepts

### 2A — Two Different Questions

**[Script:]**

"Authentication answers: who are you? Authorization answers: what are you allowed to do? These happen in sequence — you authenticate first, and only once your identity is established does authorization become a meaningful question.

A concrete example. A user logs in with an email and password — that is authentication. The system confirms the credentials match a known user. Once logged in, that same user tries to delete another user's account — authorization decides whether their role permits that action. They might be authenticated successfully and still be denied that specific action because they lack the required permission."

> 🎯 **Instructor Note:** Write this distinction on the board in the simplest possible form and keep it visible for the rest of the session — both later blocks build on it.

```
Authentication  → Who are you?         (login, credentials, identity)
Authorization   → What can you do?     (permissions, roles, access control)
```

**[Script:]**

"A common confusion: a 401 Unauthorized HTTP status code is actually about authentication — it means the request lacked valid credentials entirely. A 403 Forbidden status code is about authorization — it means the credentials were valid, but the action is not permitted for that identity. The naming is a long-standing historical inconsistency in the HTTP specification, but knowing the distinction helps you choose the right status code in your own APIs."

> 🎯 **Instructor Note:** Ask: "If a user is logged in correctly but tries to access an admin-only page, should the API respond with 401 or 403?" Answer: 403. They are authenticated — the system knows who they are — but not authorized for that specific action. This is a genuinely common point of confusion worth confirming out loud.

---

### 2B — Where Each Concept Lives in an Application

**[Script:]**

"Authentication typically happens once per session, or once per token, at login. Authorization happens on every single request that touches a protected resource — it is a check performed again and again, often using information that was established once during authentication.

The rest of today focuses primarily on authentication — proving identity through passwords and tokens. Authorization, the rules about who can do what once identified, depends on authentication working correctly first, which is why it is the foundation we build before anything else."

**Recap of Block 2 before moving on:**

- Authentication establishes identity — who the user is
- Authorization establishes permission — what that identified user is allowed to do
- Authentication happens first, typically at login; authorization is checked on each subsequent protected request
- 401 signals an authentication failure; 403 signals an authorization failure — the credentials were valid but the action is not permitted

---

## Block 3 — Password Hashing with bcrypt

### 3A — Why You Never Store Plaintext Passwords

**[Script:]**

"If you store a user's password exactly as they typed it — plaintext — anyone with access to the database can read every password directly. A breach, a careless backup, an internal employee with too much access: any of these exposes every user's actual password immediately.

The fix is hashing. A hash function takes an input — the password — and produces a fixed-length output that looks like random characters. Critically, a good hash function is one-way: you cannot reverse the hash to get the original password back. You store the hash, never the password itself."

> 🎯 **Instructor Note:** Draw this transformation on the board.

```
password: "mySecret123"
              ↓ hash function (one-way)
hash: "$2b$12$KIXQ7n5Z8mF...long random-looking string"

You can verify a password against the hash.
You cannot reverse the hash back into the password.
```

**[Script:]**

"At login, you do not decrypt the stored hash to compare it to what the user typed. You hash what the user typed using the same method, and compare the two hashes. If they match, the password was correct. The original password is never stored anywhere, and is never reconstructed."

---

### 3B — Why bcrypt Specifically

**[Script:]**

"Not every hash function is appropriate for passwords. Hash functions like MD5 or plain SHA-256 are fast — designed for speed, used for things like checksums. Fast is the wrong property for password hashing. If a hash function is fast, an attacker who steals a database of hashes can try billions of password guesses per second against it, hashing each guess and checking for a match. This is called a brute-force attack, and fast generic hash functions make it cheap to carry out.

bcrypt is specifically designed to be slow — deliberately, tunably slow. It also automatically incorporates something called a salt — a random value mixed into each password before hashing, unique per password. The salt means two users with the identical password get completely different hashes stored in the database. Without a salt, an attacker could precompute hashes for common passwords once and instantly recognize any match across an entire stolen database — this precomputed attack is called a rainbow table."

> 🎯 **Instructor Note:** Ask: "If Alice and Bob both use the password 'password123', and the hashing method uses no salt, what would their stored hashes look like? What if it does use a salt, like bcrypt automatically applies?" Answer: without a salt, identical input produces identical hash — both would have the exact same stored value, immediately revealing that they share a password. With bcrypt's automatic salting, their hashes look completely different despite the identical password, because each hash incorporates a unique random salt.

---

### 3C — Hashing and Verifying with bcrypt in Python

**[Script:]**

"In Python, bcrypt is available directly, or through `passlib`, a library that wraps multiple hashing algorithms behind a consistent interface. We will use bcrypt directly here since it is the algorithm itself."

```bash
pip install bcrypt
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I hash the same password 'mySecret123' twice using bcrypt, will the two resulting hashes be identical?" Let learners think. Answer: no — because bcrypt generates a new random salt each time, the two hashes will look completely different even though the underlying password is the same.

**Demo 1 — Hashing and verifying a password (whiteboard-friendly)**

```python
import bcrypt

password = b"mySecret123"
hashed = bcrypt.hashpw(password, bcrypt.gensalt())

print(hashed)

is_correct = bcrypt.checkpw(b"mySecret123", hashed)
is_wrong   = bcrypt.checkpw(b"wrongGuess", hashed)

print(is_correct, is_wrong)
```

**[Script:]**

"`bcrypt.gensalt()` generates a fresh random salt. `bcrypt.hashpw(password, salt)` produces the hash — note both arguments are bytes, not strings, which is why the password is prefixed with `b`. The printed hash is a long string starting with something like `$2b$12$` — that prefix encodes the algorithm version and the cost factor, the salt is embedded within it, and the rest is the actual hash output. You store this entire string in your database, as one value, in a single password column.

`bcrypt.checkpw(attempt, stored_hash)` is how you verify a login attempt. It extracts the salt from the stored hash, re-hashes the attempted password using that same salt, and compares the results. It returns `True` or `False` — you never decrypt anything, you only ever re-hash and compare.

`is_correct` is `True`. `is_wrong` is `False`. Run `bcrypt.hashpw` again on the same password and the output string will be different from the first time — different random salt, different final hash — yet `checkpw` against either hash with the correct password returns `True` for both."

> 🎯 **Instructor Note:** This is a genuine pause point. Ask: "If two different hashes are stored for the same password, how does checkpw still correctly verify both?" Answer: the salt is stored as part of the hash string itself. checkpw reads the embedded salt from whichever hash it is checking against, uses that specific salt to re-hash the attempt, and compares. The salt does not need to be stored separately — it travels inside the hash output.

**[Script:]**

"In a FastAPI route, the pattern is: at registration, hash the password with `bcrypt.hashpw` and store only the hash. At login, fetch the stored hash for that user's email, and call `bcrypt.checkpw` with the submitted password and the stored hash. Never compare passwords directly. Never store a password in a way that lets you retrieve the original."

**Recap of Block 3 before moving on:**

- Never store plaintext passwords; store only a one-way hash
- bcrypt is purpose-built for password hashing: deliberately slow, resistant to brute-force attacks
- bcrypt automatically generates a unique random salt per password, preventing identical passwords from producing identical hashes
- `bcrypt.hashpw(password, bcrypt.gensalt())` produces a hash; `bcrypt.checkpw(attempt, stored_hash)` verifies a login attempt
- The salt is embedded in the stored hash string — no need to store it separately

---

## Block 4 — JWT Tokens Introduction

### 4A — Why You Need a Token After Login

**[Script:]**

"Once a password is verified at login, the user needs a way to prove their identity on every subsequent request without resending their password each time — sending a password repeatedly increases exposure risk and is simply impractical for an API.

A token is issued at login: a piece of data the client stores and sends with future requests, proving they already authenticated successfully. JSON Web Token — JWT — is the standard format for this in modern web APIs."

---

### 4B — The Structure of a JWT

**[Script:]**

"A JWT is a string made of three parts, separated by dots: header, payload, and signature."

> 🎯 **Instructor Note:** Write the structure on the board exactly like this — the dot separators are visually important and learners need to see the three distinct sections.

```
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiAxMjN9.SflKxwRJSMeKKF2QT4fwpMeJf36P
└──────header──────┘└──────payload──────┘└──────signature──────┘
```

**[Script:]**

"The header describes the token — typically which algorithm was used to sign it. The payload — also called claims — contains the actual data, like the user's ID and any other information you choose to include. The signature is what makes the token trustworthy: it is a cryptographic signature computed over the header and payload, using a secret key that only your server knows.

Header and payload are just base64-encoded JSON — readable by anyone who decodes them, not encrypted. This is a critical point: a JWT is not encrypted, it is signed. Anyone who has the token can read its contents. What they cannot do is modify the contents and produce a valid signature, because they do not have the secret key."

> 🎯 **Instructor Note:** This distinction — signed versus encrypted — is the most commonly misunderstood part of JWTs. State it directly and ask a confirming question. "If I intercept someone's JWT, can I read their user ID inside it?" Answer: yes, trivially — decode the payload, it is plain JSON. "Can I change that user ID to someone else's and have the server accept it?" Answer: no — changing the payload invalidates the signature, because the signature was computed over the original content. The server recomputes the expected signature and rejects any mismatch.

**[Script:]**

"This means: never put sensitive secrets — passwords, credit card numbers — inside a JWT payload. Anyone with the token can read them. Put only the information needed to identify the user, like a user ID, and let the signature guarantee that information has not been tampered with."

**Recap of Block 4 before moving on:**

- A JWT lets a client prove a previously completed login on every subsequent request, without resending credentials
- A JWT has three dot-separated parts: header, payload, signature
- Header and payload are base64-encoded JSON — readable by anyone, not encrypted
- The signature, computed with a server-only secret key, prevents tampering — modifying the payload invalidates the signature
- Never place sensitive data inside the payload; only the signature's validity is protected, not the confidentiality of the contents

---

## Block 5 — Creating and Verifying JWT Tokens

### 5A — Setting Up

**[Script:]**

"In Python, `pyjwt` is the standard library for creating and verifying JWTs."

```bash
pip install pyjwt
```

**[Script:]**

"You will need a secret key — a long, random string known only to your server, used to sign and later verify tokens. In a real application this comes from an environment variable, never hardcoded directly into source code that might be committed to version control."

---

### 5B — Creating a Token

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I encode a payload containing `{'user_id': 1}` with a secret key, and then decode it without supplying the secret key, what happens?" Answer: decoding without the secret either fails outright or, if you skip verification entirely, returns the payload without confirming its authenticity — defeating the entire purpose. The secret is required to verify, not just to read.

**Demo 2 — Creating a JWT (whiteboard-friendly)**

```python
import jwt

SECRET_KEY = "a-very-long-random-secret-key"

payload = {"user_id": 1, "name": "Alice"}
token = jwt.encode(payload, SECRET_KEY, algorithm="HS256")

print(token)
```

**[Script:]**

"`jwt.encode(payload, secret, algorithm)` builds the token: encodes the header and payload, computes the signature using the secret and the chosen algorithm, and concatenates all three parts with dots. `HS256` is a common signing algorithm — HMAC using SHA-256 — and is the standard default for this kind of token signing.

The printed token is a single string. This is what you would return to the client after a successful login — typically in the response body, for the client to store and send back on future requests."

---

### 5C — Verifying a Token

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If someone takes the printed token, manually changes one character in the middle, and I try to decode it with the correct secret key, what happens?" Answer: `jwt.decode` raises an exception — the signature no longer matches the altered content. Even a single character change is detected.

**Demo 3 — Verifying a JWT (whiteboard-friendly)**

```python
import jwt

try:
    decoded = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
    print(decoded)
except jwt.InvalidTokenError:
    print("Invalid token")
```

**[Script:]**

"`jwt.decode(token, secret, algorithms=[...])` verifies the signature using the secret key and, if valid, returns the original payload as a Python dictionary — `{'user_id': 1, 'name': 'Alice'}`. If the signature does not match — because the token was tampered with, or because the wrong secret key was used to verify — it raises `jwt.InvalidTokenError`.

Notice `algorithms` is a list — you specify exactly which algorithms you accept. Never accept whatever algorithm the token claims to use without restricting it; that asymmetry is itself a known historical JWT vulnerability. Always pass an explicit list of allowed algorithms matching what you used to sign."

> 🎯 **Instructor Note:** Pause here. Ask: "In a FastAPI route, where would jwt.decode typically run — inside every protected route function, or somewhere shared?" Connect this back to the dependency injection concept if the audience has covered it: token verification is exactly the kind of shared logic that belongs in one dependency function, used across every protected route, rather than duplicated in each route individually.

**Recap of Block 5 before moving on:**

- `jwt.encode(payload, secret_key, algorithm)` creates a signed token from a Python dictionary
- `jwt.decode(token, secret_key, algorithms=[...])` verifies the signature and returns the original payload, or raises an exception if invalid
- Any modification to the token's content invalidates the signature and causes decoding to fail
- Always specify an explicit list of accepted algorithms when decoding — never trust an algorithm claimed by the token itself

---

## Block 6 — Token Expiration and Refresh Tokens

### 6A — Why Tokens Must Expire

**[Script:]**

"A JWT with no expiration is valid forever once issued. If it is ever stolen — through a compromised device, a network intercept, a leaked log file — the thief has permanent access, indistinguishable from the real user, until you take separate action like rotating your secret key, which would invalidate every token for every user simultaneously.

The fix is an expiration claim — a timestamp embedded in the payload specifying when the token stops being valid. After that time, even a perfectly valid signature is rejected, because the token itself declares it has expired."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If I create a token with an expiration of one second from now, wait two seconds, and then try to decode it, what happens — even with the correct secret key?" Answer: `jwt.decode` raises `jwt.ExpiredSignatureError`. The signature is mathematically valid, but the library checks the expiration claim and rejects it anyway.

**Demo 4 — Token expiration (whiteboard-friendly)**

```python
import jwt
import datetime

payload = {
    "user_id": 1,
    "exp": datetime.datetime.now(datetime.timezone.utc) + datetime.timedelta(minutes=15)
}
token = jwt.encode(payload, SECRET_KEY, algorithm="HS256")

try:
    decoded = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
except jwt.ExpiredSignatureError:
    print("Token has expired")
```

**[Script:]**

"`exp` is a standard JWT claim name — `jwt.decode` automatically checks it against the current time and raises `jwt.ExpiredSignatureError` if the token's expiration has passed. You do not write the expiration check yourself; the library handles it as long as you include the `exp` claim when creating the token.

The expiration time is a balance. Fifteen minutes is a common choice for an access token — short enough that a stolen token has a limited window of usefulness, long enough that users are not constantly forced to log in again."

---

### 6B — Refresh Tokens: Solving the Usability Problem

**[Script:]**

"A fifteen-minute access token means a user gets logged out every fifteen minutes unless something renews their session — that would be a poor experience. The solution is a second token: a refresh token, with a much longer expiration — days or weeks — used for exactly one purpose: obtaining a new access token without re-entering a password.

The flow: a user logs in once with their password, and receives both an access token and a refresh token. The access token is used for normal requests and expires quickly. When it expires, the client sends the refresh token to a dedicated endpoint, and if that refresh token is still valid, the server issues a brand new access token — without requiring the password again."

> 🎯 **Instructor Note:** Draw this flow on the board. The two-token relationship is the conceptual core of this block.

```
Login (password) → access_token (15 min) + refresh_token (7 days)

Access token expires
       ↓
Client sends refresh_token to /refresh
       ↓
Server validates refresh_token → issues new access_token

Refresh token expires → user must log in again with password
```

**[Script:]**

"This means a stolen access token is only useful for a short window. A stolen refresh token is more serious — it lasts longer — but refresh tokens are typically used far less frequently and can be stored more securely, and a real system can maintain a record of valid refresh tokens to revoke one immediately if a theft is detected, something that is much harder to do with self-contained access tokens alone."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If a refresh token expires, can the client get a new access token using only that refresh token?" Answer: no — once the refresh token itself has expired, the user must log in again with their password. The refresh token is what extends the session without a password; once it is also expired, there is nothing left to extend with.

**Demo 5 — Issuing access and refresh tokens (whiteboard-friendly)**

```python
def create_token(data: dict, expires_in: datetime.timedelta):
    payload = data.copy()
    payload["exp"] = datetime.datetime.now(datetime.timezone.utc) + expires_in
    return jwt.encode(payload, SECRET_KEY, algorithm="HS256")

access_token  = create_token({"user_id": 1}, datetime.timedelta(minutes=15))
refresh_token = create_token({"user_id": 1}, datetime.timedelta(days=7))
```

**[Script:]**

"One shared function, two different expiration windows. The structure of both tokens is identical — same encoding, same signature mechanism — the only difference is how long each one remains valid. This is the entire mechanical difference between an access token and a refresh token: duration of validity and what each is used for, not the format."

**Recap of Block 6 before moving on:**

- The `exp` claim sets a token's expiration; `jwt.decode` automatically rejects expired tokens with `jwt.ExpiredSignatureError`
- Short-lived access tokens limit the damage window if a token is stolen
- Refresh tokens are longer-lived and used solely to obtain a new access token without re-entering a password
- When the access token expires, the client uses the refresh token to get a new one; when the refresh token expires, the user must log in again
- Access and refresh tokens share the same JWT structure — they differ only in expiration duration and purpose

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. "What is the difference between 401 and 403? Why is bcrypt slow on purpose? Is a JWT encrypted or signed — what is the practical difference? What claim controls expiration? What is a refresh token actually for?" Keep the pace energetic to close strong on a topic that has been deliberately careful and serious throughout.

**Authentication vs Authorization Concepts**

- Authentication establishes who a user is; authorization establishes what that user is permitted to do
- Authentication happens at login; authorization is checked on each protected request afterward
- A 401 response signals a failed or missing authentication; a 403 signals valid authentication but insufficient permission

**Password Hashing with bcrypt**

- Plaintext passwords must never be stored; only a one-way hash is stored
- bcrypt is deliberately slow, resisting brute-force guessing, and automatically generates a unique salt per password
- `bcrypt.hashpw(password, bcrypt.gensalt())` creates a hash; `bcrypt.checkpw(attempt, stored_hash)` verifies a login attempt by re-hashing and comparing
- The salt is embedded inside the stored hash string and does not need separate storage

**JWT Tokens Introduction**

- A JWT lets a client prove a completed login on every subsequent request without resending credentials
- A JWT has three parts: header, payload, signature, separated by dots
- The payload is readable by anyone — JWTs are signed, not encrypted; never place sensitive data inside one
- The signature, computed with a server-only secret, makes any tampering with the payload detectable

**Creating and Verifying JWT Tokens**

- `jwt.encode(payload, secret_key, algorithm)` creates a signed token from a Python dictionary
- `jwt.decode(token, secret_key, algorithms=[...])` verifies the signature and returns the payload, or raises an exception if invalid
- Always pass an explicit list of accepted algorithms to `decode` rather than trusting the algorithm declared inside the token

**Token Expiration and Refresh Tokens**

- The `exp` claim sets an expiration timestamp; `jwt.decode` automatically rejects expired tokens
- Short-lived access tokens limit exposure if stolen; longer-lived refresh tokens renew access tokens without requiring the password again
- When the refresh token itself expires, the user must log in again with their credentials

**Why All of This Matters Together**

- Authentication and authorization are the gatekeeping layer of every backend application that has more than one user, and getting any piece wrong — storing passwords insecurely, trusting an unsigned or unverified token, never expiring access — turns a working application into a serious liability the moment it faces real users and real attackers
- Hashing with bcrypt, signing and verifying with JWTs, and balancing short-lived access tokens against longer-lived refresh tokens together form the standard, production-grade pattern behind login systems across the modern web — the same building blocks used at scale by real companies, now understood from first principles

---

*End of script.*
