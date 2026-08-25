# Lecture Script: Agentic Tool-Use and APIs
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Function Calling Fundamentals | 25 min |
| 3 | Tool-Use — Defining and Registering Tools | 25 min |
| 4 | API-Enabled Agents — Connecting Tools to Real Services | 25 min |
| 5 | Executable Agent Logic — Assembling the Full Loop | 20 min |
| 6 | Lecture Summary and Recap | 7 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience covered agentic systems conceptually in the previous session — the perceive-reason-act loop, tool use in principle, validation discipline for tool calls. This session takes that concept and makes it real, buildable code. The hook should make clear this is the "how" following last session's "what and why." Wait after the opening question.

**[Script:]**

"Last session, we drew the perceive-reason-act loop, and walked through a flight-booking example where the agent called `search_flights`, then `book_flight`, then `add_calendar_event`. That was the conceptual shape of tool use. Today we build the actual mechanism that makes it real: function calling — the specific feature LLM APIs provide that lets a model request that a real function be executed, with real arguments, based on real user input.

This is the piece that turns 'the model generates text' into 'the model can cause things to happen' — search a database, check a calendar, send an email, call a payment API. Function calling is not a separate technology from the chat APIs you already know. It is an extension of the exact same request-and-response pattern from the AI API integration session, with one addition: alongside the messages array, you also describe the tools available, and the model can choose to request one instead of just generating text.

Today covers four connected pieces: function calling as the underlying API mechanism, how to define and register tools your agent can use, how those tools connect to real external APIs and services, and how to assemble all of this into working, executable agent logic — a real loop, in real code, that perceives, reasons, and acts, exactly as we discussed conceptually last time, except now it actually runs."

---

## Block 2 — Function Calling Fundamentals

### 2A — What Function Calling Actually Is

**[Script:]**

"Function calling is a feature built into modern chat completion APIs. Alongside your usual messages array, you provide a list of tool definitions — descriptions of functions the model is allowed to request, including their names, what they do in plain language, and what parameters they accept. The model does not execute these functions itself. It cannot reach outside its own generation process. What it can do is respond with a structured request saying, in effect, 'call this specific function with these specific arguments' — and your backend code is responsible for actually running it."

> 🎯 **Instructor Note:** Draw this exchange on the board — this is the mechanical foundation for the rest of the session.

```
Your request to the API:
  messages: [...]
  tools: [ description of available functions ]

Model's response is ONE of two shapes:
  (a) A normal text response, exactly as before, OR
  (b) A tool call request: "call get_weather with city='Tokyo'"

If (b): YOUR code executes get_weather(city='Tokyo') for real
        The real result is sent back to the model as a new message
        The model then continues — often producing a final text answer
```

**[Script:]**

"The critical point: the model decides whether a tool call is needed at all, based on the user's message and the tool descriptions you provided. If someone asks 'what is the capital of France,' the model has no reason to call any tool — it answers directly from what it already knows. If someone asks 'what is the weather in Tokyo right now,' the model recognizes this is exactly the kind of question a `get_weather` tool was described to handle, and requests that tool call instead of guessing."

> 🎯 **Instructor Note:** Ask: "Why can't the model just answer a live weather question directly, the way it answers a capital-of-France question?" Answer: the model's knowledge comes from training data with a fixed cutoff — it has no access to live, current information like today's weather. This is the same limitation from the LLM fundamentals and RAG sessions, and function calling is another way of solving it: instead of retrieving stored documents, the agent calls a live function to get current information.

---

### 2B — Defining a Tool

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Based on the structured output schemas from the advanced AI integration session, what do you think is used to describe a tool's expected parameters to the API?" Guide toward: the same JSON-schema-style structure used for structured output — a formal, machine-readable description of expected fields and types, not just a plain-English sentence.

**Demo 1 — Defining a tool (whiteboard-friendly)**

```python
weather_tool = {
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get the current weather for a specific city.",
        "parameters": {
            "type": "object",
            "properties": {
                "city": {
                    "type": "string",
                    "description": "The city name, e.g. 'Tokyo'"
                }
            },
            "required": ["city"]
        }
    }
}
```

**[Script:]**

"`name` is the identifier the model uses to request this specific function. `description` is plain-English text explaining what the tool does — this matters more than it might seem, because the model relies on this description to decide when the tool is relevant, exactly the way a well-written docstring helps a human developer understand when to use a function. `parameters` describes the expected arguments using the same JSON Schema structure as the structured output schemas from the advanced AI integration session — `city` is a required string.

A vague or misleading description is a real source of bugs here — if `description` does not clearly explain what the tool does and when to use it, the model may call it at the wrong times, or fail to call it when it actually should."

> 🎯 **Instructor Note:** Emphasize this directly: "The description field is not documentation for humans — it is the actual input the model uses to decide when to call this tool. Writing a clear, specific description is closer to prompt engineering than to code comments."

---

### 2C — Making a Request with Tools Available

**Demo 2 — A chat request with tools (whiteboard-friendly)**

```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "user", "content": "What's the weather like in Tokyo?"}
    ],
    tools=[weather_tool]
)

message = response.choices[0].message
print(message.tool_calls)
```

**[Script:]**

"This should look almost identical to every chat request you have already written — the only addition is the `tools` parameter, a list of tool definitions like the one from Demo 1. Because the user's question matches what `get_weather` is described to handle, the response's `message.tool_calls` will contain a request to call `get_weather` with `city` set to `'Tokyo'`, rather than the model generating a plain text guess about the weather."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I sent the message 'What is 12 times 7?' with the exact same `tools` list, containing only `get_weather`, what would `message.tool_calls` contain?" Answer: nothing relevant — likely `None`, or an empty list — because the only available tool has nothing to do with the question asked. The model would simply answer the math question directly using its own reasoning, exactly as before tools were introduced at all.

**Recap of Block 2 before moving on:**

- Function calling lets you describe available tools alongside a normal chat request; the model can respond with a request to call one instead of generating plain text
- The model never executes anything itself — it only requests a call; your backend code is responsible for actually running it
- A tool definition includes a name, a plain-English description, and a parameters schema using the same structure as structured output
- The description field directly shapes when the model decides to use a tool — treat it as something closer to prompt engineering than a code comment

---

## Block 3 — Tool-Use: Defining and Registering Tools

### 3A — From One Tool to a Tool Registry

**[Script:]**

"A real agent typically has more than one tool available — search, calculator, calendar, database lookup. As the number of tools grows, you need a clean way to both list them for the API and actually execute whichever one the model requests. This is where a tool registry comes in: a structured mapping between tool names and the real Python functions that implement them."

---

### 3B — Building a Tool Registry

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If the model requests a tool call for a name that does not exist in your registry — perhaps due to a typo, or a tool that was removed — what should your code do?" Answer: it should not crash. It should return a clear error message back to the model as the tool result, letting the model's next reasoning step handle the situation, exactly like the invalid-arguments handling from the agentic systems session.

**Demo 3 — A tool registry with real implementations (whiteboard-friendly)**

```python
def get_weather(city: str) -> dict:
    # In reality this would call a real weather API
    return {"city": city, "temperature": 18, "condition": "Cloudy"}

def calculate_tip(bill_amount: float, percentage: float) -> dict:
    tip = bill_amount * (percentage / 100)
    return {"tip": round(tip, 2), "total": round(bill_amount + tip, 2)}

TOOL_REGISTRY = {
    "get_weather": get_weather,
    "calculate_tip": calculate_tip,
}

def execute_tool(tool_name: str, arguments: dict):
    if tool_name not in TOOL_REGISTRY:
        return {"error": f"Unknown tool: {tool_name}"}
    function = TOOL_REGISTRY[tool_name]
    return function(**arguments)
```

**[Script:]**

"`TOOL_REGISTRY` is a plain dictionary mapping each tool's name — the exact string the model will use to request it — to the actual Python function that performs the work. `execute_tool` looks up the requested name, and if it does not exist, returns a structured error instead of raising an uncaught exception. If it does exist, it calls the function using `**arguments` to unpack the dictionary of arguments into keyword arguments matching the function's parameters.

This registry pattern is exactly the reusable service function architecture from the AI API integration session, extended to handle multiple interchangeable functions selected dynamically at runtime, rather than one fixed function called directly."

> 🎯 **Instructor Note:** Ask: "Why use a dictionary lookup by name here, instead of something like a long if/elif chain checking `tool_name` against each possibility?" Answer: a dictionary lookup is cleaner and scales better — adding a new tool means adding one new entry to the registry, not adding another branch to a growing conditional chain. This is the same principle behind the RBAC role-permission dictionary from the security session — mapping-based lookups scale better than branching logic as the number of cases grows.

---

### 3C — Validating Arguments Before Execution

**[Script:]**

"You already established this discipline in the agentic systems session: a tool call is external, model-generated input, and deserves the same validation as any untrusted input to your system. Let us make that concrete with Pydantic, exactly as you would validate a FastAPI request body."

**Demo 4 — Validating tool arguments (whiteboard-friendly)**

```python
from pydantic import BaseModel, ValidationError

class CalculateTipArgs(BaseModel):
    bill_amount: float
    percentage: float

def execute_tool(tool_name: str, arguments: dict):
    if tool_name == "calculate_tip":
        try:
            validated = CalculateTipArgs(**arguments)
        except ValidationError as e:
            return {"error": f"Invalid arguments: {e}"}
        return calculate_tip(validated.bill_amount, validated.percentage)

    if tool_name not in TOOL_REGISTRY:
        return {"error": f"Unknown tool: {tool_name}"}
    return TOOL_REGISTRY[tool_name](**arguments)
```

**[Script:]**

"Before `calculate_tip` ever runs, `CalculateTipArgs` confirms the arguments the model provided actually match the expected shape — both fields present, both genuinely numeric. If the model somehow requests this tool with a missing field, or a string where a number was expected, `ValidationError` is caught and a clear error is returned as the tool's result, rather than allowing a malformed call to crash your application or silently produce nonsense output."

> 🎯 **Instructor Note:** Reinforce the connection directly: "This is not new material — it is the exact same Pydantic validation pattern from every FastAPI route you have built, applied here to a different source of input. The source changed from an HTTP request body to a model-generated tool call, but the discipline — never trust it until it is validated — is identical."

**Recap of Block 3 before moving on:**

- A tool registry maps tool names to their real implementing functions, making it easy to look up and execute whichever tool the model requests
- Requesting an unknown tool name should return a structured error, not crash — the same graceful-failure discipline from earlier sessions
- Tool arguments should be validated with Pydantic before execution, exactly like validating any FastAPI request body
- A registry using dictionary lookup scales better than a long conditional chain as the number of available tools grows

---

## Block 4 — API-Enabled Agents: Connecting Tools to Real Services

### 4A — Tools as a Bridge to External Systems

**[Script:]**

"Everything so far has used simple example functions. A real API-enabled agent's tools typically wrap actual external services — a weather API, your own application's database, a calendar service, a payment processor. The tool function itself becomes a thin wrapper: receive validated arguments, make the real external call, return the real result."

---

### 4B — Wrapping a Real External API as a Tool

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If the external weather API this tool calls is temporarily down, what should the tool function return to the agent, rather than letting the request crash?" Answer: a structured error result, exactly as with invalid arguments — this lets the model's next reasoning step decide what to do about the failure, rather than the whole agent process crashing outright.

**Demo 5 — A tool wrapping a real external API call (whiteboard-friendly)**

```python
import requests

def get_weather(city: str) -> dict:
    try:
        response = requests.get(
            "https://api.weatherprovider.com/current",
            params={"city": city, "key": WEATHER_API_KEY},
            timeout=5
        )
        response.raise_for_status()
        data = response.json()
        return {
            "city": city,
            "temperature": data["temp_c"],
            "condition": data["condition"]
        }
    except requests.RequestException as e:
        return {"error": f"Weather service unavailable: {e}"}
```

**[Script:]**

"This is the exact same reusable service function pattern from the backend integration sessions, applied to a tool instead of a route handler: make the request, handle failure explicitly, return a clean result. `timeout=5` ensures the agent does not hang indefinitely waiting on a slow or unresponsive external service — a real concern once your agent depends on services outside your own control. `response.raise_for_status()` raises an exception on an HTTP error status, caught by the `except` block, which returns a structured error instead of letting the whole tool call fail with an unhandled exception.

Whether this call succeeds or fails, the tool always returns a usable dictionary — the model's next reasoning step can work with either a real weather result or a clear error message, and can decide what to do next based on which one it received."

> 🎯 **Instructor Note:** Ask: "Why does it matter that the function returns a structured error dictionary on failure, rather than letting the exception propagate up and crash the whole agent process?" Answer: this is exactly the feedback loop principle from the agentic systems session — a failure the agent can perceive and reason about is far more useful and safer than a silent crash. An agent that can say "the weather service is unavailable, let me try again or inform the user" is more robust than one that simply stops working entirely.

---

### 4C — Authenticating Tool Calls to External Services

**[Script:]**

"Real external APIs require authentication, exactly as covered in the security sessions — API keys, OAuth tokens, and so on. This authentication belongs inside the tool function itself, never exposed to the model. The model only ever sees the tool's name, description, and expected arguments — it never sees or handles your actual API keys or credentials."

> 🎯 **Instructor Note:** This is a genuine security point, not just an implementation detail. Emphasize: "The model requests that a tool be called — it does not need, and should never receive, the actual credentials used to authenticate that call. Keep authentication entirely inside your tool implementation, loaded from environment variables exactly as in the AI API integration session, completely separate from anything the model can see or influence."

**Recap of Block 4 before moving on:**

- A real agent's tools typically wrap actual external services — weather APIs, databases, calendars, payment processors
- Tool functions follow the same reusable service pattern from earlier backend sessions: make the call, handle failure explicitly, return a clean result
- A tool should always return a usable result, success or structured error, rather than letting an unhandled exception crash the agent
- Authentication credentials belong entirely inside the tool implementation, loaded from environment variables, and are never exposed to the model itself

---

## Block 5 — Executable Agent Logic: Assembling the Full Loop

### 5A — Putting the Pieces Together

**[Script:]**

"We now have every piece needed to assemble the actual perceive-reason-act loop from the agentic systems session into real, executable code: a chat request with tools available, a way to detect and execute a requested tool call, and a way to feed the result back to the model so it can continue reasoning. Let us build the complete loop."

---

### 5B — The Complete Agent Loop

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Before showing the full code, ask: "Based on the perceive-reason-act loop from last session, how many times do you think this loop might need to run for a single user request — always exactly once, or could it vary?" Answer: it varies — a simple question needing no tools might resolve in a single pass, while a complex request might require several tool calls in sequence, each one feeding into the next reasoning step, before a final text answer is ready.

**Demo 6 — A complete, executable agent loop (whiteboard-friendly)**

```python
def run_agent(user_message: str, tools: list, max_iterations: int = 5) -> str:
    messages = [{"role": "user", "content": user_message}]

    for iteration in range(max_iterations):
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=messages,
            tools=tools
        )
        message = response.choices[0].message

        if not message.tool_calls:
            return message.content  # final answer, no more tools needed

        messages.append(message)

        for tool_call in message.tool_calls:
            tool_name = tool_call.function.name
            arguments = json.loads(tool_call.function.arguments)
            result = execute_tool(tool_name, arguments)

            messages.append({
                "role": "tool",
                "tool_call_id": tool_call.id,
                "content": json.dumps(result)
            })

    return "Reached maximum iterations without a final answer."
```

**[Script:]**

"Walk through this as the perceive-reason-act loop made concrete. `messages` starts with just the user's request — the initial perception. Each iteration of the `for` loop is one full cycle: `client.chat.completions.create` is the reasoning step, where the model decides what to do next given everything in `messages` so far.

If `message.tool_calls` is empty, the model decided no more tools are needed — this is the action of simply answering, and we return that final text directly. If tool calls are present, that is the model's chosen action — we append the model's own message to the conversation history, then for each requested tool call, we execute it using `execute_tool` from Block 3, and append the result back into `messages` with the special `role: 'tool'`, tagged with `tool_call_id` so the model can match each result to the specific call that requested it.

The loop then repeats — the next iteration's reasoning step now perceives the tool's result, alongside everything before it, and decides what to do next: call another tool, or finally respond with a text answer."

> 🎯 **Instructor Note:** This is the moment to explicitly close the loop back to the Block 2 diagram from the agentic systems session. Say directly: "This `for` loop, right here, is a literal, running implementation of the perceive-reason-act cycle we drew as a circle on the board last session. `messages` is the accumulated perception. `client.chat.completions.create` is the reasoning. Executing a tool call is the action. And appending the result back into `messages` is exactly what feeds the next perception. Everything from last session's conceptual diagram is now real, executable code."

---

### 5C — Why max_iterations Matters

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "What could happen if this loop had no `max_iterations` limit at all, and the model kept requesting tool calls indefinitely — perhaps due to a bug in a tool's error handling, or a genuinely confusing situation the model cannot resolve?" Answer: the loop could run forever, or at minimum run far longer and cost far more than intended, repeatedly calling APIs without ever reaching a final answer. This is a real operational risk in agentic systems, not a hypothetical one.

**[Script:]**

"`max_iterations` is a hard safety limit — if the agent has not reached a final text answer within five full reasoning cycles, we stop and report that explicitly, rather than letting the loop run indefinitely. This connects directly to the cost optimization session — every iteration of this loop is a full API call, with real token cost, and an unbounded loop is a genuine financial and operational risk in a production agentic system, not just a theoretical concern."

**Recap of Block 5 before moving on:**

- The complete agent loop combines a chat request with tools, tool execution, and appending results back into the conversation for the next reasoning step
- `messages.append(...)` after each tool call is what feeds the "perceive" step of the next loop iteration with the real outcome of the last action
- The loop continues until the model responds with no further tool calls — its way of signaling the task is complete
- `max_iterations` is a required safety limit, preventing runaway cost and execution time if the agent cannot resolve the task

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What does the model actually do when it 'calls' a tool — does it execute anything itself? What should a tool registry do when an unknown tool name is requested? Where should API authentication live, relative to the model? What does `messages.append(...)` accomplish after a tool result comes back? Why is `max_iterations` a required safeguard, not an optional one?"

**Function Calling Fundamentals**

- Function calling lets you describe available tools alongside a chat request; the model can respond with a request to call one instead of generating text directly
- The model only ever requests a call — it never executes anything itself; your backend code is responsible for actually running it
- A tool definition includes a name, a description that directly shapes when the model chooses to use it, and a parameters schema
- The model decides autonomously whether a tool is needed at all, based on the user's message and the available tool descriptions

**Tool-Use — Defining and Registering Tools**

- A tool registry maps tool names to their real implementing functions for clean, scalable lookup and execution
- Unknown tool requests should return a structured error, never crash the process
- Tool arguments must be validated with Pydantic before execution, exactly like any FastAPI request body

**API-Enabled Agents — Connecting Tools to Real Services**

- Real tools typically wrap external services, following the same reusable service-function pattern already used elsewhere in the backend
- A tool should always return a usable result, success or structured error, so a failure can be reasoned about rather than crashing the agent
- Authentication credentials belong entirely inside the tool implementation and are never exposed to the model itself

**Executable Agent Logic**

- The full agent loop combines a chat request with tools, tool execution, and appending results back into the conversation for continued reasoning
- Appending each tool result back into `messages` is what feeds the next reasoning step with the real outcome of the last action
- The loop ends when the model responds with no further tool calls
- `max_iterations` is a required safeguard against runaway cost and execution time

**Why All of This Matters Together**

- Function calling is the concrete API mechanism underneath the perceive-reason-act loop from the previous session — tool registries and validation apply the same discipline already used for FastAPI request handling, tool implementations reuse the same service-function architecture from backend integration, and the complete agent loop is simply that conceptual cycle made real and runnable; nothing in today's session was a new architecture, it was the assembly of patterns already known into a system that can genuinely act, not just respond

---

*End of script.*