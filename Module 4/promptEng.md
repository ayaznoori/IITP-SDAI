# Lecture Script: Prompt Engineering
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | Prompt Engineering Fundamentals | 20 min |
| 3 | Prompting Techniques — Zero-Shot, Few-Shot, Chain-of-Thought | 25 min |
| 4 | Role Prompting — Role-Playing and Personas | 15 min |
| 5 | Structured Prompting — Structured Output Prompting | 20 min |
| 6 | Context Engineering — Managing and Optimizing Context | 15 min |
| 7 | Lecture Summary and Recap | 5 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience just completed the LLM fundamentals lecture and understands how transformers predict next words. The hook should address the gap between "the model works" and "the model gives me the output I need." Open with a concrete example of two prompts producing drastically different results from the same model. Wait after the opening question.

**[Script:]**

"You understand how LLMs work now. A transformer, predicting the next word, iteratively generating text based on statistical patterns learned during training. That understanding is the foundation. But knowing how the engine works does not make you a driver.

Send the same model two different prompts and you get two completely different outputs. Not because the model changed. Because the prompt shapes which statistical patterns the model activates. A poorly written prompt sends the model down a path of common but useless predictions. A well-structured prompt guides it toward the exact output you need.

This is not magic. It is engineering — deliberate choices about how you structure the prompt, what examples you provide, what role you assign the model, and how you set up the context. The same model with a two-word prompt produces incoherent rambling. With a ten-sentence prompt designed using the techniques in this session, the same model produces professional, precise output.

The gap between 'good enough' and 'production-ready' is prompt engineering. Cost is also affected — a poorly structured prompt makes the model generate more tokens to accomplish less, driving up API costs. A well-engineered prompt accomplishes the same task in fewer tokens.

Today covers the five fundamental techniques that turn vague requests into precise instructions that models actually follow: how to structure a prompt fundamentally, how to use examples and reasoning to guide the model, how to assign a role to shape response style, how to demand structured output, and how to manage context so the model has what it needs without wasting tokens on irrelevant information.

By the end, you will write prompts not by trial and error, but by understanding what each component does and why."

---

## Block 2 — Prompt Engineering Fundamentals

### 2A — The Anatomy of a Prompt

**[Script:]**

"A prompt has multiple parts, each serving a purpose. Understanding the parts lets you build prompts deliberately instead of writing everything in one block and hoping the model understands.

The standard structure has five components."

> 🎯 **Instructor Note:** Write these on the board as a reference structure learners can apply to every prompt they build.

```
1. CONTEXT        — background information the model needs
2. INSTRUCTION    — what the model should do
3. EXAMPLES       — (optional) demonstrations of desired behavior
4. OUTPUT FORMAT  — structure or constraints on the response
5. CONSTRAINTS    — tone, length, style, boundaries
```

**[Script:]**

"Context is what the model needs to know. It might be a document you want summarized, a conversation history, or background facts about a domain. Without context, the model has no ground truth to work from.

Instruction is the explicit ask — 'summarize this', 'classify this', 'generate names for this'. Be specific. 'Do something with this text' is not a prompt; 'extract the main argument in one sentence' is.

Examples show the model what you want. These are optional but powerful — one or two examples often clarify what the model should do better than fifty words of explanation.

Output format tells the model what shape the response should take. JSON, a bulleted list, a structured table. Specifying this prevents the model from deciding on its own and producing format you must then parse.

Constraints set boundaries. 'Answer in one sentence', 'use technical terminology', 'sound like a Wikipedia article', 'list three reasons'. Constraints prevent rambling and align the response with your actual needs."

---

### 2B — Building a Prompt Step by Step

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I write 'Summarize this text' without providing the text, what does the model do?" Answer: it likely produces a generic, abstract summary about what summarization is, or asks for the text, because it has no context. This establishes why context comes first.

**Demo 1 — Prompt evolution from weak to strong (whiteboard-friendly)**

```
WEAK PROMPT:
"Classify this."

What is this? Why no context?
Model response will be confused.

BETTER:
"Classify this product review as positive, negative, or neutral."

Now there is an instruction. But there is still no example.
Model might classify randomly.

STRONG:
"Classify the following product review as positive, negative, or neutral.

Review: 'The coffee maker broke after one week. Total waste of money.'

Classification: "

The model now has clear instruction, an example, and knows the format.
It will classify the next review correctly.

STRONGER:
"You are a customer service analyst. Classify product reviews as positive, 
negative, or neutral. Respond only with the classification, no explanation.

Review: 'The coffee maker broke after one week. Total waste of money.'

Classification: "

Now there is a role, explicit constraint (no explanation), and format.
```

**[Script:]**

"Watch what happens as we add components. The first prompt is almost unanswerable because there is no context. The second has instruction but no ground truth. The third adds an example. The fourth adds a role and a constraint.

Each addition reduces ambiguity. The model is now constrained to a path that leads directly to what you need. No wasted tokens on explanation when you did not ask for it. No hedging or caveats when you do not need them.

The weak prompts are not the model being dumb. They are the model being confused — too many possible interpretations, so it picks one at random. The stronger prompts disambiguate."

> 🎯 **Instructor Note:** Ask the room: "Which of these prompts would you want to use in a production system?" Let them reason through it. The 'STRONGER' one is better, but not always needed — if you do not care about explanations, the STRONG prompt is enough. The choice depends on your actual need.

**Recap of Block 2 before moving on:**

- A prompt has five components: context (background information), instruction (what to do), examples (optional demonstrations), output format (shape of response), constraints (tone, length, style, boundaries)
- Context must come first — without it, the model has no ground truth
- Specific instructions are better than vague ones; 'classify this review' is clearer than 'do something with this'
- Examples dramatically improve model behavior by showing what you want instead of just describing it
- Output format prevents the model from choosing its own format and requiring you to parse unexpected responses

---

## Block 3 — Prompting Techniques: Zero-Shot, Few-Shot, Chain-of-Thought

### 3A — Zero-Shot Prompting

**[Script:]**

"Zero-shot prompting is the simplest: you ask the model to do something with no examples. The model relies entirely on patterns it learned during training.

Zero-shot works when the task is common and the instruction is clear. 'Summarize this article', 'translate this to Spanish', 'identify the sentiment' — these are patterns the model has seen thousands of times in training data. It can execute them without an example."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I ask a model to do something it has never seen in training, does zero-shot still work?" Answer: poorly. If I ask it to classify documents using a proprietary internal taxonomy, the model has no pattern to rely on and will guess or generalize wrongly. This is where few-shot becomes necessary.

**Demo 2 — Zero-shot prompting (whiteboard-friendly)**

```
PROMPT:
"Translate the following text to French:
English: 'Good morning, how are you today?'"

RESPONSE (zero-shot, no example):
"French: 'Bonjour, comment allez-vous aujourd'hui?'"

The model succeeds because translation is a common pattern.
No example needed.
```

**[Script:]**

"Zero-shot is efficient — shortest prompt, fastest execution, lowest cost. Use it first. Only move to few-shot if zero-shot is not producing acceptable results."

---

### 3B — Few-Shot Prompting

**[Script:]**

"Few-shot prompting provides one to five examples of the desired behavior. The model learns from these examples what pattern to follow, then applies that pattern to your actual question.

Few-shot is powerful because examples are often clearer than explanation. The model sees the pattern in the examples, learns it, and applies it — even to tasks it has not specifically seen during training."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I provide two examples of a classification task, will the model apply the exact same logic to new examples, or might it still get some wrong?" Answer: it will apply the pattern but can still make mistakes — few-shot is not a guarantee, just a dramatic improvement over zero-shot. The model is still predicting based on training patterns; examples just guide which patterns activate.

**Demo 3 — Few-shot prompting (whiteboard-friendly)**

```
ZERO-SHOT:
"Classify this company name as 'tech' or 'retail':
Company: 'FreshMart'"

Model might guess wrong because the name is ambiguous.

FEW-SHOT:
"Classify company names as 'tech' or 'retail'.

Example 1:
Company: 'CloudVault'
Classification: tech

Example 2:
Company: 'ShopHub'
Classification: retail

Now classify:
Company: 'FreshMart'
Classification: "

With two examples, the model sees the pattern:
- 'Cloud', 'Vault' suggest tech → tech
- 'Shop', 'Hub' suggest retail → retail
- 'Fresh', 'Mart' suggest retail → retail

Result: retail (likely correct)
```

**[Script:]**

"The examples teach the model the classification criterion. 'Cloud' and 'Vault' are tech words. 'Shop' and 'Hub' are retail words. 'Fresh' and 'Mart' are retail. The model learns the pattern and applies it to the new example.

One example is better than zero. Two is better than one. Three to five usually gives diminishing returns — the model has learned the pattern, and more examples do not help much. Too many examples can confuse the pattern or waste tokens."

---

### 3C — Chain-of-Thought Prompting

**[Script:]**

"Chain-of-thought prompting asks the model to show its reasoning step by step before giving an answer. This sounds simple but is incredibly powerful for complex reasoning tasks.

The mechanism: by generating intermediate reasoning steps, the model's attention patterns are guided through a more complex path. Instead of predicting the final answer directly, it predicts reasoning, then predicts more reasoning based on the first step, then predicts the answer. This step-by-step process surfaces better reasoning."

> 🎯 **Instructor Note:** Write this on the board.

```
Without chain-of-thought:
Question → [transformer predicts] → Final Answer (may be wrong)

With chain-of-thought:
Question → [transformer predicts] → Step 1 reasoning
Step 1 → [transformer predicts] → Step 2 reasoning
Step 2 → [transformer predicts] → Final Answer (usually correct)

Breaking it into steps guides the model through better patterns.
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I ask a model a hard math problem and ask it to show its work, why does that help? The model is not actually doing math; it is predicting text." Answer: by predicting reasoning steps, the model activates patterns from training data that include step-by-step solutions. Without explicit intermediate steps, it tries to predict the answer directly, often failing. Step-by-step forces it down a path of more correct patterns.

**Demo 4 — Chain-of-thought prompting (whiteboard-friendly)**

```
WITHOUT CHAIN-OF-THOUGHT:
"If a train travels 60 mph for 2.5 hours, how far does it go?"

Model might predict: "120 miles" (wrong, confuses with time)

WITH CHAIN-OF-THOUGHT:
"If a train travels 60 mph for 2.5 hours, how far does it go?
Let me think step by step:

Step 1: I know the formula is distance = speed × time
Step 2: Speed is 60 mph, time is 2.5 hours
Step 3: 60 × 2.5 = 150
Step 4: So the distance is 150 miles

Answer: 150 miles"

By forcing the model to generate intermediate steps,
it activates patterns of correct reasoning.
```

**[Script:]**

"The 'Let me think step by step' phrase is the prompt telling the model to show its reasoning. You could also ask 'Explain your reasoning' or 'Work through this problem'. The key is asking for intermediate steps, not just the final answer.

Chain-of-thought is especially powerful for: math problems, logic puzzles, code debugging, medical diagnoses, legal reasoning — any task where step-by-step reasoning is involved in human problem-solving.

The cost is higher — showing your work means more tokens in the response. But the accuracy improvement is often worth it."

> 🎯 **Instructor Note:** Ask: "Does chain-of-thought always help, or are there tasks where it does not?" Answer: tasks that are one-step, simple classification, or memorization do not benefit. Chain-of-thought helps when the task requires intermediate reasoning steps.

**Recap of Block 3 before moving on:**

- Zero-shot prompting asks the model to do something with no examples; works best for common tasks where the model has strong training patterns
- Few-shot prompting provides one to five examples; dramatically improves performance for less common tasks or proprietary classifications
- Chain-of-thought prompting asks the model to show its reasoning step by step; improves accuracy for complex reasoning tasks by guiding the model through better patterns
- Each technique trades off tokens and latency for accuracy; choose based on your actual needs

---

## Block 4 — Role Prompting: Role-Playing and Personas

### 4A — Why Role Prompting Works

**[Script:]**

"Role prompting assigns the model a persona or role, shaping how it responds. Instead of asking 'Explain machine learning', you ask 'You are a professor explaining machine learning to a high school student. Explain machine learning.' The role shapes vocabulary, depth, examples, and tone.

The model has seen thousands of examples of how different personas communicate — professors, comedians, lawyers, doctors — during training. By assigning a role, you activate those patterns."

---

### 4B — Common Roles and How They Shape Responses

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I ask the model to explain debugging code as a 'debugging expert versus as a beginner learning to code, what will be different?" Answer: expert explanation uses technical terms, assumes knowledge, gets to the point. Beginner explanation uses simple language, explains concepts, walks through basics. Same task, different persona, different output.

**Demo 5 — Role-based responses (whiteboard-friendly)**

```
TASK: Explain why the website is slow.

ROLE: You are a systems engineer:
"The latency is likely due to N+1 queries in the ORM. 
Profile your database calls; I suspect you're hitting the 
database in a loop instead of batch-loading. Implement 
joinedload or fetch relations eagerly."

ROLE: You are a marketing manager:
"The website feels slow to users, which hurts engagement 
and conversions. We need to prioritize speed improvements. 
Let's run a user survey to understand which pages feel slowest."

ROLE: You are a teacher:
"When websites feel slow, it usually means the server 
is taking too long to respond. This can happen for many reasons: 
too many database queries, slow external API calls, inefficient 
code. To find out which, we use tools called 'profilers' to 
measure where time is spent."

Same problem, three roles, three different explanations.
```

**[Script:]**

"Each role brings its own vocabulary, priorities, and depth. A systems engineer dives into technical causes. A marketing manager focuses on user impact and business metrics. A teacher explains at an accessible level with examples.

The model learned these patterns from training data — it has seen engineers explain technical issues, managers discuss business impact, teachers explain concepts. Assigning a role activates those specific patterns."

> 🎯 **Instructor Note:** Ask: "Is the model actually a systems engineer, or is it just predicting text that sounds like what a systems engineer would say?" Answer: the latter. But the distinction is semantic — the output is what matters. The model is pattern-matching on training examples of how systems engineers communicate and producing text that matches that pattern.

**Recap of Block 4 before moving on:**

- Role prompting assigns the model a persona that shapes its response vocabulary, tone, depth, and priorities
- Common roles include: expert/beginner, teacher/student, technical/non-technical, formal/casual, domain specialist
- The model learned these patterns from training data where different personas communicate differently
- Role prompting is most effective when the role is common in training data — engineer, teacher, doctor, lawyer

---

## Block 5 — Structured Prompting: Structured Output Prompting

### 5A — Why Structure Matters

**[Script:]**

"Unstructured responses are hard to parse and use in applications. If you ask the model for a list of tasks and get back a paragraph, you must parse it to extract individual items. This is error-prone.

Structured prompting asks the model to return output in a specific format — JSON, XML, YAML, a table. By being explicit about the format, you make the output machine-readable, not just human-readable."

---

### 5B — Specifying Structured Output

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I ask for JSON output but do not provide an example of the JSON structure, will the model always return valid JSON?" Answer: usually, but not guaranteed. Some models might return valid JSON, others might deviate slightly. Providing a specific example guarantees the format more reliably than just saying 'return JSON'.

**Demo 6 — Structured output prompting (whiteboard-friendly)**

```
UNSTRUCTURED REQUEST:
"List three ways to improve website performance."

Response:
"First, you can optimize images to reduce file sizes. 
Second, use a CDN to cache content closer to users. 
Third, implement lazy loading for images below the fold."

Hard to parse — is each sentence one item? Where do items start and end?

STRUCTURED REQUEST:
"List three ways to improve website performance. 
Return as JSON with the following structure:

{
  'improvements': [
    {'id': 1, 'technique': '...', 'description': '...'},
    {'id': 2, 'technique': '...', 'description': '...'},
    {'id': 3, 'technique': '...', 'description': '...'}
  ]
}

Response:
{
  'improvements': [
    {'id': 1, 'technique': 'Image Optimization', 
     'description': 'Reduce file sizes through compression'},
    {'id': 2, 'technique': 'CDN Usage', 
     'description': 'Cache content closer to users'},
    {'id': 3, 'technique': 'Lazy Loading', 
     'description': 'Load images only when needed'}
  ]
}

Now the output is machine-readable. You can parse it directly.
```

**[Script:]**

"Providing a template with an example is powerful. The model sees the exact structure you want and mirrors it. This is more reliable than just saying 'return as JSON'.

You can be even more specific with constraints. 'Return a JSON object with exactly three properties: name, description, and priority. Priority must be an integer from 1 to 5.' The model learns the constraints from the description."

> 🎯 **Instructor Note:** Ask: "What happens if the model deviates from the JSON structure you specified?" Answer: your JSON parser will fail. This is why examples are important — they make deviation less likely. But if you need absolute guarantees, some APIs have structured output modes where the model is constrained to return exactly the format you specify (this is a newer feature, worth mentioning as an aside).

**Recap of Block 5 before moving on:**

- Structured prompting asks the model to return output in a specific machine-readable format
- Specifying the format with an example is more reliable than just naming the format
- Common structured formats: JSON, XML, YAML, CSV, tables
- Structured output makes it easy to parse and integrate with downstream code

---

## Block 6 — Context Engineering: Managing and Optimizing Context

### 6A — What Context Is and Why It Matters

**[Script:]**

"Context is everything in a prompt that is not the actual question. It might be a document you want analyzed, a conversation history, relevant facts about the domain, or instructions about how to respond.

Context shapes what the model generates. With good context, the model has ground truth to work from. Without it, the model relies on generic patterns from training data, which may not apply to your specific situation.

But context has a cost: more context means more tokens, which means higher API costs, longer latency, and more GPU memory consumed. The trick is including the right context — enough to get good answers, not so much that you waste tokens."

---

### 6B — Optimizing Context

**[Script:]**

"There are four strategies for optimizing context.

First: Be selective. Include only context that is actually relevant. If you are asking about performance improvements, do not include the entire codebase. Include only the performance-critical functions.

Second: Prioritize. Put the most important context first, in the most visible part of the prompt. If the model reads the first sentence and understands the task, the rest of the context serves as supporting detail.

Third: Summarize. Do not include raw documents. Summarize them. A ten-page report can be reduced to a paragraph that captures the essential points. The model can then use that summary as context instead of the full document.

Fourth: Use retrieval. For large databases of context, do not include everything. Use a retrieval system to fetch only the most relevant pieces. This is the idea behind retrieval-augmented generation — search a database for relevant documents, then include only those in the prompt."

> 🎯 **Instructor Note:** Write these on the board as a checklist learners can apply.

```
Optimizing context:
1. Be selective — include only relevant information
2. Prioritize — put important context first
3. Summarize — condense documents, capture essentials
4. Retrieve — search for relevant pieces instead of including everything
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I include the full source code of a hundred-file project in the prompt and ask the model to debug a specific function, what happens?" Answer: the model wastes tokens reading code irrelevant to the function, making the call more expensive and slower. It would be better to include only the buggy function and any functions it calls.

**Demo 7 — Context optimization (whiteboard-friendly)**

```
UNOPTIMIZED:
"Here is the full codebase (50,000 tokens). 
Debug the search function."

Cost: 50,000+ tokens read by the model
Time: longer latency
Effectiveness: model gets lost in irrelevant code

OPTIMIZED:
"You are a Python debugging expert. 
The search function is below. It is supposed to return 
results matching a query, but returns empty results.

Context: The database has 1000 test records.

[Include only the search function and the database schema it uses]

What is the bug?"

Cost: 500 tokens
Time: faster
Effectiveness: model focuses on the actual problem
```

**[Script:]**

"Optimized context is surgical. You have thought about what the model actually needs and removed everything else. This is harder than just dumping everything in, but it is cheaper and often more effective."

> 🎯 **Instructor Note:** Connect back to the LLM lecture: "Remember, the model has to process every token. More tokens means more computation, which translates to cost and latency. By optimizing context, you are not just being efficient — you are letting the model focus on what matters."

**Recap of Block 6 before moving on:**

- Context is information in the prompt beyond the question; it shapes what the model generates
- More context costs more in latency, tokens, and money
- Optimization strategies: be selective (include only relevant information), prioritize (important context first), summarize (condense documents), retrieve (fetch only relevant pieces instead of including everything)
- For large databases of context, retrieval-augmented generation is the standard approach

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What are the five components of a prompt? When does few-shot work better than zero-shot? Why does chain-of-thought improve accuracy? What does role prompting do? Why is structured output important?"

**Prompt Engineering Fundamentals — Structure and Components of an Effective Prompt**

- A prompt has five components: context (background information), instruction (what to do), examples (optional demonstrations), output format (shape of response), constraints (tone, length, style)
- Context comes first; without it, the model has no ground truth
- Specific instructions are better than vague ones
- Examples clarify desired behavior better than descriptions
- Output format prevents the model from choosing its own format

**Prompting Techniques — Zero-Shot, Few-Shot, and Chain-of-Thought**

- Zero-shot prompting asks the model to do something with no examples; works for common tasks with strong training patterns
- Few-shot prompting provides one to five examples; dramatically improves performance for less common or proprietary tasks
- Chain-of-thought prompting asks for step-by-step reasoning; improves accuracy for complex reasoning by activating better patterns
- Each technique trades tokens and latency for accuracy; choose based on actual needs

**Role Prompting — Role-Playing and Personas to Shape Model Responses**

- Role prompting assigns the model a persona that shapes vocabulary, tone, depth, and priorities
- Common roles include expert/beginner, teacher/student, technical/non-technical, domain specialist
- The model learned communication patterns from training data; assigning a role activates those patterns
- Role prompting is most effective when the role is common in training data

**Structured Prompting — Structured Output Prompting**

- Structured prompting asks the model to return output in a machine-readable format
- Specifying the format with an example is more reliable than just naming the format
- Common formats: JSON, XML, YAML, CSV, tables
- Structured output makes parsing and integration with downstream code reliable

**Context Engineering — Managing and Optimizing Context for Better Model Responses**

- Context is information in the prompt that shapes what the model generates; more context means higher cost and latency
- Optimization strategies: be selective (include only relevant information), prioritize (important context first), summarize (condense documents), retrieve (fetch only relevant pieces)
- For large databases of context, retrieval-augmented generation is the standard approach
- Optimized context is surgical — thoughtful about what the model actually needs

**Why All of This Matters Together**

- The difference between a prompt that works and a prompt that delivers production-ready output is deliberate engineering using these five techniques; structure clarifies what you want, examples demonstrate it, chain-of-thought and role prompting guide the model toward better patterns, structured output makes responses machine-readable, and context engineering ensures the model has what it needs without wasting tokens; understanding and applying these techniques is what separates trial-and-error prompting from intentional architecture that works consistently

---

*End of script.*