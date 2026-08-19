# Lecture Script: End-to-End React Project & Vercel Deploy
**Duration:** 110 minutes | **Tools:** VS Code, Vite React, Tailwind, GitHub, Vercel | **Mode:** Build + ship

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Localhost vs live product URL |
| Why Does This Matter? | 10 min | Recruiters, GitOps, AI review |
| What Is the Concept? | 12 min | Small app, hygiene, Vercel pipeline |
| How Do We Apply It? (LOs) | 25 min | Demos then student build/deploy |
| Build + GitHub + Vercel | 47 min | Mentored ship |
| Recap | 8 min | Backend module teaser |

---

## Session Opening (8 min)

**Problem:** Lab mini screen on one laptop. Recruiter on another continent. No URL.

**[Script:]** "Today we **ship a small multi-page React + Tailwind app**. Not a marketplace. Not a capstone. Home, About, maybe Posts. We **reuse** components, state, fetch, routing. We **read AI output** before it lands. We write a **README** and **real commits**. Then **GitHub → Vercel** until an incognito tab loads. That is how **v0.dev**, indie **Twitter/X** clone demos, and half of **Product Hunt** frontends go live."

**Real-world hook:** Open a student-friendly Vercel-hosted site or **vercel.com** dashboard screenshot. Point at Production URL. "This is your shop window."

🎯 **Instructor Note:** GitHub + Vercel accounts before minute 20. Pair anyone stuck on OAuth. Vite root is the repo root unless they nested folders.

---

## Why Does This Matter?

🎯 **Instructor Note:** Hook — “Show of hands: whose GitHub is still empty this module?” No shame. “We close that today.”

**[Script:]** "At **Vercel**, **Netlify**, and internal platforms at **Shopify**, Git is the deploy button. If you unzip a build to a random host, you skip the skill. If you accept Copilot’s 400-line router you cannot read, you ship landmines. Pain: blank Vercel page because `dist` was committed wrong, or secrets in README. Success: **LinkedIn** post with a live link like a **Hashnode** blog launch."

**Real-world use:**

| Habit | Who does it |
|-------|-------------|
| Preview every push | **Frontend teams** on Vercel |
| README as front door | **GitHub** trending repos |
| Small PR-sized commits | **Linear** + GitHub shops |
| Human review of AI | **Microsoft**, startups, you |

**Pain if misunderstood:**
- Deploying `node_modules`
- Router 404 on refresh (mention only if it hits the room)
- “AI wrote it” as the project story

---

## What Is the Concept?

**Definition:** **End to end** (this session) = working SPA + GitHub + verified Vercel URL.

**Mental model:** Source on GitHub is truth. Vercel runs `npm run build` and hosts static files.

**Comparison:** Python FastAPI deploy comes later. Today is **frontend only**.

**Common mistakes:** New libraries, env files with tokens, giant AI scaffolds.

**Flow:** Short trainer demos per LO, then long studio time.

---

## How Do We Apply It?

### LO 1: Build a small multi-page React + Tailwind app end to end

**Problem:** One card is not a site. **Notion** still has multiple pages.

**Translate logic:** Router + Tailwind nav + two or three page components. Finish locally first.

**Write code:**

```jsx
<nav className="flex gap-4 p-4 bg-slate-900 text-white">
  <Link to="/">Home</Link>
  <Link to="/about">About</Link>
</nav>
```

Pages stay tiny (`<h1>` + one paragraph) until they add state.

**Predict before running:** What will happen? Local `/` and `/about` both render with nav.

**Explain result:** Multi-page = routes + layout. End to end starts when this is stable.

**Recap:** Small site, complete enough to host.

---

### LO 2: Reuse prior skills — components, state, fetch, and routing

**Problem:** Students open a new stack overflow tab for Redux.

**Translate logic:** Checklist on the board. Tick what the app uses. Fetch optional but encouraged on `/posts`.

**Write code:**

```jsx
function Home() {
  const [on, setOn] = useState(false);
  return (
    <button className="m-4 px-3 py-2 bg-blue-600 text-white rounded"
      onClick={() => setOn(!on)}>
      {on ? "On" : "Off"}
    </button>
  );
}
```

Second micro-demo: `useEffect` fetch of JSONPlaceholder user name on `Posts` or `Home` (same as fetch session, ≤10 lines). Routing already in LO 1.

**Predict before running:** What will happen? Toggle works. Fetch shows a name or loading.

**Explain result:** Same LOs as the module, now in one repo. **GitHub** profile pages combine these pieces.

🎯 **Instructor Note:** If time is tight, require state + router + Tailwind; fetch is the stretch tick.

**Recap:** Reuse beats novelty.

---

### LO 3: Review any AI-generated code before accepting it

**Problem:** A student pastes a dashboard. Nothing runs. They cannot explain `memo`.

**Translate logic:** Accept line-by-line. Reject unknown APIs. Run the app.

**Write code:** Trainer pastes a **bad** AI snippet on the board (e.g. `class=` in JSX, or an extra `axios` import). Class votes: keep or delete.

```jsx
// AI trap — do not keep
import axios from "axios";
<div class="p-4">Hello</div>
```

**Predict before running:** What will happen? `class` may warn/fail; `axios` is not installed.

**Explain result:** Review is a skill. **Copilot** at work still needs this.

**Recap:** If you cannot teach the line, do not commit the line.

---

### LO 4: Keep a clean README and meaningful commits

**Problem:** `asdf` commits and empty GitHub make **Amazon** intern reviewers bounce.

**Translate logic:** README template. Commit after each working slice.

**Write code:** README core (keep short):

```markdown
# Campus Pages
Small React + Tailwind site.
## Run
npm install
npm run dev
## Live
https://your-app.vercel.app
```

Then one commit:

```bash
git add README.md
git commit -m "Document setup and live URL placeholder"
```

**Predict before running:** What will happen? `git log --oneline` shows intent.

**Explain result:** Humans and Vercel both need a readable repo. **Open source** on GitHub lives or dies on README.

**Recap:** Hygiene is an LO, not extra credit.

---

### LO 5: Deploy to Vercel from GitHub and verify the live URL

**Problem:** Only you can see localhost. **Product Hunt** hunters cannot.

**Translate logic:** Push `main`. Import on Vercel. Wait for green build. Open URL incognito. Click every `Link`.

**Write code:** (commands, not JSX)

```bash
git push -u origin main
```

Then in Vercel UI: Import → Deploy. No extra config if Vite is at repo root.

**Predict before running:** What will happen? Build log runs `vite build`. URL serves the app.

**Explain result:** GitHub is source. Vercel is the host. **Live URL** is the proof.

🎯 **Instructor Note:** Common fails: wrong root, missing `"build"` script, GitHub app permissions. SPA refresh 404: only if students hit it. Verify on phone if possible.

**Recap:** Incognito success = session complete.

---

## Build + GitHub + Vercel (47 min)

Milestones on a timer:

| Minute | Checkpoint |
|--------|------------|
| 15 | Pages + nav work locally |
| 25 | State (and fetch if attempted) |
| 35 | README + 2+ good commits + push |
| 47 | Vercel URL in README, peer click-test |

> **In the Real World:** A **Y Combinator** demo day site is often this pipeline. **Stripe** docs and **Vercel** templates assume Git-connected deploys.

---

## Lecture Summary

- **A small multi-page React + Tailwind app** is enough to finish the frontend module
- **Reuse** components, state, fetch, and routing instead of new stacks
- **Review AI code** before it becomes your Git history
- **README and meaningful commits** make the repo usable
- **Vercel from GitHub** plus a **verified live URL** is how you ship
- **Practical value:** You can send a link — the same habit as modern product teams

**[Script:]** "Module 5 frontend React is shippable. Next modules: **Python and FastAPI**, then data, then more deploy and AI. Your React app will eventually call *your* API instead of JSONPlaceholder. Same fetch. New URL."
