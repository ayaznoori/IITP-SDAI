# Lecture Script: Building with Agent Frameworks
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Agent Frameworks — Why Not Just Keep Building It By Hand? | 20 min |
| 3 | LangChain — Core Concepts and Building an Agent | 30 min |
| 4 | Multi-Agent Systems | 25 min |
| 5 | Hierarchical Planning | 20 min |
| 6 | Lecture Summary and Recap | 7 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience built a working `run_agent` loop by hand — tool registry, validation, the full perceive-reason-act cycle in real code — and then instrumented it for debugging and performance tracking. This session introduces frameworks that provide much of that same machinery pre-built, and extends into systems with more than one agent. The hook should honestly acknowledge what was just built by hand, and motivate why a framework still matters. Wait after the opening question.

**[Script:]**

"You built a real, working agent loop by hand across the last two sessions — tool definitions, a registry, validation, the reasoning cycle, trace logging, performance metrics. That was not wasted effort; understanding what is actually happening inside that loop is exactly why the rest of today will make sense instead of feeling like magic. But building all of that from scratch, every single time, for every new agent, does not scale well as a development practice.

Agent frameworks like LangChain provide pre-built implementations of the exact loop you wrote by hand — tool calling, message management, the iteration cycle — along with a large ecosystem of ready-made integrations: connectors to databases, search engines, document loaders, and dozens of other common building blocks you would otherwise have to write yourself. Using a framework does not replace the understanding from the last two sessions; it lets you apply that understanding faster, without reimplementing the same plumbing every time.

There is also a class of problem that a single agent, however well built, genuinely struggles with: tasks that benefit from splitting work across multiple specialized agents, each good at one thing, coordinated together — a researcher agent, a writer agent, a reviewer agent, working as a team rather than one generalist agent trying to do everything. And there is a related idea, hierarchical planning, for tasks so complex that even deciding the plan itself needs to be broken down in stages, not generated all at once.

Today covers four connected ideas: why and when a framework is worth adopting instead of hand-rolling everything, the core concepts and practical use of LangChain specifically, how and why to split work across multiple coordinated agents, and hierarchical planning for genuinely complex, multi-layered tasks. By the end, you will be equipped to build agents faster using established tools, without losing the understanding of what those tools are actually doing underneath."

---

## Block 2 — Agent Frameworks: Why Not Just Keep Building It By Hand?

### 2A — What a Framework Actually Provides

**[Script:]**

"An agent framework is a library that provides pre-built implementations of the common pieces every agentic system needs — the reasoning loop, message and conversation management, tool definition and execution, and often memory and retrieval integrations as well. The value is not that these things become newly possible; you already built all of them yourself. The value is that they become faster to assemble, more consistent, and battle-tested against edge cases you have not personally encountered yet."

> 🎯 **Instructor Note:** Write this honest comparison on the board — this is not about the hand-built version being wrong, it is about tradeoffs.

```
Hand-built agent loop (last two sessions):
+ Full visibility and control over every detail
+ No dependency on an external library's design choices
- You maintain the tool registry, validation, message handling, 
  loop logic, and every edge case yourself

Framework-based agent (LangChain, this session):
+ Pre-built loop, tool handling, and a large integration ecosystem
+ Faster to assemble; battle-tested against many edge cases
- Less visibility into exact internals unless you dig in
- A dependency with its own version changes and learning curve
```

**[Script:]**

"Neither approach is universally correct. For a simple, single-purpose agent, your hand-built loop from last session is often genuinely simpler and easier to fully understand and control. For a more complex system, especially one needing many integrations — document loading, vector search, multiple tool types — a framework saves substantial, real development time."

> 🎯 **Instructor Note:** Ask: "Given what you now understand about the internals, what is the actual risk of using a framework without ever having built the loop by hand first?" Answer: without that foundation, a framework can feel like an opaque black box — when something behaves unexpectedly, you have no mental model for what is likely happening underneath, and debugging becomes guesswork instead of informed investigation. This is precisely why the hand-built sessions came first.

---

### 2B — When a Framework Is Worth Adopting

**[Script:]**

"A practical way to decide: if your agent needs a handful of custom tools and a straightforward reasoning loop, your own hand-built implementation is often perfectly sufficient, and arguably clearer to maintain. If your agent needs to integrate with many different external systems, chain together multiple processing steps, or coordinate multiple agents together — the multi-agent systems from Block 4 — a framework's pre-built integrations and coordination patterns become a genuine time-saver rather than an unnecessary abstraction."

**Recap of Block 2 before moving on:**

- An agent framework provides pre-built implementations of the reasoning loop, tool handling, and integrations that were built by hand in previous sessions
- The tradeoff is speed and a large integration ecosystem, against reduced visibility into exact internals and a new dependency to learn
- Simple, single-purpose agents often do not need a framework; complex systems with many integrations or multiple coordinated agents benefit from one substantially
- Understanding the hand-built loop first is what prevents a framework from feeling like an unpredictable black box when something goes wrong

---

## Block 3 — LangChain: Core Concepts and Building an Agent

### 3A — LangChain's Core Building Blocks

**[Script:]**

"LangChain is one of the most widely used agent frameworks, and its core concepts map directly onto pieces you already understand. Rather than learning an entirely new mental model, you are mostly learning new names and new syntax for concepts already familiar from the last two sessions."

> 🎯 **Instructor Note:** Write this mapping on the board — this is the single most important framing device for the whole block.

```
Concept you already know          LangChain's name for it
-----------------------------     ------------------------
A tool function + its schema  →   a Tool
The reasoning loop            →   an AgentExecutor
Message history                →  a memory / message store
The chat model itself          →  a chat model wrapper (LLM/ChatModel)
```

---

### 3B — Defining a Tool in LangChain

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Based on the tool definitions you wrote by hand — name, description, parameters — what do you expect LangChain's version of a tool definition to require?" Guide toward: the same essential pieces, just expressed through LangChain's specific decorator or class syntax instead of a raw dictionary.

**Demo 1 — Defining a tool in LangChain (whiteboard-friendly)**

```python
from langchain.tools import tool

@tool
def get_weather(city: str) -> str:
    """Get the current weather for a specific city."""
    # In reality this would call a real weather API
    return f"The weather in {city} is 18°C and cloudy."

@tool
def calculate_tip(bill_amount: float, percentage: float) -> str:
    """Calculate the tip and total for a given bill amount and tip percentage."""
    tip = bill_amount * (percentage / 100)
    return f"Tip: ${tip:.2f}, Total: ${bill_amount + tip:.2f}"
```

**[Script:]**

"The `@tool` decorator turns an ordinary Python function into something LangChain can offer to the model, exactly like the raw dictionary tool definitions from last session — the function's docstring becomes the tool's description, and its type-hinted parameters become the parameters schema, generated automatically instead of written by hand.

This should feel immediately familiar: it is the same name, description, and parameters structure from Block 2 of the tool-use session, just with LangChain inferring the schema from your function signature and docstring rather than you writing the JSON Schema dictionary yourself."

> 🎯 **Instructor Note:** Ask: "What are you actually trading away by letting LangChain infer the schema from the docstring, instead of writing the parameters schema explicitly by hand?" Answer: convenience in exchange for a small amount of control — the docstring now does double duty as both human documentation and the actual input the model uses to decide when to call the tool, so its quality still matters exactly as much as the explicit description field did in the hand-built version.

---

### 3C — Assembling an Agent with AgentExecutor

**Demo 2 — Building and running a LangChain agent (whiteboard-friendly)**

```python
from langchain.agents import create_tool_calling_agent, AgentExecutor
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate

llm = ChatOpenAI(model="gpt-4o-mini")
tools = [get_weather, calculate_tip]

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("human", "{input}"),
    ("placeholder", "{agent_scratchpad}")
])

agent = create_tool_calling_agent(llm, tools, prompt)
agent_executor = AgentExecutor(agent=agent, tools=tools, max_iterations=5)

result = agent_executor.invoke({"input": "What's the weather in Tokyo?"})
print(result["output"])
```

**[Script:]**

"`create_tool_calling_agent` wires the model, tools, and prompt together into an agent definition. `AgentExecutor` is the piece that actually runs the loop — this is LangChain's version of the `run_agent` function you wrote by hand, including the exact same `max_iterations` safety limit for the exact same reason: preventing runaway cost if the agent cannot resolve the task.

`agent_executor.invoke(...)` runs the full reasoning cycle — perceive the input, reason about whether a tool is needed, act by calling one if so, observe the result, and repeat, exactly as your hand-built loop did — until a final answer is produced or `max_iterations` is reached. `result['output']` is the final text answer, equivalent to what your `run_agent` function returned."

> 🎯 **Instructor Note:** This is the moment to explicitly connect back to the hand-built loop. Say directly: "Everything AgentExecutor does internally is the same for-loop, the same tool-call detection, the same message appending you wrote by hand in the tool-use session. You are not learning a new agent architecture right now — you are learning a pre-built implementation of the one you already understand."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If you set `verbose=True` on the AgentExecutor, based on what you know is happening internally, what would you expect to see printed as the agent runs?" Answer: the equivalent of the trace logging built by hand last session — each reasoning step, which tool was called, with what arguments, and what it returned, printed as the loop executes, rather than only seeing the final answer at the end.

**Recap of Block 3 before moving on:**

- LangChain's core concepts map directly onto pieces already built by hand: tools, the reasoning loop, and message history
- The `@tool` decorator turns a Python function into a usable tool, inferring name, description, and parameters from the function signature and docstring
- `AgentExecutor` is LangChain's implementation of the same reasoning loop from the hand-built `run_agent` function, including the same `max_iterations` safety pattern
- The framework provides pre-built plumbing for concepts already understood, not a new underlying architecture

---

## Block 4 — Multi-Agent Systems

### 4A — Why One Agent Is Not Always Enough

**[Script:]**

"A single agent, however well built, tends to struggle as a task grows more complex and spans genuinely different kinds of work — researching a topic, writing polished content about it, and critically reviewing that content for accuracy are three different skills, each benefiting from a different focus, different tools, and sometimes a different system prompt entirely. Asking one generalist agent to do all three in a single reasoning loop often produces mediocre results at each stage, since the same context and instructions are trying to serve three different jobs at once.

A multi-agent system splits this work across multiple agents, each specialized for one part of the task, coordinated together to accomplish something none of them could do as well alone."

---

### 4B — Coordination Patterns

**[Script:]**

"There are a few common ways multiple agents can be coordinated together."

> 🎯 **Instructor Note:** Write these coordination patterns on the board.

```
Sequential (pipeline):
  Agent A's output → becomes Agent B's input → becomes Agent C's input
  Example: Researcher → Writer → Reviewer

Supervisor (orchestrator):
  A central "supervisor" agent decides which specialized agent 
  to invoke next, based on the current state of the task
  Example: Supervisor routes a question to either a Sales agent 
  or a Support agent, depending on what the user actually needs

Parallel:
  Multiple agents work on different sub-parts of a task 
  simultaneously, and their results are combined afterward
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "For a task like 'research a topic and write a well-supported summary article about it,' which coordination pattern — sequential, supervisor, or parallel — fits best, and why?" Answer: sequential — research genuinely needs to happen before writing can meaningfully begin, since the writer agent depends on the researcher's output as its actual input. The steps have a natural, required order.

---

### 4C — Building a Sequential Multi-Agent Pipeline

**Demo 3 — A sequential researcher-writer pipeline (whiteboard-friendly)**

```python
researcher_agent = create_agent(
    llm, tools=[web_search_tool],
    system_prompt="You are a research specialist. Gather key facts on the given topic."
)

writer_agent = create_agent(
    llm, tools=[],
    system_prompt="You are a writing specialist. Write a clear, engaging summary using only the provided research."
)

def run_pipeline(topic: str) -> str:
    research_result = researcher_agent.invoke({"input": f"Research: {topic}"})
    research_findings = research_result["output"]

    writing_result = writer_agent.invoke({
        "input": f"Write a summary using this research:\n{research_findings}"
    })
    return writing_result["output"]
```

**[Script:]**

"`researcher_agent` has access to a search tool and a system prompt focused entirely on gathering facts — nothing about writing style or tone. `writer_agent` has no tools at all — its job is purely to take the research findings, already gathered, and turn them into well-written prose, with a system prompt focused entirely on writing quality, not fact-finding.

`run_pipeline` is the coordination logic: run the researcher first, take its output, and feed it as input into the writer. Each agent is simpler and more focused than a single generalist agent trying to do both jobs at once — this is the same specialization principle behind splitting any large system into smaller, focused components, applied here to agents instead of functions or services."

> 🎯 **Instructor Note:** Ask: "Why give the writer agent no tools at all, rather than the same search tool the researcher has?" Answer: this is a deliberate constraint — restricting the writer agent to only the already-gathered research findings, with no ability to independently search for new information, enforces grounding: the writer's output is guaranteed to be based on what the researcher actually found, not on the writer agent introducing its own unverified claims from a separate search.

**Recap of Block 4 before moving on:**

- Multi-agent systems split complex tasks across multiple specialized agents, each focused on one part of the work, rather than one generalist agent trying to do everything
- Common coordination patterns: sequential (a pipeline, one agent's output feeds the next), supervisor (a central agent routes work to specialists), and parallel (agents work simultaneously on different sub-parts)
- A sequential pipeline is appropriate when steps have a natural, required order, such as research before writing
- Deliberately restricting a specialized agent's tools — such as giving a writer agent no search access — can enforce grounding in the output of an earlier agent, rather than allowing it to introduce ungrounded claims of its own

---

## Block 5 — Hierarchical Planning

### 5A — When Even Planning Needs to Be Broken Down

**[Script:]**

"Recall planning from the agentic systems session: an agent forms a sequence of intended steps before executing them. For a moderately complex task, this works well as a single planning step. But for a genuinely large, multi-layered task — 'build a complete market analysis report covering five different competitors, each requiring its own research, comparison, and summary' — generating one single flat plan with every low-level step spelled out upfront becomes unwieldy, and the agent often loses coherence partway through a very long sequence of steps.

Hierarchical planning addresses this by breaking the planning itself into levels: a high-level plan of major phases, where each phase is only planned in detail once the agent actually reaches it, rather than the entire multi-layered plan being generated all at once at the very start."

> 🎯 **Instructor Note:** Draw this contrast on the board.

```
Flat planning (single level):
  Plan ALL steps upfront, in full detail, before starting:
  1. Search Competitor A, 2. Analyze A, 3. Summarize A,
  4. Search Competitor B, 5. Analyze B, 6. Summarize B, ... 
  (15 detailed steps generated before any work begins)

Hierarchical planning (multiple levels):
  High-level plan: 
    Phase 1: Research all 5 competitors
    Phase 2: Compare findings
    Phase 3: Write final report
  
  Each phase is only planned in detailed steps once the agent 
  actually reaches it — not all 15 detailed steps upfront
```

---

### 5B — Why Hierarchical Planning Helps

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If an agent generates all 15 detailed steps upfront in a flat plan, and step 4 reveals unexpected information that should change how steps 10 through 15 are approached, what happens to the rest of that original flat plan?" Answer: the rest of the flat plan is now potentially outdated or wrong, but was already fully generated in detail — the agent either has to discard and regenerate much of it, or, worse, may follow the outdated plan anyway. This is exactly the plan-revision problem from the agentic systems session, made significantly worse at a larger scale.

**[Script:]**

"Hierarchical planning reduces this risk. Because only the current phase is planned in detail at any given time, new information learned during Phase 1 can directly inform how Phase 2 is planned, once the agent actually reaches it — rather than Phase 2's detailed steps having already been locked in, based on assumptions made before Phase 1 even started. This is the same plan-revision principle from the agentic systems session, applied deliberately at every phase boundary instead of only reactively, after something already went wrong."

> 🎯 **Instructor Note:** Reinforce this connection directly: "Remember the flight-booking plan revision — the agent discovered the flight search failed, and revised its plan in response. Hierarchical planning builds that same responsiveness into the structure of the plan itself, checking in and re-planning at each phase boundary by design, not only when something unexpectedly breaks."

---

### 5C — A Hierarchical Planning Pattern

**Demo 4 — Two-level hierarchical planning (whiteboard-friendly)**

```python
def run_hierarchical_task(overall_goal: str) -> str:
    # Level 1 — high-level plan (phases only, not detailed steps)
    high_level_plan = planner_agent.invoke({
        "input": f"Break this goal into 3-5 major phases: {overall_goal}"
    })
    phases = parse_phases(high_level_plan["output"])

    results = []
    for phase in phases:
        # Level 2 — detailed plan for THIS phase only, generated 
        # only once the agent actually reaches it
        phase_result = execution_agent.invoke({
            "input": f"Complete this phase: {phase}. "
                     f"Context from previous phases: {results}"
        })
        results.append(phase_result["output"])

    return summarize_agent.invoke({"input": f"Combine these results: {results}"})["output"]
```

**[Script:]**

"`planner_agent` produces only the high-level phases — three to five major steps, not a fully detailed execution plan. Each phase is then executed by `execution_agent`, one at a time, and critically, each phase's invocation includes `results` from every phase completed so far — meaning the detailed work of executing a later phase can genuinely be informed by what was actually learned in earlier phases, not just by an assumption made before any of it happened.

This is a direct extension of the multi-agent coordination from Block 4 — a sequential pipeline, but where the sequence itself, the phases, was decided by a planning step first, rather than being manually hardcoded by the developer in advance."

> 🎯 **Instructor Note:** Ask a closing synthesis question: "How does this hierarchical pattern relate to the multi-agent pipeline from Block 4, and how does it relate to the plan-revision idea from the agentic systems session? Try to state the connection in your own words." Guide toward: it combines both — the phases are executed sequentially like the researcher-writer pipeline, and each phase incorporates real results from prior phases before being planned in detail, which is the plan-revision principle applied systematically at every phase boundary, not just as a reactive fix.

**Recap of Block 5 before moving on:**

- Hierarchical planning breaks a large task into high-level phases first, only generating detailed steps for each phase once the agent actually reaches it
- This reduces the risk from flat planning, where a fully detailed upfront plan can become outdated by new information learned partway through execution
- Each phase's detailed execution can incorporate real results from earlier completed phases, extending the plan-revision principle systematically rather than only reactively
- Hierarchical planning combines naturally with multi-agent coordination — a planning agent determines the phases, and specialized agents execute each one in sequence

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What does a framework actually provide that the hand-built loop did not already achieve conceptually? What does the `@tool` decorator infer automatically? What are the three multi-agent coordination patterns, and when does each fit? Why does hierarchical planning only plan one phase in detail at a time?"

**Agent Frameworks — Why Not Just Keep Building It By Hand?**

- Frameworks provide pre-built implementations of the reasoning loop, tool handling, and integrations already understood from hand-built sessions
- The tradeoff is speed and ecosystem access, against reduced visibility into internals and a new dependency
- Simple agents often do not need a framework; complex, multi-integration, or multi-agent systems benefit substantially
- Understanding the hand-built loop first is what keeps a framework from feeling like an unpredictable black box

**LangChain — Core Concepts and Building an Agent**

- LangChain's core concepts map directly onto concepts already built by hand: tools, the reasoning loop, and message history
- `@tool` infers a tool's name, description, and parameters schema from the function signature and docstring
- `AgentExecutor` is a pre-built implementation of the same reasoning loop as the hand-built `run_agent` function, including the same `max_iterations` safeguard

**Multi-Agent Systems**

- Multi-agent systems split complex tasks across specialized agents rather than relying on one generalist agent
- Coordination patterns include sequential pipelines, supervisor-routed systems, and parallel execution, chosen based on the task's actual structure
- Deliberately restricting a specialized agent's tools can enforce grounding in an earlier agent's verified output

**Hierarchical Planning**

- Hierarchical planning generates only high-level phases upfront, planning each phase's detailed steps only once the agent actually reaches it
- This avoids the risk of a fully detailed upfront plan becoming outdated by information learned during execution
- Each phase can incorporate real results from prior phases, extending plan-revision systematically to every phase boundary
- Hierarchical planning combines naturally with multi-agent coordination, using a planning agent alongside specialized execution agents

**Why All of This Matters Together**

- Every concept in this session is a more powerful, pre-built, or more systematically organized version of something already built by hand across the previous two sessions — frameworks industrialize the reasoning loop, multi-agent systems apply specialization to break one overloaded agent into focused collaborators, and hierarchical planning applies plan-revision systematically instead of only reactively; understanding the fundamentals first, and only then adopting the tools and patterns that industrialize them, is what makes it possible to build genuinely complex agentic systems while still being able to reason clearly about what they are actually doing underneath

---

*End of script.*
