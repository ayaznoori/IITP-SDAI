# Lecture Script: React Fundamentals
**Duration:** 110 minutes | **Tools:** VS Code, Vite, Terminal, Browser | **Language:** React (JSX)

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 8 min | Why React, real products |
| Why Does This Matter? | 12 min | Jobs, scale, component thinking |
| What Is the Concept? | 25 min | Vite, JSX, components, props |
| How Do We Apply It? (LOs) | 55 min | Build skill list app |
| Live lab | 5 min | Compose mini portfolio in React |
| Recap | 5 min | useState preview |

---

## Session Opening (8 min)

**[Script:]** "You built a portfolio with HTML, CSS, and vanilla JS. It works. Now imagine adding 50 more project cards, a filter bar, and a dark mode — your one file becomes unmaintainable. **React** is how the industry builds UIs that scale. **Instagram's** web app, **Airbnb's** search UI, countless dashboards — component architecture."

**Real-world hook:** Show React job posting count on Naukri/LinkedIn — "This skill is hireable."

🎯 **Instructor Note:** Verify Node.js installed — `node -v`, `npm -v`. Help installs in first 10 min max.

---

## Why Does This Matter?

**Component thinking maps to teams:**

| Plain HTML project | React project |
|------------------|---------------|
| One developer edits giant file | Team owns `Navbar.jsx`, `Card.jsx` |
| Copy-paste card HTML 20 times | `<ProductCard />` × 20 |
| Manual DOM updates | Declarative UI (state next session) |

> **In the Real World:** **Swiggy** web and mobile-adjacent teams use component libraries. Even if framework changes, **component mental model** transfers to Vue, Svelte, React Native.

**Pain if misunderstood:**
- Missing `key` on lists — subtle UI bugs
- Mutating props (don't — props read-only)
- Giant `App.jsx` — same mess as giant HTML

---

## What Is the Concept?

### Vite Setup (Live)

```bash
npm create vite@latest iitp-portfolio-react -- --template react
cd iitp-portfolio-react
npm install
npm run dev
```

**[Script:]** "Vite is the modern dev server — fast refresh when you save. Industry moved from older tools to Vite for speed."

### JSX and Components

```jsx
// src/components/Header.jsx
function Header({ title }) {
  return (
    <header>
      <h1>{title}</h1>
    </header>
  );
}
export default Header;
```

```jsx
// src/App.jsx
import Header from './components/Header';

function App() {
  return (
    <div className="app">
      <Header title="Priya's Portfolio" />
    </div>
  );
}
export default App;
```

**Note:** `className` not `class` — JSX quirk tied to HTML `class` attribute.

### Props and List Rendering

```jsx
const projects = [
  { id: 1, name: "Portfolio HTML", stack: "HTML/CSS" },
  { id: 2, name: "DOM Lab", stack: "JavaScript" },
];

function ProjectList() {
  return (
    <section>
      <h2>Projects</h2>
      <ul>
        {projects.map((p) => (
          <li key={p.id}>
            <strong>{p.name}</strong> — {p.stack}
          </li>
        ))}
      </ul>
    </section>
  );
}
```

🎯 **Instructor Note:** Demo missing `key` warning in console — fix with `p.id`.

---

## How Do We Apply It?

### LO 1: Explain component-based UI

**Discussion:** Whiteboard HTML portfolio sections → boxes labeled `Header`, `ProjectList`, `Footer` components.

**Real-world:** **Figma** design systems map to React components 1:1 at many companies.

---

### LO 2: Set up React project with Vite

Students run setup commands. Mentor circulates. Everyone hits localhost success screen.

---

### LO 3: Write functional components in JSX

Build `Footer.jsx` with copyright prop `year`.

---

### LO 4: Pass data with props

`SkillBadge` component — props: `label`, `level` ("beginner" | "intermediate").

---

### LO 5: Render list with map and key

`skills` array → `SkillBadge` list. Use stable `id` keys.

---

## Live Lab (5 min)

Compose `App` from: `Header`, `ProjectList`, `SkillList`, `Footer`. Screenshot for Git commit.

> **In the Real World:** First day on React job — you will compose existing components before building new ones. Same skill.

---

## Lecture Summary

- **React** scales UI through reusable components
- **Vite** provides fast modern React development environment
- **JSX** expresses UI as JavaScript with HTML-like syntax
- **Props** pass read-only data parent → child
- **`map` + `key`** render dynamic lists efficiently
- **Practical value:** React is the most common path to frontend roles — you took the first real step today
