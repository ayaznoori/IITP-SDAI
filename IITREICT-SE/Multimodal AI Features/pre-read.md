# Pre-Read: Multimodal AI Features

## 1. What You'll Learn

In this pre-read, you'll discover:

- How **multimodal AI** differs from **text-only** LLM features
- How to **call a multimodal API** with an **image** (or other non-text)
- How to **build one feature** that accepts image input
- How to **validate** multimodal inputs and **handle API errors**
- How to **integrate** that feature into a **simple product workflow** end to end

---

## 2. Detailed Explanation

### Text-Only vs Multimodal

A **text-only** LLM reads tokens from strings. **Multimodal** models can also take **images** (and sometimes audio). The model then still **outputs text** in the patterns you already know (JSON, summaries).

**One-line definition:** Multimodal means the input can include more than text — in this session, an image plus instructions.

**Analogy:** A support agent who can **read the email and look at the photo** of the damaged box.

> **In the Real World:** **Google Photos** captions, **BeMyEyes**-style helpers, **Amazon** "damage photo" flows, **Swiggy** spill photos, **UPI** apps reading QR images — products already mix pixels and language. **OpenAI** vision-style APIs let builders do a small version of that.

### Call a Multimodal API with an Image

Chat Completions can send a **user message** that is a list of parts: text + image.

Typical image send options (instructor will pick one):
- A **public URL** to an image
- **Base64** data of a small JPEG/PNG

You still pass `system` rules: "JSON only. Do not invent brands."

Keep images **small**. Huge photos cost tokens and time (budgets from last sessions still apply).

### One Feature: Image In

Pick a **narrow** product job, for example:

**Bookstore returns:** user uploads a photo of the book/parcel. API returns JSON:

```json
{ "damage_visible": true, "brief": "Corner crushed on paperback." }
```

Not: full insurance platform.

Python sketch (shape only — class will use the current SDK style):

```python
user_content = [
    {"type": "text", "text": "Describe damage. JSON: damage_visible, brief."},
    {"type": "image_url", "image_url": {"url": image_url}},
]
```

### Validate Inputs and Handle Errors

**Validate before the call:**
- File is present
- Content type is `image/jpeg` or `image/png` (allow-list)
- Size under a cap (e.g. 2 MB)
- Optional: dimensions not tiny/not huge

**Reject** PDFs, random `.exe`, 40 MB RAW files.

**API errors:** same professionalism as text — retries for 429/timeouts, generic 503 to the client, no key in the body. Extra: "image unreadable" if the provider rejects the payload.

### End-to-End Product Workflow

A simple flow:

1. User opens Returns page
2. Uploads photo + optional note
3. Frontend `POST`s to your FastAPI route (`multipart` or JSON with URL)
4. Server validates, calls multimodal API, allow-lists JSON keys
5. UI shows `brief` and a badge for `damage_visible`
6. Errors: 422 (bad file), 503 (AI busy)

That is **integrate into a workflow** — not a notebook sitting alone.

**Why It Matters:** Capstone may need "one meaningful AI feature." Vision is a clear demo **if** you keep validation and errors.

**Benefits:**
- Richer evidence for support
- Same contract discipline as text classify
- Users see a full loop, not an SDK snippet

### Building Blocks

- Difference: extra input type
- Image + text message parts
- One JSON feature
- File allow-list + size
- UI → API → model → UI

---

## 3. Practice Exercises

**Exercise 1 — Difference**  
In two sentences, contrast text-only classify vs image damage check.

**Exercise 2 — Message parts**  
What two parts does the user message need for "caption this parcel photo"?

**Exercise 3 — Feature slice**  
Write the JSON response keys for your damage feature. Keep it to two keys.

**Exercise 4 — Validation**  
A user uploads a 20 MB BMP. Should you call the model? Why or why not?

**Exercise 5 — Workflow**  
Number the steps from "choose file" to "see brief on screen," including one error path.
