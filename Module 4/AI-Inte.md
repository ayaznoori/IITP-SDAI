# Lecture Script: AI API Integration
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | AI API Integration — LLM APIs and Popular Providers | 15 min |
| 3 | API Setup — Authentication Basics | 15 min |
| 4 | Chat APIs — Roles and Chat-Based Structure | 25 min |
| 5 | Backend Integration — Sending Requests and Reading Responses | 25 min |
| 6 | Building Reusable API Service Functions | 15 min |
| 7 | Lecture Summary and Recap | 7 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience understands prompt engineering and how transformers work conceptually. They have also built FastAPI backends, connected to databases, and implemented authentication. This lecture is where those two skill sets converge. The hook should make clear that everything up to now was conceptual — this is where it becomes a working feature in a real application. Wait after the opening question.

**[Script:]**

"You know how to write an excellent prompt. You know how transformers process text. None of that is useful yet, because none of it is connected to an application. A great prompt sitting in a text file helps nobody.

Today we close that gap. We take everything from the prompt engineering session and wire it into a real backend — the same kind of FastAPI backend you have already built with databases and authentication. By the end of today, your application will send a request to an LLM provider, receive a response, and return it to your users through your own API.

This involves several concrete pieces you have not yet touched. You need to choose a provider — OpenAI, Google, Anthropic, and others each have their own API, their own pricing, their own strengths. You need to authenticate with that provider using an API key, exactly like the API key authentication you built in the security session, except now you are the client, not the server. You need to understand the specific format these APIs expect — not raw text, but a structured conversation with roles: system, user, and assistant. And you need to build this into your FastAPI backend in a way that is clean, reusable, and does not hardcode your API key directly into your route handlers.

This is the session where prompt engineering, backend development, and API authentication all come together into a feature you could ship today."

---

## Block 2 — AI API Integration: LLM APIs and Popular Providers

### 2A — What an LLM API Provides

**[Script:]**

"An LLM API is a service that lets you send text to a hosted language model and get a generated response back, over HTTP, exactly like any other API you have used. You do not run the model yourself — running a large model requires enormous computational resources, specialized hardware, and significant engineering. The provider runs the model on their infrastructure; you send a request and pay per token processed.

This is no different in shape from any third-party API you have integrated before — you send a request with your data, you get a response, you pay according to your usage. The specifics differ from provider to provider, but the pattern is identical to any REST API integration you have already done."

---

### 2B — The Major Providers

**[Script:]**

"Three providers dominate the current landscape, and it is worth understanding what each offers at a high level."

> 🎯 **Instructor Note:** Write this comparison table on the board. Keep the framing neutral and factual — this is orientation, not a recommendation of one over another.

```
Provider    Models                  Notable characteristics
--------    ----------------------  --------------------------------
OpenAI      GPT family              Widely adopted, large ecosystem,
                                     broad tooling and community support

Google      Gemini family           Deep integration with Google Cloud
                                     and Google Workspace products

Anthropic   Claude family           Strong focus on safety and reliability,
                                     large context windows
```

**[Script:]**

"Each provider exposes broadly similar capabilities through their API — send text, get generated text back — but the specific request format, authentication method, available models, and pricing differ. Once you understand the pattern with one provider, switching to another is mostly a matter of adjusting field names and endpoint URLs, not relearning the whole approach.

For today, we will use OpenAI's API as our working example, since its format has become something of a de facto standard that many other providers, including some open-source tools, have adopted or closely mirrored. The concepts transfer directly."

> 🎯 **Instructor Note:** Ask: "If you already know how to integrate one provider's chat API, how much new knowledge does switching to a different provider require?" Answer: relatively little — the core concepts (roles, requests, responses, authentication) are consistent across providers; you are mainly adapting to different field names, endpoints, and pricing models.

**Recap of Block 2 before moving on:**

- An LLM API lets you send text to a hosted model over HTTP and receive generated text back, following the same request/response pattern as any third-party API
- The provider runs the model on their infrastructure; you pay based on usage, typically measured in tokens
- OpenAI, Google, and Anthropic are the three major current providers, each with their own models, formats, and strengths
- The underlying concepts — authentication, structured requests, roles — transfer across providers even though specific details differ

---

## Block 3 — API Setup: Authentication Basics

### 3A — API Keys for LLM Providers

**[Script:]**

"You already understand API key authentication from the security session — a key generated once, stored securely, sent with every request to prove your identity. LLM providers use exactly this pattern. You sign up for an account, generate an API key from their dashboard, and that key authenticates every request you make.

The critical difference from the security session: there, you were the one issuing and verifying API keys as the server. Here, you are the client. The provider issues you a key, and you are responsible for keeping it secret and sending it correctly."

---

### 3B — Storing and Using API Keys Safely

**[Script:]**

"An API key for an LLM provider is a sensitive secret. If it leaks — committed to a public GitHub repository, exposed in client-side code, printed to logs — anyone can use it to make requests billed to your account. This is not a hypothetical risk; leaked API keys being exploited is a common and expensive real-world problem.

The rule: never hardcode an API key directly into your source code. Store it in an environment variable, loaded at runtime, and never committed to version control."

> 🎯 **Instructor Note:** This point deserves emphasis — write it on the board and treat it as a hard rule, not a suggestion.

```
NEVER do this:
api_key = "sk-abc123..."          # hardcoded in source code

ALWAYS do this:
import os
api_key = os.environ.get("OPENAI_API_KEY")   # loaded from environment
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I commit a file containing a hardcoded API key to a public GitHub repository, and later remove it in a new commit, is the key still exposed?" Answer: yes — the key remains visible in the commit history even after removal from the latest version. The only safe fix is to revoke the key entirely and generate a new one.

**Demo 1 — Loading an API key safely (whiteboard-friendly)**

```python
import os
from dotenv import load_dotenv

load_dotenv()  # reads variables from a .env file into the environment

api_key = os.environ.get("OPENAI_API_KEY")

if not api_key:
    raise ValueError("OPENAI_API_KEY environment variable is not set")
```

**[Script:]**

"`load_dotenv()` reads key-value pairs from a `.env` file in your project and loads them as environment variables. The `.env` file itself is never committed to version control — it belongs in `.gitignore`, alongside anything else containing secrets.

`os.environ.get('OPENAI_API_KEY')` retrieves the value at runtime. If it is missing, we raise an error immediately rather than letting the application fail confusingly later when the first API call is made without credentials."

> 🎯 **Instructor Note:** Ask: "Why raise an error immediately instead of waiting until the API call happens?" Answer: failing fast with a clear message is far easier to debug than a cryptic authentication error deep inside a request several steps later. This is a general good practice, not specific to API keys.

**Recap of Block 3 before moving on:**

- LLM providers authenticate requests using API keys, following the same pattern as any API key authentication system
- Never hardcode an API key directly in source code; use environment variables loaded at runtime
- A `.env` file stores secrets locally and must be excluded from version control via `.gitignore`
- Fail fast with a clear error if a required API key is missing, rather than letting a request fail confusingly later

---

## Block 4 — Chat APIs: Roles and Chat-Based Structure

### 4A — Why Chat APIs Are Structured, Not Plain Text

**[Script:]**

"You might expect an LLM API to simply take a string of text and return a string of text. Modern chat-based LLM APIs do not work quite that way. Instead, they expect a structured list of messages, where each message has a role and content.

This structure exists because these models are trained specifically to handle conversations — a back-and-forth between different participants, not just a single block of text. The roles tell the model who is speaking in each part of the conversation, which shapes how it responds."

---

### 4B — The Three Roles

**[Script:]**

"There are three standard roles used across chat-based APIs."

> 🎯 **Instructor Note:** Write this on the board and keep it visible through the rest of the block.

```
system:      instructions that set the model's behavior and persona
             (not visible to the end user, sent once per conversation)

user:        messages from the actual end user

assistant:   the model's own prior responses, included so it has
             memory of what it already said in this conversation
```

**[Script:]**

"The `system` role sets the stage. This is exactly the role prompting technique from the previous session, expressed structurally instead of as text buried inside a single prompt. 'You are a customer service assistant. Be concise and professional.' This message is typically sent once, at the start of the conversation, and shapes every response that follows.

The `user` role represents what the actual person is asking. 'How do I reset my password?'

The `assistant` role represents the model's own previous responses. This matters because these APIs are stateless between requests — the provider does not remember your previous conversation on their own. Every time you send a request, you must include the full conversation history yourself, with each past message correctly labeled by role, so the model has the context of what has already been said."

> 🎯 **Instructor Note:** This statelessness point is one of the most important and most commonly misunderstood facts about these APIs. Ask: "If I send a request with just the user's newest message, without any of the previous conversation, will the model remember what was discussed three messages ago?" Answer: no — the provider does not store conversation state on their end by default. Your application is responsible for maintaining and resending the full conversation history with every single request.

**Demo 2 — A structured chat request (whiteboard-friendly)**

```python
messages = [
    {"role": "system", "content": "You are a helpful customer service assistant. Be concise."},
    {"role": "user", "content": "How do I reset my password?"},
    {"role": "assistant", "content": "Go to Settings, then Security, then click 'Reset Password'."},
    {"role": "user", "content": "What if I don't remember my email?"}
]
```

**[Script:]**

"Read this from top to bottom exactly as a conversation transcript. One system message setting behavior. Then user and assistant messages alternating, building the conversation history. The final message is the newest question from the user — 'what if I don't remember my email' — and it only makes sense in context because the previous exchange about resetting the password is included right above it.

Every request you send includes this entire array. As the conversation grows, so does the size — and cost — of every subsequent request, since you are resending the whole history each time."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I remove the system message entirely and just send the user and assistant messages, does the API still work?" Answer: yes — the system message is optional. Without it, the model falls back to generic default behavior rather than the specific persona or instructions you would have set. This connects directly back to role prompting from the previous session.

**Recap of Block 4 before moving on:**

- Chat-based LLM APIs expect a structured list of messages, not a single plain-text string
- Three roles: `system` sets behavior and persona once at the start; `user` represents the end user's messages; `assistant` represents the model's own prior responses
- The system message is the structural equivalent of role prompting from the previous session
- These APIs are stateless — your application must resend the full conversation history, correctly labeled by role, with every request
- Conversation size and cost grow as history accumulates, since the entire history is resent each time

---

## Block 5 — Backend Integration: Sending Requests and Reading Responses

### 5A — Making a Request to the API

**[Script:]**

"With an API key loaded safely and an understanding of the message structure, sending a request is a standard HTTP call — the same shape as any external API call you have made from Python before."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before the demo, ask: "Based on what you know about REST APIs, what do you expect this request to need? Think about what you have sent to other APIs before." Guide toward: a URL, headers including authentication, and a body containing the actual data — in this case, the messages array and which model to use.

**Demo 3 — Sending a request to an LLM API (whiteboard-friendly)**

```python
import os
from openai import OpenAI

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "What is the capital of France?"}
    ]
)

print(response.choices[0].message.content)
```

**[Script:]**

"`OpenAI(api_key=...)` creates a client object configured with your credentials — this handles the authentication header for every request made through it, so you do not manually attach it each time.

`client.chat.completions.create(...)` is the actual API call. `model` specifies which model to use — different models vary in capability, speed, and cost. `messages` is the structured conversation array from Block 4.

The response is not just a plain string — it is a structured object. `response.choices[0].message.content` navigates into that structure to reach the actual generated text. `choices` is a list because some requests can ask for multiple alternative responses at once; `[0]` takes the first one."

> 🎯 **Instructor Note:** Ask: "Why is the response a nested structured object instead of just a plain string?" Answer: the response includes more than just the generated text — usage statistics (how many tokens were used), metadata about which model responded, and potentially multiple alternative completions. A plain string could not carry all of that.

---

### 5B — Reading the Full Response

**[Script:]**

"Beyond the generated text, the response object contains useful metadata — particularly token usage, which directly determines cost."

**Demo 4 — Inspecting the full response (whiteboard-friendly)**

```python
print(response.choices[0].message.content)   # the generated text
print(response.usage.prompt_tokens)          # tokens in your input
print(response.usage.completion_tokens)      # tokens the model generated
print(response.usage.total_tokens)           # sum of both
```

**[Script:]**

"`usage.prompt_tokens` tells you how many tokens your entire request consumed — this connects directly to context engineering from the previous session; a bloated, unoptimized context shows up here as a larger number. `completion_tokens` is how many tokens the model generated in response. `total_tokens` is the sum, and this is what determines your bill for that single request.

Logging or monitoring these numbers in a real application is how you track cost in production. A single unexpected spike in `prompt_tokens` often means someone's context is including far more than it needs to — exactly the inefficiency the context engineering session covered."

---

### 5C — Integrating Into a FastAPI Route

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Based on the FastAPI routes you have already built, what would a route that accepts a user's question and returns the model's answer need to look like?" Guide toward: a POST route accepting a request body with the question, calling the API inside the route, and returning the response as JSON — the same shape as any other POST route they have written.

**Demo 5 — A FastAPI route calling an LLM API (whiteboard-friendly)**

```python
from fastapi import FastAPI
from pydantic import BaseModel
from openai import OpenAI
import os

app = FastAPI()
client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))

class QuestionRequest(BaseModel):
    question: str

@app.post("/ask")
def ask_question(request: QuestionRequest):
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": request.question}
        ]
    )
    return {"answer": response.choices[0].message.content}
```

**[Script:]**

"This should look familiar. `QuestionRequest` is a Pydantic model, exactly as in any other FastAPI route you have built, validating the incoming request body. The route function accepts the validated request, calls the LLM API using the question the user provided, and returns the extracted answer as a JSON response.

Notice what this route does not yet handle: multi-turn conversation history, error handling if the API call fails, or any reuse if you need this same logic in more than one route. We address the last two next."

> 🎯 **Instructor Note:** Ask: "What happens to this route if the OpenAI API is temporarily down, or the API key is invalid?" Answer: as written, an unhandled exception occurs and FastAPI returns a 500 error with a potentially confusing message. Real applications need explicit error handling around external API calls — a preview of a robustness concern that becomes important as this code is used more heavily.

**Recap of Block 5 before moving on:**

- Sending a request to an LLM API follows the same shape as any external API call: a client configured with authentication, a method call with the request body, and a structured response
- The response object contains the generated text plus metadata, including token usage that determines cost
- `usage.prompt_tokens` and `usage.completion_tokens` connect directly to context engineering — bloated context shows up as inflated prompt token counts
- Integrating an LLM call into FastAPI follows the exact same route pattern as any other POST route: a Pydantic request model, a route function, and a JSON response

---

## Block 6 — Building Reusable API Service Functions

### 6A — Why Not Call the API Directly in Every Route

**[Script:]**

"The route in the previous block works, but imagine you need this same LLM call in five different routes — one for customer questions, one for generating summaries, one for classifying feedback. Copying the same client setup and API call into every route means five places to update if the model changes, five places to add error handling, five places that could drift out of sync.

The fix is the same principle you already use for database access and authentication: extract the shared logic into a reusable function, and have routes call that function instead of duplicating the logic."

---

### 6B — Building a Service Function

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Based on how you structured reusable database logic in earlier sessions, what would a reusable function for calling the LLM API need as its inputs and outputs?" Guide toward: inputs like the user's message and optionally a system prompt or conversation history; output should be just the generated text, hiding the response object's internal structure from the caller.

**Demo 6 — A reusable service function (whiteboard-friendly)**

```python
from openai import OpenAI
import os

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))

def get_ai_response(user_message: str, system_prompt: str = "You are a helpful assistant.") -> str:
    try:
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[
                {"role": "system", "content": system_prompt},
                {"role": "user", "content": user_message}
            ]
        )
        return response.choices[0].message.content
    except Exception as e:
        raise RuntimeError(f"AI API call failed: {e}")
```

**[Script:]**

"`get_ai_response` takes a user message and an optional system prompt, with a sensible default. It returns just the plain text string — the caller does not need to know anything about `choices[0].message.content`; that detail is hidden inside the function.

The `try/except` wraps the API call. If it fails for any reason — network issue, invalid key, provider outage — we catch it and raise a clearer error rather than letting the raw exception propagate with a confusing message. This is the beginning of proper error handling for external API dependencies."

**Demo 7 — Using the service function across multiple routes (whiteboard-friendly)**

```python
@app.post("/ask")
def ask_question(request: QuestionRequest):
    answer = get_ai_response(request.question)
    return {"answer": answer}

@app.post("/summarize")
def summarize_text(request: TextRequest):
    answer = get_ai_response(
        request.text,
        system_prompt="You are an expert at writing concise summaries. Summarize the following text in two sentences."
    )
    return {"summary": answer}
```

**[Script:]**

"Both routes call the same underlying function. The first uses the default system prompt for general questions. The second overrides it with a summarization-specific instruction. If you need to change the model, add logging, or adjust error handling, you change it once, in `get_ai_response`, and every route that depends on it benefits immediately.

This is the exact same architectural principle behind extracting a shared database session dependency, or a shared authentication check — write the logic once, depend on it everywhere it is needed."

> 🎯 **Instructor Note:** Ask: "If you needed to add conversation history support to this function so it could handle multi-turn conversations, would you have to change every route that uses it?" Answer: only if the routes need that new capability — you can add an optional parameter with a sensible default so existing calls continue working unchanged, while new calls can opt into the new behavior. This is a natural extension point, worth naming even without building it out fully today.

**Recap of Block 6 before moving on:**

- Duplicating LLM API calls across multiple routes creates maintenance burden and inconsistency, just as duplicating database or authentication logic would
- A reusable service function centralizes the client setup, the API call, error handling, and response extraction in one place
- Routes call the service function and receive a plain, simple return value, hiding the internal response structure
- Wrapping the API call in error handling produces clearer failures than letting raw exceptions propagate
- Optional parameters with sensible defaults let a service function support new use cases without breaking existing callers

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "Why should an API key never be hardcoded? What are the three roles in a chat API and what does each do? Why are these APIs stateless, and what does that require of your application? What information does the response object contain beyond the generated text? Why extract a reusable service function instead of calling the API directly in every route?"

**AI API Integration — LLM APIs and Popular Providers**

- An LLM API lets you send text to a hosted model over HTTP and receive generated text back, following the same request/response pattern as any third-party API
- OpenAI, Google, and Anthropic are the three major current providers; each has its own models and specifics, but the underlying concepts transfer across all of them

**API Setup — Authentication Basics**

- LLM providers authenticate requests using API keys, following the same pattern as any API key authentication
- Never hardcode an API key in source code; load it from an environment variable via a `.env` file excluded from version control
- Fail fast with a clear error if a required API key is missing

**Chat APIs — Roles and Chat-Based Structure**

- Chat-based LLM APIs expect a structured list of messages, not plain text
- Three roles: `system` sets behavior and persona once; `user` represents end-user messages; `assistant` represents the model's own prior responses
- These APIs are stateless — your application must resend the full conversation history, correctly labeled by role, with every request

**Backend Integration — Sending Requests and Reading Responses**

- Sending a request follows the standard external API call shape: an authenticated client, a method call with the request body, a structured response
- The response contains generated text plus metadata, including token usage that determines cost
- Integrating an LLM call into a FastAPI route follows the same pattern as any other route: a Pydantic request model, a route function, a JSON response

**Building Reusable API Service Functions**

- Duplicating LLM calls across routes creates the same maintenance burden as duplicating database or authentication logic
- A reusable service function centralizes client setup, the API call, error handling, and response extraction
- Routes depend on the service function and receive a simple return value; internal response structure stays hidden
- Optional parameters with defaults let a service function support new use cases without breaking existing callers

**Why All of This Matters Together**

- This session is where prompt engineering, backend architecture, and API authentication converge into a shippable feature — the roles you use to structure a chat request are the same role prompting technique from the previous session, the API key handling follows the same discipline as any credential in your system, and the reusable service function follows the same architectural principle you already apply to database access; the result is that adding an AI-powered feature to a real application is not a separate skill from backend development, it is backend development, using a new kind of external service

---

*End of script.*