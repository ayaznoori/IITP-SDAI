# Lecture Script: Defining Agentic Systems
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Agent Definition and the Perception-Reasoning-Action Loop | 22 min |
| 3 | Planning and Reasoning | 22 min |
| 4 | Memory | 18 min |
| 5 | Tool Use | 18 min |
| 6 | Environment Interaction and Feedback Loops | 17 min |
| 7 | Lecture Summary and Recap | 5 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has built AI-powered features — chat integration, structured output, vision, embeddings — and knows how to optimize and control costs. Everything so far has been a single request producing a single response. This lecture introduces a fundamentally different shape: a system that takes multiple steps toward a goal on its own. The hook should make that shift concrete before any formal definitions appear. Wait after the opening question.

**[Script:]**

"Every AI feature you have built so far follows the same shape: the user sends something, your backend sends one request to the model, the model returns one response, you return that to the user. One request in, one response out. Even when the request is complex — a receipt image plus a structured schema — it is still fundamentally one exchange.

Now consider a different kind of task: 'find the cheapest flight to Tokyo next month, book it if it is under my budget, and add it to my calendar.' This is not one request with one response. It requires searching for flights, comparing multiple results, checking a budget constraint, deciding whether to proceed, actually making the booking, and then performing a completely different action — writing to a calendar. No single prompt-response exchange accomplishes this. It requires a system that can take an action, observe what happened, decide what to do next based on that observation, and keep going until the goal is actually achieved.

This is what an agentic system is: not a bigger, smarter version of the chat feature you already built, but a fundamentally different architecture, where the model is not just generating text — it is participating in a loop that perceives its situation, reasons about what to do, and takes actions in a real environment, repeatedly, until a goal is met.

This is also where the risks change. A single bad response from a chat feature is an inconvenience — a wrong answer. A single bad decision inside an autonomous multi-step agent that books flights or moves money can cause real, hard-to-reverse consequences. Understanding what an agent actually is, and the components that make one work, is the foundation for building these systems responsibly, not just capably.

Today: what an agent actually is, the perception-reasoning-action loop that all agentic systems share, how agents plan and reason across multiple steps, how they use memory, how they use tools to affect the real world, and how feedback loops let them course-correct instead of blindly executing a fixed plan."

---

## Block 2 — Agent Definition and the Perception-Reasoning-Action Loop

### 2A — What Makes a System "Agentic"

**[Script:]**

"An agent, in this context, is a system built around an LLM that can autonomously take a sequence of actions toward a goal, rather than simply generating a single response to a single prompt. The word 'autonomously' is doing real work in that definition — the system decides its own next step based on what has happened so far, rather than following a script you wrote in advance.

This is a spectrum, not a strict binary. A chatbot that answers one question is not agentic. A system that decides which of three tools to call based on the user's question, calls one, and returns the result is minimally agentic. A system that plans a multi-step task, executes each step, observes the outcome, and adjusts its plan based on what it learns is fully agentic. Where a given system sits on this spectrum depends on how much autonomous, multi-step decision-making it actually does."

> 🎯 **Instructor Note:** Draw this spectrum on the board — it gives learners a way to place systems they will encounter later, rather than a strict yes/no test.

```
Not agentic                                          Fully agentic
    |------------------|------------------|------------------|
Single response    Tool selection      Multi-step         Autonomous
to one prompt      for one task        task execution      goal pursuit
                                        with fixed plan     with adaptation
```

---

### 2B — The Perception-Reasoning-Action Loop

**[Script:]**

"Every agentic system, regardless of how sophisticated, is built around the same fundamental cycle: perceive, reason, act. This loop is the single most important mental model for this entire session — everything else we cover today is a more detailed look at one part of this cycle."

> 🎯 **Instructor Note:** Draw this loop on the board as a literal circle with arrows, and keep it visible for the entire session — every later block will point back to a specific part of this diagram.

```
        ┌─────────────┐
        │  PERCEIVE   │  ← observe current state of the environment
        └──────┬──────┘
               ↓
        ┌─────────────┐
        │   REASON    │  ← decide what to do next, given the goal
        └──────┬──────┘
               ↓
        ┌─────────────┐
        │     ACT     │  ← take an action that changes the environment
        └──────┬──────┘
               │
               └──────────────┐
                               ↓
                    (loop back to PERCEIVE)
```

**[Script:]**

"Perceive means gathering information about the current state of the world — the result of a search query, the content of a file, the response from an API call, a user's follow-up message. Reason means the LLM processing that information against the goal and deciding what to do next — this is where the model's language understanding does its real work. Act means actually doing something that changes the state of the world — sending a request, writing a file, calling an API, responding to the user.

Critically, the loop does not stop after one pass. The result of the action becomes the next perception. The agent perceives the outcome of what it just did, reasons about whether it is closer to the goal, and decides on the next action. This repeats until the goal is reached, a stopping condition is met, or the agent determines it cannot proceed further."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Walk through the flight-booking example from the hook using this loop. Ask: "Using the perceive-reason-act cycle, what would the first three steps of the flight-booking agent look like?" Guide the room through it collaboratively before revealing the demo — this cements the loop as something concrete, not just an abstract diagram.

**Demo 1 — The loop applied to a concrete task (whiteboard-friendly)**

```
Goal: "Find the cheapest flight to Tokyo next month, book it if 
       under $800, and add it to my calendar."

Cycle 1:
  PERCEIVE: initial goal, no information yet
  REASON:   "I need to search for flights first"
  ACT:      call flight_search_tool(destination="Tokyo", month="next")

Cycle 2:
  PERCEIVE: flight search results — 3 options, cheapest is $650
  REASON:   "$650 is under the $800 budget, I should book it"
  ACT:      call book_flight_tool(flight_id="XJ204")

Cycle 3:
  PERCEIVE: booking confirmation received, confirmation number ABC123
  REASON:   "Booking succeeded, now I need to add this to the calendar"
  ACT:      call add_calendar_event_tool(event="Flight to Tokyo", date=...)

Cycle 4:
  PERCEIVE: calendar event created successfully
  REASON:   "All parts of the goal are complete"
  ACT:      report completion to the user, stop looping
```

**[Script:]**

"Notice each cycle's action produces exactly the information the next cycle's perception needs. The agent does not know in advance that the cheapest flight will be $650 — it finds that out by acting, observing the result, and then reasoning about what that result means for the next step. This is fundamentally different from a fixed script that assumes a specific outcome in advance."

> 🎯 **Instructor Note:** Ask a pointed question here: "What would have happened in Cycle 2 if the cheapest flight had been $950 instead of $650?" Answer: the reasoning step would have concluded the flight exceeds budget, and the action would have been something different — reporting back to the user that no flight was found within budget, rather than proceeding to book. This is the adaptability that distinguishes an agent from a fixed script; the same loop structure produces a different outcome depending on what is actually perceived.

**Recap of Block 2 before moving on:**

- An agent is a system built around an LLM that autonomously takes a sequence of actions toward a goal, rather than producing a single response to a single prompt
- "Agentic" is a spectrum from single-response systems to fully autonomous, adaptive, multi-step systems
- Every agentic system is built around the perceive-reason-act loop: observe the environment, decide the next action, take that action, then repeat with the new observation
- The loop's adaptability — reasoning based on actual observed outcomes rather than assumed ones — is what distinguishes an agent from a fixed script

---

## Block 3 — Planning and Reasoning

### 3A — Why Reasoning Alone Is Not Enough

**[Script:]**

"The reason step in the loop handles one decision at a time — what to do next, given what has been observed so far. But many real goals require thinking several steps ahead before taking any action at all. This is where planning comes in, as a distinct capability layered on top of the basic loop.

Planning means the agent generates a sequence of intended steps toward a goal before executing them, rather than deciding purely one step at a time. This connects directly to chain-of-thought prompting from the prompt engineering session — planning is essentially chain-of-thought applied to a sequence of actions instead of a sequence of reasoning statements."

> 🎯 **Instructor Note:** Draw the contrast between step-by-step reasoning without a plan, and reasoning with an upfront plan.

```
Without explicit planning:
Reason about step 1 → Act → Reason about step 2 (from scratch) → Act → ...
Risk: the agent may take a locally sensible action that is 
      globally inefficient or contradicts a later step

With explicit planning:
Reason: "Here is my full plan: 1) search flights, 2) check budget, 
         3) book if valid, 4) add to calendar"
Then: Act on step 1 → check against the plan → Act on step 2 → ...
Benefit: each action is chosen in the context of the whole goal, 
         not just the immediately preceding observation
```

**[Script:]**

"Planning does not replace the perceive-reason-act loop — it happens inside the reasoning step, typically at the very beginning of a task, and often gets revisited as new information arrives. A plan is not a guarantee; it is a starting hypothesis about how to reach the goal, one that the agent should be willing to revise."

---

### 3B — Reasoning Techniques Inside an Agent

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If an agent's plan assumes a flight will be available and then the flight search returns zero results, what should a well-designed agent do?" Answer: it should recognize the plan's assumption failed, and reason about an alternative — searching different dates, reporting back to the user, or trying a different destination — rather than mechanically continuing with the next step of the original plan as if the search had succeeded.

**Demo 2 — Plan revision based on new information (whiteboard-friendly)**

```
Initial plan (formed before any action):
1. Search flights to Tokyo
2. Check cheapest option against $800 budget
3. Book if within budget
4. Add to calendar

Cycle 1 — ACT: search flights
Cycle 1 — PERCEIVE: zero results found for the requested month

REASON (plan revision):
"My original plan assumed flights would be available. That 
assumption was wrong. I should either try adjacent months, 
or report back to the user that no flights were found rather 
than proceeding with a plan built on a false premise."

Revised plan:
1. Search flights for the following month instead
2. (continue original plan from step 2)
```

**[Script:]**

"This is the difference between a rigid, brittle agent and a genuinely useful one. A plan formed at the start is valuable — it gives direction and avoids purely reactive, short-sighted decisions — but it must be treated as revisable in light of what is actually observed. An agent that mechanically executes a fixed plan regardless of new information is not really reasoning; it is just following a script with extra steps.

This is why reasoning and planning are named as a connected pair rather than separate topics — planning without ongoing reasoning becomes brittle, and reasoning without any planning becomes shortsighted and inefficient. A well-built agent does both together."

> 🎯 **Instructor Note:** Reinforce the connection to chain-of-thought explicitly. Say: "Remember why chain-of-thought helped accuracy on hard reasoning problems — breaking a problem into visible intermediate steps guides the model through better patterns. Planning is that same idea, just applied across multiple real-world actions instead of multiple sentences of reasoning."

**Recap of Block 3 before moving on:**

- Planning means generating a sequence of intended steps toward a goal before executing them, rather than deciding purely one step at a time
- Planning happens within the reasoning step of the loop, typically at the start of a task and revisited as new information arrives
- A plan is a starting hypothesis, not a guarantee — a well-designed agent revises its plan when observed outcomes contradict its assumptions
- Planning and ongoing reasoning work together: planning alone becomes brittle, reasoning alone becomes shortsighted

---

## Block 4 — Memory

### 4A — Why Agents Need Memory Beyond a Single Conversation

**[Script:]**

"You already know from the chat APIs session that a conversation's history must be resent with every request, since the underlying model itself is stateless. That conversation history is a form of memory — but it is short-term, limited to what fits in the current context window, and it disappears once the conversation ends.

Agentic systems often need more than this. They need to remember facts learned earlier in a long task even after those details scroll out of the immediate conversation. They may need to remember information across entirely separate sessions — what a user prefers, what has already been tried and failed, what the outcome of a previous task was. This requires memory architectures beyond simply resending the chat history."

---

### 4B — Types of Agent Memory

**[Script:]**

"There are two broad categories of memory worth distinguishing."

> 🎯 **Instructor Note:** Write this distinction on the board.

```
Short-term memory (working memory):
- The current conversation or task's context window
- Exists only for the duration of the current task or session
- What we already know from chat APIs — resent with every request

Long-term memory (persistent memory):
- Stored outside the model, retrieved when relevant
- Survives across sessions and conversations
- Often implemented using embeddings and similarity search
```

**[Script:]**

"Short-term memory is exactly the message history pattern from the chat APIs session — it works, but it is bounded by context window size and vanishes once the session ends. Long-term memory is architecturally different: information is stored somewhere persistent — a database, a vector store — and retrieved only when relevant to the current situation, rather than being kept in the context window at all times."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Based on the embeddings session, how might an agent find a relevant memory from thousands of stored past interactions, without including all of them in every prompt?" Answer: the exact retrieval pattern from the embeddings session — embed the current situation, compare it against embeddings of stored memories, and retrieve only the closest matches to include in context. Long-term agent memory is a direct application of that same retrieval mechanism.

**Demo 3 — Long-term memory retrieval in an agent (whiteboard-friendly)**

```python
def get_relevant_memories(current_situation: str, top_k: int = 3) -> list[str]:
    situation_embedding = generate_embedding(current_situation)
    
    all_memories = memory_store.get_all()  # e.g. past user preferences, past outcomes
    scored = [
        (memory, cosine_similarity(situation_embedding, memory.embedding))
        for memory in all_memories
    ]
    scored.sort(key=lambda x: x[1], reverse=True)
    
    return [memory.text for memory, score in scored[:top_k]]

# Used inside the reasoning step, before deciding the next action:
relevant_context = get_relevant_memories("booking a flight to Tokyo")
# → ["User prefers morning flights", "User previously flagged high 
#     layover times as unacceptable"]
```

**[Script:]**

"This should look immediately familiar — it is the exact embedding-and-similarity pattern from the advanced AI integration session, applied here to retrieving relevant past context instead of retrieving relevant documents. The agent's reasoning step is enriched with only the memories relevant to the current situation, not the agent's entire memory history, for the same context-optimization reasons covered in the cost optimization session — including everything would waste tokens and dilute relevance."

> 🎯 **Instructor Note:** Connect explicitly: "This is not a new technique. It is the retrieval pattern from embeddings, applied to a new kind of content — the agent's own past experience instead of a document database." Naming this connection directly helps learners see agent memory as composition of known parts, not a new unfamiliar system.

**Recap of Block 4 before moving on:**

- Short-term memory is the conversation's context window — bounded, and does not survive past the current session
- Long-term memory is stored persistently outside the model and retrieved only when relevant, often via embeddings and similarity search
- Long-term memory retrieval directly reuses the embedding and similarity pattern from the embeddings session, now applied to an agent's own past experience
- Retrieving only relevant memories, rather than including everything, follows the same context-optimization discipline covered in the cost optimization session

---

## Block 5 — Tool Use

### 5A — Why Agents Need Tools

**[Script:]**

"An LLM on its own can only generate text. It cannot search the web, query a database, send an email, or book a flight. Tool use is what bridges this gap — giving the agent the ability to call external functions, and incorporating the results of those calls back into its reasoning.

This is the 'act' step of the perceive-reason-act loop made concrete. Every action in the Block 2 flight-booking example — searching, booking, adding to a calendar — was a tool call. Tool use is not a separate add-on to the loop; it is what makes the act step meaningful beyond just generating a text response."

---

### 5B — How Tool Use Actually Works

**[Script:]**

"Mechanically, tool use works through a pattern you have already seen the pieces of: the model is given a description of available tools — their names, what they do, what parameters they take — much like the structured output schemas from earlier. Rather than generating a plain text answer, the model can generate a structured request to call a specific tool with specific arguments. Your backend code executes that actual function call, and the result is fed back into the conversation as a new message, which the model then reasons over."

> 🎯 **Instructor Note:** Draw this exchange on the board step by step — this mechanical flow is the concrete implementation of the "act" step from the Block 2 loop diagram.

```
1. Model is given tool definitions:
   search_flights(destination, month) → list of flights
   book_flight(flight_id) → confirmation

2. Model reasons: "I need flight options first"
   Model outputs a structured tool call, not plain text:
   { "tool": "search_flights", "args": {"destination": "Tokyo", "month": "March"} }

3. Your backend code actually executes search_flights(...)
   Gets back real results: [{"id": "XJ204", "price": 650}, ...]

4. Result is added to the conversation as a new message
   Model perceives this result and reasons about the next step
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If the model requests a tool call with a destination spelled incorrectly, or with a required parameter missing, what should happen?" Answer: your backend code should validate the request before executing it — exactly like validating any other external input with Pydantic — and return an error message back to the model as the result, rather than crashing or executing with bad data. The model can then reason about the error and retry with corrected arguments.

**Demo 4 — Defining and validating a tool call (whiteboard-friendly)**

```python
from pydantic import BaseModel

class SearchFlightsArgs(BaseModel):
    destination: str
    month: str

def execute_tool_call(tool_name: str, raw_args: dict):
    if tool_name == "search_flights":
        try:
            args = SearchFlightsArgs(**raw_args)
        except Exception as e:
            return {"error": f"Invalid arguments: {e}"}
        return search_flights(args.destination, args.month)
    return {"error": f"Unknown tool: {tool_name}"}
```

**[Script:]**

"This should look exactly like validating any other external input in your application — a Pydantic model defines what a valid tool call looks like, and invalid input produces a clear error rather than an uncontrolled failure. The critical design point: never execute a tool call's arguments blindly, exactly as you would never trust unvalidated input from a user-facing API endpoint. A tool call from the model is external input, generated by a probabilistic process, and deserves the same validation discipline as any request coming from outside your system."

> 🎯 **Instructor Note:** This is a genuine safety point, not just a code-quality point. Emphasize: "A tool call is the point where the model's output stops being just text and starts having real effects — sending money, deleting data, sending an email. Treat every tool call exactly as skeptically as you would treat unvalidated input from an anonymous user, because structurally, that is what it is."

**Recap of Block 5 before moving on:**

- Tool use gives an agent the ability to call external functions and incorporate real results back into its reasoning — this is what makes the "act" step in the loop meaningful
- The model generates a structured tool call rather than plain text; your backend code executes the actual function and returns the result
- Tool call arguments must be validated before execution, exactly like any other external input — a model-generated tool call is not inherently trustworthy input
- Invalid tool calls should return a clear error the model can reason about and retry, rather than causing an uncontrolled failure

---

## Block 6 — Environment Interaction and Feedback Loops

### 6A — What "Environment" Means for an Agent

**[Script:]**

"The environment is everything outside the agent that it perceives and acts upon — a filesystem, a web browser, an API, a database, a physical device. Every tool from Block 5 is a specific way of interacting with some part of the environment. The environment is also the source of every perception in the loop — it is where the agent's actions actually take effect and where the consequences of those actions become observable."

> 🎯 **Instructor Note:** Connect back to the Block 2 diagram directly. Say: "Look back at the loop we drew at the start. The environment is what sits between the ACT arrow and the PERCEIVE arrow — it is the real world the agent is acting on and observing, whatever that happens to be for a specific agent: a codebase, a browser, a set of business APIs."

---

### 6B — Feedback Loops

**[Script:]**

"A feedback loop is the mechanism by which the outcome of an action informs the next decision — this is, in a sense, the entire perceive-reason-act loop viewed from a different angle, with emphasis on how errors and unexpected outcomes specifically get incorporated rather than ignored.

Good feedback loops make agents robust to the real world being messier than any plan anticipated. A tool call might fail. A search might return no results. An action might partially succeed. How the agent handles these moments — whether it notices them at all, and what it does in response — determines whether it is a genuinely reliable system or one that quietly does the wrong thing and reports success anyway."

> 🎯 **Instructor Note:** This last point deserves emphasis. Ask directly: "What is worse — an agent that fails and reports the failure honestly, or an agent that fails silently and reports success anyway?" Let the room reason through it. The silent failure is worse in almost every real scenario, because it removes the human's ability to notice and intervene. A core design goal for feedback loops is making failures visible, not hiding them behind an optimistic final report.

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If the calendar step in the flight-booking example fails after the flight was already successfully booked, what should a well-designed feedback loop do?" Answer: it should recognize that only part of the goal succeeded, and report that specifically — 'flight booked successfully, but I was unable to add it to your calendar' — rather than either reporting complete success or discarding the successful booking information. Partial success is a real, common outcome that feedback loops need to represent accurately.

**Demo 5 — A feedback loop handling partial failure (whiteboard-friendly)**

```
Cycle 3 — ACT: call add_calendar_event_tool(...)
Cycle 3 — PERCEIVE: error returned — "Calendar API authentication failed"

REASON:
"The flight booking in Cycle 2 succeeded — I have a confirmation 
number. The calendar step failed for an unrelated reason. I should 
not treat this as a total failure, and I should not silently drop 
the calendar step and pretend everything is fine. I will report 
the partial outcome clearly."

ACT: report to user —
"Your flight is booked (confirmation ABC123), but I could not add 
it to your calendar due to an authentication error. You may want 
to add it manually or reconnect your calendar."
```

**[Script:]**

"This is what a well-designed feedback loop produces: an honest, specific account of what actually happened, incorporating both the success and the failure, rather than collapsing a mixed outcome into a false 'all done' or an unhelpfully vague 'something went wrong'. This is only possible because the agent's reasoning step actually processes the perceived outcome of each action rather than assuming success and moving on.

This closes the loop back to where we started this session. Perceive, reason, act — repeated, with real attention paid to what is actually perceived at each step — is not just the mechanical structure of an agent. It is also what makes an agent trustworthy or untrustworthy, useful or dangerous, depending on how carefully that middle reasoning step actually engages with reality instead of assuming its plan is working."

**Recap of Block 6 before moving on:**

- The environment is everything outside the agent that it perceives and acts upon; tools are the specific mechanisms for interacting with it
- A feedback loop is the perceive-reason-act loop viewed with emphasis on how outcomes — especially unexpected or failed ones — are incorporated into the next decision
- Silent failure, where an agent reports success despite a real failure, is significantly worse than honest failure reporting, because it removes the opportunity for human intervention
- A well-designed feedback loop accurately represents partial success and partial failure, rather than collapsing outcomes into an oversimplified "done" or "failed"

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What are the three steps of the core agent loop? What is the difference between planning and one-step-at-a-time reasoning? What are the two types of agent memory, and how does long-term memory retrieval actually work? Why must tool calls be validated? What makes a feedback loop good or bad?"

**Agent Definition and the Perception-Reasoning-Action Loop**

- An agent autonomously takes a sequence of actions toward a goal, rather than producing a single response to a single prompt; "agentic" is a spectrum, not a binary
- Every agentic system is built around perceive, reason, act — observe, decide, act, then repeat with the new observation
- The loop's adaptability to actual observed outcomes, rather than assumed ones, is what distinguishes an agent from a fixed script

**Planning and Reasoning**

- Planning generates a sequence of intended steps before execution, extending chain-of-thought reasoning across multiple real-world actions
- A plan is a starting hypothesis, not a guarantee, and should be revised when observations contradict its assumptions
- Planning and ongoing reasoning work together — planning alone is brittle, reasoning alone is shortsighted

**Memory**

- Short-term memory is the current context window, bounded and session-limited, same as chat history from earlier sessions
- Long-term memory is stored persistently and retrieved only when relevant, typically via the same embedding and similarity pattern from the embeddings session
- Retrieving only relevant memories rather than everything follows the same context-optimization discipline as cost optimization

**Tool Use**

- Tool use gives the agent the ability to call external functions and incorporate real results into its reasoning — this is the concrete implementation of the "act" step
- The model generates a structured tool call; backend code executes the actual function and validates the arguments first
- A tool call is external, model-generated input and must be treated with the same validation discipline as any untrusted input

**Environment Interaction and Feedback Loops**

- The environment is everything outside the agent that it perceives and acts upon; tools are the mechanism for interacting with it
- A feedback loop is the perceive-reason-act loop viewed through how outcomes, especially failures, get incorporated into the next decision
- Silent failure is more dangerous than honest failure reporting, because it removes the opportunity for a human to notice and intervene
- Good feedback loops accurately represent partial success and partial failure rather than collapsing outcomes into a false "done"

**Why All of This Matters Together**

- An agentic system is not a bigger version of a single-request AI feature — it is a different architecture built around a repeating loop, where planning gives direction, memory provides continuity, tools connect the model to the real world, and feedback loops keep the whole system honest about what has actually happened; understanding these components individually, and how they compose into the same underlying perceive-reason-act cycle, is the foundation for building agentic systems that are not just capable, but trustworthy enough to act with real consequences

---

*End of script.*