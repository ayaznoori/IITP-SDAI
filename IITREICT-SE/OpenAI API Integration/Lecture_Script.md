# Lecture Script: OpenAI API Integration
**Duration:** 110 minutes | **Tools:** VS Code, Python venv, OpenAI SDK | **Context:** Prompt + parameter bundles from sessions 39–40

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Playground is not a product |
| Why Does This Matter? | 10 min | Keys, cost, uptime |
| What Is the Concept? | 22 min | Auth, chat roles, response, errors |
| How Do We Apply It? (LOs) | 55 min | Live Python client |
| Failure drill | 13 min | 401 vs 429 vs timeout |
| Recap | 5 min | FastAPI next |

---

## Session Opening (5 min)

**[Script:]** "A prompt in the browser is a prototype. **Flipkart**-scale features call models from **servers**. Today you wire **Python** to **Chat Completions**: auth, messages, reading the response, retries, and the knobs you already know."

**Problem hook:** Show a screenshot of a leaked key on GitHub. "This is how student credits vanish overnight."

🎯 **Instructor Note:** Provide a shared classroom key **or** students use their own. Never project a live key. Use `.env` only.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "Where should the key live: source file, README, or environment?" Wait for environment.

**[Script:]** "Support products at **Intercom**, internal tools at **Stripe**, and Indian SaaS all share one pattern: **server-side key**, structured messages, and **calm failure**. If you `print(resp)` into a user ticket, you may leak IDs and usage. If you ignore **rate limits**, one loop can freeze the feature for everyone."

**Pain if misunderstood:**
- Key in git → revoke + incident
- Wrong path into response → `None` in UI
- Retrying 400 bad request → infinite fail
- Forgetting `max_tokens` → surprise cost

---

## What Is the Concept?

### Auth

**Definition:** The API key proves your project may spend quota.

**Mental model:** ATM PIN. Not in photos. Not in chat logs.

```python
import os
from openai import OpenAI
client = OpenAI()  # reads OPENAI_API_KEY
```

### System / User / Assistant

**Definition:** Chat is a **transcript** with roles, not one blob.

**Python vs JS:** Same JSON idea as `fetch`. You already know request bodies from FastAPI.

**Common mistake:** Putting the whole few-shot script in `system` as one novel. Prefer short system + example turns.

### Response Structure

**Mental model:** Envelope (`resp`) → letter (`choices[0].message`) → body (`content`).

Also note **usage** for later LLMOps.

### Errors, Retries, Rate Limits

| Situation | Professional action |
|-----------|---------------------|
| Missing/invalid key | Fail fast, fix env, do not retry loop |
| 400 bad schema | Fix code, do not retry |
| 429 rate limit | Wait, retry few times, then user-friendly busy |
| Timeout / 5xx | Limited retries |
| Success | Use `content` |

**Backoff idea:** wait longer each retry. Do not hammer.

---

## How Do We Apply It?

### LO 1: Set up OpenAI API auth in Python

**Problem:** Script works on your laptop, fails on a friend's clone.

**Translate logic:** Friend lacks `.env`. Document `export OPENAI_API_KEY=...` without writing the value.

**Walkthrough:** venv, `pip install openai python-dotenv` **only if** class agrees dotenv is in scope — alternatively `os.environ` already set in terminal. Keep to env var.

**Demo:**

```python
import os
assert os.environ.get("OPENAI_API_KEY"), "Set OPENAI_API_KEY"
```

**Predict before running:** What happens if assert fails?

**Explain result:** Crash with a clear message beats a confusing 401 later.

---

### LO 2: Chat Completions with system, user, assistant

**Problem:** Bookstore classifier from last week, now in code.

**Write:**

```python
messages = [
    {"role": "system", "content": "JSON keys intent, urgency. No markdown."},
    {"role": "user", "content": "Book arrived wet. Need help today."},
]
```

Add few-shot as extra user/assistant pairs **if time**.

**Predict:** Which role holds the standing format rule?

**Explain result:** System = policy. User = this ticket. Assistant = prior completions in a thread.

> **In the Real World:** **Zendesk** apps keep a stable system prompt and swap the ticket body in `user`.

---

### LO 3: Read the response structure

**Live call (instructor model name):**

```python
resp = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=messages,
)
print(resp.choices[0].message.content)
print(resp.usage)
```

**Predict before running:** Is `content` a dict or a string?

**Explain result:** String (maybe JSON text). You `json.loads` next if needed. `usage` shows token spend.

🎯 **Instructor Note:** If SDK objects differ slightly, show `.model_dump()` or dict access — stay on `choices / message / content`.

---

### LO 4: Handle errors, retries, rate limits professionally

**Problem:** Demo airplane mode or invalid key **on purpose** (separate snippet).

**Translate logic:**

```python
import time

def complete(messages):
    last_err = None
    for attempt in range(3):
        try:
            return client.chat.completions.create(
                model="gpt-4o-mini",
                messages=messages,
                temperature=0,
                max_tokens=64,
            )
        except Exception as e:
            last_err = e
            time.sleep(1 * (attempt + 1))
    raise last_err
```

**Predict:** Is this perfect production code? (No — it retries even 401. Improve live.)

**Explain result:** Professionals **classify** errors. Retry rate limits and timeouts. Do not retry auth failures. User message: "Service busy. Try again." Never leak `e` internals to end users.

---

### LO 5: Pass temperature, max_tokens, stop

**Reuse Team A bundle** from last session:

```python
resp = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=messages,
    temperature=0,
    max_tokens=80,
    stop=["\n\n"],
)
```

**Predict:** If `stop` never appears, does the call still succeed?

**Explain result:** Stop is optional. Max tokens still caps cost. Temperature 0 matches classifier.

---

## Failure Drill (13 min)

Pairs map four cards: 401, 429, 400, timeout → retry? user copy? log what?

Share one **bad** log line that prints the API key. Rewrite it.

---

## Recap (5 min)

**[Script:]** "Next you wrap this client in **FastAPI** and treat user text as **untrusted**. The SDK call stays the same. The **door** changes."

---

## Lecture Summary

- **Auth** belongs in environment variables, never in git
- **Chat Completions** send **system / user / assistant** messages
- Read **`choices[0].message.content`** (and usage when useful)
- **Retry** transient limits and timeouts; do not retry bad keys or bad requests forever
- Pass **temperature, max_tokens, stop** from your product bundle
- **Practical value:** You can now call an LLM from Python the way shipping products do
