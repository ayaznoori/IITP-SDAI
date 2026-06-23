# Lecture Script: Advanced Security and Authorization
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | OAuth2 Implementation Basics | 25 min |
| 3 | Protecting Routes with Dependencies | 20 min |
| 4 | Role-Based Access Control (RBAC) | 25 min |
| 5 | Security Headers and Best Practices | 15 min |
| 6 | API Key Authentication | 10 min |
| 7 | Lecture Summary and Recap | 5 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has built JWT-based login systems and understands authentication vs authorization at a basic level. The hook should address the real-world problems that arise once applications have users with different permission levels, external authentication needs, or distributed systems. Open with a scenario that makes the jump from "authentication works" to "real permission problems" clear. Wait after the opening question.

**[Script:]**

"You have a working authentication system. Users can log in with their credentials, get a JWT, and use that JWT to access protected routes. That works perfectly for an application where every authenticated user has identical permissions. But the moment you need roles — an admin who can delete users, a regular user who can edit only their own posts, an API consumer with read-only access — a simple JWT with a user ID inside is no longer enough. You need a permission system.

A second problem: what if you want to let users log in through their Google or GitHub account instead of managing passwords yourself? That is not a nice-to-have — it is becoming the default expectation. Rolling your own single-sign-on is a security minefield. Using OAuth2 is the standard solution.

Third: your API is not just used by your own web frontend. It is used by mobile apps, third-party integrations, and services that do not have a user account. These need a different way to authenticate — typically an API key, generated once and used repeatedly for direct service-to-service communication.

And finally: you have implemented authentication correctly, you have set up permissions, but attackers are still finding ways in through surprising vectors — missing security headers that would prevent clickjacking, responses that leak sensitive information, cookies without the right flags. Security is not just about who is logged in; it is defense in depth.

Today covers all of it: OAuth2 as a standard for delegated authentication, protecting routes with dependency injection to enforce permissions, role-based access control so multiple permission levels work consistently, security headers that prevent entire classes of attacks, and API keys for service-to-service authentication. These five cover the majority of the security concerns in a real backend that grows beyond a one-developer project."

---

## Block 2 — OAuth2 Implementation Basics

### 2A — What OAuth2 Is and Why It Exists

**[Script:]**

"OAuth2 is a standard protocol for delegated authentication. It solves a specific problem: how do I let a user prove they are who they are without them ever giving me their actual credentials?

The classic example: a user wants to log in with their Google account. Without OAuth2, you would ask them for their Google email and password. You would have their Google credentials in your database. That is a disaster — you are now responsible for protecting their Google password, an attacker stealing your database gets direct access to their Google account, and you're holding secret credentials you should never have access to.

OAuth2 solves this by never touching the user's actual credentials. The user logs in directly with Google's servers — not your application. Google verifies who they are. If they consent to share their identity with your application, Google sends your application a token proving the user was authenticated. Your application uses that token and never sees the password."

> 🎯 **Instructor Note:** Draw the flow on the board to make the delegation clear.

```
Old way (bad):
User → gives password to your app → you store it → security disaster

OAuth2 way (good):
User → redirected to Google
User → logs in with Google directly (password never seen by your app)
Google → checks if user consents to share identity
Google → redirects back to your app with a token
Your app → uses token to confirm identity
```

**[Script:]**

"The token is what matters. Your application does not need the password. It only needs the token proving the user was authenticated by a trusted authority — Google, GitHub, any OAuth2 provider.

OAuth2 is a standard, which means it works the same way for hundreds of different providers. Once you understand the pattern with one provider, you can integrate another almost mechanically."

---

### 2B — The OAuth2 Flow in FastAPI

**[Script:]**

"Implementing OAuth2 in FastAPI typically means using a library like `authlib` or `python-oauth2` that handles the protocol details. But understanding the conceptual flow is more important than memorizing API calls.

The steps: redirect the user to the provider's login page, the provider redirects back with a code, you exchange that code for a token, you use the token to get the user's information, you create or retrieve that user in your database, you issue your own JWT so the user stays logged in across requests."

> 🎯 **Instructor Note:** This is the one place where we will not write a full working demo — OAuth2 integration requires external services and configuration that does not fit a whiteboard example. Instead, name the pattern and show the shape of the code.

**Demo 1 — OAuth2 flow structure in FastAPI (whiteboard-friendly, pseudocode)**

```python
from fastapi import FastAPI
from authlib.integrations.starlette_client import OAuth

app = FastAPI()
oauth = OAuth()

oauth.register(
    name='google',
    client_id='YOUR_GOOGLE_CLIENT_ID',
    client_secret='YOUR_GOOGLE_CLIENT_SECRET',
    server_metadata_url='https://accounts.google.com/.well-known/openid-configuration',
    client_kwargs={'scope': 'openid email profile'}
)

@app.get('/login')
async def login(request: Request):
    redirect_uri = request.url_for('callback')
    return await oauth.google.authorize_redirect(request, redirect_uri)

@app.get('/callback')
async def callback(request: Request):
    token = await oauth.google.authorize_access_token(request)
    user_info = token.get('userinfo')
    # Create or get user from database using user_info
    # Issue your own JWT
    return {"access_token": your_jwt, "token_type": "bearer"}
```

**[Script:]**

"The shape: register the provider with credentials and configuration, an endpoint that redirects the user to the provider, a callback endpoint that handles the provider's response, extract the user information from the token, and issue your own JWT for the session.

The library handles the cryptographic signature verification, the redirect URLs, and the token exchange. Your job is to take the user information and decide whether to create a new user in your database or retrieve an existing one. Then you issue a JWT exactly as you did in the authentication session — the difference is where the identity proof came from."

> 🎯 **Instructor Note:** Ask: "Why do you issue your own JWT at the end instead of just using the provider's token?" Answer: the provider's token is for authenticating with the provider; your JWT is for authenticating with your application. Your JWT can carry permissions, roles, and application-specific data. It is scoped to your system. This is the bridge between OAuth2 (proving identity) and your application's authorization system.

**Recap of Block 2 before moving on:**

- OAuth2 is a standard for delegated authentication where the user authenticates with a trusted provider, not directly with your application
- The provider issues a token proving the user was authenticated; your application uses that token without ever seeing the user's actual credentials
- In FastAPI, OAuth2 integration typically means using a library that handles protocol details; the core job is to extract user information and issue your own JWT
- OAuth2 works with hundreds of providers — the pattern is standard, the configuration differs

---

## Block 3 — Protecting Routes with Dependencies

### 3A — Dependencies for Authentication and Authorization

**[Script:]**

"You already know FastAPI dependencies from prior sessions. A dependency is a function that runs automatically before a route handler, and if it raises an exception, the route never runs. This is exactly the right mechanism for protecting routes.

A typical pattern: a dependency that verifies the JWT, extracts the user ID, and injects the user object into the route. Another dependency that checks whether the user has a specific role or permission, and raises an exception if they do not."

> 🎯 **Instructor Note:** This is consolidation of material learners have seen, but in a security context. The dependency pattern is not new; the application to authorization is.

---

### 3B — Writing Authentication and Authorization Dependencies

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "If a route depends on `verify_token`, and the token is invalid or missing, what happens to the route?" Answer: the dependency raises an exception before the route runs; the client gets a 401 or 403 response depending on what we choose.

**Demo 2 — Authentication and authorization dependencies (whiteboard-friendly)**

```python
from fastapi import Depends, HTTPException, status
import jwt

def verify_token(token: str = Header(...)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
        user_id = payload.get("user_id")
        if not user_id:
            raise HTTPException(status_code=401, detail="Invalid token")
        return user_id
    except jwt.InvalidTokenError:
        raise HTTPException(status_code=401, detail="Invalid token")

def verify_admin(user_id: int = Depends(verify_token)):
    user = db.get(User, user_id)
    if not user or user.role != "admin":
        raise HTTPException(status_code=403, detail="Admin access required")
    return user

@app.delete("/users/{user_id}")
def delete_user(user_id: int, admin: User = Depends(verify_admin)):
    db.delete(db.get(User, user_id))
    db.commit()
    return {"deleted": user_id}
```

**[Script:]**

"`verify_token` extracts and validates the JWT. If anything fails, it raises a 401. If valid, it returns the user ID.

`verify_admin` depends on `verify_token` — it gets the user ID, fetches the full user object, and checks if their role is admin. If not, it raises a 403 — not a 401, because the user is authenticated, they are just not authorized for this action.

The delete endpoint depends on `verify_admin`, which means it can only be called by an admin user. The dependency chain automatically protects the route. Change the delete logic, and the protection stays in place. Add ten more admin-only routes, and each one gets the protection for free by depending on `verify_admin`."

> 🎯 **Instructor Note:** Emphasize the composition: `verify_admin` depends on `verify_token`, and the route depends on `verify_admin`. Each layer does one thing — token validation, user lookup, role check — and the chain combines them. This is both clean code and flexible security.

**Recap of Block 3 before moving on:**

- FastAPI dependencies run before a route handler; if they raise an exception, the route never executes
- Authentication dependencies verify credentials and extract identity; authorization dependencies check permissions and raise 403 if denied
- Dependencies can depend on other dependencies, creating a chain: token → user → role check → route
- The same dependency can be reused across many routes — changing the dependency logic automatically updates all routes using it

---

## Block 4 — Role-Based Access Control (RBAC)

### 4A — What RBAC Is

**[Script:]**

"Role-based access control — RBAC — is a permission system based on roles. Instead of assigning individual permissions to every user — this user can edit posts, this user can delete comments, this user can view reports — you define roles, assign permissions to roles, and assign users to roles.

A user might have the admin role, which grants all permissions. Another user has the editor role, which allows editing existing posts but not deleting them. Another has the viewer role, which allows only reading. One user can have multiple roles."

> 🎯 **Instructor Note:** Write a small role-permission matrix on the board.

```
Roles and permissions:
admin:    delete_users, delete_posts, view_reports
editor:   create_posts, edit_own_posts, delete_own_posts
viewer:   read_posts, read_comments
```

**[Script:]**

"The benefit is simplicity at scale. A new feature needs a new permission. Instead of finding and updating every user individually, you assign the permission to a role. All users with that role now have access."

---

### 4B — Implementing RBAC in FastAPI

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If a user has role='editor' and the route requires permission 'delete_users', what should happen?" Answer: the dependency checks whether the editor role has the delete_users permission, finds it does not, and raises a 403. The check happens at dependency time, before the route runs.

**Demo 3 — Role-based access control (whiteboard-friendly)**

```python
from enum import Enum

class Role(str, Enum):
    admin = "admin"
    editor = "editor"
    viewer = "viewer"

ROLE_PERMISSIONS = {
    "admin": ["create", "read", "update", "delete"],
    "editor": ["create", "read", "update"],
    "viewer": ["read"]
}

def verify_permission(required_permission: str):
    def check_permission(user_id: int = Depends(verify_token)):
        user = db.get(User, user_id)
        permissions = ROLE_PERMISSIONS.get(user.role, [])
        if required_permission not in permissions:
            raise HTTPException(status_code=403, detail="Permission denied")
        return user
    return check_permission

@app.delete("/posts/{post_id}")
def delete_post(post_id: int, user: User = Depends(verify_permission("delete"))):
    db.delete(db.get(Post, post_id))
    db.commit()
    return {"deleted": post_id}
```

**[Script:]**

"`ROLE_PERMISSIONS` is a simple dict mapping roles to the permissions they have. `verify_permission` is a factory function — it takes a required permission and returns a dependency function that checks whether the authenticated user's role has that permission.

The route uses `Depends(verify_permission('delete'))`, which means the user must have the delete permission. If they have the admin or editor role but not the viewer role, the check passes or fails based on whether the permission is in that role's list, not based on the specific role name."

> 🎯 **Instructor Note:** The factory-function pattern is where learners sometimes get stuck. Explain it clearly: "verify_permission is a function that returns a dependency function. When you call `verify_permission('delete')`, you get back a specific dependency that checks for the delete permission. FastAPI calls that returned function before the route runs." This nesting is powerful but worth explicitly describing.

**[Script:]**

"This pattern scales. Add a new role, define its permissions, assign it to users, and any route already using the RBAC system automatically respects the new role. Create a new feature that needs a new permission, add it to the ROLE_PERMISSIONS dict, declare it on routes, and the system enforces it."

**Recap of Block 4 before moving on:**

- RBAC assigns permissions to roles and users to roles, instead of individual permissions per user
- A ROLE_PERMISSIONS dict maps roles to the list of permissions they have
- A dependency factory function checks whether an authenticated user's role has a required permission
- Adding a new role or permission only requires updating the mapping, not changing individual routes

---

## Block 5 — Security Headers and Best Practices

### 5A — What Security Headers Do

**[Script:]**

"HTTP response headers can tell a browser how to handle content, what requests to allow, and what attacks to defend against. These headers are the first line of defense against certain classes of attacks.

There are several critical ones to know about."

> 🎯 **Instructor Note:** Write these on the board as a list learners can reference.

```
X-Frame-Options: DENY                  — prevent clickjacking
X-Content-Type-Options: nosniff        — prevent MIME-type sniffing
Strict-Transport-Security: max-age=... — force HTTPS
Content-Security-Policy: ...           — restrict where scripts can load from
X-XSS-Protection: 1; mode=block        — enable browser XSS filtering
```

**[Script:]**

"`X-Frame-Options: DENY` prevents your page from being embedded in an iframe on another site — a common setup for clickjacking attacks where an attacker makes a user unknowingly click something by embedding it invisibly.

`X-Content-Type-Options: nosniff` tells the browser: trust the Content-Type header I send; do not try to guess the type. Without this, a browser might treat a file I said was text/plain as if it were application/javascript and run scripts in it.

`Strict-Transport-Security` forces HTTPS. If a user ever accidentally visits your site over HTTP, this header tells their browser to always use HTTPS next time, preventing man-in-the-middle attacks.

`Content-Security-Policy` is powerful but complex — it restricts where inline scripts can run, where stylesheets can load from, and more. A restrictive CSP is a strong defense against many injection attacks.

Adding these headers to every response is defense in depth. A user with an old browser might not respect them, but a modern browser will apply these protections automatically."

---

### 5B — Adding Security Headers in FastAPI

**Demo 4 — Security headers middleware (whiteboard-friendly)**

```python
from fastapi import FastAPI
from fastapi.middleware.base import BaseHTTPMiddleware
from starlette.responses import Response

class SecurityHeadersMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        response = await call_next(request)
        response.headers["X-Frame-Options"] = "DENY"
        response.headers["X-Content-Type-Options"] = "nosniff"
        response.headers["Strict-Transport-Security"] = "max-age=31536000; includeSubDomains"
        response.headers["Content-Security-Policy"] = "default-src 'self'"
        return response

app = FastAPI()
app.add_middleware(SecurityHeadersMiddleware)
```

**[Script:]**

"A middleware adds headers to every response automatically. The three settings: X-Frame-Options prevents framing, X-Content-Type-Options prevents MIME sniffing, Strict-Transport-Security forces HTTPS with a year-long timeout.

Content-Security-Policy with `default-src 'self'` is restrictive — scripts can only load from your own domain. Adjust it based on your actual needs, but start restrictive and loosen only when necessary."

> 🎯 **Instructor Note:** Ask: "If I add these headers but my application is running over HTTP, not HTTPS, what protection am I missing?" Answer: Strict-Transport-Security requires HTTPS. If you are not using HTTPS, include everything else but acknowledge that HTTPS is foundational. Do not recommend using that specific header without HTTPS.

**Recap of Block 5 before moving on:**

- Security headers tell browsers how to defend against specific attack vectors — clickjacking, MIME sniffing, injection
- `X-Frame-Options: DENY` prevents clickjacking; `X-Content-Type-Options: nosniff` prevents MIME sniffing; `Strict-Transport-Security` forces HTTPS
- Middleware adds these headers to every response automatically, ensuring consistent defense across all endpoints
- Start with a restrictive Content-Security-Policy and loosen it only as needed for legitimate use cases

---

## Block 6 — API Key Authentication

### 6A — Why API Keys Exist

**[Script:]**

"Not all API access comes from users logging in with passwords. External services, mobile apps, batch jobs, and third-party integrations need a way to authenticate without involving a user account or a password.

An API key is generated once, stored by the client, and sent with every request to prove identity. Unlike a password, an API key is machine-readable, application-specific, and can be revoked independently without changing a password."

**[Script:]**

"API keys are less secure than OAuth2 for user-facing scenarios — they are long-lived, cannot be easily rotated for every request, and if leaked, grant full access until explicitly revoked. But for service-to-service communication, they are standard and appropriate. The client stores the key securely, and the server validates it on each request."

---

### 6B — Implementing API Key Authentication

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If an API key is sent in the X-API-Key header and does not match any valid key in the database, what status code should the route return?" Answer: 401 — the request lacked valid credentials. Same as a missing or invalid JWT.

**Demo 5 — API key authentication (whiteboard-friendly)**

```python
from fastapi import Security, HTTPException, status
from fastapi.security import APIKeyHeader

api_key_header = APIKeyHeader(name="X-API-Key")

def verify_api_key(api_key: str = Security(api_key_header)):
    db_key = db.query(APIKey).filter(APIKey.key == api_key).first()
    if not db_key:
        raise HTTPException(status_code=401, detail="Invalid API key")
    if db_key.is_revoked:
        raise HTTPException(status_code=401, detail="API key revoked")
    return db_key

@app.get("/data")
def get_data(api_key: APIKey = Depends(verify_api_key)):
    return {"data": "only accessible with valid API key"}
```

**[Script:]**

"`APIKeyHeader` from FastAPI's security module defines that the API key comes in the X-API-Key header. `verify_api_key` looks it up in the database, checks it is not revoked, and returns the key object if valid, or raises a 401 if not.

The route depends on `verify_api_key`. The dependency runs before the route, so the route only executes if the API key is valid.

API keys stored in the database should be hashed with bcrypt — not stored plaintext. When the client sends the key in the request, you hash it and compare the hashes, same pattern as password verification."

> 🎯 **Instructor Note:** Emphasize: "API keys should be hashed before storage, just like passwords. If you store them in plaintext and the database is compromised, the API key is compromised." This connects back to password hashing best practices from the authentication session.

**[Script:]**

"To revoke an API key, you mark it `is_revoked = True` in the database. The next request with that key fails. No new secret needs to be deployed. This is why API keys are better for programmatic access than passwords — you can revoke one without changing a user's credentials."

**Recap of Block 6 before moving on:**

- API keys are for service-to-service or application-to-application authentication, where no user account exists
- A key is generated once, stored by the client, sent with requests, and verified against a database on the server
- API keys can be revoked independently without affecting other credentials
- Like passwords, API keys should be hashed before storage and compared via hash verification

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming each point. "What does OAuth2 protect you from doing wrong? What is the dependency chain for authentication and authorization? How does RBAC scale better than per-user permissions? Why should API keys be hashed? Why do security headers need to be on every response?"

**OAuth2 Implementation Basics**

- OAuth2 is a standard for delegated authentication where users authenticate with a trusted provider, not directly with your application
- The provider issues a token proving authentication; your application uses that token without handling the user's actual credentials
- After OAuth2 authentication, you extract user information, create or retrieve the user in your database, and issue your own JWT for your application's session

**Protecting Routes with Dependencies**

- FastAPI dependencies run before a route handler; if they raise an exception, the route never executes
- Authentication dependencies verify credentials and extract identity; authorization dependencies check permissions
- Dependencies can depend on other dependencies, creating a chain that combines token validation, user lookup, and permission checks
- The same dependency protects every route that depends on it — updating the logic automatically updates all protected routes

**Role-Based Access Control (RBAC)**

- RBAC assigns permissions to roles and users to roles, instead of individual permissions per user
- A role-permission mapping defines what actions each role can perform
- A dependency factory function checks whether an authenticated user's role has a required permission
- Adding roles or permissions requires only updating the mapping, not modifying individual routes

**Security Headers and Best Practices**

- Security headers tell browsers how to defend against specific attack vectors
- `X-Frame-Options: DENY` prevents clickjacking; `X-Content-Type-Options: nosniff` prevents MIME sniffing; `Strict-Transport-Security` forces HTTPS
- Middleware adds security headers to every response automatically, ensuring consistent defense
- Content-Security-Policy restricts where scripts can load from and is a powerful defense against injection attacks

**API Key Authentication**

- API keys are for service-to-service authentication where no user account exists
- A key is generated once, stored by the client, sent with requests, and verified against a database
- API keys should be hashed before storage and verified using hash comparison, same as passwords
- API keys can be revoked independently by marking them revoked in the database

**Why All of This Matters Together**

- OAuth2, role-based access control, protected routes, security headers, and API keys together form a defense-in-depth authentication and authorization system that handles multiple access patterns — user login, external services, delegated providers — and defends against the full range of attack vectors that appear in real production systems; knowing all five means you are no longer building simple single-user prototypes but architecting systems where security is not an afterthought but built into every layer from the start

---

*End of script.*