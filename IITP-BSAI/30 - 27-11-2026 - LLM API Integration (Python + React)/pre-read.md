# Pre-Read: LLM API Integration (Python + React)

## Session Mindmap

This diagram shows where this session sits in the course.

```mermaid
flowchart TB
    subgraph foundation ["Foundation"]
        direction TB
        P1["<b>Previous Module</b><br/>Module 5: React Frontend<br/><i>[Fetch · State]</i><br/>UI can call your API"]
        P2["<b>Previous Module</b><br/>Module 6: Backend FastAPI<br/><i>[POST · Env]</i><br/>Server holds secrets"]
        CM["<b>Current Module Until Previous Session</b><br/>Module 10: Software 3.0<br/><i>[Prompts]</i><br/>System vs user messages"]
    end

    subgraph current ["Current Session"]
        CS["<b>Current Session</b><br/>LLM API Integration Python + React<br/><i>Mental shift:</i> from <b>chat tab</b> to <b>your wrapped endpoint</b><br/>Auth · FastAPI · error UI"]
    end

    subgraph value ["Course & Real-Life Value"]
        CV["<b>Course Value</b><br/>AI feature on the BSAI stack"]
        RL["<b>Real-Life Use</b><br/>Summarise · title · assist without key leaks"]
    end

    subgraph future ["Upcoming Modules"]
        direction TB
        U1["<b>Upcoming Module</b><br/>Module 11: Industry Spotlight<br/><i>[Specs · RAG]</i><br/>When to specify and retrieve"]
        U2["<b>Upcoming Module</b><br/>Module 11 continues<br/><i>[Portfolio]</i><br/>Show the live AI hop"]
        U3["<b>Upcoming Module</b><br/>Module 12: Capstone<br/><i>[FE · BE]</i><br/>Use AI only if the brief needs it"]
    end

    P1 ==>|&nbsp;Foundation&nbsp;| P2
    P2 ==>|&nbsp;Builds on&nbsp;| CM
    CM ==>|&nbsp;Blueprint&nbsp;| CS
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

- How to **call a chat API from Python** with an API key
- How to **hide that call behind one FastAPI endpoint**
- How **React fetch** shows the model’s reply
- How to **show a basic error** in the UI
- Why **prompts and secrets stay off the frontend**

---

## 2. Detailed Explanation

### Why Not Call the LLM from React?

If React holds the API key, anyone can steal it from the browser. They spend **your** money.

**Pattern:** Browser → your FastAPI → LLM provider. The key lives in server env vars.

**Analogy:** A hotel front desk. Guests never enter the vault. Your API is the desk.

> **In the Real World:** **ChatGPT**’s website does not embed the raw provider key in JavaScript for you to copy. **Vercel AI** demos still push keys to the server. Same rule.

**Why It Matters**

- Stolen keys mean surprise bills
- System prompts are product IP
- One backend endpoint is testable

**Benefits**

- Matches how you deployed secrets on Render
- UI stays simple: one fetch
- Errors can be turned into human messages

### Python Call with Auth

You send an HTTP request with a **Bearer token** (the API key) and a messages list (system + user).

Keep provider details in lecture. Conceptually:

```python
headers = {"Authorization": f"Bearer {os.getenv('LLM_API_KEY')}"}
```

Never print the key. Never commit `.env`.

### One FastAPI Endpoint

```python
@app.post("/ai/title")
def ai_title(body: UserText):
    # call provider with system prompt + body.text
    return {"result": text}
```

React only posts `{ "text": "..." }`. It does not send the system prompt.

### React Shows the Result

`fetch` POST → set state with `result` → render a paragraph.

### Basic API Error in the UI

If status is not OK, set `error` state: "Could not reach AI. Try again."

Do not dump stack traces to users.

**Messy to Clear**

**Messy:** `VITE_LLM_API_KEY` in the React app.

**Clear:** only `VITE_API_URL` pointing at FastAPI.

> **In the Real World:** **Perplexity** and **Cursor** keep model access on backends. Students copy that architecture in miniature.

### Building Blocks Checklist

- [ ] I know the three-hop path
- [ ] I can name the env var for the key
- [ ] I can sketch POST `/ai/...`
- [ ] I can show result or error in React
- [ ] I will not put the system prompt in JS

---

## 3. Practice Exercises

**Exercise 1 — Path**
Draw arrows: React, FastAPI, LLM provider.

**Exercise 2 — Auth**
Where is `LLM_API_KEY` read — browser or server?

**Exercise 3 — Endpoint**
Write the JSON body React sends (user text only).

**Exercise 4 — UI error**
Write the sentence you will show on a 500.

**Exercise 5 — Leak hunt**
Name two files that must not contain the key (`App.jsx`, git-tracked `.env`).
