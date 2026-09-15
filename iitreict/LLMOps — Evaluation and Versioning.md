# Lecture Script: LLMOps — Evaluation and Versioning
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | LLMOps Foundations | 22 min |
| 3 | Prompt Versioning | 25 min |
| 4 | Evaluation Sets | 25 min |
| 5 | Regression Testing for Prompts | 25 min |
| 6 | Lecture Summary and Recap | 5 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has built AI-powered features, optimized them for cost, built and debugged agents, and just learned how to deploy a FastAPI application to production with stable configuration. This session addresses a category of production concern that is specific to LLM-powered features and does not have a direct equivalent in traditional backend deployment. Open by naming that gap precisely. Wait after the opening question.

**[Script:]**

"You now know how to deploy your FastAPI application safely — packaged, environment-aware, with secrets handled correctly. But deploying an application that calls an LLM introduces a category of problem that ordinary backend deployment does not have an answer for.

Your database migration session covered how to safely change a schema without breaking existing data. Your refactoring session covered how to verify code behavior did not change unexpectedly. But what is the equivalent discipline when the thing that changed is your prompt? You update the system prompt for your customer support feature, hoping to make responses more concise. You ship it. Two days later, someone notices the AI has started giving factually wrong answers to a category of question it used to handle perfectly well. What changed? When did it change? How would you have caught this before it reached real users?

This is the exact gap LLMOps exists to close — the operational discipline specifically around managing, evaluating, and safely changing LLM-powered behavior over time, the same way traditional DevOps manages the operational discipline around deploying and running traditional software. A prompt is not just a string sitting in your code. It is a piece of your application's actual logic, and it deserves the same discipline you already apply to code: version control, a way to test whether a change made things better or worse, and a way to detect when a change accidentally broke something that used to work.

Today covers four connected pieces: what LLMOps actually means as a discipline, how to version prompts the way you already version code, how to build evaluation sets that let you objectively measure whether a prompt is actually working well, and how to run regression tests specifically for prompts, so a change that fixes one thing does not silently break something else that used to work correctly."

---

## Block 2 — LLMOps Foundations

### 2A — What LLMOps Actually Means

**[Script:]**

"LLMOps is the set of practices for reliably developing, deploying, and maintaining LLM-powered features in production, drawing directly from the discipline of traditional DevOps and MLOps, but adapted for the specific characteristics of LLMs: non-deterministic outputs, prompts that function as executable logic, and quality that is often subjective rather than a simple pass-or-fail test."

> 🎯 **Instructor Note:** Draw this comparison on the board — it grounds LLMOps in something the audience already understands from traditional software practice.

```
Traditional software:
  Code change → automated tests → deploy → monitor for errors
  "Correct" is usually well-defined: the test either passes or fails

LLM-powered features:
  Prompt or model change → ??? → deploy → monitor for what, exactly?
  "Correct" is often a matter of degree: is this response GOOD, 
  not just "did it crash"

LLMOps fills in the "???" — bringing the same operational rigor 
to prompts and LLM behavior that traditional software already has 
for code.
```

**[Script:]**

"The core insight is that a prompt is functionally part of your application's logic, exactly as much as any function you have written — it directly determines your application's behavior. But prompts do not naturally get the same treatment as code in most projects: they are often edited directly in a string in your source file, deployed with no specific testing beyond 'I tried it a few times and it looked fine,' and changed without any record of what the previous version actually produced. LLMOps is the practice of closing that gap."

---

### 2B — Why Non-Determinism Makes This Harder Than Normal Testing

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If you run the exact same prompt against the exact same model twice, do you expect to get the exact same output both times?" Answer: not necessarily — LLM outputs have some inherent randomness by default, meaning identical inputs can produce meaningfully different, though often similarly structured, outputs across separate calls. This is the foundational complication that makes prompt testing genuinely different from testing deterministic code.

**[Script:]**

"A traditional unit test asserts an exact expected output for a given input, and that assertion either holds or it does not — deterministic and binary. A prompt does not behave this way by default. The same input might produce a correct, well-formatted answer on one run and a correct answer with slightly different phrasing on the next, and neither is 'wrong' — they are just different instances of an acceptable response.

This means prompt evaluation cannot simply check for exact output equality the way a normal test does. It requires different techniques — checking for the presence of required elements, checking against acceptable ranges or categories, or in some cases using another LLM call specifically to judge whether a response meets certain criteria. Block 4 covers exactly how to build evaluation sets that handle this non-determinism properly, rather than naively expecting exact matches."

> 🎯 **Instructor Note:** Ask: "Given that identical prompts can produce different outputs, does this mean prompt testing is essentially impossible, or does it just require a different approach than exact-match testing?" Answer: it requires a different approach, not impossibility — you can still test for required properties of a response (does it contain the right information, does it avoid a forbidden pattern, does it stay within an acceptable length) even when you cannot test for one single exact string. This reframing is what makes evaluation sets, covered in Block 4, actually work.

**Recap of Block 2 before moving on:**

- LLMOps applies the operational discipline of traditional DevOps and MLOps specifically to LLM-powered features — prompts, model behavior, and non-deterministic quality
- A prompt is functionally part of an application's logic, and deserves the same rigor as code, even though it is often treated far more casually
- LLM non-determinism means identical inputs can produce different, still-acceptable outputs, which is why prompt evaluation requires different techniques than exact-match testing
- The rest of this session covers three specific LLMOps practices: prompt versioning, evaluation sets, and regression testing, each addressing a specific part of this gap

---

## Block 3 — Prompt Versioning

### 3A — Why a Prompt String in Code Is Not Enough

**[Script:]**

"You likely already have prompts embedded as strings directly in your application code, tracked by whatever version control you already use for the rest of your code. This is a reasonable starting point, but it has real limitations once a prompt is genuinely important to your application's behavior. There is no easy way to see the full history of how a specific prompt evolved without digging through unrelated code changes in the same commits. There is no way to run two different prompt versions side by side to compare their actual output quality. And there is no clean way to roll back specifically a prompt change without also rolling back unrelated code changes that happened to be in the same commit."

---

### 3B — Structuring Prompts as Versioned, Independent Assets

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If your system prompt is a plain string hardcoded inside a Python function, and you want to compare how version 3 and version 4 of that prompt actually perform against the same set of test questions, what would you currently have to do to run that comparison?" Answer: manually check out an old commit, temporarily swap the prompt string, run your tests, then switch back — a slow, manual, error-prone process. This motivates treating prompts as separately versioned assets rather than being buried inside application code.

**Demo 1 — Structuring prompts as independent, versioned assets (whiteboard-friendly)**

```python
# prompts/customer_support_v3.txt
"""
You are a customer support assistant for an e-commerce platform.
Answer questions concisely, using only information from the 
provided order context. If the context does not contain enough 
information, say so directly rather than guessing.
"""

# prompts/customer_support_v4.txt
"""
You are a customer support assistant for an e-commerce platform.
Answer questions in two sentences or fewer, using only information 
from the provided order context. If the context does not contain 
enough information, say so directly rather than guessing.
"""
```

**[Script:]**

"Each version lives as its own file, under a clear naming convention that makes the version explicit. The difference between v3 and v4 here is deliberate and small — a length constraint was added, likely in response to feedback that responses were too long — and because each version is its own separate file, that specific change is immediately visible by comparing the two files directly, completely independent of whatever other application code changed around the same time.

This is not a different discipline from version-controlling code — it is applying the same discipline specifically and deliberately to prompts, so a prompt's own history is not buried inside unrelated commits."

---

### 3C — Loading a Specific Prompt Version at Runtime

**Demo 2 — Loading a versioned prompt with a clear reference point (whiteboard-friendly)**

```python
PROMPT_VERSION = "v4"

def load_system_prompt(version: str) -> str:
    with open(f"prompts/customer_support_{version}.txt") as f:
        return f.read()

system_prompt = load_system_prompt(PROMPT_VERSION)
```

**[Script:]**

"`PROMPT_VERSION` is a single, explicit variable declaring exactly which version is currently active — a deliberate choice, not an implicit consequence of whatever happens to be the latest edit to a single hardcoded string. If v4 turns out to perform worse in production, reverting to v3 is a one-line change, with zero risk of accidentally reverting unrelated application logic that happened to be edited alongside it.

This also directly enables the comparison problem from the top of this block: running the exact same evaluation set, covered in Block 4, against both v3 and v4, by simply changing this one variable and re-running, gives you an objective, side-by-side comparison of the two versions' actual real performance."

> 🎯 **Instructor Note:** Ask: "Beyond just reverting easily, what does having an explicit `PROMPT_VERSION` variable let you do that a hardcoded string does not?" Answer: it enables direct, controlled comparison between versions — running the identical evaluation set against v3 and then v4 by changing one variable, which is exactly the kind of controlled, one-change-at-a-time comparison from the iterative refinement discipline in the agentic debugging session, now applied specifically to prompts.

**Recap of Block 3 before moving on:**

- A prompt buried inside application code makes its own change history hard to isolate and makes side-by-side version comparison difficult
- Structuring prompts as their own versioned files, separate from application logic, makes each version's specific changes clearly visible
- An explicit version reference, rather than a single hardcoded string, allows clean rollback and direct comparison between versions
- Prompt versioning is the same version-control discipline already applied to code, deliberately applied to prompts specifically

---

## Block 4 — Evaluation Sets

### 4A — What an Evaluation Set Is

**[Script:]**

"An evaluation set is a curated collection of representative test inputs, each paired with criteria for judging whether a given output for that input is actually acceptable. This is the mechanism that answers the non-determinism problem from Block 2: instead of asserting one exact expected output, an evaluation set defines what a good response for a given input needs to satisfy, and checks any produced output against that."

> 🎯 **Instructor Note:** Draw this structure on the board.

```
An evaluation set entry:
  input:    a representative example input the feature should handle
  criteria: what a genuinely acceptable output for this input 
            must satisfy — not one single exact expected string

A full evaluation set = many such entries, chosen deliberately 
to cover the real range of situations the feature must handle 
well, including known-tricky edge cases.
```

---

### 4B — Building a Representative Evaluation Set

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If your evaluation set for a customer support assistant only contains five easy, straightforward questions, and all five pass, what confidence does that actually give you about how the assistant performs on a genuinely confusing or ambiguous customer question?" Answer: very little — an evaluation set that only covers easy cases tells you the prompt handles easy cases, not that it is actually reliable across the real range of situations it will encounter in production, including harder or unusual ones.

**Demo 3 — A small but deliberately varied evaluation set (whiteboard-friendly)**

```python
evaluation_set = [
    {
        "input": "What is the status of order #12345?",
        "context": "Order #12345: shipped, arriving March 15",
        "criteria": ["mentions 'shipped'", "mentions the delivery date"]
    },
    {
        "input": "Can I get a refund for a broken laptop I never ordered?",
        "context": "No matching order found in customer's account",
        "criteria": ["does not fabricate an order", "clearly states no matching order was found"]
    },
    {
        "input": "What is your return policy?",
        "context": "",  # deliberately empty — no relevant context provided
        "criteria": ["does not guess or fabricate a policy", "states it does not have this information"]
    }
]
```

**[Script:]**

"Notice this is deliberately not three easy, similar questions. The first is a straightforward case with clear context available. The second is a trap — a question referencing an order that does not actually exist in the provided context, testing whether the assistant fabricates a plausible-sounding but false answer rather than correctly recognizing no matching order exists. The third has genuinely no relevant context at all, testing whether the assistant honestly says so rather than guessing at a generic return policy — this is the exact grounding behavior from the RAG session, now captured as something concretely testable.

A well-built evaluation set deliberately includes cases like these — not just the easy, expected-to-succeed cases, but the specific situations where a plausible-sounding but wrong answer is a realistic risk."

> 🎯 **Instructor Note:** Connect this directly to the RAG session. Say: "The second and third examples here are directly testing the grounding behavior from the RAG session — does the assistant honestly say 'I don't have that information' rather than confidently fabricating something plausible. An evaluation set is how you turn that principle from something you hope is true into something you can actually, repeatedly verify."

---

### 4C — Running an Evaluation

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Given that outputs are not deterministic, how do you think the 'criteria' for each evaluation set entry should actually be checked — comparing against one exact expected string, or something else?" Answer: something else — checking whether specific required elements are present, checking for the absence of specific forbidden patterns like a fabricated order number, or in more complex cases, using a separate LLM call specifically prompted to judge whether the criteria are satisfied.

**Demo 4 — Running an evaluation set against a prompt version (whiteboard-friendly)**

```python
def evaluate_response(response: str, criteria: list[str]) -> dict:
    # Simplified: a real implementation might use keyword checks,
    # or a separate LLM call to judge each criterion
    results = {c: (c.split("'")[1].lower() in response.lower() if "'" in c else None) for c in criteria}
    return results

def run_evaluation(prompt_version: str, evaluation_set: list) -> dict:
    system_prompt = load_system_prompt(prompt_version)
    results = []

    for case in evaluation_set:
        response = call_llm(system_prompt, case["input"], case["context"])
        check = evaluate_response(response, case["criteria"])
        results.append({"input": case["input"], "response": response, "check": check})

    return results
```

**[Script:]**

"`run_evaluation` loops through every case in the evaluation set, generates a real response using the specified prompt version, and checks that response against the case's criteria. This produces a structured record of exactly how a given prompt version performed against every case in your evaluation set — not a single pass-or-fail number, but a detailed, inspectable breakdown you can actually look through.

In practice, checking sophisticated criteria like 'does not fabricate an order' often benefits from using a separate LLM call specifically to judge the response — sometimes called an LLM-as-judge pattern — rather than simple keyword matching, since fabrication is a semantic property that simple string checks cannot reliably detect. The keyword-matching version here is simplified specifically to keep the mechanics visible."

> 🎯 **Instructor Note:** Ask: "Why might a real evaluation system use a separate LLM call to judge whether a response satisfies a criterion like 'does not fabricate an order,' rather than a simple keyword check?" Answer: fabrication is a semantic judgment, not something a simple keyword search can reliably detect — a response could avoid the specific words "order" and "fabricate" while still inventing false information in a different phrasing. An LLM judge, given the criterion and the response, can make a more nuanced semantic assessment closer to actual human judgment, though this itself introduces a new dependency on another model's reliability worth being aware of.

**Recap of Block 4 before moving on:**

- An evaluation set is a curated collection of representative inputs paired with criteria for judging output quality, rather than one exact expected output
- A well-built evaluation set deliberately includes edge cases and known-tricky situations, not only easy cases likely to pass
- Criteria are checked through methods suited to non-deterministic output — required elements, forbidden patterns, or an LLM-as-judge for more semantic judgments
- Running an evaluation set against a specific prompt version produces a structured, inspectable record of real performance, not a single pass-or-fail result

---

## Block 5 — Regression Testing for Prompts

### 5A — Why a Prompt Improvement Can Still Be a Regression

**[Script:]**

"A regression is when a change intended to fix or improve one thing accidentally breaks something that used to work correctly. This is a familiar concept from the refactoring session, applied there to code — the same risk applies directly to prompts, and arguably more so, since a prompt change can shift behavior in ways that are much harder to predict from reading the change alone than a code change usually is.

Recall the customer support prompt from Block 3 — v4 added a length constraint, 'answer in two sentences or fewer,' likely in direct response to feedback that responses were too long. That seems like a clear, reasonable improvement. But what if enforcing that length constraint causes the assistant to omit an important caveat it used to reliably include — for instance, failing to mention that a return requires an original receipt? The prompt got better at the thing it was changed to fix, and quietly worse at something else it used to do correctly."

> 🎯 **Instructor Note:** This scenario is the central teaching example for the entire block — return to it explicitly in the demo that follows.

---

### 5B — Regression Testing by Comparing Evaluation Results

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Using the evaluation set structure from Block 4, how would you actually detect the kind of regression just described — a prompt getting better at one thing while quietly getting worse at something else?" Guide toward: run the exact same evaluation set against both the old and new prompt versions, and directly compare the results case by case, not just look at whether the new version seems better overall.

**Demo 5 — Regression testing by comparing two prompt versions on the same evaluation set (whiteboard-friendly)**

```python
def compare_prompt_versions(old_version: str, new_version: str, evaluation_set: list):
    old_results = run_evaluation(old_version, evaluation_set)
    new_results = run_evaluation(new_version, evaluation_set)

    regressions = []
    for old, new in zip(old_results, new_results):
        old_passed = all(v for v in old["check"].values() if v is not None)
        new_passed = all(v for v in new["check"].values() if v is not None)

        if old_passed and not new_passed:
            regressions.append({
                "input": old["input"],
                "old_response": old["response"],
                "new_response": new["response"]
            })

    return regressions

regressions = compare_prompt_versions("v3", "v4", evaluation_set)
```

**[Script:]**

"`compare_prompt_versions` runs the identical evaluation set against both the old and new prompt versions, then checks each case individually: did this specific case pass under the old version but fail under the new one? If so, that is exactly a regression — something that used to work correctly, broken by a change intended to improve something else entirely.

This is the direct prompt-specific equivalent of running your existing test suite after a code refactor, from the refactoring session — the exact same 'verify nothing that used to work now breaks' discipline, applied here through re-running the same evaluation set rather than a traditional test suite, since traditional exact-match tests do not work well against non-deterministic LLM output."

> 🎯 **Instructor Note:** Ask: "If this comparison found that the length-constraint prompt from Block 3 does cause a regression — omitting the receipt requirement on cases where the old version reliably mentioned it — what would the right next step actually be, based on the iterative refinement discipline from the agentic debugging session?" Answer: not simply reverting entirely, and not shipping the regression anyway — the disciplined next step is a targeted refinement: adjust the prompt again, perhaps explicitly instructing it to keep responses concise while still always including required caveats like the receipt requirement, then re-run this exact same comparison to confirm the regression is actually resolved without reintroducing the original length problem.

---

### 5C — Regression Testing as an Ongoing, Not One-Time, Practice

**[Script:]**

"This comparison is not something you run once and forget. Every time a prompt changes — a small wording tweak, a new instruction, a change in response to new feedback — running this same comparison against your evaluation set is what actually confirms whether the change is a genuine improvement, a genuine regression, or some mix of both across different cases. Over time, as real production usage surfaces new edge cases and failure patterns, those specific cases belong added to your evaluation set as well, so your regression testing keeps pace with what you have actually learned about how the feature can fail."

> 🎯 **Instructor Note:** Close with a synthesis question tying the whole session together. Ask: "How do prompt versioning from Block 3, evaluation sets from Block 4, and regression testing from this block actually depend on each other — could you do regression testing without the other two?" Answer: no — regression testing specifically requires having distinct, clearly identified prompt versions to compare, which is exactly what Block 3's versioning discipline provides, and it requires a representative evaluation set with checkable criteria to actually run the comparison against, which is exactly what Block 4 provides. None of the three works well in isolation; they form one connected practice.

**Recap of Block 5 before moving on:**

- A prompt change intended to fix one issue can silently introduce a regression elsewhere, exactly like a code refactor can, and prompt behavior shifts are often harder to predict by simply reading the change
- Regression testing for prompts means running the same evaluation set against both the old and new version and comparing results case by case, not just checking whether the new version seems better overall
- This is the direct prompt-specific equivalent of re-running an existing test suite after a code refactor, adapted for non-deterministic output
- Regression testing is an ongoing practice tied to every prompt change, and the evaluation set itself should grow over time as real production usage reveals new edge cases worth testing for

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "Why does LLM non-determinism make traditional exact-match testing insufficient for prompts? What does structuring prompts as separate versioned files enable that a hardcoded string does not? Why should an evaluation set deliberately include tricky edge cases, not just easy ones? How do you actually detect that a prompt change introduced a regression?"

**LLMOps Foundations**

- LLMOps applies traditional DevOps and MLOps discipline specifically to LLM-powered features — prompts, model behavior, and non-deterministic quality
- A prompt is functionally part of an application's logic and deserves the same operational rigor as code
- LLM non-determinism means prompt evaluation requires different techniques than exact-match testing — checking for required properties, not one exact expected output

**Prompt Versioning**

- A prompt embedded directly in application code makes its change history hard to isolate and version comparison difficult
- Structuring prompts as separate, explicitly versioned files makes each version's specific changes clearly visible
- An explicit version reference enables clean rollback and direct, controlled comparison between prompt versions

**Evaluation Sets**

- An evaluation set pairs representative inputs with criteria for judging output quality, rather than one exact expected output
- A well-built evaluation set deliberately includes edge cases and known-tricky situations, including tests for honest grounding rather than fabrication
- Criteria can be checked through required elements, forbidden patterns, or an LLM-as-judge for more semantic judgments

**Regression Testing for Prompts**

- A prompt change that improves one thing can silently regress something that used to work correctly, exactly like a code refactor can
- Regression testing means running the same evaluation set against old and new prompt versions and comparing case by case
- This is an ongoing practice tied to every prompt change, with the evaluation set itself growing as real usage reveals new edge cases

**Why All of This Matters Together**

- Prompt versioning, evaluation sets, and regression testing are not three separate practices — they depend directly on each other, forming one connected discipline: versioning gives you distinct things to compare, evaluation sets give you a way to measure them objectively despite non-deterministic output, and regression testing is what you get when you apply that measurement to compare versions before and after a change; together, this is what closes the exact gap named at the start of this session — the same rigor already applied to code and infrastructure deployment, now applied specifically to the prompts that drive an LLM-powered feature's actual behavior in production

---

*End of script.*
