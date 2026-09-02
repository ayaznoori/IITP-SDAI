# Lecture Script: Agentic Debugging and Performance
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Agent Debugging | 25 min |
| 3 | Reasoning Evaluation | 22 min |
| 4 | Performance Tracking | 25 min |
| 5 | Iterative Logic Refinement | 22 min |
| 6 | Lecture Summary and Recap | 8 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience just built a complete, executable agent loop in the previous session — the `run_agent` function, tool registry, and multi-iteration reasoning cycle. This session addresses what happens when that agent does not behave the way it is supposed to, which is a matter of when, not if. Open by naming why debugging an agent is genuinely different from debugging a normal program. Wait after the opening question.

**[Script:]**

"You already know how to debug regular code — the debugging-with-AI session covered reproducing a bug with a specific input, tracing execution, testing a fix. That approach assumes something important: that running the same code with the same input produces the same result every time. Agents break that assumption.

Your `run_agent` function from last session might call `search_flights` first on one run, and reason its way to a completely different sequence of tool calls on a functionally identical run — because the model's output is not fully deterministic, and because a five-step reasoning loop has many more places something can subtly go wrong than a single function call does. The bug might not be a crash at all. It might be an agent that runs successfully, calls all the right tools, and still arrives at a wrong or unhelpful final answer, with no error anywhere in the trace to point you toward the problem.

This changes what debugging, evaluation, and improvement actually mean for agentic systems. You need to inspect not just whether the code ran without crashing, but whether the reasoning at each step actually made sense. You need to track performance — cost, latency, iteration count — because an agent that works but takes twelve iterations and costs ten times more than expected is a real production problem, not a success. And when you find a real weakness in how the agent reasons or acts, you need a disciplined way to actually improve it, rather than randomly tweaking the system prompt and hoping.

Today covers four connected skills: debugging an agent when something goes wrong, evaluating whether its reasoning was actually sound even when it technically produced an answer, tracking performance metrics that matter specifically for agentic systems, and refining agent logic iteratively based on what you observe — not through guesswork, but through the same disciplined process you would apply to any other engineering problem."

---

## Block 2 — Agent Debugging

### 2A — Why Agent Debugging Is Different

**[Script:]**

"A normal function has one input and one output, and a bug means the output is wrong given that input — you can reproduce it, trace it, fix it. An agent's `run_agent` function has one input, but potentially many internal steps, each involving a probabilistic model decision, each capable of going wrong in a different way, and each affecting every step after it.

This means agent debugging requires visibility into the entire trace, not just the final result. If the final answer is wrong, you need to see every reasoning step and every tool call that led there, to find out where things actually diverged from what you expected — the wrong tool might have been called, the right tool might have been called with wrong arguments, or the right tool might have returned a correct result that the model then reasoned about incorrectly."

> 🎯 **Instructor Note:** Write this contrast on the board.

```
Debugging a normal function:
  input → [black box] → wrong output
  Trace: reproduce, inspect the function's internal logic, fix

Debugging an agent:
  input → reason → act → observe → reason → act → observe → ... → output
  Trace: inspect EVERY step, since the failure could be at any one
         of them, and later steps depend entirely on earlier ones
```

---

### 2B — Logging the Full Agent Trace

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If your `run_agent` function from last session only logs the final return value, and an agent gives a wrong answer after three tool calls, what information do you actually have to debug the problem?" Answer: almost none — you know the final answer was wrong, but nothing about which tool calls happened, what arguments were used, or what the model's reasoning looked like at each step. This makes the point that logging only the final output is not enough for agentic systems.

**Demo 1 — Instrumenting the agent loop with full trace logging (whiteboard-friendly)**

```python
def run_agent(user_message: str, tools: list, max_iterations: int = 5) -> str:
    messages = [{"role": "user", "content": user_message}]
    trace = []

    for iteration in range(max_iterations):
        response = client.chat.completions.create(
            model="gpt-4o-mini", messages=messages, tools=tools
        )
        message = response.choices[0].message

        if not message.tool_calls:
            trace.append({"iteration": iteration, "type": "final_answer", "content": message.content})
            log_trace(trace)
            return message.content

        messages.append(message)
        for tool_call in message.tool_calls:
            tool_name = tool_call.function.name
            arguments = json.loads(tool_call.function.arguments)
            result = execute_tool(tool_name, arguments)

            trace.append({
                "iteration": iteration,
                "type": "tool_call",
                "tool": tool_name,
                "arguments": arguments,
                "result": result
            })

            messages.append({"role": "tool", "tool_call_id": tool_call.id, "content": json.dumps(result)})

    trace.append({"iteration": max_iterations, "type": "max_iterations_reached"})
    log_trace(trace)
    return "Reached maximum iterations without a final answer."
```

**[Script:]**

"This is the exact same `run_agent` loop from last session, with one addition: `trace`, a list that records every tool call, its arguments, its result, and the final answer, in order. `log_trace(trace)` — a function you would implement to write this somewhere durable, like a file or a logging service — gives you a complete, ordered record of exactly what the agent did on this specific run.

This is not optional infrastructure for a real agentic system. Without a trace like this, a wrong final answer is nearly undebuggable — you would be guessing at which of several reasoning steps went wrong, with no way to actually confirm it."

> 🎯 **Instructor Note:** Ask: "If a user reports an agent gave a wrong answer, but you have no trace logging in place, what would your only option be to investigate?" Answer: attempting to reproduce the exact same request and hope the agent behaves the same way again — which, given the model's non-deterministic behavior, is not guaranteed. Trace logging turns an unreliable "try to reproduce it" into a reliable "look at exactly what happened."

---

### 2C — Reading a Trace to Find the Failure

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show this trace on the board. Ask: "Before I tell you what went wrong — read through this trace and see if you can spot where the agent's reasoning likely diverged from what it should have done." Give the room real time to look.

**Demo 2 — Diagnosing a failure from a trace (whiteboard-friendly)**

```
User: "What's the total cost for a $45 bill with an 18% tip?"

Trace:
iteration 0 — tool_call: calculate_tip
  arguments: {"bill_amount": 45, "percentage": 18}
  result: {"tip": 8.1, "total": 53.1}

iteration 1 — final_answer:
  "The total cost, including an 18% tip on your $63 bill, is $53.10."
```

**[Script:]**

"The tool call itself is correct — right tool, right arguments, right result. The failure is in the final reasoning step: the model's text response says '$63 bill' when the actual bill amount, both in the request and in the tool call it made, was $45. The tool executed perfectly. The model's final language generation, summarizing that correct result, introduced an error that has nothing to do with the tool at all.

Without the full trace, all you would see is a wrong final answer, and you might incorrectly suspect the `calculate_tip` function itself has a bug. With the trace, it is immediately clear the tool worked correctly, and the actual problem is in how the model summarized the result — a completely different fix, in a completely different place."

> 🎯 **Instructor Note:** This is the entire value of trace-based debugging demonstrated concretely. Emphasize: "Without this trace, you would likely have gone looking for a bug in `calculate_tip` — the wrong place entirely. The trace immediately narrows the problem to exactly where it actually is: not the tool, but the final language step summarizing a tool result that was correct all along."

**Recap of Block 2 before moving on:**

- Agent debugging requires visibility into every step of the reasoning-action cycle, not just the final output, since a failure can originate at any single step
- A full trace records every tool call, its arguments, its result, and the final answer, in order, for a given agent run
- Trace logging turns an unreliable "try to reproduce it" into a reliable, inspectable record of exactly what happened
- A wrong final answer does not necessarily mean a tool is broken — the trace often reveals the actual failure is in a completely different step than the one you would have suspected

---

## Block 3 — Reasoning Evaluation

### 3A — Why "It Produced an Answer" Is Not Enough

**[Script:]**

"An agent can run to completion, hit no errors, call all technically correct tools, and still produce output that reflects poor reasoning — an unnecessary tool call, an inefficient path to the answer, a conclusion not actually well-supported by what was retrieved. Debugging, from Block 2, finds cases where something is clearly broken. Reasoning evaluation is a step further: judging whether the agent's reasoning was actually good, even when nothing crashed and no factual error occurred."

---

### 3B — Criteria for Evaluating Agent Reasoning

**[Script:]**

"There are several concrete dimensions worth evaluating in a trace, beyond simply 'did it get the right answer.'"

> 🎯 **Instructor Note:** Write these evaluation dimensions on the board.

```
Reasoning evaluation dimensions:
- Necessity     — was each tool call actually needed for this task?
- Efficiency    — could the same result have been reached in fewer steps?
- Grounding     — is the final answer actually supported by tool results,
                  or does it add unsupported claims?
- Recovery      — when a tool failed or returned an unexpected result, 
                  did the agent adapt sensibly?
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show this trace on the board. Ask: "Evaluate this trace against the four dimensions just listed, before I walk through it — is anything inefficient or unnecessary here?" Let the room look for a moment before revealing the analysis.

**Demo 3 — Evaluating a technically successful but inefficient trace (whiteboard-friendly)**

```
User: "What's the weather in Tokyo?"

Trace:
iteration 0 — tool_call: get_weather
  arguments: {"city": "Tokyo"}
  result: {"city": "Tokyo", "temperature": 18, "condition": "Cloudy"}

iteration 1 — tool_call: get_weather
  arguments: {"city": "Tokyo"}
  result: {"city": "Tokyo", "temperature": 18, "condition": "Cloudy"}

iteration 2 — final_answer:
  "The weather in Tokyo is currently 18°C and cloudy."
```

**[Script:]**

"The final answer is correct. Nothing crashed. But `get_weather` was called twice, with identical arguments, producing an identical result both times — the second call was completely unnecessary, wasting an API call, tokens, latency, and cost, for zero additional information. This would not show up as a bug in the traditional sense — no error, no wrong answer — but it fails the efficiency dimension of reasoning evaluation clearly.

This is exactly the kind of issue that only becomes visible when you evaluate the trace deliberately against criteria like these, rather than just checking whether the final answer happens to be correct."

> 🎯 **Instructor Note:** Ask: "What real-world cost does this redundant tool call actually have, beyond just looking inefficient on paper?" Answer: this connects directly to the cost optimization session — every unnecessary tool call and reasoning iteration is a real API cost and added latency. At the scale of many users making many requests, redundant reasoning like this compounds into a genuinely significant and avoidable expense.

---

### 3C — Evaluating Grounding in Agent Reasoning

**[Script:]**

"The grounding dimension connects directly to the RAG session — is the agent's final conclusion actually supported by what its tools returned, or does it introduce claims that were never in any tool result at all? This is the agentic equivalent of hallucination, and it is worth checking for specifically, since a confident final answer can still contain unsupported details woven in alongside the accurate ones."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Looking back at Demo 2 from Block 2 — the tip calculation with the wrong bill amount stated in the final answer — which reasoning evaluation dimension does that failure actually belong to?" Answer: grounding — the tool result was correct and accurate, but the final answer's language introduced a detail ($63 instead of $45) that was not actually supported by anything the tool returned. This connects Block 2's debugging example directly to this block's evaluation framework.

**[Script:]**

"This is a genuinely important distinction to hold onto: a debugging failure, from Block 2, is usually something clearly broken — a crash, a wrong tool called, invalid arguments. A reasoning evaluation failure can be much subtler — everything technically worked, and the final answer still is not fully trustworthy. Both matter, and they require different techniques to catch: debugging relies on tracing execution; reasoning evaluation relies on deliberately checking the trace against quality criteria like these, even when nothing appears broken on the surface."

**Recap of Block 3 before moving on:**

- Reasoning evaluation judges the quality of an agent's reasoning, separate from whether it technically produced an answer without crashing
- Key evaluation dimensions include necessity, efficiency, grounding, and recovery from unexpected tool results
- A trace can show a technically correct final answer that still reflects poor reasoning — unnecessary tool calls, wasted iterations, or claims not actually supported by tool results
- Grounding failures in agent reasoning are the agentic equivalent of hallucination, and require deliberately checking the final answer against actual tool results, not just checking whether the answer sounds right

---

## Block 4 — Performance Tracking

### 4A — What to Actually Measure

**[Script:]**

"Beyond correctness and reasoning quality, an agentic system needs measurable performance tracking — the same discipline as the cost optimization session, applied specifically to a multi-step agent rather than a single API call. Several metrics matter specifically because an agent has multiple steps, where a single chat request does not."

> 🎯 **Instructor Note:** Write these agent-specific metrics on the board.

```
Agent performance metrics:
- Iteration count     — how many reasoning cycles did this run take?
- Tool call count      — how many total tool calls were made?
- Total tokens         — across ALL iterations, not just one request
- Total latency        — end-to-end time for the full loop to complete
- Success rate         — across many runs, how often does the agent 
                          reach a valid final answer within max_iterations?
```

**[Script:]**

"Recall from the cost optimization session that `response.usage` gives you token counts for one single API call. An agent loop makes multiple calls per run — one per iteration — so tracking cost and performance means aggregating this across every iteration in the loop, not just looking at a single request's usage."

---

### 4B — Instrumenting the Loop for Performance Tracking

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If an agent takes five iterations to answer a question that a well-designed agent should resolve in two, what does that tell you, even if the final answer is completely correct?" Answer: the agent is technically working, but inefficiently — this is exactly the kind of issue performance tracking surfaces that pure correctness checking would miss entirely, since the final answer being right hides the fact that it took far more cost and time than necessary to get there.

**Demo 4 — Tracking performance metrics across the full loop (whiteboard-friendly)**

```python
def run_agent(user_message: str, tools: list, max_iterations: int = 5) -> dict:
    messages = [{"role": "user", "content": user_message}]
    total_tokens = 0
    tool_call_count = 0
    start_time = time.time()

    for iteration in range(max_iterations):
        response = client.chat.completions.create(
            model="gpt-4o-mini", messages=messages, tools=tools
        )
        total_tokens += response.usage.total_tokens
        message = response.choices[0].message

        if not message.tool_calls:
            return {
                "answer": message.content,
                "iterations": iteration + 1,
                "tool_calls": tool_call_count,
                "total_tokens": total_tokens,
                "latency_seconds": round(time.time() - start_time, 2)
            }

        messages.append(message)
        for tool_call in message.tool_calls:
            tool_call_count += 1
            # ... execute tool and append result, as before ...

    return {"answer": None, "iterations": max_iterations, "tool_calls": tool_call_count,
            "total_tokens": total_tokens, "latency_seconds": round(time.time() - start_time, 2)}
```

**[Script:]**

"`total_tokens` accumulates `response.usage.total_tokens` across every single iteration of the loop, not just the last one — this is the actual cost driver for the entire request, and it is easy to underestimate if you only look at one call's usage. `tool_call_count` tracks how many actual tool executions happened. `start_time` and the final elapsed calculation give you true end-to-end latency for the whole multi-step process, not just one API call's response time.

The function now returns a structured dictionary instead of just a plain answer string — this is deliberate, so every caller of `run_agent` has access to these metrics alongside the actual result, which you would then log and aggregate over many real runs to spot patterns: which kinds of requests take the most iterations, which cost the most, where latency is highest."

> 🎯 **Instructor Note:** Ask: "Why track these metrics per-run and aggregate them over time, rather than just checking them occasionally when something seems slow?" Answer: aggregated data over many real runs reveals patterns a single spot-check cannot — whether certain types of questions consistently take more iterations, whether a specific tool is unusually slow, whether cost is trending upward as usage grows. This is the same "log and aggregate" discipline from the cost optimization session, applied here to a richer set of agent-specific metrics.

**Recap of Block 4 before moving on:**

- Agent performance tracking requires metrics specific to multi-step systems: iteration count, tool call count, total tokens across the whole run, end-to-end latency, and success rate
- Token cost must be aggregated across every iteration of the loop, not read from a single API call's usage
- A correct final answer can still represent poor performance if it took far more iterations, tokens, or time than a well-designed agent should need
- Logging structured metrics per run and aggregating over time reveals patterns a single spot-check cannot surface

---

## Block 5 — Iterative Logic Refinement

### 5A — Refinement as a Disciplined Process, Not Guesswork

**[Script:]**

"Once debugging, reasoning evaluation, and performance tracking surface real problems — a redundant tool call, an ungrounded final answer, a consistently high iteration count for a certain kind of question — the next step is actually fixing the agent's logic. This should follow the same discipline as any other engineering refinement: identify the specific problem from real evidence, form a specific hypothesis about the cause, make one targeted change, and re-evaluate against the same criteria to confirm the change actually helped."

> 🎯 **Instructor Note:** Write this refinement cycle on the board — it is the practical takeaway of the entire session.

```
Iterative refinement cycle:
1. Observe a specific problem (from trace, evaluation, or metrics)
2. Form a specific hypothesis about the cause
3. Make ONE targeted change
4. Re-run and re-evaluate against the same criteria
5. Confirm improvement, or revert and try a different hypothesis
```

**[Script:]**

"The word 'one' in step three matters. Changing the system prompt, the tool descriptions, and the max_iterations limit all at once might produce a better result, but you will not know which change actually caused the improvement — and you might have introduced a new problem alongside the fix, hidden by an unrelated improvement elsewhere. This is exactly the same test-one-change-at-a-time discipline from the refactoring session, applied to agent behavior instead of code structure."

---

### 5B — Refining Based on a Reasoning Evaluation Finding

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Return to the redundant `get_weather` call from Demo 3 in Block 3. Ask: "Given that the problem was an unnecessary duplicate tool call, what is a specific, targeted change you would try first — not a vague 'make it better,' but something concrete?" Guide toward: improving the tool's description to make it clearer that repeated identical calls are unnecessary, or explicitly instructing the system prompt not to call the same tool with the same arguments twice.

**Demo 5 — A targeted refinement based on observed evidence (whiteboard-friendly)**

```python
# BEFORE — vague tool description, redundant calls observed
weather_tool_before = {
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get the current weather for a specific city.",
        "parameters": { ... }
    }
}

# AFTER — targeted change based on the specific evidence from Block 3
weather_tool_after = {
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": (
            "Get the current weather for a specific city. "
            "Only call this once per city per conversation — "
            "the result does not change within a single request."
        ),
        "parameters": { ... }
    }
}
```

**[Script:]**

"This is a single, targeted change — the tool description now explicitly discourages the exact redundant behavior observed in the trace. This is not a guess; it is a direct response to specific evidence gathered in Block 3. The next step, following the refinement cycle, is to re-run the same or similar requests and check the trace again: did the redundant call actually stop happening, without introducing a new problem elsewhere?"

> 🎯 **Instructor Note:** Ask: "How would you actually confirm this change worked, rather than just assuming a clearer description fixed the problem?" Answer: re-run the same weather question, or ideally several similar questions, and check the resulting traces specifically for the redundant-call pattern. If it is gone across multiple re-runs, and tool call counts and token usage from Block 4's metrics have measurably decreased for this type of question, that is real confirmation — not just an assumption that a better-worded description must have helped.

---

### 5C — Refining Based on Debugging and Performance Evidence

**[Script:]**

"The same disciplined cycle applies to fixes originating from Block 2's debugging or Block 4's performance tracking. If a trace revealed a grounding failure — like the incorrect bill amount from Demo 2 — a targeted fix might be adding an explicit instruction to the system prompt: 'when summarizing a tool result, use only the exact values returned by the tool, and do not restate or paraphrase numeric values from memory.' If performance metrics revealed consistently high iteration counts for a certain category of question, a targeted fix might be adding a more capable tool that resolves that category in fewer steps, or improving the system prompt's guidance on which tool to reach for first.

In every case, the pattern is the same: a specific problem, observed with real evidence from the techniques covered in this session, drives one specific change, which is then re-evaluated using those same techniques to confirm it actually worked."

> 🎯 **Instructor Note:** Close with a synthesis question. Ask: "If you made a change intended to fix a grounding problem, re-ran your evaluation, and the grounding issue was gone — but iteration count had gone up significantly — what would you do next?" Answer: recognize that the fix may have introduced a new tradeoff, and investigate whether the new instruction caused the agent to take extra, overly cautious steps. This might be an acceptable tradeoff, or might need further refinement — but you would only know this by continuing to monitor all the relevant metrics together, rather than declaring success the moment the original problem is gone.

**Recap of Block 5 before moving on:**

- Refinement follows a disciplined cycle: observe a specific problem, form a specific hypothesis, make one targeted change, re-evaluate, confirm or revert
- Changing multiple things at once makes it impossible to know which change actually caused an observed improvement, and can hide new problems introduced alongside a fix
- Refinements are targeted responses to specific evidence from debugging, reasoning evaluation, or performance tracking — not vague, general improvements
- Confirming a fix worked means re-running and re-checking against the same criteria that surfaced the original problem, and watching for new tradeoffs introduced elsewhere

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "Why is a full trace necessary for agent debugging, beyond just the final output? What is the difference between a debugging failure and a reasoning evaluation failure? Why must token cost be aggregated across the whole loop, not just one API call? Why is changing only one thing at a time essential during refinement?"

**Agent Debugging**

- Agent debugging requires visibility into every step of the loop, since a failure can originate at any single step and affect everything after it
- A full trace records every tool call, arguments, results, and the final answer, in order, for a given run
- A wrong final answer does not necessarily mean the tool that ran is broken — the trace often reveals the actual failure is elsewhere, such as in the model's final summarization step

**Reasoning Evaluation**

- Reasoning evaluation judges the quality of an agent's reasoning even when it technically produced an answer without crashing
- Key dimensions: necessity, efficiency, grounding, and recovery from unexpected tool results
- Grounding failures — a final answer containing claims not actually supported by tool results — are the agentic equivalent of hallucination

**Performance Tracking**

- Agent-specific metrics include iteration count, tool call count, total tokens across the entire run, end-to-end latency, and success rate
- Token cost must be summed across every iteration of the loop, not read from a single call's usage
- A correct final answer can still represent poor performance if it took excessive iterations, tokens, or time

**Iterative Logic Refinement**

- Refinement follows a disciplined cycle: observe a specific problem, form a hypothesis, make one targeted change, re-evaluate, confirm or revert
- Changing one thing at a time is essential to know which change actually caused an observed improvement
- Every refinement should be a targeted response to specific evidence, and every fix should be re-confirmed against the same criteria that surfaced the original problem

**Why All of This Matters Together**

- Debugging, reasoning evaluation, and performance tracking are three different lenses for finding what is actually wrong with an agent — a crash, a technically-correct-but-poor decision, or an inefficient use of cost and time — and iterative refinement is the disciplined process that turns what you find into a real, confirmed improvement rather than a guess; together these four skills are what separates an agent that merely runs from one that is genuinely reliable, efficient, and trustworthy enough to keep improving over time as real usage reveals its actual weaknesses

---

*End of script.*
