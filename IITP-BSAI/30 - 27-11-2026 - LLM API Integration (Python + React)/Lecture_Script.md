# Lecture Script: LLM API Integration (Python + React)
**Duration:** 110 minutes | **Tools:** Python, FastAPI, React, provider dashboard (OpenAI or Claude) | **Language:** Python + JSX

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Keys in the browser are a bill |
| Why Does This Matter? | 12 min | Product architecture |
| What Is the Concept? | 20 min | Auth header, one endpoint |
| How Do We Apply It? (LOs) | 55 min | Python → FastAPI → React → errors |
| Live lab | 10 min | End-to-end one prompt |
| Recap | 5 min | Specs next |

---

## Session Opening (8 min)

**[Script:]** "Last time you designed prompts. Today those prompts become **server code**. Python calls the **chat API with auth**. FastAPI exposes **one endpoint**. React **fetches** and **renders**. If the call fails, the UI says so. The **system prompt and API key never ship in JavaScript**."

**Real-world hook:** Open DevTools on a student site that accidentally used `VITE_` for a secret (or simulate). "This is how invoices happen."

🎯 **Instructor Note:** One provider for the room. Free-tier rate limits: have a canned JSON fallback for demo if the network dies. Do not teach streaming or tool calling.

---

## Why Does This Matter?

🎯 **Instructor Note:** Draw three boxes. Ask where the key belongs. Wait until the class says “server.”

**[Script:]** "**Intercom**, **Notion AI**, and **Google** AI features all have a backend hop. Your BSAI stack is FastAPI, so that hop is a single route. Hiring managers ask: did you put the key in React? If yes, the interview cools. If no, you sound like you have shipped."

**Pain if misunderstood:**
- CORS confusion — fix CORS, do not move the key to Vite
- Showing raw provider errors to users
- Sending the full system prompt from the client (users will jailbreak it)

> **In the Real World:** **Slack** AI features fail gracefully with a toast. Your `error` state is that toast.

---

## What Is the Concept?

**Auth:** `Authorization: Bearer <key>` from env.

**Wrapper endpoint:** app-specific, not a generic proxy of every provider feature.

**UI states:** loading, success, error.

**Common mistake:** Logging response objects that include secrets.

---

## How Do We Apply It?

### LO 1: Call an LLM chat API from Python with authentication

```python
key = os.getenv("LLM_API_KEY")
headers = {"Authorization": f"Bearer {key}"}
```

**Predict before running:** Empty key — what should happen? (Fail fast, 500 with generic message.)

**Walkthrough:** messages array with system + user. Print only the assistant text.

🎯 **Instructor Note:** Match the provider’s actual small example from their docs. Keep under ten lines in the live snippet.

---

### LO 2: Expose the call behind one FastAPI endpoint

```python
@app.post("/ai/summarise")
def summarise(item: TextIn):
    return {"result": call_llm(item.text)}
```

**Predict:** Should this be GET with the essay in the query string? (No — use POST body.)

**Explain result:** Swagger can test the AI route without React.

---

### LO 3: Call that endpoint from React and show the result

```jsx
const res = await fetch(`${api}/ai/summarise`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ text }),
});
const data = await res.json();
setResult(data.result);
```

**Predict before running:** Need CORS? (Yes, as with CRUD.)

---

### LO 4: Handle a basic API error in the UI

If `!res.ok`, `setError("AI is unavailable. Try again.")`.

**Predict:** Should we show the provider’s raw JSON? (No.)

---

### LO 5: Keep prompts and secrets out of the frontend

**Audit live:** search the React repo for `sk-` and for the system prompt string.

**[Script:]** "If grep finds the prompt in `src/`, you failed the LO even if the demo looks pretty."

> **In the Real World:** **Bank** chatbots treat prompts as confidential. Copy that seriousness.

---

## Live Lab (10 min)

Working POST from Swagger, then React success + forced error (stop backend) to see the message.

---

## Recap (5 min)

**[Script:]** "You wrapped a model. Next: **when to spec first** versus vibe a prototype — so AI features do not become mush."

---

## Lecture Summary

- **Python** calls the chat API with a server-side key
- **One FastAPI endpoint** is the product boundary
- **React** displays the returned text
- A **basic error state** keeps the UI honest when the model or network fails
- **Prompts and secrets stay on the server**
- **Practical value:** You can add a small AI feature without leaking the company wallet
