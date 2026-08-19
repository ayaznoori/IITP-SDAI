# Lecture Script: Hands-on: React Build Lab
**Duration:** 110 minutes | **Tools:** VS Code, Vite React, Tailwind, Browser, Mentors | **Mode:** Workshop

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Definition of done, no new framework |
| Why Does This Matter? | 10 min | Intern tickets, debug culture |
| What Is the Concept? | 12 min | Mini screen recipe + error board |
| How Do We Apply It? (LOs) | 20 min | Live recipe then students build |
| Mentored build | 50 min | Working screen, error clinic |
| Recap | 10 min | Demo two screens, deploy preview |

---

## Session Opening (8 min)

**Problem:** Students wait for “the next lecture topic.” There is none. The blank editor is the topic.

**[Script:]** "This is a **build lab**. You will leave with a **working mini screen**: components, props, `useState`, Tailwind. Mentors exist to unstick **beginner React errors** — not to type the project for you. We are not starting a company in 110 minutes. Think **one Notion widget**, one **Spotify** now-playing stub, one **Amazon** quantity row — small and done."

**Real-world hook:** Show a 15-line `Card` from a public **GitHub** React repo or your demo. "Production is many of these, not one monster file."

🎯 **Instructor Note:** Staff ratio: roam, do not lecture from minute 30. Pin a “definition of done” on the board.

---

## Why Does This Matter?

🎯 **Instructor Note:** Hook — “Who has a tutorial repo they cannot change?” Hands. “Today you change yours.”

**[Script:]** "At **PhonePe**, **CRED**, or a **Google** intern pod, nobody ships your tutorial clone. They ask you to fix a toggle. If you freeze at `useState is not defined`, you stall the ticket. Pain: copy-pasting AI into `App.jsx` and failing when asked to explain. Success: a screen you can narrate like a **Figma**-to-UI intern demo."

**Real-world use:**

| Mini screen | Product cousin |
|-------------|----------------|
| Like + count | **Instagram** |
| Name field + hello | **Slack** set-status |
| Flex header | **GitHub** top bar |
| Toggle label | **YouTube** autoplay |

**Pain if misunderstood:**
- Scope creep (router + fetch + five pages) → nothing works
- Silent struggle → 90 minutes wasted
- Fighting CSS files instead of utilities

---

## What Is the Concept?

**Definition:** A **working mini screen** is interactive, styled, component-based, and stable in the browser.

**Mental model:** Recipe, not buffet. Components + props → state → Tailwind → debug.

**Comparison:** Prior sessions taught ingredients. Lab cooks one plate.

**Common mistakes:** Start with Router and JSONPlaceholder before a button works. Ban that order unless their router app already runs.

**Flow:** Trainer builds a 10-line skeleton per LO, then students copy the **idea**, not the pixels.

---

## How Do We Apply It?

### LO 1: Build a small React UI with components and props

**Problem:** One file cannot stay readable, even for a **Trello** card.

**Translate logic:** `App` composes `TitleBar` with a `title` prop.

**Write code:**

```jsx
function TitleBar({ title }) {
  return <h1 className="text-xl font-bold">{title}</h1>;
}

export default function App() {
  return <TitleBar title="Lab screen" />;
}
```

**Predict before running:** What will happen? Heading shows the prop text.

**Explain result:** UI is composition. Same as **Airbnb** listing title vs page shell.

🎯 **Instructor Note:** Students add a second component (`Hint`, `Footer`) with one prop.

**Recap:** Components + props first, even in a lab.

---

### LO 2: Manage state with useState

**Problem:** Static title is a poster. **LinkedIn** “Connect” must change.

**Translate logic:** Boolean or number in `App`. Button calls setter. Pass display value down if useful.

**Write code:**

```jsx
function App() {
  const [n, setN] = useState(0);
  return (
    <div>
      <TitleBar title={`Stars ${n}`} />
      <button onClick={() => setN(n + 1)}>Add</button>
    </div>
  );
}
```

**Predict before running:** What will happen? Click updates heading.

**Explain result:** State in parent, text via props. **YouTube** like count pattern.

**Recap:** One piece of state is enough to pass the LO.

---

### LO 3: Style the UI with Tailwind utilities

**Problem:** Default button looks unfinished next to **Linear**.

**Translate logic:** Flex column/row, padding, gap, rounded box. Optional `md:flex-row`.

**Write code:**

```jsx
<div className="p-6 max-w-sm bg-white rounded-lg shadow flex flex-col gap-3">
  <TitleBar title={`Stars ${n}`} />
  <button className="px-3 py-2 bg-blue-600 text-white rounded">Add</button>
</div>
```

Keep the surrounding component as already written; this is the styled shell (≤10 lines).

**Predict before running:** What will happen? Card layout, not a naked heading.

**Explain result:** Utilities only. No new CSS file.

**Recap:** Looks like a product widget, not a tutorial dump.

---

### LO 4: Fix common React beginner errors with mentor support

**Problem:** Lab time dies on one typo. Mentors must **teach the read**, not hijack the keyboard.

**Translate logic:** Read console → match the error table → change one line → reload.

**Write code:** Broken vs fixed (board):

```jsx
// Broken: useState() without import
// Fixed:
import { useState } from "react";
```

Second 10-line demo: `class` → `className`.

**Predict before running:** What will happen? Missing import: crash. After import: runs.

**Explain result:** Errors are specific. **VS Code** red squiggles + browser overlay.

🎯 **Instructor Note:** Clinic protocol: student shares screen, reads error aloud, mentor asks “what does this line claim?” Hands off after the fix. Track top 3 errors on the board.

**Recap:** Mentor support is a learning objective, not a shortcut around thinking.

---

### LO 5: Leave a working mini screen by end of lab

**Problem:** “Almost done” is not done. **Vercel** later needs something that runs.

**Translate logic:** Checklist. Then 60-second demo to a peer.

**Write code:** No new code. Run `npm run dev`. Click the interaction. Zoom the Tailwind layout.

**Predict before running:** What will happen? If the checklist is honest, the peer sees a change on click.

**Explain result:** Working means **browser**, not “it looks fine in the editor.”

**Recap:** Mini screen in the bag. Next session: small multi-page app + GitHub + Vercel.

---

## Mentored Build (50 min)

Suggested student choices (pick one):

1. **Campus club card** — name prop, join toggle, Tailwind card (**Discord**-like)
2. **Canteen counter** — item name + count (**Swiggy** stepper lite)
3. **Status bar** — controlled input + preview (**Slack** status)

Freeze scope at minute 40. Polish, do not add Router unless already done.

> **In the Real World:** Standup at **Spotify** or **Atlassian**: “I shipped a working widget and unblocked a `className` crash.” That sentence is this lab.

---

## Lecture Summary

- **Components and props** structure a small React UI
- **`useState`** makes the screen interactive
- **Tailwind utilities** finish the look
- **Mentor-backed debugging** turns beginner errors into fixes
- **A working mini screen** is the exit ticket
- **Practical value:** This is how real junior work feels — small, styled, explainable

**[Script:]** "Photograph the URL localhost and the UI. Next time we connect pages, README, commits, and a **live Vercel URL**. Bring GitHub access."
