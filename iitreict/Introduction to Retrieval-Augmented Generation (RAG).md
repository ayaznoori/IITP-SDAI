# Lecture Script: Introduction to Retrieval-Augmented Generation (RAG)
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | RAG Foundations | 25 min |
| 3 | Vector Search | 25 min |
| 4 | Knowledge Grounding | 25 min |
| 5 | Accuracy Improvement | 20 min |
| 6 | Lecture Summary and Recap | 7 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience already understands embeddings and cosine similarity from the advanced AI integration session, and understands that LLMs learn statistical patterns from training data, which can lead to hallucination, from the LLM fundamentals session. This lecture connects those two facts into a solution. Open with the concrete limitation that RAG exists to fix. Wait after the opening question.

**[Script:]**

"You know that an LLM's knowledge comes entirely from its training data, frozen at whatever point that training ended. Ask it about your company's internal documentation, a product that launched last week, or a specific customer's account history, and it has no way to know the answer — because it was never trained on that information. It may confidently tell you something anyway. That confident wrongness is hallucination, and you already know why it happens: the model is predicting statistically likely text, not consulting a source of truth.

You could try to solve this by cramming the relevant documents directly into the prompt, every single time. You have already seen why that breaks down — from the context engineering session, more context means more tokens, more cost, and more latency. And there is a hard ceiling: you cannot fit an entire product manual, an entire codebase, or an entire company's documentation into a single prompt's context window.

Retrieval-Augmented Generation — RAG — is the standard solution to exactly this problem. Instead of hoping the model already knows the answer, or dumping everything you have into every request, you search for the specific pieces of information relevant to the current question, and include only those in the prompt. The model then generates its answer grounded in that retrieved information, not just in whatever patterns it happened to memorize during training.

Today covers the foundations of how RAG works, how vector search finds the relevant pieces of information in the first place, what it actually means to ground a model's output in real data, and why this measurably improves accuracy compared to relying on the model alone. This is the technique behind nearly every 'chat with your documents' or 'ask questions about your data' feature you have encountered, and by the end of today you will understand exactly how those features work under the hood."

---

## Block 2 — RAG Foundations

### 2A — The Two-Phase Structure of RAG

**[Script:]**

"RAG stands for Retrieval-Augmented Generation, and the name describes the process directly: generation — the model producing a response — is augmented, or enhanced, by retrieval — first searching for and pulling in relevant information. It is a combination of two separate capabilities you already understand individually: search, and text generation.

The overall system has two distinct phases, and it is worth being precise about which phase does what."

> 🎯 **Instructor Note:** Write these two phases on the board and keep them visible through the entire session — every later block maps to one of these two phases.

```
Phase 1 — INDEXING (done ahead of time, once, or periodically)
  Take your documents → break into chunks → generate embeddings 
  for each chunk → store in a searchable database

Phase 2 — QUERY TIME (done for every user question)
  User asks a question → embed the question → search for the 
  most similar stored chunks → include those chunks in the prompt 
  → generate an answer grounded in that retrieved content
```

**[Script:]**

"Indexing happens ahead of time — you prepare your knowledge base once, or update it periodically as documents change, completely separate from any specific user question. Query time happens live, every time a user asks something — and it is fast, because the expensive work of embedding and organizing your documents already happened during indexing."

> 🎯 **Instructor Note:** Ask: "Why does it matter that indexing happens ahead of time, separately from query time?" Answer: embedding an entire document collection can be slow and costly if it involves thousands of documents — doing that work once, in advance, means each individual user query only requires embedding the short question itself, which is fast, rather than re-processing the entire knowledge base on every request.

---

### 2B — Walking Through a Complete RAG Request

**[Script:]**

"Let us trace a concrete example end to end, to see how these two phases connect. Imagine a company knowledge base with internal documentation, and an employee asks a question about it."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If a user asks 'what is our refund policy for annual subscriptions,' what do you think happens first — does the system generate an answer directly, or does something happen before generation?" Guide toward: retrieval happens first — the system needs to find the relevant policy document before it can generate an accurate answer about it.

**Demo 1 — Tracing a RAG request end to end (whiteboard-friendly)**

```
User question: "What is our refund policy for annual subscriptions?"

Step 1 — Embed the question
  question_embedding = generate_embedding(user_question)

Step 2 — Search the indexed knowledge base
  relevant_chunks = vector_search(question_embedding, top_k=3)
  → Returns the 3 most similar stored chunks, e.g.:
    - "Annual subscriptions are refundable within 30 days..."
    - "Refund requests must be submitted through the billing portal..."
    - "Partial refunds are not offered after the 30-day window..."

Step 3 — Build an augmented prompt
  prompt = f"""
  Answer the question using only the following context.
  
  Context:
  {relevant_chunks}
  
  Question: {user_question}
  """

Step 4 — Generate the answer
  response = call_llm(prompt)
  → "Annual subscriptions can be refunded within 30 days of 
     purchase. Requests must be submitted through the billing 
     portal, and partial refunds are not available after that 
     30-day window."
```

**[Script:]**

"Notice the final answer did not come from the model's training data at all — it came from the three retrieved chunks, which the model then synthesized into a coherent, direct answer to the specific question asked. If your refund policy changes tomorrow, you update the underlying document and re-index it; you do not need to retrain or fine-tune the model itself. This is one of RAG's most practical advantages: your knowledge base can be current and specific to your organization, while the underlying model stays exactly the same."

> 🎯 **Instructor Note:** Emphasize this point directly, since it is a common point of confusion: "RAG does not change what the model knows internally, and it does not retrain the model. It changes what information the model has available in front of it for this one specific request — every single query is answered fresh, using whatever is retrieved at that moment."

**Recap of Block 2 before moving on:**

- RAG combines retrieval — search — with generation — producing text — so the model's output is grounded in retrieved information rather than relying solely on training data
- Indexing happens ahead of time: documents are chunked, embedded, and stored; this is separate from any specific user question
- Query time happens live for each question: the question is embedded, relevant chunks are retrieved, and those chunks are included in the prompt before generation
- RAG does not retrain or fine-tune the model; it changes what information is available in the prompt for each specific request

---

## Block 3 — Vector Search

### 3A — Vector Search as the Retrieval Engine

**[Script:]**

"Vector search is the specific mechanism that powers the retrieval phase, and you already know its foundation from the embeddings session: convert text into a vector, and measure similarity between vectors using cosine similarity. RAG is really this same technique, applied specifically to the problem of finding relevant document chunks for a given question.

The difference in this context is scale. In the embeddings session, we compared a handful of pieces of text by hand. A real RAG system might have a knowledge base with tens of thousands, or millions, of chunks. Comparing a query's embedding against every single one, one at a time, in a loop, does not scale — this is where a vector database becomes necessary."

---

### 3B — Chunking Documents Before Embedding

**[Script:]**

"Before anything can be embedded, documents need to be broken into chunks — smaller pieces, typically a few hundred words each, rather than embedding an entire lengthy document as one single vector.

This matters for two reasons. First, embeddings work best on focused, coherent pieces of text — a single vector representing an entire fifty-page manual would blur together many unrelated topics, making it far less useful for finding the one specific relevant section. Second, when a chunk is retrieved, it is what actually gets inserted into the prompt — you want that chunk to be an appropriately sized, self-contained piece of relevant information, not an entire document."

> 🎯 **Instructor Note:** Draw this chunking process on the board.

```
Original document (long):
"Section 1: Company Overview... [500 words]
 Section 2: Refund Policy... [300 words]
 Section 3: Shipping Policy... [400 words]
 Section 4: Contact Information... [100 words]"

After chunking (each chunk embedded separately):
Chunk 1: "Section 1: Company Overview..." → embedding A
Chunk 2: "Section 2: Refund Policy..."    → embedding B
Chunk 3: "Section 3: Shipping Policy..."  → embedding C
Chunk 4: "Section 4: Contact Information" → embedding D

A question about refunds should match closely with embedding B 
specifically, not the whole document as one blurred-together vector.
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If chunks are too large — say, an entire fifty-page document as one chunk — what problem does that cause at retrieval time, even if the embedding technically still works?" Answer: the retrieved chunk would be far too large to usefully fit in the prompt without wasting a huge number of tokens on irrelevant content, and the single blurred embedding representing the whole document would be less precise at matching specifically relevant questions. Chunk size is a real design decision, not an afterthought.

**[Script:]**

"Chunk size is a genuine tradeoff. Chunks too large blur together multiple topics and waste tokens when retrieved. Chunks too small lose context — a chunk might contain a sentence that only makes sense alongside the paragraph around it, which got separated into a different chunk. Common practice is chunking by natural boundaries where possible — paragraphs, sections — combined with a target size range, often a few hundred words per chunk, sometimes with slight overlap between adjacent chunks so context is not abruptly cut off at a boundary."

---

### 3C — Vector Databases

**[Script:]**

"A vector database is a specialized data store built specifically for fast similarity search across large numbers of vectors — this was mentioned briefly in the embeddings session, and now we look at why it matters concretely for RAG.

The core problem it solves: comparing a query embedding against a million stored embeddings, one at a time, in a simple loop, is far too slow for a live user-facing request. Vector databases use specialized indexing structures internally that make finding the closest matches dramatically faster than a brute-force comparison, without you needing to understand the internal indexing algorithm to use one effectively."

**Demo 2 — Storing and querying a vector database (conceptual, whiteboard-friendly)**

```python
# Indexing phase — done once, ahead of time
for chunk in document_chunks:
    embedding = generate_embedding(chunk.text)
    vector_db.insert(id=chunk.id, embedding=embedding, text=chunk.text)

# Query time — done for every user question
question_embedding = generate_embedding(user_question)
results = vector_db.search(query_embedding=question_embedding, top_k=3)

for result in results:
    print(result.text, result.similarity_score)
```

**[Script:]**

"`vector_db.insert` happens during indexing, once per chunk, storing both the embedding and the original text together, since you need the actual text back at query time, not just the vector. `vector_db.search` happens at query time, taking the question's embedding and returning the `top_k` most similar stored entries, ranked by similarity score.

This is the same conceptual operation as the manual cosine similarity loop from the embeddings session — find the closest vectors — just implemented with infrastructure built to do that efficiently at real scale. Popular vector database options include specialized products built for exactly this purpose, as well as vector search features added to databases you may already be familiar with."

> 🎯 **Instructor Note:** Ask: "Conceptually, what is the vector database actually doing that is different from the manual loop-based cosine similarity comparison from the embeddings session?" Answer: nothing conceptually different — it is finding the closest vectors by similarity, exactly as before. The difference is purely about doing that efficiently at scale, using indexing structures that avoid comparing against every single stored vector one by one.

**Recap of Block 3 before moving on:**

- Vector search is the embeddings-and-cosine-similarity technique from the embeddings session, applied to finding relevant document chunks at scale
- Documents are broken into chunks before embedding, since a single vector cannot usefully represent an entire lengthy document
- Chunk size is a real tradeoff — too large blurs topics together and wastes tokens; too small loses surrounding context
- A vector database stores embeddings alongside their original text and enables fast similarity search across large collections, avoiding a slow one-by-one comparison loop

---

## Block 4 — Knowledge Grounding

### 4A — What "Grounding" Actually Means

**[Script:]**

"Grounding means constraining the model's generated response to be based on specific, provided information, rather than letting it rely purely on whatever patterns it learned during training. This is the actual mechanism by which RAG reduces hallucination — not by making the model inherently smarter or more careful, but by giving it real source material to work from and instructing it to use that material specifically.

The retrieval phase finds the relevant chunks. Grounding is what happens next: how those chunks are actually used to shape the generated answer, through prompt structure and explicit instruction."

---

### 4B — Prompting for Grounded Generation

**[Script:]**

"Simply including retrieved chunks in the prompt is necessary, but not sufficient on its own. The prompt must also explicitly instruct the model to rely on that provided context, and — critically — what to do when the context does not actually contain the answer. Without this instruction, the model may still fall back on its own training-data patterns, defeating the purpose of retrieval in the first place."

> 🎯 **Instructor Note:** This connects directly to the prompt engineering session's structure — context, instruction, constraints — now applied specifically to grounding. Write the two prompt versions on the board side by side.

```
UNGROUNDED PROMPT (retrieval happened, but grounding wasn't enforced):
"Here is some context: {retrieved_chunks}
 Answer this question: {user_question}"

Risk: the model may blend the provided context with its own 
      training-data assumptions, or answer confidently even if 
      the context does not actually contain the answer.

GROUNDED PROMPT:
"Answer the question using ONLY the information in the context 
below. If the context does not contain enough information to 
answer the question, say so explicitly rather than guessing.

Context:
{retrieved_chunks}

Question: {user_question}"
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If the retrieved chunks do not actually contain the answer to the user's question — retrieval found the closest available chunks, but none of them are truly relevant — what should a well-grounded prompt cause the model to do?" Answer: explicitly state that the available information does not answer the question, rather than generating a plausible-sounding but ungrounded guess. This is the entire point of the explicit fallback instruction in the grounded prompt.

**Demo 3 — Grounded response with an explicit fallback (whiteboard-friendly)**

```
Retrieved chunks (about refund policy — closest matches found, 
but the user actually asked something unrelated):
"Annual subscriptions are refundable within 30 days..."
"Refund requests must be submitted through the billing portal..."

User question: "Do you offer student discounts?"

Grounded response:
"The provided context does not contain information about 
student discounts. I can only answer based on the refund 
policy information available to me."
```

**[Script:]**

"This is exactly the behavior you want from a grounded system — an honest 'I don't know, based on what I have access to' instead of a confident, fabricated answer about student discounts that happens to sound plausible. This connects directly to the feedback loop discussion from the agentic systems session: an honest failure to answer is far more valuable and trustworthy than a silent, confident wrong answer."

> 🎯 **Instructor Note:** Reinforce this connection explicitly: "This is the same principle as honest failure reporting in an agent's feedback loop — a RAG system that says 'the available information doesn't cover this' is doing its job correctly, even though it did not produce a direct answer. That refusal to guess is the actual value grounding provides."

**Recap of Block 4 before moving on:**

- Grounding means constraining generation to rely on specific, retrieved information rather than the model's own training-data patterns
- Retrieval alone is not sufficient — the prompt must explicitly instruct the model to use only the provided context
- A well-grounded prompt includes an explicit fallback instruction for when the retrieved context does not actually answer the question
- An honest "the available information does not cover this" is the correct, desired outcome of a well-grounded system, not a failure of it

---

## Block 5 — Accuracy Improvement

### 5A — Why RAG Measurably Improves Accuracy

**[Script:]**

"Put together, retrieval and grounding directly address the two root causes of poor accuracy you already understand from earlier sessions: outdated or missing knowledge, and hallucination from relying purely on statistical training patterns.

RAG improves accuracy in two distinct, measurable ways. First, it gives the model access to information it was never trained on at all — private company data, real-time information, anything created after the model's training cutoff. Second, even for information the model might technically know from training, grounding it in an authoritative retrieved source reduces the chance of it confidently stating a subtly wrong version of that information from memory."

> 🎯 **Instructor Note:** Write these two distinct accuracy benefits on the board — they are often conflated, but are genuinely separate mechanisms.

```
Accuracy benefit 1 — NEW KNOWLEDGE
  The model answers correctly about information it was never 
  trained on at all, because retrieval supplies it directly.

Accuracy benefit 2 — REDUCED HALLUCINATION ON KNOWN TOPICS
  Even for topics the model has some training knowledge of, 
  grounding in a specific authoritative source reduces the 
  chance of confidently stating an incorrect detail from memory.
```

---

### 5B — RAG Is Not a Complete Guarantee

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If the retrieval step returns chunks that are only loosely related to the actual question — not truly relevant, just the closest available matches — can grounding still guarantee an accurate answer?" Answer: no — grounding constrains the model to use the provided context, but if that context itself is poor or irrelevant, the model can still produce an inaccurate answer, or ideally should say the context is insufficient, as covered in Block 4. RAG's accuracy improvement depends entirely on retrieval quality.

**[Script:]**

"This is the critical limitation to understand: RAG's accuracy benefit is only as good as the retrieval step underneath it. If your chunking strategy is poor, if your knowledge base is outdated or incomplete, or if the vector search genuinely fails to find the truly relevant chunk for a given question, grounding cannot manufacture accuracy out of nothing — it can only faithfully use whatever was actually retrieved.

This means improving a RAG system's accuracy in practice is often less about the generation step, and much more about improving retrieval quality: better chunking, keeping the knowledge base current, and evaluating whether the right chunks are actually being retrieved for real user questions."

> 🎯 **Instructor Note:** Ask a synthesis question: "If a RAG-powered support bot starts giving wrong answers about a policy that changed last week, where would you look first to diagnose the problem — the prompt, the model, or something else?" Guide toward: check whether the knowledge base was actually re-indexed with the updated policy document. If the underlying document was updated but never re-embedded and re-indexed, retrieval is still surfacing the old, outdated chunk — the model is being faithfully grounded in stale information, which looks like a model problem but is actually a retrieval and indexing problem.

**[Script:]**

"This is a genuinely important operational point: RAG systems require maintenance. An indexed knowledge base is a snapshot, and it goes stale exactly like any other cached data the moment the underlying source documents change without a corresponding re-index."

**Recap of Block 5 before moving on:**

- RAG improves accuracy in two distinct ways: supplying entirely new knowledge the model was never trained on, and reducing hallucination on topics the model partially knows by grounding it in an authoritative source
- Grounding is only as accurate as the underlying retrieval — poor chunking, an outdated knowledge base, or weak retrieval results limit how accurate the final answer can be
- Improving a RAG system in practice is often more about improving retrieval quality than adjusting the generation prompt
- A knowledge base is a snapshot that requires ongoing maintenance — re-indexing whenever underlying source documents change

---

## Block 6 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What are the two phases of RAG, and what happens in each? Why does chunking matter before embedding a document? What does grounding actually require beyond just including retrieved context in the prompt? What are the two distinct ways RAG improves accuracy, and what is its core limitation?"

**RAG Foundations**

- RAG combines retrieval (search) with generation (text production), grounding the model's output in retrieved information rather than relying solely on training data
- Indexing happens ahead of time — chunking, embedding, and storing documents; query time happens live for every user question — embedding the question, retrieving relevant chunks, and generating a grounded answer
- RAG does not retrain or fine-tune the model; it changes what information is available in the prompt for each specific request

**Vector Search**

- Vector search applies the embeddings and cosine similarity technique from the embeddings session to finding relevant chunks at scale
- Documents are chunked before embedding, since a single vector cannot usefully represent an entire lengthy document; chunk size is a real tradeoff between context loss and topic blur
- A vector database stores embeddings with their original text and enables fast similarity search across large collections without a slow one-by-one comparison loop

**Knowledge Grounding**

- Grounding constrains generation to rely on retrieved information rather than the model's own training-data patterns
- Simply including retrieved context is not enough — the prompt must explicitly instruct the model to rely on it, with an explicit fallback for when the context is insufficient
- An honest "the available information does not cover this" is the correct outcome of a well-grounded system, not a failure of it

**Accuracy Improvement**

- RAG improves accuracy by supplying new knowledge the model was never trained on, and by reducing hallucination on partially-known topics through grounding in an authoritative source
- Grounding is only as accurate as the underlying retrieval; poor chunking or an outdated knowledge base limits how accurate answers can be
- A knowledge base requires ongoing maintenance — re-indexing whenever the underlying source documents change, since it is otherwise a stale snapshot

**Why All of This Matters Together**

- RAG is not a single new technique — it is embeddings and vector similarity from the embeddings session, prompt structure and grounding instructions from the prompt engineering session, and the same honesty-over-false-confidence principle from the agentic systems feedback loop discussion, all combined to solve one specific, common problem: giving a language model accurate, current, and organization-specific knowledge it could never have learned during training; understanding RAG as this composition of known parts, rather than as an unfamiliar new system, is what lets you actually build, diagnose, and improve one

---

*End of script.*