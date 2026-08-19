# Lecture Script: Multimodal AI Features
**Duration:** 110 minutes | **Tools:** FastAPI, OpenAI multimodal/vision-capable chat, sample JPEG, simple HTML or Swagger upload | **Context:** Secure AI endpoint + LLMOps budgets

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Photo of a crushed box |
| Why Does This Matter? | 10 min | Returns, KYC-lite photos |
| What Is the Concept? | 20 min | Parts, validation |
| How Do We Apply It? (LOs) | 55 min | End-to-end damage JSON |
| Flow rehearsal | 15 min | UI → API → UI |
| Recap | 5 min | Module 6 close |

---

## Session Opening (5 min)

**[Script:]** "Your classifier reads **words**. Many tickets are **photos**. **Multimodal** models accept an image plus instructions and still return text you can parse. Today: how it differs, **call the API**, **one feature**, **validate and errors**, **full workflow**."

**Problem hook:** Show a torn book photo. "Text ticket: 'it's damaged.' Photo: which corner, how bad. Support at **Amazon** already thinks this way."

🎯 **Instructor Note:** Use tiny JPEGs. Confirm model name that accepts images. If vision is blocked, demo on instructor key; students still write validation + contract.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask who has sent a screenshot to support instead of a paragraph.

**[Script:]** "**Google Lens**, **ChatGPT** image ask, **PhonePe**/UPI QR, **Zomato** order issues with photos — users expect pixels. A notebook cell is not a product. A **FastAPI** route plus a tiny form is."

**Pain if misunderstood:**
- Trusting any file type
- Huge images → timeout and cost
- No JSON contract → UI guessing
- Leaky errors
- Feature not wired to a user step

---

## What Is the Concept?

### Multimodal vs Text-Only

Extra input modality. Output still steered with prompts, temperature, max tokens.

### API Call

User content as **list**: text part + image part (URL or base64).

### Feature Slice

One job, two JSON keys.

### Validation / Errors

Allow-list MIME, size cap, 422 vs 503.

### Workflow

Human path with loading and error banner.

---

## How Do We Apply It?

### LO 1: How multimodal differs from text-only

**Board:** text classify vs image damage.

**Predict:** Does adding an image remove the need for a system prompt?

**Explain result:** No. You still need role, JSON, "do not invent." Image is **more data**, not a new religion.

---

### LO 2: Call multimodal API with image/non-text

**Live call** with a sample URL or local base64.

```python
messages = [
    {"role": "system", "content": "JSON keys: damage_visible (bool), brief (max 20 words). No markdown."},
    {"role": "user", "content": [
        {"type": "text", "text": "Inspect the parcel photo. DATA is the image."},
        {"type": "image_url", "image_url": {"url": SAMPLE_URL}},
    ]},
]
```

**Predict before running:** Is `content` still a string in the **response**?

**Explain result:** Usually yes — you parse JSON text like before.

> **In the Real World:** **OpenAI** vision and similar APIs power damage and document-lite features; keep the slice small.

---

### LO 3: Build one feature that accepts image input

**FastAPI:** `POST /ai/damage` with `UploadFile` or `{ "image_url": "..." }`. Return `DamageOut`.

**Predict:** Should this endpoint refund money?

**Explain result:** No. It **labels**. Humans or later code decide. Same as Module 4 security.

---

### LO 4: Validate multimodal inputs and handle API errors

**Checks:** filename, `content_type` in `{image/jpeg, image/png}`, `size < 2_000_000`.

**Demo:** upload a `.txt` → 422, no OpenAI call.

**Errors:** wrap provider failures in 503 `"AI service unavailable"`.

**Predict:** Do we retry a 400 from a malformed base64 forever?

**Explain result:** No. Fix payload or reject. Retry 429 only.

---

### LO 5: Integrate into a simple product workflow end to end

**Tiny HTML** or Swagger:
1. Choose file
2. Submit
3. Show brief + badge
4. Show banner on 422/503

Walk the **happy photo** and the **bad file** path.

**Predict:** Where should the API key live in this flow?

**Explain result:** Server only. Browser sends the image to **your** API.

🎯 **Instructor Note:** Skip fancy React. End-to-end beats polish.

---

## Flow Rehearsal (15 min)

Pairs: write the 6-step user journey on paper, including loading. Swap. Missing error state = fail.

---

## Recap (5 min)

**[Script:]** "Module 6 complete: deploy, LLMOps, tests/review, multimodal. **Capstone** (next module) is you combining these — we are not starting it today. You already have the builder habits."

---

## Lecture Summary

- **Multimodal** adds non-text input (here, images); output is still steered text
- You **call** the API with text + image parts
- **One feature** (damage JSON) is enough
- **Validate** type/size; handle errors without leaks
- Wire **UI → FastAPI → model → UI** for a real workflow
- **Practical value:** You can demo a photo-aware AI step with the same professionalism as text classify
