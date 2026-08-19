# Lecture Script: Masterclass — RAG Pipeline
**Duration:** 110 minutes | **Tools:** Whiteboard, one public doc excerpt, chat UI for contrast | **Tone:** Professor / mentor masterclass

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 10 min | Hallucinated handbook |
| Why Does This Matter? | 15 min | Private, fresh, legal |
| What Is the Concept? | 25 min | RAG, embeddings, store |
| How Do We Apply It? (LOs) | 48 min | Pipeline + one quality check |
| Seminar | 7 min | Design a tiny campus RAG |
| Recap | 5 min | Capstone uses judgement |

---

## Session Opening (10 min)

**[Script:]** "An LLM is a brilliant student with a dated public library in its head. It has **not** read your institute circular from this morning. If we ask it anyway, it will **sound sure** and still be wrong. **Retrieval-Augmented Generation** is the academic name for an open-book method: **find relevant passages, then generate**. Today we stay at ideas. We will **not** implement embedding libraries in depth. We will leave you able to **explain and outline**."

**Real-world hook:** Ask the chat UI a question about a fake internal policy not in the prompt. Watch a confident invention. "That failure is the syllabus."

🎯 **Instructor Note:** Professor pacing. Ban pip-installing vector DBs in class unless a tiny optional demo is already prepared. Whiteboard is the product.

---

## Why Does This Matter?

🎯 **Instructor Note:** Contrast “paste the whole PDF” vs “retrieve two paragraphs.” Point at the context window from the prompt session.

**[Script:]** "Enterprises in **healthcare**, **fintech**, and **education** cannot dump all records into a prompt. **Notion**, **Glean**, and **Google** Cloud search-AI products are RAG-shaped. If you join an AI team as a junior, your first ticket is often **chunking and retrieval quality**, not training a foundation model. If you join a product team, you must know **when RAG is the right pattern** versus a simple LLM wrapper you already built."

**Pain if misunderstood:**
- Treating RAG as magic memory
- Skipping retrieval checks and blaming the model
- Building RAG when a SQL GET would do (your tasks list is a database, not a vector problem)

> **In the Real World:** **Air Canada**’s chatbot case made news when the assistant invented a policy. Retrieval of the **official** policy is a product and legal issue, not a toy.

---

## What Is the Concept?

**RAG:** retrieve then generate, with your sources in the prompt.

**Embedding:** meaning as a vector — nearest neighbours ≈ related text.

**Vector store:** similarity search over those vectors.

**Mental model:** search engine + LLM, not LLM alone.

**Python vs JS:** both just call embedding and chat APIs later. No deep library tour.

**Common mistake:** “We stored vectors, so the model is trained on our data.” Storage is not training.

---

## How Do We Apply It?

### LO 1: Explain why RAG is needed when LLMs lack fresh or private context

**Cases:** new fee circular; employee handbook; user notes in Neon that should **not** all be pasted.

**Predict:** Does RAG update the model weights? (No.)

**Seminar question:** When is a FastAPI GET enough instead of RAG? (Structured rows you already query.)

---

### LO 2: Describe embeddings in plain words

**Whiteboard:** two refund sentences close; a recipe sentence far.

**[Script:]** "You may later call an embeddings API. Today you must only own the metaphor. No dimensionality lectures."

**Predict:** If we embed ‘bank’ as money vs river, why might retrieval fail? (Ambiguity — quality is not automatic.)

---

### LO 3: Explain what a vector store is used for

**Use:** nearest-chunk lookup at question time.

**Not use:** replacing your entire Postgres CRUD app.

**Predict:** Do we still need the LLM? (Yes — store retrieves; model writes the answer.)

---

### LO 4: Outline a minimal RAG pipeline end to end

Live six boxes: Load → Embed → Store → Retrieve → Prompt → Answer.

Walk one campus example: “hostel mess refund.”

**Predict before filling boxes:** Where does the user question enter? (Retrieve and Prompt.)

> **In the Real World:** **AWS** and **OpenAI** reference architectures draw the same boxes. You can now read those diagrams without panic.

---

### LO 5: Discuss one basic retrieval quality check at a high level

**Check:** gold question → must retrieve the known correct chunk in top-k.

If fail, do not tune the poem-like prompt first. Fix chunks, query, or k.

**Predict:** Perfect prose with the wrong chunk — pass or fail? (Fail the retrieval check.)

---

## Seminar (7 min)

Groups outline RAG vs “just SQL” vs “just LLM” for three prompts you provide (policy PDF, task list, world capital).

---

## Recap (5 min)

**[Script:]** "Capstone is not ‘add RAG because it is fashionable.’ It is **choose the pattern that fits the spec**. You now have names for that choice."

---

## Lecture Summary

- **RAG** supplies **fresh or private** passages an LLM would otherwise lack or invent
- **Embeddings** turn meaning into vectors so similar text can be found
- A **vector store** is for **similarity retrieval**, not for all app data
- A **minimal pipeline** is load, embed, store, retrieve, prompt, answer
- One **retrieval quality check** asks whether the right chunk was fetched
- **Practical value:** You can discuss enterprise AI patterns honestly, without fake implementation depth
