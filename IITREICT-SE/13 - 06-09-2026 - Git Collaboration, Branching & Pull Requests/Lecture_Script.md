# Lecture Script: Git Collaboration, Branching & Pull Requests
**Duration:** 110 minutes | **Tools:** Git, GitHub, VS Code | **Context:** Shared class organization repo or pairs repo

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Parallel work problem |
| Why Does This Matter? | 12 min | Team workflows, code review |
| What Is the Concept? | 22 min | Branches, merge, PRs, conflicts |
| How Do We Apply It? (LOs) | 55 min | Live branch → PR → merge |
| Code review role-play | 10 min | Author vs reviewer |
| Recap & summary | 6 min | Module 1 Git complete |

---

## Session Opening (5 min)

**[Script:]** "Yesterday you saved snapshots and pushed to GitHub solo. Real products have multiple developers. Two people editing `main` at once is like two people editing one Google Doc without track changes — things break. Branches and pull requests fix that."

**Problem hook:** Diagram two devs committing to `main` — last push wins, first dev's work overwritten. "Branches give each dev a lane. PRs are the merge gate."

🎯 **Instructor Note:** Use a shared **class sandbox repo** with mentor admin access, or pairs with one repo owner — plan before class.

---

## Why Does This Matter?

🎯 **Instructor Note:** "Has anyone had a group project where someone emailed the wrong file version?" Connect to lived experience.

**[Script:]** "Every internship and job uses branch + PR workflow. Saying 'I know Git' in an interview means you can branch, open a PR, respond to review comments, and resolve a basic conflict — not just `git commit`."

**Real-world use:**
- Feature development isolated from production
- Code review before merge
- CI checks on PRs (preview for later modules)

**Pain if misunderstood:**
- Long-lived branches — painful merges
- Merging without pull — overwrite remote work
- Ignoring conflict markers — broken code in `main`
- Giant PRs — reviewers miss bugs

---

## What Is the Concept?

### Branches

**Definition:** A branch is a movable pointer to a line of commits.

**`main`:** Stable, deployable code (convention).

**Feature branch:** Short-lived workspace for one task.

```bash
git switch main
git pull origin main
git switch -c feature-add-greeting
```

**Mental model:** Parallel universes. Merge picks how universes combine.

### Merging

```bash
git switch main
git merge feature-add-greeting
```

**Fast-forward:** `main` simply moves forward if no divergence.

**Merge commit:** Created when both branches had unique commits.

### Merge Conflicts

**Definition:** Same lines changed differently on both branches.

**Markers:**

```
<<<<<<< HEAD
message = "Hello"
=======
message = "Hi"
>>>>>>> feature-add-greeting
```

**Resolution steps:**
1. Open file — read both sides
2. Choose correct code (or combine)
3. Remove markers
4. `git add file`
5. `git commit` (merge commit)

🎯 **Instructor Note:** **Intentionally create** a simple conflict in demo repo — students watch resolution live.

### Pull Requests

**Definition:** GitHub UI to propose merging a branch, discuss diff, and review before merge.

**Flow:**
1. Push branch: `git push -u origin feature-add-greeting`
2. GitHub → Open PR into `main`
3. Reviewer comments
4. Author pushes fixes to same branch — PR updates
5. Merge button on GitHub
6. Local: `git switch main && git pull`

**[Script:]** "A PR is a conversation attached to a diff. The merge button is the contract: we agree this code enters `main`."

### Code Review Basics

**Reviewer checks:**
- Does it solve the stated problem?
- Readable names and structure?
- Obvious bugs or edge cases?
- No secrets or `venv/` committed?

**Author etiquette:** Respond to comments, push fixes, keep PR scope small.

---

## How Do We Apply It?

### LO 1: Create and switch branches for parallel development

**Problem:** Add greeting feature without touching stable `main` until ready.

**Commands:**

```bash
git switch main
git pull origin main
git switch -c feature-greeting
```

Edit `greetings.py`:

```python
def greet(name):
    return f"Hello, {name}!"
```

```bash
git add greetings.py
git commit -m "Add greet function"
git push -u origin feature-greeting
```

**Predict:** Does `main` contain `greetings.py` yet?

**Explain result:** No — commit exists only on feature branch until merge.

---

### LO 2: Merge branches using standard Git workflow

**Problem:** Integrate feature into `main` locally (mentor may also show GitHub merge).

**Option A — local merge:**

```bash
git switch main
git pull origin main
git merge feature-greeting
git push origin main
```

**Predict:** After merge, does `main` have `greet`?

**Explain result:** Yes — branch history combined.

🎯 **Instructor Note:** Prefer **GitHub PR merge** as primary industry pattern; local merge as supplementary understanding.

---

### LO 3: Open, review, and merge a pull request on GitHub

**Problem:** Team requires PR before `main` changes.

**Live GitHub walkthrough:**
1. Push branch (from LO 1)
2. Yellow banner → **Compare & pull request**
3. Title: `Add greet function`
4. Description: what, why, how to test
5. Assign reviewer (classmate)
6. Reviewer approves
7. **Squash and merge** or **Merge commit** — explain either is OK at this level
8. Delete branch on GitHub (hygiene)

**Predict:** What happens to PR if author pushes another commit to the branch?

**Explain result:** PR updates automatically with new diff.

---

### LO 4: Identify and resolve a basic merge conflict

**Problem:** Two branches edit same line in `config.py`.

**Setup (mentor pre-seeds or live):**
- `main` has `APP_NAME = "Utility"`
- Branch changes to `APP_NAME = "Text Utility"`
- `main` also got commit changing to `APP_NAME = "My Utility"`

**Resolution (narrated):**

```python
# Resolved:
APP_NAME = "Text Utility"
```

```bash
git add config.py
git commit -m "Resolve APP_NAME conflict keeping feature name"
```

**Predict:** Can you commit merge without `git add` after fixing?

**Explain result:** No — Git must know conflict is resolved.

🎯 **Instructor Note:** Pair exercise — each pair resolves identical conflict in sandbox repo.

---

### LO 5: Participate in code review as author and reviewer

**Problem:** PR merged with bug — `greet()` crashes on empty string.

**Role-play:**
- **Author** opens PR with known edge-case gap
- **Reviewer** comments: "What if `name` is empty?"
- **Author** pushes fix commit
- **Reviewer** approves

**Review comment examples (on board):**
- "Consider empty input — line 2"
- "Nice function name — readable"
- "Add one-line docstring?"

**[Script:]** "Review is not personal attack. It is quality control for the product."

---

## Code Review Role-Play (10 min)

Rotate: each student reviews one PR checklist item on a neighbor's real GitHub PR from today.

---

## Recap (6 min)

🎯 **Instructor Note:** Quiz — `git switch -c` vs `git switch`? When to `pull`? What is a PR?

---

## Lecture Summary

- **Branches** isolate feature work from stable `main`
- **Merge** integrates branch history — conflicts need manual resolution
- **Pull requests** add review and discussion before code enters `main`
- **Conflict markers** must be fully resolved before commit
- **Code review** improves quality and is standard industry practice
- **Practical value:** Branch + PR workflow is how you will ship code on every professional team

**[Script:]** "Module 1 developer setup — complete. You write Python, structure projects, handle JSON, and collaborate with Git. Module 2 opens the web: HTML today in your next sessions, then CSS and JavaScript. Git travels with you for all of it."
