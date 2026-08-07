# Lecture Script: AI Cost Optimization and Performance
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Token Counting and Cost Calculation | 22 min |
| 3 | Prompt Optimization Techniques | 20 min |
| 4 | Caching Strategies | 22 min |
| 5 | Batch Processing | 18 min |
| 6 | Model Selection Strategies | 15 min |
| 7 | Lecture Summary and Recap | 5 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has already built working AI-powered features across several sessions — chat integration, structured output, vision, embeddings. This lecture is about what happens after the feature works: making it affordable and fast enough to actually run in production at real usage volumes. Open with a concrete cost scenario, not an abstract warning. Wait after the opening question.

**[Script:]**

"Your feature works. It calls the API, gets a good response, returns it to the user. You demo it, everyone is happy, you ship it. Then real usage arrives, and the bill arrives with it.

Picture this: a support-chat feature that resends full conversation history with every message, as you learned it must, since these APIs are stateless. A conversation that grows to thirty messages means every new message resends the previous twenty-nine. Multiply that by a thousand active conversations a day, and your token usage — and your bill — is growing quadratically with conversation length, not linearly. Nobody decided this on purpose. It is simply what happens when you build the feature correctly but do not think about cost.

Or picture this: two nearly identical requests, seconds apart, from two different users asking the exact same common question. Your application pays for and waits on two full API calls to generate what is functionally the same answer twice.

Or this: you are processing ten thousand product descriptions overnight, one API call at a time, sequentially, when the provider offers a way to submit all ten thousand as a single batch at a fraction of the cost.

None of these are bugs. The features work correctly. But 'works correctly' and 'works affordably at scale' are different bars, and closing that gap is what today is about: counting and predicting cost before it surprises you, optimizing prompts to use fewer tokens without losing quality, caching to avoid redundant calls entirely, batching to process volume efficiently, and choosing the right model for each specific task instead of defaulting to the most powerful one everywhere."

---

## Block 2 — Token Counting and Cost Calculation

### 2A — Why You Need to Count Tokens Before You Send Them

**[Script:]**

"You already know from earlier sessions that API responses include `usage.prompt_tokens` and `usage.completion_tokens` — but that tells you the cost after the request already happened. For real cost control, you need to estimate token counts before sending a request: to enforce budgets, to warn users, to decide whether a request is even worth making."

**[Script:]**

"Recall from the LLM fundamentals session that tokenization splits text into subword pieces, not whole words. As a rough rule of thumb for English text, one token is approximately four characters, or roughly three-quarters of a word. This is an approximation — the actual count depends on the specific tokenizer — but it is close enough for budgeting and estimation."

> 🎯 **Instructor Note:** Write this approximation on the board, and immediately caveat it as an estimate, not an exact rule.

```
Rough estimate: 1 token ≈ 4 characters ≈ 0.75 words (English text)

This is an approximation for planning purposes.
For exact counts, use the provider's tokenizer library directly.
```

---

### 2B — Exact Token Counting

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I count the characters in a sentence and divide by four, will that match the exact token count a model actually uses?" Answer: approximately, but not exactly — punctuation, numbers, and unusual words tokenize differently than the rough estimate predicts. For anything cost-critical, exact counting is worth the extra step.

**Demo 1 — Exact token counting with tiktoken (whiteboard-friendly)**

```python
import tiktoken

encoding = tiktoken.encoding_for_model("gpt-4o-mini")

text = "The quick brown fox jumps over the lazy dog."
tokens = encoding.encode(text)

print(len(tokens))    # exact token count, e.g. 10
print(tokens[:5])     # the actual token IDs
```

**[Script:]**

"`tiktoken` is OpenAI's tokenizer library — the same tokenizer the model itself uses, so this count is exact, not estimated. `encoding.encode(text)` returns the actual list of token IDs; `len(tokens)` gives you the precise count before you ever send the request.

This matters most for two things: enforcing a maximum context length before a request fails with an error, and calculating exact cost for budgeting or billing users of your own application."

---

### 2C — Calculating Cost

**[Script:]**

"Providers charge per token, with different rates for input tokens — what you send — and output tokens — what the model generates. Output tokens are typically priced higher than input tokens, since generation is more computationally expensive than processing existing text."

> 🎯 **Instructor Note:** Ask learners to reason about this before confirming: "Why would output tokens cost more than input tokens per token, if both are processed by the same model?" Answer: input tokens can be processed in parallel — the model reads the whole prompt at once. Output tokens must be generated one at a time, sequentially, each one depending on everything generated before it, which is inherently more expensive per token.

**Demo 2 — Cost calculation (whiteboard-friendly)**

```python
# Example pricing (illustrative — always check current provider pricing)
INPUT_COST_PER_1M = 0.15   # dollars per 1 million input tokens
OUTPUT_COST_PER_1M = 0.60  # dollars per 1 million output tokens

def calculate_cost(prompt_tokens: int, completion_tokens: int) -> float:
    input_cost = (prompt_tokens / 1_000_000) * INPUT_COST_PER_1M
    output_cost = (completion_tokens / 1_000_000) * OUTPUT_COST_PER_1M
    return input_cost + output_cost

cost = calculate_cost(prompt_tokens=1500, completion_tokens=300)
print(f"${cost:.6f}")
```

**[Script:]**

"Pricing is always per million tokens, so we divide by a million before multiplying by the rate. This single request costs a fraction of a cent — but multiply this by thousands of requests a day, and small inefficiencies compound quickly. This is exactly the calculation you should run on your `response.usage` values after every call, logged and aggregated, so you can see real cost trends before they become a surprising bill."

> 🎯 **Instructor Note:** Emphasize: "Prices change and differ by model and provider — never hardcode a number you saw once and assume it stays accurate. Always check current pricing pages before making real budgeting decisions." This is a factual accuracy caveat worth stating directly.

**Recap of Block 2 before moving on:**

- A rough estimate is one token per four characters of English text; exact counts require the provider's actual tokenizer, such as `tiktoken`
- Exact token counting lets you enforce context limits and calculate cost before sending a request
- Input and output tokens are priced separately, with output typically costing more, since generation happens sequentially rather than in parallel
- Logging and aggregating `response.usage` values across real requests is how you track actual cost trends in production

---

## Block 3 — Prompt Optimization Techniques

### 3A — Reducing Tokens Without Reducing Quality

**[Script:]**

"This connects directly to context engineering from the prompt engineering session — but here the lens is specifically cost and speed, not just relevance. Every unnecessary token in your prompt costs money and adds latency, since the model has to process it before generating anything.

Several concrete techniques reduce token count without hurting output quality."

> 🎯 **Instructor Note:** Write these on the board as a practical checklist.

```
Prompt optimization techniques:
1. Trim instructions — remove redundant or overly verbose phrasing
2. Compress examples — use fewer, more targeted few-shot examples
3. Avoid repetition — do not restate the same instruction multiple ways
4. Shorten system prompts — keep persona and rules concise
5. Truncate or summarize long context instead of including it whole
```

---

### 3B — Optimization in Practice

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Show the unoptimized prompt first. Ask: "Where do you see wasted tokens here, purely as text — before even worrying about whether it changes the response?" Let learners spot redundancy themselves before revealing the optimized version.

**Demo 3 — Prompt optimization side by side (whiteboard-friendly)**

```
UNOPTIMIZED (verbose, repetitive):
"I would like you to please carefully analyze the following customer 
review and determine, to the best of your ability, whether the 
sentiment expressed by the customer in this review is positive, 
negative, or neutral in nature. Please make sure to think about 
this carefully and provide an accurate classification. Here is 
the review that I would like you to analyze: 'Great product, 
fast shipping!'"

≈ 75 tokens

OPTIMIZED:
"Classify the sentiment of this review as positive, negative, or 
neutral: 'Great product, fast shipping!'"

≈ 20 tokens
```

**[Script:]**

"Same task, same expected output — but the optimized version uses roughly a quarter of the tokens. The unoptimized version has phrases like 'I would like you to please carefully' and 'to the best of your ability' that add no information the model needs to complete the task correctly. Politeness phrasing does not improve classification accuracy; it only adds cost.

At a single request this difference is invisible. At a hundred thousand requests, this is the difference between a real cost and a much smaller one, for identical output quality."

> 🎯 **Instructor Note:** Ask: "Is there ever a case where a longer, more elaborate prompt is worth the extra tokens?" Answer: yes — chain-of-thought prompting from the prompting techniques session intentionally adds tokens because the accuracy improvement is worth the cost for genuinely complex reasoning tasks. Optimization is not "always minimize tokens" — it is "do not spend tokens on things that do not improve the result." A simple classification task does not need elaborate framing; a hard reasoning task might benefit from it.

**[Script:]**

"The same principle applies to few-shot examples. Three sharp, well-chosen examples usually outperform six redundant ones, at half the token cost. Quality and relevance of context matters more than sheer quantity."

**Recap of Block 3 before moving on:**

- Every unnecessary token costs money and adds latency; optimization means removing tokens that do not improve the result
- Trim verbose phrasing, avoid restating instructions, keep system prompts concise, and summarize long context instead of including it whole
- Optimization is not about always minimizing length — chain-of-thought and well-chosen few-shot examples are token costs that are often worth paying for the accuracy they provide
- The goal is removing waste, not removing everything that adds tokens

---

## Block 4 — Caching Strategies

### 4A — Why Caching Applies to AI APIs

**[Script:]**

"You already understand caching as a general concept in backend systems — avoid redoing expensive work when the same result was already computed. AI API calls are expensive work, in both cost and latency, which makes them an excellent candidate for caching.

There are two levels of caching worth understanding here: your own application-level caching of full responses, and provider-side prompt caching, a feature offered by major providers that reduces cost automatically for repeated content."

---

### 4B — Application-Level Response Caching

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If a hundred different users ask your support bot the exact same frequently-asked question, without caching, how many API calls does that generate? With caching, how many?" Answer: without caching, one hundred separate API calls, each paying full cost and full latency for an identical answer. With caching, one API call the first time, and ninety-nine fast, free cache hits after that.

**Demo 4 — Response caching (whiteboard-friendly)**

```python
import hashlib

response_cache = {}

def get_cached_ai_response(prompt: str, system_prompt: str) -> str:
    cache_key = hashlib.sha256((system_prompt + prompt).encode()).hexdigest()

    if cache_key in response_cache:
        return response_cache[cache_key]

    result = get_ai_response(prompt, system_prompt)
    response_cache[cache_key] = result
    return result
```

**[Script:]**

"A cache key is built by hashing the combination of the system prompt and the user's actual input — identical input produces an identical key, so identical requests hit the cache instead of the API. `get_ai_response` is the reusable service function from the backend integration session; caching wraps around it without changing its own logic at all.

In a real production application, this in-memory dictionary would be replaced with something like Redis — shared across multiple server instances and surviving a restart — but the caching logic itself is unchanged."

> 🎯 **Instructor Note:** Ask directly: "What kind of requests should you cache, and what kind should you never cache?" Guide toward: cache requests where identical input should reasonably produce the same or similar output — FAQ-style questions, static content generation, common classifications. Never cache anything involving real-time or personalized data, like 'what is my current account balance' or anything where correctness depends on the current moment, not just the input text.

---

### 4C — Provider-Side Prompt Caching

**[Script:]**

"Separately from anything you build yourself, several providers offer prompt caching as a built-in API feature. When a large portion of your prompt — a long system prompt, a lengthy document used as context — repeats identically across many requests, the provider can cache the model's internal processing of that repeated portion and charge a reduced rate for it on subsequent requests, since it does not need to be fully reprocessed.

This is particularly valuable for the exact scenario from the hook — a long conversation history resent with every message. The unchanged earlier portion of the conversation benefits from provider-side caching, while only the new final message is processed at full cost."

> 🎯 **Instructor Note:** Note this is provider-specific and evolves quickly. Say: "The exact mechanics and pricing of provider-side caching differ by provider and change over time — check current documentation when implementing this. The concept to remember is that unchanging, repeated content in your prompts is often eligible for a cost discount you get largely for free by structuring your prompts consistently."

**Recap of Block 4 before moving on:**

- AI API calls are expensive in cost and latency, making them strong candidates for caching
- Application-level caching stores full responses keyed by a hash of the input, avoiding redundant identical calls
- Cache FAQ-style or static content; never cache anything involving real-time or personalized data
- Provider-side prompt caching automatically reduces cost for repeated portions of a prompt, such as a long system prompt or growing conversation history

---

## Block 5 — Batch Processing

### 5A — When Batching Applies

**[Script:]**

"Not every AI workload needs a response immediately. Processing ten thousand product descriptions overnight, generating summaries for a backlog of documents, classifying a large historical dataset — none of these need a response in real time the way a user-facing chat does.

For these workloads, sending requests one at a time, sequentially, waiting for each response before sending the next, is slow and often more expensive than necessary. Batch processing APIs, offered by major providers, let you submit a large collection of requests together, processed asynchronously, typically at a significant cost discount compared to real-time requests — often around half the price."

> 🎯 **Instructor Note:** Draw the contrast on the board.

```
Real-time processing:
Request 1 → wait → response 1 → Request 2 → wait → response 2 → ...
Fast turnaround per request, full price, needed for user-facing features

Batch processing:
Submit all requests together → provider processes asynchronously →
retrieve all results once complete (often minutes to hours later)
Slower overall turnaround, reduced price, ideal for non-urgent bulk work
```

---

### 5B — Using a Batch API

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If you need a response back within two seconds for a live user-facing feature, is a batch API the right tool?" Answer: no — batch processing trades immediacy for cost efficiency, and is designed for workloads where a delay of minutes to hours is acceptable. Choosing batch vs real-time is a decision about the nature of the workload, not just about cost.

**Demo 5 — Structuring batch requests (whiteboard-friendly)**

```python
import json

# Each line represents one independent request in the batch
batch_requests = [
    {
        "custom_id": f"product-{i}",
        "method": "POST",
        "url": "/v1/chat/completions",
        "body": {
            "model": "gpt-4o-mini",
            "messages": [
                {"role": "system", "content": "Write a one-sentence product description."},
                {"role": "user", "content": product_name}
            ]
        }
    }
    for i, product_name in enumerate(product_names)
]

with open("batch_input.jsonl", "w") as f:
    for request in batch_requests:
        f.write(json.dumps(request) + "\n")

# Submitted separately via the provider's batch upload and processing endpoints
```

**[Script:]**

"Each entry is one independent request, tagged with a `custom_id` so you can match each result back to the product it belongs to once processing completes. These are written to a `.jsonl` file — one JSON object per line — the format most batch APIs expect for bulk submission.

The actual submission and result retrieval happens through separate API calls specific to the provider's batch endpoints, which vary by provider and are worth checking in current documentation. The core idea to take away is the shape: collect many independent requests, submit them together, retrieve results once processing finishes, rather than looping through them one at a time in your own code."

> 🎯 **Instructor Note:** Ask: "Why does batching many independent requests together let the provider offer a cost discount, compared to the same number of individual real-time requests?" Answer: batch processing lets the provider schedule the work flexibly across their infrastructure, filling capacity that would otherwise sit idle between urgent real-time requests, rather than guaranteeing immediate dedicated processing for each request. That flexibility is what funds the discount.

**Recap of Block 5 before moving on:**

- Batch processing suits non-urgent, high-volume workloads — bulk classification, summarization, content generation — where immediate response is not required
- Batch APIs process many requests asynchronously, typically at a significant cost discount compared to real-time processing
- Requests are submitted together, often as a `.jsonl` file with a `custom_id` per entry, and results are retrieved once processing completes
- Choosing batch versus real-time is a decision about whether the workload tolerates delay, not purely about cost

---

## Block 6 — Model Selection Strategies

### 6A — Why the Most Powerful Model Is Not Always the Right Choice

**[Script:]**

"Every provider offers multiple models at different price and capability points — a large, expensive, highly capable model, and smaller, cheaper, faster models. The instinct is often to default to the most powerful model everywhere, since it produces the best results. But 'best results' is not the only variable that matters, and many tasks do not need the most powerful model to produce a perfectly acceptable result."

> 🎯 **Instructor Note:** Ask: "For a simple sentiment classification task — positive, negative, or neutral — would you expect a noticeably better result from the most expensive available model compared to a smaller, cheaper one?" Answer: usually not significantly — simple, well-defined classification tasks are exactly the kind of common pattern that smaller models handle reliably, since the underlying task does not require deep reasoning.

---

### 6B — Matching Model to Task

**[Script:]**

"A practical strategy: match model capability to task complexity, not to the most impressive available option by default."

> 🎯 **Instructor Note:** Write this decision framework on the board.

```
Model selection guide:
Simple, well-defined tasks (classification, extraction, short answers)
  → smaller, cheaper, faster model

Complex reasoning, nuanced writing, ambiguous or high-stakes tasks
  → larger, more capable model

Rule of thumb: start with the cheapest model that could plausibly work,
test it against real examples, upgrade only if quality is insufficient.
```

**[Script:]**

"This connects directly to chain-of-thought from the prompting techniques session — sometimes the right optimization is not a bigger model but a better-structured prompt on a smaller one. Before reaching for a more expensive model, ask whether the actual problem is model capability, or whether it is prompt quality, missing context, or an unclear task definition that a better prompt would fix regardless of model size."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If a smaller model gives inconsistent results on a classification task, what should you try before assuming you need a bigger model?" Answer: first try few-shot examples to clarify the exact classification criteria, and structured output mode to constrain the response format — both from earlier sessions. Only after those fail to close the gap does upgrading the model make sense; jumping straight to a bigger model can mask a fixable prompting problem and cost significantly more going forward.

**[Script:]**

"A common real-world pattern is a tiered approach within a single application: use a small, fast model for an initial pass — classifying which category a request falls into, for instance — and route only the requests that genuinely need deeper reasoning to a larger, more expensive model. This way you pay the higher cost only where it actually earns its value."

**Recap of Block 6 before moving on:**

- The most powerful available model is not automatically the right choice; task complexity should drive model selection, not habit
- Simple, well-defined tasks like classification and extraction are usually handled well by smaller, cheaper models
- Before upgrading to a larger model to fix quality issues, check whether better prompting, examples, or structured output would close the gap first
- A tiered strategy — small model for routing or simple cases, large model only where needed — captures cost savings without sacrificing quality where it matters

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "Why do output tokens typically cost more than input tokens? What is the difference between optimization and just deleting words? What should never be cached? When does batch processing make sense over real-time? What should you try before upgrading to a bigger model?"

**Token Counting and Cost Calculation**

- One token is roughly four characters of English text as an estimate; exact counts require the provider's actual tokenizer
- Input and output tokens are priced separately, with output typically costing more due to sequential generation
- Logging `response.usage` across real requests is how actual cost trends are tracked in production

**Prompt Optimization Techniques**

- Every unnecessary token costs money and adds latency; optimization removes tokens that do not improve the result
- Trim verbose phrasing, avoid redundant instructions, keep system prompts concise, summarize long context
- Chain-of-thought and well-chosen examples are token costs that are often worth paying for; optimization is not blind minimization

**Caching Strategies**

- Application-level caching stores full responses keyed by input, avoiding redundant identical calls
- Cache static or FAQ-style content; never cache real-time or personalized data
- Provider-side prompt caching reduces cost automatically for repeated portions of a prompt, such as growing conversation history

**Batch Processing**

- Batch APIs suit non-urgent, high-volume workloads at a significant cost discount compared to real-time processing
- Requests are submitted together and results retrieved once processing completes, trading immediacy for cost efficiency
- The decision to batch is about whether a workload tolerates delay, not purely about cost

**Model Selection Strategies**

- Match model capability to task complexity rather than defaulting to the most powerful available model
- Simple, well-defined tasks are usually handled well by smaller, cheaper models
- Try better prompting, examples, and structured output before upgrading model size to fix a quality problem
- A tiered strategy routes simple cases to cheap models and reserves expensive models for tasks that genuinely need them

**Why All of This Matters Together**

- Token counting tells you what you are actually paying for; prompt optimization and model selection reduce what each request costs; caching and batching reduce how many full-price requests you make at all — together these five techniques are what separates an AI feature that works in a demo from one that remains affordable and fast once it is actually used at real production volume, and applying them is not a one-time setup but an ongoing discipline as usage grows

---

*End of script.*