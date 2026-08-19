# Lecture Script: React State Management
**Duration:** 110 minutes | **Tools:** VS Code, Vite React app from prior session, Browser | **Language:** React (JSX)

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Static cards vs live products |
| Why Does This Matter? | 12 min | Likes, carts, search — state everywhere |
| What Is the Concept? | 22 min | useState, re-render, controlled inputs, conditionals |
| How Do We Apply It? (LOs) | 50 min | Five LOs: Problem → logic → code → predict → explain |
| Mini screen lab | 10 min | One interactive greeting / like screen |
| Recap | 8 min | Bridge to Tailwind styling |

---

## Session Opening (8 min)

**Problem:** Open last session’s React portfolio. Click a “Like” button that does nothing. Props never change from inside the card.

**[Script:]** "Your components look like Instagram cards, but they are posters. Instagram’s heart fills. Amazon’s cart badge ticks up. Swiggy’s search box remembers what you typed. That memory is **state**. Today we give React a notebook, and we let the UI reprint whenever the notebook changes."

**Real-world hook:** Rapid-fire product tour (30 seconds each screenshot or live site): **Instagram** like, **Amazon** quantity, **Gmail** unread count, **Zomato** veg filter chip, **Figma** zoom %, **Spotify** play/pause.

🎯 **Instructor Note:** Everyone should already have a Vite React app. If not, pair them. Do not spend the hour on `create vite`.

---

## Why Does This Matter?

🎯 **Instructor Note:** Hook question — “When you type in Google, does the page reload?” Wait for no. “Something in memory is updating the suggestions. Same idea as `useState`.”

**[Script:]** "If you skip state, you will fight React. You will poke the DOM with `querySelector` like Module 2. It will look like it works once, then break when React re-renders and wipes your manual edits. State is how you stay on React’s team."

**Real-world use:**

| Product | State you can name |
|---------|-------------------|
| **WhatsApp Web** | Draft message in the input |
| **Netflix** | Mute toggle on a trailer |
| **LinkedIn** | “Connect” vs “Pending” on a button |
| **Uber** | Pickup text field |
| **YouTube** | Like / dislike on a video |

**Pain if misunderstood:**
- Assigning `count++` — UI never updates
- Forgetting `value` on inputs — React and the DOM disagree
- Putting `useState` inside an `if` — hooks break (mention: call at top of component only)

---

## What Is the Concept?

**Definition:** **`useState`** is a React hook that stores a value and a setter. Calling the setter **re-renders** the component.

**Mental model:** Component function runs → reads current state → returns JSX. Setter queues new state → function runs again → new JSX.

**Comparison (plain JS vs React):** In DOM labs you did `el.textContent = n`. In React you do `setN(n)` and put `{n}` in JSX. React writes the DOM for you.

**Common mistakes:**
- Mutating arrays/objects in place (stay on numbers, strings, booleans today)
- Expecting `console.log(count)` right after `setCount` to show the new value in the same click

**Flow for the room:** Problem → Explain → Walkthrough → Demo → Recap (use this on every LO).

---

## How Do We Apply It?

### LO 1: Manage local state with useState

**Problem:** Show a number that goes up when a user taps, like a **YouTube** like count (simplified).

**Translate logic:** Start at 0. On click, ask React to store previous + 1. Draw the number from state.

**Write code:**

```jsx
import { useState } from "react";

function LikeCount() {
  const [likes, setLikes] = useState(0);
  return (
    <button onClick={() => setLikes(likes + 1)}>
      Likes: {likes}
    </button>
  );
}
```

**Predict before running:** What will happen? First paint shows `Likes: 0`. Each click adds 1.

**Explain result:** `setLikes` triggers a re-render. JSX reads the new `likes`. No `getElementById`.

🎯 **Instructor Note:** Stretch if fast: `setLikes((prev) => prev + 1)` — “safer when updates stack.” Keep under 10 lines.

**Recap:** Local state lives in this component. Parent props stay separate.

---

### LO 2: Build controlled form inputs bound to state

**Problem:** **Uber**-style pickup field: what you type must match what React stores.

**Translate logic:** State owns the string. Input displays it. Every `onChange` writes `e.target.value`.

**Write code:**

```jsx
function PickupField() {
  const [place, setPlace] = useState("");
  return (
    <input
      value={place}
      onChange={(e) => setPlace(e.target.value)}
      placeholder="Pickup location"
    />
  );
}
```

**Predict before running:** What will happen? Typing updates `place`. Empty start. Controlled — you can `setPlace("")` later to clear.

**Explain result:** The input is not an independent DOM box. It is a window onto `place`.

🎯 **Instructor Note:** Demo omitting `value` once — “uncontrolled.” Then add `value={place}`. Contrast with **Gmail** search box staying in sync.

**Recap:** `value` + `onChange` = controlled input.

---

### LO 3: Apply conditional rendering for simple UI states

**Problem:** **Amazon** empty cart vs cart with items. We use a boolean and a string, not a real cart.

**Translate logic:** If message is empty, show hint. Else show the message. Optional: show error with `&&`.

**Write code:**

```jsx
function CartHint({ items }) {
  const [note, setNote] = useState("");
  return (
    <div>
      <input value={note} onChange={(e) => setNote(e.target.value)} />
      {note ? <p>Note: {note}</p> : <p>Cart is quiet</p>}
      {items === 0 && <p>Add something</p>}
    </div>
  );
}
```

Keep the demo short: pass `items={0}` from `App`. If over 10 lines, split: first ternary only, then `&&`.

**Predict before running:** What will happen? Empty input → “Cart is quiet”. Type → note appears. `items === 0` shows extra line.

**Explain result:** JSX expressions can be `&&` or ternary. They are the if/else of UI.

**Recap:** `&&` hide/show. Ternary pick A or B. **Netflix** “Continue watching” is the `&&` idea.

---

### LO 4: Explain that a state update re-renders the component

**Problem:** Students think `setCount` mutates `count` on the next line like a Python variable.

**Translate logic:** This click still sees old state. The **next render** sees new state. Prove with console.

**Write code:**

```jsx
function RenderDemo() {
  const [n, setN] = useState(0);
  console.log("render", n);
  return (
    <button onClick={() => setN(n + 1)}>n is {n}</button>
  );
}
```

**Predict before running:** What will happen? First log `render 0`. Click → log `render 1`. Button text updates after the new render.

**Explain result:** Re-render = React calls `RenderDemo` again. That is why `{n}` in JSX stays correct.

🎯 **Instructor Note:** Pause. “If you `console.log(n)` inside onClick after setN, you still see old n. That is not a bug.”

**Recap:** Setter schedules. Render applies. UI follows state.

---

### LO 5: Build one small interactive screen driven by state

**Problem:** Mini **LinkedIn** “intro” widget: name field + interested toggle.

**Translate logic:** Two pieces of state. Controlled input. Ternary for greeting. `&&` for thanks message.

**Write code:**

```jsx
function IntroCard() {
  const [name, setName] = useState("");
  const [ok, setOk] = useState(false);
  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={() => setOk(!ok)}>Interested</button>
      {name ? <p>Hi, {name}</p> : <p>Type your name</p>}
      {ok && <p>Thanks!</p>}
    </div>
  );
}
```

**Predict before running:** What will happen? Name live-updates. Toggle shows/hides Thanks. Empty name shows prompt.

**Explain result:** One screen, two states, no extra files required. This is how **Spotify** mini-players and **Slack** status editors start — small state, clear UI.

🎯 **Instructor Note:** 10-minute lab: students copy structure with their own labels. Mentors hunt `count = count + 1` and missing `value`.

**Recap:** Interactive screen = state + events + JSX that reads state.

---

## Mini Screen Lab (10 min)

Students keep `IntroCard` (or equivalent) running. Commit if Git is already set up. No new libraries.

> **In the Real World:** First week on a React team at **Razorpay** or **PhonePe**, you will fix a toggle or a form field — exactly this skill.

---

## Lecture Summary

- **`useState`** holds local values a component can update
- **Controlled inputs** bind `value` and `onChange` to that state
- **`&&` and ternary** choose simple UI states
- **A state update re-renders** the component so JSX matches data
- **One small screen** can combine input, toggle, and messages
- **Practical value:** This is the difference between a brochure site and a product like Instagram or Amazon

**[Script:]** "Next session we make these screens look intentional with Tailwind. State first, paint second. You now own the notebook React reads."
