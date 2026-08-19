# Lecture Script: Advanced DOM Manipulation
**Duration:** 110 minutes | **Tools:** VS Code, Chrome/Edge DevTools | **Project:** Dynamic todo list **or** two-tab widget

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening | 6 min | Static HTML list vs live list |
| Why Does This Matter? | 10 min | Feeds, carts, tabs |
| What Is the Concept? | 22 min | create/remove, classList, traverse |
| How Do We Apply It? (LOs) | 58 min | Build list + tabs + debug |
| Recap | 14 min | State lives in the DOM |

---

## Session Opening (6 min)

**Problem:** Students duplicate ten `<li>` by hand. New homework items need a page edit.

**[Script:]** "Last time the nodes were already in HTML. Products **grow the tree**. **Notion** blocks, **Jira** tickets, **WhatsApp Web** messages — `createElement` in a loop. Today: create, remove, replace, styles, classes, a **list or tabs**, traversal, and **DevTools**."

> **In the Real World:** **Google Keep** notes appear without a full reload. That is runtime DOM.

🎯 **Instructor Note:** Show `ul` in Elements. Click Add. Watch a new `li` appear. Celebrate the tree changing.

---

## Why Does This Matter?

🎯 **Instructor Note:** "If you only change `textContent` on one node, can you show 50 search results?" Lead to **create in a loop**.

**[Script:]** "React will later **create** UI for you. If you skip this, hooks will feel like magic. If you do this, hooks will feel like a helper over the same tree."

**Pain if misunderstood:**
- Memory leaks from listeners on removed nodes (mention lightly)
- `innerHTML +=` that destroys listeners
- Debugging by guessing instead of inspecting

---

## What Is the Concept?

### Runtime nodes

`document.createElement(tag)` → set properties → `parent.append(child)`.  
`node.remove()`.  
`old.replaceWith(newNode)`.

### Styles and classes

`element.style.color` — inline, wins many fights, messy at scale.  
`classList.add/remove/toggle/contains` — matches CSS you already wrote.

### List vs tabs

Both are **UI state**. List: array of items reflected as `li`. Tabs: which panel is visible.

### Traversal

`parentElement`, `children`, `nextElementSibling`. Find the `li` from a button inside it.

### DevTools

Elements, computed styles, event listeners, `console.dir(element)`.

**Common mistakes:** `append` the same node twice (it **moves**). Creating but forgetting to append. Toggling class on the wrong parent.

---

## How Do We Apply It?

### LO 1: Create, remove, replace at runtime

**Problem:** Grocery list.

**Translate logic:** Read input → create `li` → append → clear input. Delete button removes its `li`. Replace used for "edit" lite: swap text node via `replaceWith` or recreate.

**Write code:**

```javascript
const li = document.createElement("li");
li.textContent = input.value;
ul.append(li);
```

```javascript
li.remove();
```

```javascript
const neu = document.createElement("li");
neu.textContent = "Updated";
li.replaceWith(neu);
```

**Predict before running:** After `remove`, does `querySelectorAll("li")` still count it?

**Explain result:** No. It is gone from the document.

> **In the Real World:** **Amazon** cart line-item delete removes a DOM row.

---

### LO 2: Inline styles and toggle CSS classes

**Problem:** Mark item complete. Highlight the active tab.

**Write code:**

```javascript
li.style.textDecoration = "line-through";
li.classList.toggle("done");
```

**Predict before running:** If CSS `.done { opacity: 0.5 }` exists, which should we prefer?

**Explain result:** **Class**. Inline is for quick demos or dynamic pixel values. Production UIs prefer classes.

**Demo:** `classList.toggle("hidden")` on a panel.

---

### LO 3: Build a dynamic list or simple tab component

**Problem (pick one as the live lab; show the other in 5 minutes):**

**List app:** input + Add + each row has Delete.

**Tabs:** two buttons, two panels. Only one `panel.active`.

**Tab sketch:**

```javascript
tabs.forEach((btn) => {
  btn.addEventListener("click", () => {
    panels.forEach((p) => p.classList.remove("active"));
    document.getElementById(btn.dataset.panel).classList.add("active");
  });
});
```

**Predict before running:** Click tab 2 — is panel 1 still `active`?

**Explain result:** Only if you forgot to remove. Teach **single source of visible panel**.

> **In the Real World:** **Chrome** settings pages and **Stripe** dashboard sections are tab patterns.

🎯 **Instructor Note:** 20-minute paired build. Mentor circulates. No frameworks.

---

### LO 4: Traverse and update DOM for UI state

**Problem:** Delete button is inside `li`. Counter at the top must show remaining count.

**Translate logic:** `event.target.closest("li")` if you teach `closest` — it is traversal and very useful. If staying strictly to parent/children: `event.target.parentElement`.

**Write code:**

```javascript
ul.addEventListener("click", (e) => {
  if (e.target.matches(".del")) {
    e.target.parentElement.remove();
    count.textContent = ul.children.length;
  }
});
```

**Predict before running:** Event on `ul` vs each button — which is easier when items are new?

**Explain result:** **Delegation** on parent. New `li`s still work. Mention as "listen on the list."

**State:** Remaining count is **derived** from `ul.children.length`. DOM is the source of truth in this session (React will later keep state in JS).

---

### LO 5: Debug DOM/event issues in DevTools

**Problem:** "Click does nothing."

**Walkthrough (live break, then fix):**
1. Wrong id — Console `document.getElementById("addd")` → `null`
2. Listener on element not in DOM yet
3. `pointer-events: none` in CSS — show Computed
4. Event Listeners pane empty — script never ran

**Predict before running:** If `getElementById` is `null`, what happens if you call `addEventListener` on it?

**Explain result:** **TypeError**. DevTools Console shows the stack. Fix the selector.

> **In the Real World:** Junior tickets at **Freshworks** often start with "repro in DevTools" — this is that skill.

---

## Recap (14 min)

Gallery walk: each pair shows list or tabs. Instructor picks one bug to debug live.

**[Script:]** "You now **mutate the tree**. Next: how the **network** brings data. Then you will **createElement from JSON**."

---

## Lecture Summary

- **Create, remove, replace** nodes while the page is running
- **Inline styles** vs **classList** for UI chrome
- **Dynamic lists and tabs** are DOM state machines
- **Traversal** connects buttons to their parent items
- **DevTools** proves what the tree and listeners actually are
- **Practical value:** This is the raw skill React later wraps — you can already ship small interactive widgets
