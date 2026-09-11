# Lecture Script: Building an Application with Claude Code
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | The Disciplined Workflow — Plan, PRD, and Clarifying Questions | 25 min |
| 3 | Small Diffs and PR-Based Review with Edge Case Detection | 25 min |
| 4 | Pair Programming with AI | 25 min |
| 5 | UI/UX with AI | 20 min |
| 6 | Lecture Summary and Recap | 7 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has used Copilot's inline suggestions and has practiced the debugging-and-review discipline of verifying AI output rather than trusting it blindly. Claude Code is a different category of tool — an agentic coding assistant that can plan, make multi-file changes, and work more autonomously than an inline autocomplete tool. The hook should distinguish this category clearly before diving into workflow. Wait after the opening question.

**[Script:]**

"Copilot suggests the next few lines as you type. That is genuinely useful, but it operates at a small scale — one line, one function, one file at a time, with you driving every step. Claude Code operates at a different scale entirely: you can describe a feature, and it can plan the implementation, make changes across multiple files, run and check its own work, and prepare a change ready for your review — closer to delegating a task to a capable teammate than to accepting an autocomplete suggestion.

This is genuinely powerful, and it is exactly why it demands more discipline, not less. The verification habit from the debugging and review session — never trust a suggestion just because it looks plausible — still applies completely here, but the stakes are higher, because the scope of what could be silently wrong is larger. A single wrong line from Copilot is easy to spot. A multi-file change that looks reasonable at a glance, but has a subtle architectural problem or misses an edge case three files away from where you were actually looking, is much easier to miss.

The discipline that makes this genuinely safe and effective is not complicated, but it does need to be deliberate: have the agent propose a plan before it writes any code, ask it to draft something like a product requirements document for anything non-trivial, let it ask you clarifying questions rather than guessing at ambiguous requirements, keep changes small enough to actually review carefully, and put every change through the same pull request review process you would apply to a human teammate's work — including deliberately checking for edge cases the plan or the diff might have missed.

Today covers that disciplined workflow end to end, how to genuinely pair program with an AI agent rather than just delegating and waiting, and finally how AI fits specifically into UI and UX work — a domain with real, additional judgment calls beyond just 'does the code work correctly.'"

---

## Block 2 — The Disciplined Workflow: Plan, PRD, and Clarifying Questions

### 2A — Why Starting with Code Is the Wrong First Step

**[Script:]**

"The instinctive way to use an agentic coding tool is to describe what you want and let it start writing code immediately. For a small, well-defined task, this can work fine. For anything genuinely complex — a new feature with several interacting pieces, a change that touches existing behavior other parts of the application depend on — starting with code immediately skips the step where misunderstandings are cheapest to catch: before any code exists at all."

> 🎯 **Instructor Note:** Draw this cost curve on the board — it is the entire motivation for the workflow this block establishes.

```
Cost of catching a misunderstanding, by stage:

Before any code exists (plan/PRD stage)  →  cheapest — a conversation
During review of a small diff            →  moderate — a requested change
After a large change is already built    →  expensive — significant rework
After it ships to production             →  most expensive — a real incident
```

---

### 2B — Having the Agent Propose a Plan First

**[Script:]**

"Before any code is written, ask the agent to propose a plan: what files it expects to touch, what approach it intends to take, and what the major steps are. This costs very little time to produce and read, and it surfaces a wrong understanding of the task immediately, while it is still just a paragraph of text you can correct in seconds, rather than a change you have to unwind."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If you ask an agent to 'add user authentication' with no further detail, and it goes straight to writing code without proposing a plan first, what kind of misunderstanding might only become visible once the code is already written?" Answer: something like using session-based authentication when you actually wanted JWT-based authentication from the authentication fundamentals session, or building a new user table when your application already has one — a fundamental approach mismatch that a plan, read before any code exists, would have caught immediately and cheaply.

**Demo 1 — Requesting a plan before any code (whiteboard-friendly)**

```
Prompt to the agent:
"I want to add password reset functionality to the app. Before 
writing any code, propose a plan: what files you expect to touch, 
what approach you'll take, and the major steps. Don't write code yet."

Expected kind of response:
"Plan for password reset:
1. Add a `password_reset_tokens` table (user_id, token, expires_at)
2. New endpoint: POST /auth/request-reset (generates token, sends email)
3. New endpoint: POST /auth/confirm-reset (validates token, updates password)
4. Reuse existing bcrypt hashing from the auth module
5. Token expires after 1 hour

Files expected to touch: models.py, routes/auth.py, services/email.py (new)

Does this match what you had in mind? Any constraints I should know 
about — e.g., should old tokens be invalidated when a new one is requested?"
```

**[Script:]**

"Notice the plan ends with a genuine question, not just a restatement of the task. This is exactly the clarifying-question behavior worth deliberately encouraging — a specific, well-reasoned question about something the request did not actually specify, rather than the agent silently picking an assumption and building around it. Reading this plan costs you thirty seconds. Discovering the same mismatch after the tables, endpoints, and email integration are already built costs considerably more."

> 🎯 **Instructor Note:** Ask: "What would you have missed if the agent had skipped straight to writing code, without asking about token invalidation on repeated requests?" Answer: this is a genuine edge case with real security implications — if old reset tokens are not invalidated when a new one is requested, multiple valid tokens could exist simultaneously, which is exactly the kind of subtle correctness gap that is much easier to address in a plan than to notice buried inside a working implementation.

---

### 2C — Drafting a PRD for Non-Trivial Features

**[Script:]**

"For anything larger than a small, self-contained change, it is worth going a step further than a short plan: have the agent draft something like a lightweight product requirements document — a PRD — covering the actual requirements, explicit constraints, and what is deliberately out of scope. This is not bureaucracy for its own sake; it is making the agent's understanding of the task fully explicit and reviewable before a large amount of work is invested based on that understanding."

> 🎯 **Instructor Note:** Write the essential PRD components on the board.

```
A lightweight PRD, for agent-assisted development, typically covers:
- The actual problem being solved, in plain terms
- Specific requirements — what the feature must do
- Explicit constraints — what it must NOT do, or must respect
- What is deliberately out of scope for this specific change
- Open questions the requester needs to answer before work begins
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If a PRD for the password reset feature explicitly lists 'rate limiting reset requests' as out of scope for this change, rather than the agent silently deciding not to build it, what problem does that explicit statement prevent?" Answer: it prevents a false assumption on either side — the requester does not mistakenly believe rate limiting was included, and the agent is not silently making a scope decision on your behalf without your awareness. Explicit scope boundaries, even for things intentionally excluded, prevent a specific and common kind of miscommunication.

**[Script:]**

"The discipline here is the same reasoning-evaluation mindset from the agentic debugging and performance session, applied before implementation rather than after: does this understanding of the task actually hold up, is it complete, and does it match what you genuinely intended — checked while it is still cheap to correct."

**Recap of Block 2 before moving on:**

- Misunderstandings are cheapest to catch before any code exists, and increasingly expensive to catch at every later stage
- Requesting a plan before any code is written surfaces approach mismatches while they are still a quick correction, not a rework
- A well-functioning plan or PRD includes genuine clarifying questions about ambiguous requirements, rather than silently picking an assumption
- A lightweight PRD for non-trivial features should state explicit requirements, constraints, and what is deliberately out of scope, preventing miscommunication about scope specifically

---

## Block 3 — Small Diffs and PR-Based Review with Edge Case Detection

### 3A — Why Diff Size Matters More With an Agent Than With a Human

**[Script:]**

"You already know from the code review session that review quality degrades as the size of a change grows — a reviewer's attention is a limited resource, and a five-thousand-line diff gets a much shallower review than a two-hundred-line one, purely because of how much a person can actually hold in mind at once. This is true for human-written code, and it is at least as true, arguably more so, for agent-generated code, because an agent can produce a large, plausible-looking diff very quickly — faster than the corresponding careful review can actually happen."

> 🎯 **Instructor Note:** Ask: "If an agent can generate a thousand-line, multi-file change in under a minute, but a careful human review of that same change realistically takes thirty minutes to do properly, what pressure does that speed mismatch create?" Answer: a strong, easy-to-fall-into temptation to skip or rush the review, specifically because the code was produced so quickly it feels like it should also be quick to approve — even though the actual review effort required has not gotten any smaller. Naming this pressure directly is the first step to resisting it.

---

### 3B — Requesting Small, Reviewable Diffs

**[Script:]**

"The practical discipline: explicitly ask the agent to break a larger task into a sequence of small, independently reviewable changes, rather than one large change covering the entire feature. This is the same principle from the iterative refinement discipline in the agentic debugging session — one targeted change at a time — now applied to how implementation work itself gets structured and delivered, not just how it gets debugged afterward."

**Demo 2 — Requesting incremental, reviewable diffs (whiteboard-friendly)**

```
Instead of:
"Implement the full password reset feature."

Ask for:
"Implement the password reset feature as a sequence of small, 
independently reviewable changes:
1. First, just the database table and model — I'll review that alone
2. Then, the request-reset endpoint — I'll review that separately
3. Then, the confirm-reset endpoint
Stop after each step and let me review before continuing."
```

**[Script:]**

"Each step now produces a diff small enough to actually review carefully — a new table and model is a genuinely reviewable unit on its own, separate from an endpoint's logic, separate again from the second endpoint's logic. If something is wrong with the table design, that is caught and fixed before two more layers of code have already been built on top of it."

> 🎯 **Instructor Note:** Ask: "What is the actual cost of stopping and reviewing after each small step, compared to reviewing everything at once at the end?" Answer: slightly more total review interactions, but each one is genuinely shallow and fast, and problems are caught before subsequent steps are built on a flawed foundation — the same principle as catching a plan mismatch early from Block 2, now applied at the implementation stage instead of the planning stage.

---

### 3C — Reviewing Through Pull Requests, with Deliberate Edge Case Detection

**[Script:]**

"Every change, however it was produced, should go through the same pull request review process you would apply to a teammate's work — this is not a special, reduced process for AI-generated code, it is the same process, applied consistently. And exactly as covered in the code review and risk-identification sessions, review should deliberately check for risk categories, not just correctness on the happy path."

> 🎯 **Instructor Note:** This connects directly back to the risk-identification framework from the AI-assisted debugging and review session. Write it on the board again here, as a direct callback.

```
Reviewing an agent-generated PR — check deliberately for:
- Edge cases: empty inputs, expired tokens, already-used tokens
- Security: does the reset token generation use a secure random source?
- Consistency: does this match the existing codebase's patterns 
  and conventions, or does it introduce a new, inconsistent style?
- Scope: does the diff match exactly what the plan or PRD described, 
  or has something crept in — or been left out — unexpectedly?
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show this diff snippet on the board. Ask: "Reviewing this specifically for edge cases — what is missing?" Give the room a genuine moment before revealing the answer.

**Demo 3 — Reviewing an agent-generated diff for a missed edge case (whiteboard-friendly)**

```python
@app.post("/auth/confirm-reset")
def confirm_reset(token: str, new_password: str):
    reset_record = db.query(PasswordResetToken).filter_by(token=token).first()
    if not reset_record:
        raise HTTPException(status_code=400, detail="Invalid token")

    user = db.get(User, reset_record.user_id)
    user.password_hash = hash_password(new_password)
    db.commit()
    return {"status": "password updated"}
```

**[Script:]**

"The code runs correctly for the happy path — a valid token results in a successful password update. But reviewing specifically for edge cases: nothing here checks whether the token has expired, even though the plan from Block 2 explicitly specified a one-hour expiration. Nothing invalidates or deletes the token after it is used, meaning the exact same reset link could be used again later to reset the password a second time — a real security gap. Nothing enforces even a minimal password strength requirement on `new_password`.

This diff is small, which made it genuinely reviewable — exactly the value of Block 3B's discipline — and the deliberate edge-case check specifically, rather than a general 'does this look okay' read, is what actually caught these three real gaps."

> 🎯 **Instructor Note:** Ask: "Would a quick, general read of this code — 'does it look reasonable' — have caught these three issues, or did catching them require the deliberate, specific edge-case checklist?" Answer: almost certainly the specific checklist — the code looks entirely reasonable and well-structured at a glance, which is exactly why a deliberate, structured check for specific risk categories catches things that a general impression of code quality does not.

**Recap of Block 3 before moving on:**

- Review quality degrades as diff size grows, and agents can produce large diffs fast enough to create real pressure to under-review them
- Explicitly requesting small, independently reviewable diffs, stopping for review after each step, catches problems before later work is built on a flawed foundation
- Every agent-generated change should go through the same PR review process as human-written code, not a reduced process
- Deliberate, structured edge case review — checking specific risk categories rather than a general impression — catches real gaps that look entirely reasonable at a glance

---

## Block 4 — Pair Programming with AI

### 4A — Pairing Versus Delegating

**[Script:]**

"Everything in Blocks 2 and 3 describes a delegation pattern: you specify a task, the agent works somewhat independently, you review the result. Pair programming is a different mode entirely — working alongside the agent in a tighter, more continuous loop, closer to how you would pair with a human colleague: thinking out loud together, making decisions collaboratively as you go, rather than handing off a fully-specified task and waiting for a finished result."

> 🎯 **Instructor Note:** Draw this contrast on the board.

```
Delegation mode:
  You specify task → agent works mostly independently → 
  you review the finished result
  Good for: well-defined, scoped tasks

Pairing mode:
  You and the agent work through the problem together, in a 
  tighter loop — discussing approach, making decisions jointly, 
  course-correcting continuously as you go
  Good for: exploratory work, genuinely uncertain problems, 
  learning a new part of a codebase together
```

---

### 4B — What Effective AI Pairing Actually Looks Like

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If you are debugging a genuinely confusing, intermittent production issue with an unclear root cause, is delegation mode or pairing mode more likely to actually help?" Answer: pairing mode — the task is not well-specified enough to delegate cleanly, since you do not yet know exactly what the fix should be; working through hypotheses together, checking each one against real evidence as you go, fits a genuinely uncertain problem far better than handing off a task whose actual definition is still unclear.

**[Script:]**

"Effective pairing means treating the agent's suggestions as one voice in an ongoing conversation, not a final answer — proposing a hypothesis, checking it together against real evidence, and being willing to redirect the agent explicitly when its reasoning heads somewhere you do not think is right, exactly the same real-time correction you would offer a human pairing partner."

**Demo 4 — A pairing exchange redirecting the agent's approach (whiteboard-friendly)**

```
You: "Users are intermittently getting logged out. I don't know why yet."

Agent: "A few possibilities: JWT expiration set too short, a race 
condition in token refresh, or the token being cleared on certain 
navigation events. Want me to check the JWT expiration configuration first?"

You: "Actually, I just remembered — this started right after we 
deployed the caching layer last week. Let's look there first instead."

Agent: "Good lead. Let me check whether the cache is possibly 
storing and serving a stale or invalid token."
```

**[Script:]**

"Notice the redirect in the middle — the agent proposed a reasonable starting hypothesis, but you had specific, relevant context it did not have: a recent deployment that correlates suspiciously well with when the problem started. Real pairing means offering that context immediately and explicitly, redirecting the investigation, rather than letting the agent continue down a plausible-but-less-likely path just because it was already the one being explored."

> 🎯 **Instructor Note:** Ask: "What would have happened if you had not redirected the agent here, and just let it investigate JWT expiration configuration first, even after remembering the caching deployment?" Answer: likely wasted time investigating a less probable cause first, when you actually had strong, specific context pointing somewhere else — this is exactly the value of active pairing over passive delegation: your own knowledge and intuition should actively steer the collaboration in real time, not sit unused because the agent already proposed a plausible-sounding first step.

---

### 4C — When Pairing Beats Delegation

**[Script:]**

"Pairing tends to fit exploratory debugging, learning an unfamiliar part of a codebase together, and design discussions where the right approach is not yet clear even to you. Delegation tends to fit well-specified, scoped tasks where the plan-and-PRD discipline from Block 2 can actually produce a clear enough specification upfront. Neither mode is universally better — the right choice depends on how well-defined the task actually is before you start."

> 🎯 **Instructor Note:** Ask a closing synthesis question: "Could you use pairing mode initially to figure out what the actual fix should be, and then switch to delegation mode to have the agent implement that now-clear fix as a small, reviewable diff?" Answer: yes, and this is a genuinely common and effective pattern — pair through the uncertain, exploratory part of a problem until the actual solution becomes clear, then switch into the more structured plan-diff-review discipline from Blocks 2 and 3 to actually implement that now well-understood solution.

**Recap of Block 4 before moving on:**

- Delegation hands off a well-specified task for mostly independent work; pairing works through a problem together in a tighter, continuous loop
- Effective pairing means treating agent suggestions as one voice in a conversation, actively redirecting with your own relevant context rather than passively accepting the first plausible direction
- Pairing fits exploratory debugging and genuinely uncertain problems; delegation fits well-specified, already-scoped tasks
- The two modes combine naturally: pair through uncertainty to reach clarity, then delegate the now-clear implementation as a small, reviewable diff

---

## Block 5 — UI/UX with AI

### 5A — Why UI/UX Work Has a Different Evaluation Problem

**[Script:]**

"Everything so far has largely concerned backend logic, where correctness is often relatively well-defined — a password reset either works correctly or it does not, an edge case either is handled or it is not. UI and UX work introduces a category of judgment that is not purely a matter of correctness: does this actually look good, does this genuinely feel intuitive to use, does this match your specific brand and product's tone. An AI agent can generate visually reasonable, functional UI code quickly, but 'functional' and 'actually good design for your specific product' are not the same bar, and only one of them is something the agent can fully judge on its own."

> 🎯 **Instructor Note:** Ask: "Based on everything from the earlier frontend design and UI-related work in this course, why might 'the code renders without errors and looks like a plausible UI' not be a sufficient bar for a real product's interface?" Answer: functional and error-free is a low bar — genuinely good UI/UX also requires consistency with the specific product's existing design language, appropriate information hierarchy for the actual users, and design choices grounded in real usability principles, not just generically plausible layout and styling that could belong to any product.

---

### 5B — Giving an Agent the Right Design Context

**[Script:]**

"Exactly as with Copilot's context engineering, the quality of AI-generated UI work depends heavily on what context it actually has available — existing design patterns in your codebase, your color palette and typography conventions, and explicit direction about the intended user experience, not just a functional description of what the interface needs to display."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If you ask an agent to 'build a settings page' with no other context at all, versus asking it to build the same page while pointing it at your existing component library and design conventions, which do you expect to actually fit your product visually?" Answer: the version with real design context is far more likely to actually match — without it, the agent has no way to know your product's specific conventions and will generate something generically plausible rather than something that genuinely looks like it belongs in your actual application.

**Demo 5 — Providing design context for UI generation (whiteboard-friendly)**

```
WEAK PROMPT:
"Build a settings page with account, notifications, and privacy sections."

STRONGER PROMPT:
"Build a settings page with account, notifications, and privacy 
sections. Follow the existing patterns in components/SettingsCard.jsx 
and components/Toggle.jsx — reuse these components rather than 
creating new ones. Match the spacing and typography conventions 
already used in components/ProfilePage.jsx. Group related settings 
under clear section headings, consistent with how the Notifications 
page currently organizes its content."
```

**[Script:]**

"The stronger prompt points directly at existing, real files — this is the UI equivalent of the context engineering discipline from the Copilot session, applied here to design consistency specifically rather than code correctness. The agent is not inventing a UI pattern from general training data; it is extending patterns that already exist in your specific application, which is exactly what makes the result feel like it genuinely belongs, rather than looking like a plausible but disconnected addition."

---

### 5C — Why Human Judgment Remains Essential for UX

**[Script:]**

"Even with excellent context, some genuinely important UX decisions are not fully answerable by an agent at all — how should this flow feel for a first-time user versus a returning power user, does this particular interaction actually reduce friction for your real users or just look clean in isolation, does this match your product's actual voice and brand. An agent can implement a well-specified design decision extremely well, and can even propose reasonable options, but the final judgment call on genuinely subjective UX questions specific to your product and your users belongs with a human who understands that product and those users directly."

> 🎯 **Instructor Note:** Close with a synthesis question connecting this back to the whole session's throughline. Ask: "How does this final point about UX judgment connect to the human-review discipline from Block 3, and the delegation-versus-pairing distinction from Block 4?" Answer: it is the same underlying principle applied to a new domain — an agent can implement, propose options, and move quickly, but a human still needs to review and make the genuinely subjective final calls, exactly as a human reviewer makes the final call on a pull request, and exactly as pairing mode is the right choice specifically when the right answer is not yet clear even to you. UI/UX with AI is not a different discipline from the rest of this session — it is the same discipline, applied to a domain where the correctness bar is inherently more subjective.

**Recap of Block 5 before moving on:**

- UI/UX evaluation includes genuinely subjective judgment — does this look and feel right for this specific product — beyond just functional correctness
- Providing real design context — existing components, conventions, and specific files to follow — produces results that actually fit a product, rather than generically plausible UI
- This is the same context engineering discipline from the Copilot session, applied specifically to design consistency
- Final judgment on genuinely subjective UX decisions specific to a product and its users belongs with a human, even when an agent implements the resulting decision well

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "Why is catching a misunderstanding at the plan stage cheaper than catching it after code is written? Why does requesting small, sequential diffs matter more with an agent than it might with a human? When does pairing mode fit better than delegation mode? Why does UI/UX work require a different kind of evaluation than backend logic does?"

**The Disciplined Workflow — Plan, PRD, and Clarifying Questions**

- Misunderstandings are cheapest to catch before any code exists, and increasingly expensive at every later stage
- Requesting a plan before code, and a PRD for non-trivial features, makes the agent's understanding explicit and reviewable early
- A well-functioning plan includes genuine clarifying questions and explicit scope boundaries, rather than silent assumptions

**Small Diffs and PR-Based Review with Edge Case Detection**

- Agents can generate large diffs fast enough to create real pressure to under-review them; requesting small, sequential, independently reviewable changes counters this
- Every agent-generated change should go through the same PR review process as human-written code
- Deliberate, structured edge case review — checking specific risk categories — catches real gaps that look entirely reasonable at a glance

**Pair Programming with AI**

- Delegation hands off a well-specified task; pairing works through a problem together in a tighter, continuous loop
- Effective pairing means actively redirecting the agent with your own relevant context, not passively accepting its first plausible direction
- Pairing fits exploratory and genuinely uncertain problems; delegation fits well-specified, already-scoped tasks; the two combine naturally

**UI/UX with AI**

- UI/UX evaluation includes genuinely subjective judgment beyond functional correctness
- Providing real design context — existing components and conventions — produces results that actually fit a specific product
- Final judgment on subjective UX decisions specific to a product and its users belongs with a human, even when an agent implements the decision well

**Why All of This Matters Together**

- Every practice in this session is the same underlying discipline from earlier in the course — verify before trusting, catch problems as early and cheaply as possible, keep changes small enough to genuinely review, provide deliberate context rather than vague requests — applied specifically to working with a more capable, more autonomous agentic coding tool; the increased capability of a tool like Claude Code is not a reason to relax that discipline, it is exactly why the discipline matters more, since a more capable tool can move faster and further before a human ever looks at the result

---

*End of script.*
