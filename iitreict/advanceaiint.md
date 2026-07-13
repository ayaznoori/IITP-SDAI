# Lecture Script: Advanced AI Integration
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 8 min |
| 2 | Structured Output Generation Using Schemas | 25 min |
| 3 | Vision APIs and Image Understanding | 20 min |
| 4 | Embeddings API and Vector Generation | 25 min |
| 5 | Multimodal Integration — Handling Multimodal Inputs and Outputs | 15 min |
| 6 | Implementing Advanced API Features in Backend Applications | 12 min |
| 7 | Lecture Summary and Recap | 5 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has built a working FastAPI route that calls an LLM, understands chat roles, and has a reusable service function pattern. This lecture extends that foundation into more powerful, production-grade capabilities. The hook should surface the limitations of plain text-in-text-out that this audience has already hit or will hit soon. Wait after the opening question.

**[Script:]**

"Your `get_ai_response` function from the last session takes a question and returns text. That covers a huge range of use cases. But it breaks down the moment you need something more specific.

You ask the model to return JSON for your application to parse, and it usually works — but 'usually' is not good enough for a production system. Occasionally the model adds an explanation before the JSON, or a trailing comma breaks your parser, or a field is missing. You need a guarantee, not a best effort.

You want your application to understand an uploaded image — a user submits a photo of a receipt and wants the total extracted, or a product photo and wants a description generated. Plain text input cannot carry an image.

You want to build a search feature that finds semantically similar content — not just keyword matching, but understanding that 'a place to get coffee' and 'a nearby café' mean nearly the same thing. That requires a completely different kind of API call, one that turns text into a mathematical representation rather than generating more text.

And you may want a single feature that combines several of these — a user uploads an image and asks a question about it, and the response needs to be both accurate and structured.

Today covers four capabilities that take your AI integration from a proof of concept to something production-grade: structured output generation with schema guarantees, vision APIs for image understanding, embeddings for semantic search and similarity, and how to combine multiple modalities and features cleanly inside your backend."

---

## Block 2 — Structured Output Generation Using Schemas

### 2A — The Problem with Asking Nicely for JSON

**[Script:]**

"In the last session, you saw structured output prompting — asking the model to return JSON by describing the format you want and showing an example. This works most of the time. But 'most of the time' means your application needs defensive parsing, retry logic, and error handling for the times it does not work.

Modern LLM APIs solve this more reliably with structured output modes — a feature where you provide a formal schema, and the provider constrains the model's output generation itself to match that schema exactly. This is not the model being asked nicely; it is the API enforcing the structure at the generation level."

> 🎯 **Instructor Note:** Draw this distinction clearly on the board — it is the core conceptual point of the block.

```
Structured PROMPTING (previous session):
"Please return JSON like this: {...}"
→ Model tries to comply, usually succeeds, occasionally deviates

Structured OUTPUT mode (this session):
API is given a formal schema
→ Generation itself is constrained to match the schema
→ Deviation is not possible by construction
```

---

### 2B — Defining a Schema with Pydantic

**[Script:]**

"You already know Pydantic models from building FastAPI request and response validation. Structured output APIs use that same tool — you define a Pydantic model describing the shape you want, and pass it to the API. The provider guarantees the response matches it."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I define a Pydantic model with a required integer field called `rating`, and ask the model to analyze a review that does not mention a numeric rating at all, what happens with structured output mode versus plain prompting?" Answer: with plain prompting, the model might omit the field, hallucinate a number, or add explanatory text around it. With structured output mode, the model is constrained to produce a valid integer for that field regardless — it has to commit to a value that fits the schema.

**Demo 1 — Structured output with a schema (whiteboard-friendly)**

```python
from pydantic import BaseModel
from openai import OpenAI
import os

client = OpenAI(api_key=os.environ.get("OPENAI_API_KEY"))

class ReviewAnalysis(BaseModel):
    sentiment: str
    rating: int
    summary: str

response = client.beta.chat.completions.parse(
    model="gpt-4o-mini",
    messages=[
        {"role": "system", "content": "Analyze the product review."},
        {"role": "user", "content": "This blender is amazing, blends anything in seconds!"}
    ],
    response_format=ReviewAnalysis
)

result = response.choices[0].message.parsed
print(result.sentiment, result.rating, result.summary)
```

**[Script:]**

"`ReviewAnalysis` is an ordinary Pydantic model — three fields, no different from any request or response model you have already written. `response_format=ReviewAnalysis` is the new piece: it tells the API to constrain its output to match this exact schema.

`response.choices[0].message.parsed` gives you back an actual instance of `ReviewAnalysis`, already validated, already typed — not a raw string you then have to parse yourself. `result.sentiment`, `result.rating`, `result.summary` are accessed as normal Python attributes."

> 🎯 **Instructor Note:** Emphasize this is a meaningfully different guarantee than before. Say directly: "The difference between this and the JSON prompting from last session is the difference between asking someone to follow a form and physically constraining them so they cannot deviate from it. One is a best effort; the other is structural."

**[Script:]**

"This is especially valuable when the structured output feeds directly into your database, another API call, or business logic that assumes a specific shape. A malformed response in a proof-of-concept demo is an inconvenience. A malformed response silently corrupting production data is a real cost."

**Recap of Block 2 before moving on:**

- Structured output prompting (asking nicely) usually works but is not a guarantee; structured output mode constrains generation itself to match a schema
- A Pydantic model defines the schema, the same tool already used for FastAPI request and response validation
- `response_format=YourModel` passed to the API call enforces the schema at generation time
- The response comes back as a parsed, validated instance of your Pydantic model, not a raw string requiring manual parsing
- This matters most when structured output feeds directly into downstream logic that assumes an exact shape

---

## Block 3 — Vision APIs and Image Understanding

### 3A — How Images Enter a Chat Request

**[Script:]**

"You already know the chat message structure — a list of messages, each with a role and content. Vision-capable models extend this: instead of content being just a string, it can be a list containing both text and image data.

The model does not 'see' pixels the way a human eye does. It processes the image through a separate encoding step that converts visual information into a representation the transformer can attend to, alongside the text tokens in the same request. Conceptually, this is the multimodal extension of the tokenization and embedding ideas from the LLM fundamentals session — images get their own path into the same underlying architecture."

> 🎯 **Instructor Note:** Connect this explicitly to the LLM fundamentals lecture. Ask: "Do you remember how text tokens become vectors before entering the transformer? Vision models do something conceptually similar for images — converting visual content into a representation the model can process alongside your text tokens."

---

### 3B — Sending an Image in a Request

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Based on the message structure you already know — role and content — how do you think an image gets included in a request alongside a text question?" Guide toward: content becomes a list with multiple parts, one being the text, one being the image, rather than content being a single string.

**Demo 2 — Vision API request (whiteboard-friendly)**

```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "What is the total on this receipt?"},
                {"type": "image_url", "image_url": {"url": "https://example.com/receipt.jpg"}}
            ]
        }
    ]
)

print(response.choices[0].message.content)
```

**[Script:]**

"The `content` field is now a list of parts instead of a plain string. One part has `type: text` with the actual question. The other has `type: image_url` pointing to the image. The model processes both together — the text question guides what it looks for in the image, and the image provides the visual content to answer from.

Images can also be sent as base64-encoded data directly in the request instead of a URL, which matters when the image is a local file the user just uploaded rather than something already hosted somewhere public."

**Demo 3 — Sending an uploaded image as base64 (whiteboard-friendly)**

```python
import base64

with open("receipt.jpg", "rb") as f:
    image_data = base64.b64encode(f.read()).decode("utf-8")

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "What is the total on this receipt?"},
                {"type": "image_url", "image_url": {"url": f"data:image/jpeg;base64,{image_data}"}}
            ]
        }
    ]
)
```

**[Script:]**

"Same request shape, but instead of a public URL, the image bytes themselves are base64-encoded and embedded directly in the request as a data URL. This is the pattern you use in a FastAPI route that receives a file upload from a user — you do not need to host the image anywhere first; you send the bytes directly."

> 🎯 **Instructor Note:** Ask: "Why would you use base64 encoding instead of a URL for a user-uploaded file in your backend?" Answer: a user-uploaded file typically is not hosted anywhere with a public URL — it exists only as bytes in your server's memory or a temporary file. Base64 lets you send those bytes directly without needing an intermediate hosting step.

**Recap of Block 3 before moving on:**

- Vision-capable models accept images as part of the same chat message structure, with `content` becoming a list of typed parts instead of a plain string
- One part carries the text question; another carries the image, either as a public URL or base64-encoded data
- Base64 encoding is the practical choice for user-uploaded images that are not hosted at a public URL
- Conceptually, images are processed through their own encoding path into the same transformer architecture that processes text tokens

---

## Block 4 — Embeddings API and Vector Generation

### 4A — What an Embedding Is

**[Script:]**

"Everything so far generates text or structured data — you send input, the model produces new content. Embeddings are fundamentally different. An embedding API does not generate anything. It converts text into a vector — a fixed-length list of numbers — that represents the meaning of that text in a mathematical space.

Two pieces of text with similar meaning produce vectors that are close together in that space, measured by mathematical distance. Two pieces of text with unrelated meaning produce vectors that are far apart. This is what makes semantic search possible — finding content by meaning, not by exact keyword matching."

> 🎯 **Instructor Note:** Draw this concept on the board — the spatial intuition is the key thing to land here.

```
"a place to get coffee"     → [0.12, -0.45, 0.88, ...]  (vector A)
"a nearby café"              → [0.14, -0.42, 0.85, ...]  (vector B)
"a recipe for lasagna"       → [-0.71, 0.33, -0.10, ...] (vector C)

distance(A, B) is small  → similar meaning
distance(A, C) is large  → unrelated meaning
```

**[Script:]**

"Note that vector A and vector B share no words at all — 'coffee' versus 'café', 'place' versus 'nearby' — yet they land close together, because the embedding captures meaning, not exact wording. This is precisely what a keyword search cannot do."

---

### 4B — Generating Embeddings

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If I generate embeddings for 'happy' and 'joyful', and separately for 'happy' and 'car', which pair do you expect to be closer together in the vector space?" Answer: 'happy' and 'joyful', since they share similar meaning, despite having no overlapping letters or being related by any surface-level pattern.

**Demo 4 — Generating an embedding (whiteboard-friendly)**

```python
response = client.embeddings.create(
    model="text-embedding-3-small",
    input="a place to get coffee"
)

vector = response.data[0].embedding
print(len(vector))     # e.g. 1536
print(vector[:5])      # first five numbers of the vector
```

**[Script:]**

"`client.embeddings.create` takes text as input and returns a vector — in this case, 1536 numbers. That length is fixed by the model; every embedding from this model has exactly this many dimensions, regardless of how long or short the input text was.

`response.data[0].embedding` is the vector itself, just a plain Python list of floats. There is no 'generated text' to read here — the vector is the entire output, and it is meant to be stored and compared mathematically, not read by a human."

---

### 4C — Comparing Embeddings for Similarity

**[Script:]**

"To measure how similar two pieces of text are, you generate an embedding for each and compute the distance between the two vectors. The standard measure is cosine similarity — a number from -1 to 1, where 1 means identical direction in the vector space, and lower values mean less similarity."

**Demo 5 — Comparing two embeddings (whiteboard-friendly)**

```python
import numpy as np

def cosine_similarity(a, b):
    a, b = np.array(a), np.array(b)
    return np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))

vec1 = client.embeddings.create(model="text-embedding-3-small", input="a place to get coffee").data[0].embedding
vec2 = client.embeddings.create(model="text-embedding-3-small", input="a nearby café").data[0].embedding
vec3 = client.embeddings.create(model="text-embedding-3-small", input="a recipe for lasagna").data[0].embedding

print(cosine_similarity(vec1, vec2))   # high, e.g. 0.85
print(cosine_similarity(vec1, vec3))   # low, e.g. 0.15
```

**[Script:]**

"The similarity between 'a place to get coffee' and 'a nearby café' is high, despite sharing no words. The similarity between 'a place to get coffee' and 'a recipe for lasagna' is low, as expected — the meanings are unrelated.

This is the foundation of semantic search: embed your search query, embed every document in your database ahead of time, and find the documents whose embeddings are closest to the query's embedding. This is also the foundation of the retrieval step in retrieval-augmented generation, mentioned briefly in the context engineering session — retrieval works by embedding a query and finding the most similar stored content."

> 🎯 **Instructor Note:** Connect explicitly back to context engineering. Say: "Remember the fourth context optimization strategy from the prompt engineering session — retrieve relevant pieces instead of including everything? Embeddings are the mechanism that makes that retrieval step work. You embed the query, compare it against embeddings of your stored content, and pull back only the closest matches."

**[Script:]**

"In practice, storing raw vectors and comparing them one by one in Python does not scale past a small number of documents. Real applications store embeddings in a vector database — a specialized data store optimized for fast similarity search across millions of vectors. That is a deeper topic beyond today, but understanding what an embedding is and how similarity is computed is the foundation for it."

**Recap of Block 4 before moving on:**

- Embeddings convert text into a fixed-length vector representing its meaning, rather than generating new text
- Semantically similar text produces vectors that are close together, even without shared words
- `client.embeddings.create` returns a vector; the vector's length is fixed by the model regardless of input length
- Cosine similarity measures how close two vectors are, forming the basis for semantic search
- Embeddings are the mechanism behind retrieval-augmented generation and semantic search features; production systems typically store them in a specialized vector database

---

## Block 5 — Multimodal Integration: Handling Multimodal Inputs and Outputs

### 5A — Combining Modalities in a Single Feature

**[Script:]**

"A multimodal feature is one that combines more than one type of input or output — text and images together, or text generation alongside structured data, or vision combined with a structured schema for the response. You now have all the individual pieces from this session; multimodal integration is about combining them deliberately.

Consider a receipt-scanning feature: a user uploads a photo, and your application needs to extract structured data — vendor name, date, total, line items — not just a free-text description of what is in the photo. This combines vision input with structured output, both covered earlier in this session."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "Based on what you have seen with vision requests and structured output separately, what would you expect the request for a combined receipt-scanning feature to look like?" Guide toward: an image in the content list, exactly as in the vision demo, combined with a `response_format` schema, exactly as in the structured output demo — the two features compose directly.

**Demo 6 — Combining vision input with structured output (whiteboard-friendly)**

```python
class ReceiptData(BaseModel):
    vendor: str
    date: str
    total: float

response = client.beta.chat.completions.parse(
    model="gpt-4o-mini",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "Extract the vendor, date, and total from this receipt."},
                {"type": "image_url", "image_url": {"url": f"data:image/jpeg;base64,{image_data}"}}
            ]
        }
    ],
    response_format=ReceiptData
)

receipt = response.choices[0].message.parsed
print(receipt.vendor, receipt.date, receipt.total)
```

**[Script:]**

"Notice this is not new syntax — it is the vision request from Block 3 and the structured output request from Block 2, combined in one call. The `content` list carries the image exactly as before. `response_format=ReceiptData` constrains the output exactly as before. The result is a validated `ReceiptData` object, built directly from what the model saw in the image.

This is the essential idea of multimodal integration at this level: the individual capabilities compose. You are not learning a fourth, entirely new pattern — you are combining patterns you already know."

> 🎯 **Instructor Note:** This is worth stating explicitly as the core takeaway of the block: "Multimodal is not a separate skill from vision and structured output — it is those two skills used together in the same request. Once you know the building blocks, combining them is mostly about deciding which combination solves your actual problem."

**Recap of Block 5 before moving on:**

- Multimodal integration combines the individual capabilities already covered — vision input, structured output, text generation — in a single feature
- These capabilities compose directly in the same API call rather than requiring separate, unrelated techniques
- A receipt-scanning feature combining an uploaded image with a structured schema is a practical example of this composition
- Identifying which combination of capabilities solves a given problem is the core design skill for multimodal features

---

## Block 6 — Implementing Advanced API Features in Backend Applications

### 6A — Extending the Reusable Service Pattern

**[Script:]**

"You already built a reusable `get_ai_response` service function in the previous session. The same architectural principle applies here: do not scatter structured output schemas, vision request logic, and embedding calls across individual route handlers. Extend your service layer."

**Demo 7 — A service function for structured, multimodal extraction (whiteboard-friendly)**

```python
def extract_receipt_data(image_base64: str) -> ReceiptData:
    try:
        response = client.beta.chat.completions.parse(
            model="gpt-4o-mini",
            messages=[
                {
                    "role": "user",
                    "content": [
                        {"type": "text", "text": "Extract the vendor, date, and total from this receipt."},
                        {"type": "image_url", "image_url": {"url": f"data:image/jpeg;base64,{image_base64}"}}
                    ]
                }
            ],
            response_format=ReceiptData
        )
        return response.choices[0].message.parsed
    except Exception as e:
        raise RuntimeError(f"Receipt extraction failed: {e}")


@app.post("/scan-receipt")
async def scan_receipt(file: UploadFile):
    contents = await file.read()
    image_base64 = base64.b64encode(contents).decode("utf-8")
    receipt = extract_receipt_data(image_base64)
    return {"vendor": receipt.vendor, "date": receipt.date, "total": receipt.total}
```

**[Script:]**

"`extract_receipt_data` is a service function with exactly the same shape as `get_ai_response` from before — it hides the API call details, handles errors, and returns a clean, typed result. The route itself is thin: read the uploaded file, encode it, call the service function, return the result. This should look like the file upload handling from the FastAPI advanced features session, combined with the service function pattern from the previous AI integration session."

> 🎯 **Instructor Note:** Ask: "What would you need to change if you wanted to add an embedding-generation service function alongside this one?" Answer: the same pattern — a function that wraps `client.embeddings.create`, handles errors, and returns just the vector, keeping route handlers thin and consistent with the rest of the service layer.

**[Script:]**

"As your application grows, you might have `get_ai_response`, `extract_receipt_data`, `generate_embedding`, `find_similar_documents` — each a small, focused service function, each hiding the details of a specific API capability, each reusable across as many routes as need it. This is the same modular structure you already use for database access functions."

**Recap of Block 6 before moving on:**

- Advanced API features follow the same service-function pattern established for basic AI integration — hide the API details, handle errors, return a clean typed result
- Route handlers stay thin: receive input, call the service function, return the result
- A growing application accumulates a small library of focused service functions, one per capability, mirroring the structure already used for database access
- This consistency makes advanced features (vision, structured output, embeddings, or combinations of them) no harder to maintain than the simplest text-generation feature

---

## Block 7 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What is the difference between structured prompting and structured output mode? How does an image enter a chat request? What does an embedding represent, and how do you measure similarity between two of them? Why is multimodal integration not really a fourth new skill?"

**Structured Output Generation Using Schemas**

- Structured output mode constrains generation itself to match a formal schema, unlike structured prompting which is a best-effort request
- A Pydantic model defines the schema, passed via `response_format`
- The response comes back as a validated, typed instance of that model, not a raw string requiring manual parsing

**Vision APIs and Image Understanding**

- Vision-capable models accept images inside the same chat message structure, with `content` becoming a list of typed parts
- Images can be provided as a public URL or as base64-encoded data, the latter being the practical choice for user-uploaded files
- Images are processed through their own encoding path into the same underlying transformer architecture used for text

**Embeddings API and Vector Generation**

- An embedding converts text into a fixed-length vector representing its meaning, rather than generating new text
- Semantically similar text produces vectors that are close together, measured by cosine similarity
- Embeddings are the mechanism behind semantic search and the retrieval step in retrieval-augmented generation
- Production systems typically store embeddings in a specialized vector database for fast similarity search at scale

**Multimodal Integration**

- Multimodal features combine the individual capabilities already covered — vision, structured output, text generation — in a single request
- These capabilities compose directly; combining them is not a separate, unfamiliar technique
- Identifying which combination of capabilities solves a given problem is the core design skill

**Implementing Advanced API Features in Backend Applications**

- Advanced features follow the same reusable service-function pattern as basic AI integration: hide API details, handle errors, return a clean typed result
- Route handlers stay thin, calling into a growing library of focused service functions
- This keeps advanced, multimodal features as maintainable as the simplest text-generation feature

**Why All of This Matters Together**

- Structured output, vision, and embeddings are not separate technologies bolted onto the basic chat API — they are extensions of the same request-and-response pattern, the same Pydantic models, and the same service-function architecture already established; understanding how these capabilities compose, rather than memorizing each in isolation, is what lets you design and ship a genuinely production-grade AI feature instead of a fragile demo that happens to work most of the time

---

*End of script.*