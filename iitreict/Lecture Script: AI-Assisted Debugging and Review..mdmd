# Lecture Script: AI-Assisted Debugging and Review
**Format:** Facilitator-facing live script | **Duration:** 135 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | Debugging with AI | 30 min |
| 3 | Code Review Fundamentals | 20 min |
| 4 | Identifying Risks | 25 min |
| 5 | Refactoring Suggestions | 20 min |
| 6 | AI-Assisted Code Review in Practice | 20 min |
| 7 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has built full backend features using AI APIs directly, and separately has written real application code across the course. This lecture is a shift in framing: using AI as a tool to improve their own development process, not just as a feature inside an application. The hook should make that shift explicit and grounded in a familiar frustration. Wait after the opening question.

**[Script:]**

"Every session so far has been about building AI into an application for your users. Today is different. Today the AI is helping you — the developer — do your job better.

Think about the last time you hit a bug that took an hour to find, and the actual fix was one line. Think about the last time you submitted a pull request and a reviewer caught something you completely missed — a race condition, an unhandled edge case, a security gap. Think about code you wrote quickly to get something working, that you knew needed cleanup, but never quite got back to.

These are not signs of being a bad developer. They are the normal cost of writing software, and they have existed as long as software has. What has changed recently is that AI tools can meaningfully help with all three — not by writing your code for you, but by acting as a second set of eyes that never gets tired, never assumes it already knows the codebase, and can process an entire file or diff in seconds.

Today covers four connected skills: using AI to debug faster and more systematically, applying real code review principles whether the reviewer is human or AI, identifying risks in code before they become production incidents, and getting useful refactoring suggestions. Then we bring it together — how AI-assisted code review actually fits into a real development workflow, alongside human reviewers, not instead of them.

By the end, you will know how to use AI as a debugging and review partner without becoming dependent on it, and without blindly trusting output that sounds confident but might be wrong."

---

## Block 2 — Debugging with AI

### 2A — What AI Debugging Actually Does

**[Script:]**

"When you paste a stack trace or a broken function into an AI tool and ask 'why is this failing', the model is doing something specific: pattern-matching your code and error message against the enormous amount of code, bugs, and fixes it saw during training. It is not executing your code. It is not running a debugger. It is predicting, based on patterns, what is most likely wrong.

This matters because it shapes what AI debugging is good at and what it is not. It is very good at common, well-documented bugs — off-by-one errors, null reference issues, common misuse of a popular library, syntax errors, obvious logic mistakes. It is much weaker at bugs that depend on your specific runtime state, your specific data, or an interaction between multiple parts of your system that only manifests in production."

> 🎯 **Instructor Note:** This distinction is the foundation for the entire block. Write it on the board.

```
AI debugging is strong at:
- Common, well-documented bug patterns
- Syntax and obvious logic errors
- Misuse of well-known libraries and frameworks
- Explaining unfamiliar error messages

AI debugging is weak at:
- Bugs dependent on your specific runtime data or state
- Bugs from interactions across multiple parts of a large system
- Anything requiring actual execution to observe behavior
```

**[Script:]**

"Knowing this changes how you use the tool. You do not hand over a vague description and wait for a magic answer. You give it the specific error, the relevant code, and enough context for it to pattern-match effectively."

---

### 2B — Structuring a Debugging Prompt

**[Script:]**

"This connects directly to prompt engineering. A debugging request is a prompt, and it benefits from the same structure — context, instruction, and constraints. The context is your error message and relevant code. The instruction is what you actually want: identify the bug, explain why it happens, suggest a fix. The constraint might be: do not rewrite the whole function, just point out the issue."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I give an AI tool just the error message 'TypeError: unsupported operand type' with no code at all, what kind of answer can I expect?" Answer: a generic explanation of what that error type generally means, not a specific diagnosis — the model has no code to pattern-match against your actual situation. This sets up why including the actual code matters.

**Demo 1 — A weak versus strong debugging prompt (whiteboard-friendly)**

```
WEAK:
"My code doesn't work, here's the error: TypeError: unsupported 
operand type(s) for +: 'int' and 'str'"

Response will be generic — an explanation of what this error type 
usually means, without being able to point to the actual cause.

STRONG:
"This function raises 'TypeError: unsupported operand type(s) for +: 
'int' and 'str'' when called with calculate_total([10, 20, '30']).

def calculate_total(prices):
    total = 0
    for price in prices:
        total += price
    return total

What is causing this, and what is the safest fix?"

Now the model has the actual code, the actual error, and the actual 
input that triggers it. It can identify precisely that the list 
contains a string where a number was expected, and suggest either 
validating input types or converting the value.
```

**[Script:]**

"The strong prompt includes three things: the exact error, the relevant code, and the specific input that triggers it. This is not different from the debugging discipline you would use without AI — reproduce the bug with a minimal example, then investigate. AI does not remove that discipline; it benefits from it just as much as you do."

> 🎯 **Instructor Note:** Ask: "Why does including the specific triggering input — the list with a string in it — matter so much here? What would happen without it?" Answer: without the specific input, the model can only guess generically about type errors. With it, the model can trace exactly which element causes the failure and give a precise, actionable diagnosis rather than a general lecture about type safety.

---

### 2C — Verifying AI's Debugging Suggestions

**[Script:]**

"Here is the critical discipline: an AI's explanation of a bug can be wrong, and it can sound completely confident while being wrong. It might correctly identify a real issue in your code that is not actually the issue causing your specific error. It might suggest a fix that resolves the symptom without addressing the underlying cause.

Treat every AI debugging suggestion as a hypothesis, not a verdict. Test it. If the suggested fix is 'validate that all elements are numeric before summing', apply that fix and confirm the error is actually gone, not just plausible-sounding."

> 🎯 **Instructor Note:** This is the single most important safety point in the block. Ask: "Why is it dangerous to apply an AI-suggested fix without testing it?" Answer: a fix can look reasonable, use correct syntax, and address a genuine code smell — while still not being the actual cause of the bug you are experiencing. Confidence in the explanation is not evidence the diagnosis is correct.

**[Script:]**

"A good habit: ask the AI to also explain why the bug happens, not just what the fix is. If the explanation does not match what you observe when you actually test it, that is a signal the diagnosis might be wrong, and you should keep investigating rather than accepting the fix because it compiled and the immediate error went away."

**Recap of Block 2 before moving on:**

- AI debugging works by pattern-matching your code and error against training data, not by executing your code
- It excels at common, well-documented bugs and struggles with bugs dependent on specific runtime state or complex system interactions
- A strong debugging prompt includes the exact error, the relevant code, and the specific input that triggers it — the same discipline as debugging without AI
- Treat every AI debugging suggestion as a hypothesis to test, not a verdict to trust — confident-sounding explanations can still be wrong

---

## Block 3 — Code Review Fundamentals

### 3A — What Good Code Review Looks For

**[Script:]**

"Before layering AI into code review, it is worth being explicit about what code review is actually for — this applies whether the reviewer is a human colleague or an AI tool. Good code review looks at several distinct dimensions, not just 'does this work'."

> 🎯 **Instructor Note:** Write this list on the board and keep it visible through the rest of this block and the next two.

```
Code review dimensions:
1. Correctness    — does the code do what it claims to do?
2. Readability     — can someone else understand this in six months?
3. Consistency     — does it follow the codebase's existing patterns?
4. Risk            — security, performance, edge cases (Block 4)
5. Maintainability — will this be easy to change later? (Block 5)
```

**[Script:]**

"Correctness is the most obvious dimension, but it is often the easiest to verify — tests either pass or they do not. Readability and consistency are subtler and matter enormously for a codebase that multiple people will touch over time. Risk and maintainability are where the highest-value review comments usually live, and where we will spend the most time in the next two blocks."

---

### 3B — Reviewing for Readability and Consistency

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show this function on the board before revealing any review comments. Ask: "Setting correctness aside — assume this works — what would you flag in a code review?" Let learners generate their own list before showing the demo's comments.

**Demo 2 — Reviewing for readability and consistency (whiteboard-friendly)**

```python
def proc(d, f):
    r = []
    for x in d:
        if f(x):
            r.append(x)
    return r
```

**[Script:]**

"This function works. It filters a list based on a predicate function. But a reviewer should flag: the names `proc`, `d`, `f`, `r`, `x` communicate nothing about what this does — `filter_items`, `data`, `predicate`, `results`, `item` would immediately clarify intent. And — worth naming explicitly — this function reimplements Python's built-in `filter()`, so depending on the codebase's conventions, a reviewer might suggest using the built-in directly instead of a custom loop.

Neither of these is a bug. Both are exactly the kind of comment that keeps a codebase maintainable as it grows, and exactly the kind of feedback AI tools are quite good at generating consistently, because naming and idiom patterns are heavily represented in training data."

> 🎯 **Instructor Note:** Ask: "Why does naming quality matter enough to be a standard part of code review, if the code works correctly either way?" Answer: code is read far more often than it is written. Poor names cost every future reader time trying to reconstruct intent that clear naming would have made immediate — this compounds across a team and across the life of a codebase.

**Recap of Block 3 before moving on:**

- Code review evaluates correctness, readability, consistency, risk, and maintainability — not just whether the code runs
- Correctness is usually the easiest dimension to verify; readability and consistency matter for long-term codebase health
- Naming, idiom usage, and consistency with existing patterns are areas where AI review tools are consistently strong, since these patterns are heavily represented in training data

---

## Block 4 — Identifying Risks

### 4A — Categories of Risk in Code

**[Script:]**

"Risk is the dimension of code review most likely to prevent a real production incident, and it is also the dimension most valuable to get AI assistance on, because a fresh set of eyes — human or AI — is often better at spotting risk than the person who wrote the code and has become blind to their own assumptions."

> 🎯 **Instructor Note:** Write these risk categories on the board.

```
Risk categories to review for:
- Security     — injection, exposed secrets, missing authorization checks
- Edge cases   — empty inputs, null values, boundary conditions
- Error handling — unhandled exceptions, silent failures
- Performance  — N+1 queries, unbounded loops, missing pagination
- Concurrency  — race conditions, unsafe shared state
```

**[Script:]**

"Several of these should feel familiar — N+1 queries from the SQLAlchemy loading discussion, SQL injection from parameterized queries, missing authorization checks from the RBAC session. Risk review is where the security and database sessions from earlier in this course directly pay off: you already know what these problems look like. The skill here is applying that knowledge systematically, every time, rather than only when you happen to remember to check."

---

### 4B — Using AI to Surface Risks

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show this code on the board. Ask: "Before any AI involvement — what risks do you personally see here?" Give the room a real moment to look. Then reveal what a risk-focused review would surface, and compare.

**Demo 3 — Identifying risk in a route (whiteboard-friendly)**

```python
@app.get("/users/{user_id}/orders")
def get_orders(user_id: int):
    orders = db.query(Order).filter(Order.user_id == user_id).all()
    return orders
```

**[Script:]**

"A risk-focused review — whether from a human or an AI tool prompted specifically to look for risk — should surface at least two things here. First: there is no authorization check. Any authenticated user, or possibly any unauthenticated request, can request any other user's orders just by changing the ID in the URL — a serious access control gap. Second: there is no pagination. If a user has ten thousand orders, this returns all of them in a single response, which is both a performance problem and a potential denial-of-service vector.

Neither of these would show up in a correctness-focused review, because the code works exactly as written for the happy path. They only surface when you deliberately review for risk."

> 🎯 **Instructor Note:** Emphasize the prompting lesson here explicitly: "If you ask an AI tool to 'review this code' generically, you often get comments about naming and style — the easy, surface-level stuff. If you ask it to 'review this code specifically for security and performance risks', you get a very different, much more valuable set of comments. The prompt you write determines what kind of review you get — this is the code review application of prompt engineering."

**Demo 4 — Prompting specifically for risk (whiteboard-friendly)**

```
WEAK PROMPT:
"Review this code."

STRONG PROMPT:
"Review this FastAPI route specifically for security risks 
(authorization, injection, data exposure) and performance risks 
(unbounded queries, missing pagination). List each risk found, 
why it matters, and a suggested fix."
```

**[Script:]**

"The strong prompt does two things: it names the specific risk categories to check, and it asks for structure — risk, why it matters, fix — which makes the response far easier to act on than an unstructured paragraph. This is the same structured-output thinking from earlier sessions, applied to a review workflow instead of an application feature."

**Recap of Block 4 before moving on:**

- Risk review covers security, edge cases, error handling, performance, and concurrency — categories that a correctness-only review misses entirely
- Many of these risks connect directly to earlier course material: authorization checks, N+1 queries, injection vulnerabilities
- A generic "review this code" prompt tends to surface only surface-level style comments
- Prompting specifically for named risk categories, with a structured output format, produces dramatically more useful review results

---

## Block 5 — Refactoring Suggestions

### 5A — What Refactoring Is and Is Not

**[Script:]**

"Refactoring means changing the internal structure of code without changing its external behavior — same inputs, same outputs, cleaner implementation. This is distinct from fixing a bug, which changes behavior, and distinct from adding a feature, which adds new behavior.

AI tools are genuinely strong at suggesting refactors, because refactoring patterns — extracting a function, removing duplication, simplifying nested conditionals, replacing a manual loop with a built-in — are extremely well represented in training data. This is one of the highest-value, lowest-risk uses of AI in a development workflow, precisely because the goal is preserving existing behavior, which is easy to verify with existing tests."

---

### 5B — Evaluating a Refactoring Suggestion

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the original code. Ask: "What would you refactor here, before seeing any suggestion?" Then reveal the AI-style suggestion and compare.

**Demo 5 — A refactoring suggestion (whiteboard-friendly)**

```python
# Original
def get_discount(user):
    if user.is_premium:
        if user.years_active > 5:
            return 0.20
        else:
            return 0.10
    else:
        if user.years_active > 5:
            return 0.05
        else:
            return 0.0

# Suggested refactor
def get_discount(user):
    if user.is_premium and user.years_active > 5:
        return 0.20
    if user.is_premium:
        return 0.10
    if user.years_active > 5:
        return 0.05
    return 0.0
```

**[Script:]**

"The nested conditionals in the original are hard to scan — you have to track two levels of branching in your head. The refactor flattens this into a sequence of independent checks, each returning immediately. Same behavior for every possible combination of `is_premium` and `years_active`, but far easier to read and to extend later if a new tier is added.

This is exactly the kind of mechanical, pattern-based improvement AI tools handle well. But — critical point — 'same behavior' is a claim that needs verification, not just trust."

> 🎯 **Instructor Note:** Ask: "How would you actually confirm this refactored version behaves identically to the original for every input?" Answer: run the existing test suite against the refactored version, and if none exists, write a few test cases covering each combination of true/false for both conditions before accepting the refactor. This is not optional — it is the entire safety net that makes refactoring safe.

**[Script:]**

"If your codebase has good test coverage, accepting a refactor is low-risk: run the tests, confirm they still pass, done. If your codebase has poor or no test coverage, an AI-suggested refactor is a good opportunity — write tests for the current behavior first, then apply and verify the refactor. Never accept a structural change to code you cannot verify still behaves the same way."

**Recap of Block 5 before moving on:**

- Refactoring changes code structure without changing external behavior; this is distinct from bug fixes and feature additions
- AI tools are strong at pattern-based refactors — extracting functions, removing duplication, flattening nested conditionals
- "Same behavior" is a claim, not a guarantee — verify a refactor with existing tests, or write tests first if none exist
- Good test coverage is what makes accepting AI refactoring suggestions low-risk; without it, refactoring is inherently riskier regardless of who or what suggested it

---

## Block 6 — AI-Assisted Code Review in Practice

### 6A — Where AI Review Fits in a Real Workflow

**[Script:]**

"Bringing this all together: how does AI-assisted review actually fit into how a team ships code? The most effective pattern is not 'replace the human reviewer with AI'. It is 'use AI as a first pass, before the human reviewer sees the code'.

An AI review pass, prompted specifically for the risk categories and readability concerns from earlier blocks, catches a meaningful fraction of issues quickly and consistently, at essentially no cost in a human reviewer's time. This means by the time a human reviewer looks at the code, the obvious issues are already caught and fixed, and the human's attention — a genuinely scarce resource — is spent on the things AI is worse at: whether this change makes sense given the team's actual priorities, whether the approach fits how the rest of the system is architected, whether this is really the right solution to the underlying problem at all."

> 🎯 **Instructor Note:** Draw this workflow on the board — it is the practical takeaway of the whole session.

```
Effective workflow:
1. Write code
2. AI review pass — risk categories, readability, refactor suggestions
3. Fix what AI catches; verify suggestions before accepting them
4. Human review — architecture, priorities, business logic, judgment
5. Merge

AI review is a filter that reduces noise reaching the human reviewer,
not a replacement for the human reviewer's judgment.
```

---

### 6B — Limitations to Keep in Mind

**[Script:]**

"A few limitations are worth naming directly, because they matter for how much trust to place in this workflow.

AI review tools do not have the context a human teammate has — they do not know that this endpoint is about to be deprecated next sprint, or that a similar bug caused a production incident two months ago, or that the team deliberately chose a slower but simpler approach for a good reason not visible in the code itself. AI review operates entirely on what is in front of it, not on team history or business context.

AI review can also miss risks that require understanding how this code interacts with the rest of a large, unfamiliar system — the same limitation named in the debugging block. A risk that only appears when this function is called from three different places with subtly different assumptions is much harder for AI to catch than a risk visible within a single function.

And AI review, like AI debugging, can produce comments that sound authoritative but are wrong — flagging something as a bug that is actually intentional, or suggesting a 'fix' that breaks an edge case the reviewer did not have visibility into."

> 🎯 **Instructor Note:** Ask the room to close with a real judgment question: "Given everything in this session, would you ever skip human review entirely and rely only on AI review for a change going to production?" There is no single correct answer, but a well-reasoned response should reference: the size and risk of the change, whether test coverage exists, and whether the AI review was prompted specifically enough to be trustworthy for that particular change.

**Recap of Block 6 before moving on:**

- The most effective workflow uses AI review as a first pass before human review, not as a replacement for it
- AI review reduces noise reaching the human reviewer, letting human attention focus on architecture, priorities, and judgment calls AI cannot make
- AI review lacks team context, business history, and system-wide understanding that a human teammate has
- Confident-sounding but incorrect review comments are a real risk; verify before accepting, exactly as with debugging suggestions

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What is AI debugging strong at versus weak at? What are the five dimensions of code review? Why does a generic review prompt underperform a risk-specific one? What makes a refactor genuinely safe to accept? Where does AI review fit relative to human review?"

**Debugging with AI**

- AI debugging pattern-matches your code and error against training data; it does not execute your code
- Strong for common, well-documented bugs; weak for bugs dependent on specific runtime state or complex system interactions
- A strong debugging prompt includes the exact error, relevant code, and the specific triggering input
- Every AI debugging suggestion is a hypothesis to test, not a verdict to trust

**Code Review Fundamentals**

- Code review covers correctness, readability, consistency, risk, and maintainability
- Correctness is usually the easiest dimension to verify; readability and consistency matter for long-term codebase health
- Naming and idiom consistency are areas where AI review is consistently strong

**Identifying Risks**

- Risk categories include security, edge cases, error handling, performance, and concurrency — missed entirely by a correctness-only review
- Many risks connect directly to earlier course material — authorization, N+1 queries, injection
- Prompting for specific, named risk categories with a structured response format produces far more useful review results than a generic request

**Refactoring Suggestions**

- Refactoring changes structure without changing behavior; distinct from bug fixes and feature work
- AI tools are strong at pattern-based refactors, since these patterns are heavily represented in training data
- "Same behavior" must be verified with tests, existing or newly written, before accepting a refactor

**AI-Assisted Code Review in Practice**

- The most effective workflow uses AI as a first-pass filter before human review, not as a replacement
- This frees human reviewer attention for architecture, priorities, and judgment calls AI cannot make
- AI review lacks team context and system-wide understanding, and can produce confident but incorrect comments

**Why All of This Matters Together**

- Debugging, code review, risk identification, and refactoring are four facets of the same underlying skill — critically evaluating code, your own or someone else's, for correctness and quality — and AI tools genuinely accelerate all four, but only when used with the same discipline you would apply to any other tool: give it good context, ask specific rather than generic questions, and verify its output rather than trusting it by default; used this way, AI review is a force multiplier on your judgment, not a substitute for it

---

*End of script.*