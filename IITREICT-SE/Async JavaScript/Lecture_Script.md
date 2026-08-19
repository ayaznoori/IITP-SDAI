# Lecture Script: Async JavaScript
**Duration:** 110 minutes | **Tools:** Browser Console, VS Code, simple HTML page | **Data:** Timer-based demos (no Fetch yet)

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 6 min | Frozen button vs buzzer |
| Why Does This Matter? | 12 min | UX, Gmail, checkout |
| What Is the Concept? | 24 min | Call stack, callbacks, Promises |
| How Do We Apply It? (LOs) | 56 min | Five LOs with live predicts |
| Recap | 12 min | Bridge to DOM and Fetch |

---

## Session Opening (6 min)

**Problem:** A `for` loop of 1e9 iterations freezes the tab. Students think "JS is slow." The real issue is **blocking the main thread**.

**[Script:]** "JavaScript in the browser is a **single waiter**. If they stand still counting to a billion, nobody gets water. **Async** means take the order, give a buzzer, keep serving. Today: **callbacks**, **Promises**, **async/await**, **try/catch**, and **`setTimeout`**."

> **In the Real World:** **PhonePe** payment confirmation is not a frozen spinner forever without a plan. **Slack** still lets you type while messages load.

🎯 **Instructor Note:** Freeze the tab with a tight loop for 3 seconds (warn first). Then contrast with `setTimeout`. Students feel the difference.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "What should happen if a 2-second wait runs: freeze the whole page, or show a message and keep the page clickable?"

**[Script:]** "Every API call you will write is a wait. If you only know sync, you will copy `sleep`-style thinking from Python and freeze Chrome. Recruiters watch whether you can **chain `then`** and **`await` with try/catch**."

**Pain if misunderstood:**
- Callbacks nested five levels deep
- Unhandled Promise rejections
- Assuming `setTimeout(fn, 0)` runs before the next line

---

## What Is the Concept?

### Sync vs async

**Synchronous execution:** Each line finishes before the next starts.

**Asynchronous execution:** You **schedule** work. The engine continues. Later, a **callback** or Promise handler runs.

**Mental model:** Kitchen tickets. The cook (Web APIs / timers) works off-thread from your viewpoint. The waiter (JS) only plates when the ticket is ready.

**Python vs JS:** `time.sleep(2)` blocks the Python process. `setTimeout` does **not** block the next `console.log`.

### Callbacks

Function passed as "call me when done."

### Promises

Object with **pending / fulfilled / rejected**. `then` + `catch` (+ mention `finally` as optional cleanup — keep demos to then/catch).

### async/await + try/catch

Syntactic sugar over Promises. `await` only inside `async`. Errors become exceptions.

**Common mistakes:**
- Forgetting `await` — you log a Promise object
- Using `try/catch` around sync code only and ignoring rejected Promises
- Nested `then` that should be `async` functions

---

## How Do We Apply It?

### LO 1: Explain sync vs async JS execution

**Problem:** Predict this output:

```javascript
console.log("start");
setTimeout(() => console.log("mid"), 0);
console.log("end");
```

**Translate logic:** `setTimeout` registers a timer. `"end"` runs on the same turn.

**Predict before running:** What order?

**Explain result:** `start`, `end`, `mid`. Async delay, even at `0`.

**Walkthrough:** Draw call stack vs timer queue on the board.

> **In the Real World:** **YouTube** keep-alive UI stays responsive while a comment list loads later.

---

### LO 2: Implement callbacks and Promise chains using then/catch

**Problem:** Simulate "OTP sent" then "verified" without nested pyramids.

**Translate logic:** Callback version first, then Promise chain.

**Write code — callback:**

```javascript
function sendOtp(cb) {
  setTimeout(() => cb("sent"), 400);
}
sendOtp((msg) => console.log(msg));
```

**Write code — Promise:**

```javascript
function sendOtp() {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("sent"), 400);
  });
}
sendOtp()
  .then((msg) => {
    console.log(msg);
    return "verified";
  })
  .then((msg) => console.log(msg))
  .catch((err) => console.log(err));
```

**Predict before running:** Two logs, in order?

**Explain result:** `sent` then `verified`. Each `then` waits for the previous return.

**Demo reject:**

```javascript
Promise.reject("network down").catch((e) => console.log(e));
```

> **In the Real World:** **Razorpay** checkout steps are a chain: create order → confirm → receipt. Failures must `catch`.

---

### LO 3: Write async functions using async/await

**Problem:** Students can read `then` but cannot write `await`.

**Translate logic:** Same Promise, linear looking code.

**Write code:**

```javascript
function wait(ms) {
  return new Promise((r) => setTimeout(r, ms));
}

async function checkout() {
  await wait(300);
  console.log("paid");
  return "ok";
}

checkout();
```

**Predict before running:** Does `"paid"` appear immediately?

**Explain result:** After ~300ms. `await` pauses **this function**, not the whole tab.

**Live check:** Click a button while waiting — page still works.

---

### LO 4: Handle errors with try/catch

**Problem:** `await` of a rejected Promise throws.

**Write code:**

```javascript
async function pay() {
  try {
    await Promise.reject("card declined");
    console.log("never");
  } catch (err) {
    console.log(err);
  }
}
pay();
```

**Predict before running:** `"never"` or `"card declined"`?

**Explain result:** `"card declined"`. **try/catch** is the `async` cousin of `.catch()`.

🎯 **Instructor Note:** Show unhandled rejection in console if you omit try/catch — then fix it.

> **In the Real World:** **IRCTC** payment failure must show a message, not a blank tab.

---

### LO 5: Demonstrate setTimeout and Promise-based patterns in the browser

**Problem:** Build a tiny "searching…" then "3 trains found" UI. No Fetch. Fake wait.

**HTML sketch:**

```html
<button id="go">Search trains</button>
<p id="out">Idle</p>
```

**Write code:**

```javascript
const out = document.getElementById("out");
document.getElementById("go").onclick = async () => {
  out.textContent = "Searching…";
  await new Promise((r) => setTimeout(r, 800));
  out.textContent = "3 trains found";
};
```

**Predict before running:** Immediate text? Then update?

**Explain result:** Immediate `Searching…`, then `3 trains found`. Browser + Promise + await.

**Stretch same LO:** Wrap `setTimeout` in a reusable `wait(ms)` helper. Students reuse it.

---

## Recap (12 min)

Board: Sync vs async | Callback | Promise then/catch | async/await | try/catch | setTimeout helper

**[Script:]** "Next you will attach this to **DOM events** and later **Fetch**. The pattern does not change. Only the source of the Promise changes."

---

## Lecture Summary

- **Sync** runs now, in order; **async** schedules later so the page stays alive
- **Callbacks** and **Promise `then`/`catch`** handle completion and failure
- **`async`/`await`** writes the same flow in a straight line
- **`try`/`catch`** handles rejected awaits
- **`setTimeout` and Promise wrappers** are the browser training ground for future API calls
- **Practical value:** Every spinner, OTP wait, and API load in this course uses this model
