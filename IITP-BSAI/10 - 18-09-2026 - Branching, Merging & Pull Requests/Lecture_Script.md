# Lecture Script: Branching, Merging & Pull Requests
**Duration:** 110 minutes | **Tools:** VS Code Terminal, Git, GitHub | **Project:** Shared class portfolio repo

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & real-world hook | 8 min | How Stripe ships 100+ PRs/day |
| Why Does This Matter? | 14 min | Team workflows, career skills |
| What Is the Concept? | 22 min | Branches, merge, PR anatomy |
| How Do We Apply It? (LOs) | 50 min | Live branch → PR → merge lab |
| Conflict resolution clinic | 12 min | Guided conflict on shared file |
| Code review mini-workshop | 4 min | Author + reviewer roles |
| Recap & AI/React preview | 0 min | *(absorbed above)* |

---

## Session Opening (8 min)

**[Script:]** "Last session you put your portfolio on GitHub. One person. One branch. `main`. Today we level up to how **real teams** work — multiple people, multiple features, zero chaos. By the end of this session, you will create a feature branch, open a Pull Request, get it reviewed, and merge it. This is the workflow at every company from a 5-person startup to Google."

**Real-world hook:** "Stripe's engineering blog describes their default: no direct pushes to `main`. Every change goes through a Pull Request with review. Netflix, Razorpay, Swiggy — same pattern. You are learning the exact workflow recruiters expect on Day 1."

🎯 **Instructor Note:** Display a real merged PR from an open-source repo (e.g. `vercel/next.js` or a small educational repo). Point at: title, description, files changed, review comments, merge button.

---

## Why Does This Matter?

🎯 **Instructor Note:** Quick poll — "Who has ever lost work because two people edited the same file?" Collect 2–3 stories.

**[Script:]** "Without branches, every developer is editing the same line of code at the same time. Someone pushes. Someone else pushes. Someone loses work. With branches, you each have your own timeline. `main` stays stable. Features merge only when ready and reviewed."

**Real-world scenarios:**

| Company | Branching practice | Why |
|---------|-------------------|-----|
| **GitHub** | Feature branch per issue | Traceability, rollback |
| **Flipkart** | Release branches for big sales | Hotfix without breaking features |
| **Freshworks** | PR required before merge | Quality gate, knowledge sharing |

**Pain if misunderstood:**
- Pushing broken code to `main` → teammates blocked, deploy fails
- Never pulling → push rejected, confusing errors
- Huge PRs (50 files) → reviews take days, bugs slip through
- Ignoring conflicts → broken code silently merged

---

## What Is the Concept?

### Branches as Parallel Timelines

**Definition:** A branch is a movable pointer to a commit. `main` is the default branch. Feature branches diverge and rejoin via merge.

**Mental model:** Train tracks. `main` is the express line. Feature branches are sidings where work happens before rejoining.

```bash
git branch                    # list branches
git switch -c feature/nav     # create + switch
git switch main               # back to main
```

**[Script:]** "`git switch` replaced the older `git checkout` for changing branches. If you see checkout in tutorials, switch does the same job for branches."

### Merge — Rejoining Timelines

```bash
git switch main
git pull origin main          # get latest remote main
git merge feature/nav
git push origin main
```

**Fast-forward merge:** `main` had no new commits — Git simply moves the pointer forward.

**Three-way merge:** Both branches have new commits — Git creates a merge commit.

🎯 **Instructor Note:** Draw two lines diverging and rejoining on whiteboard. Label commits A, B, C.

### Pull Before Push

```bash
git pull origin main
```

**Definition:** `pull` = `fetch` + `merge`. Downloads remote commits and integrates them into your current branch.

> **In the Real World:** At **Atlassian** (makers of Jira), engineers sync with `main` multiple times per day. Stale branches = painful conflicts. Pull early, pull often.

### Pull Requests on GitHub

**Anatomy of a good PR:**

| Section | Content |
|---------|---------|
| Title | `Add responsive navigation to portfolio` |
| Description | What, why, how to test |
| Files changed | Usually < 15 for beginners |
| Reviewers | Mentor or classmate |
| Status checks | CI green (later in programme) |

**[Script:]** "A PR is a conversation, not just a button. The description helps your reviewer test in 2 minutes. Respect their time."

### Merge Conflicts

When two branches edit the same lines:

```css
<<<<<<< HEAD
color: navy;
=======
color: #1a1a2e;
>>>>>>> feature/dark-mode
```

**Resolution steps:**
1. Open conflicted file
2. Remove markers, keep correct code
3. `git add file`
4. `git commit` (or complete merge)

### Code Review — 60-Second Intro

**Reviewer asks:**
- Does it work as described?
- Is it readable?
- Is scope small enough?

**Author responds:** Fix or explain. Push to same branch — PR updates.

---

## How Do We Apply It?

### LO 1: Create and switch branches for a feature

**Real-world case study:** A Razorpay engineer needs to add UPI payment option. She does NOT edit `main`. She runs `git switch -c feature/upi-button`, builds the feature, commits daily. `main` stays deployable throughout.

**Problem:** Add a "Projects" section to portfolio without touching stable `main` directly.

**Live terminal lab:**

```bash
cd ~/projects/iitp-portfolio
git switch main
git pull origin main
git switch -c feature/projects-section
```

Edit `index.html` — add a `<section id="projects">` with two project cards.

```bash
git add index.html styles.css
git commit -m "Add projects section with two portfolio cards"
git log --oneline
```

**Predict before running:** What branch name appears in `git log` output? (feature/projects-section)

**Explain result:** Commits exist only on feature branch. `main` unchanged.

🎯 **Instructor Note:** Verify every student is on a feature branch with `git branch` showing `*`.

---

### LO 2: Merge a branch into main using standard workflow

**Problem:** Feature is done. Integrate into `main`.

**Translate logic:**
1. Switch to `main`
2. Pull latest
3. Merge feature branch
4. Push

```bash
git switch main
git pull origin main
git merge feature/projects-section
git push origin main
```

**Predict:** After merge, does `git log` show the feature commit on `main`?

**Explain result:** `main` now contains projects section. Feature branch can be deleted later.

> **In the Real World:** Some teams use **squash merge** on GitHub (combines all feature commits into one). Today we use standard merge; you will see squash in open source.

**Common mistakes:**
- Merging while on wrong branch — always check `git branch` first
- Forgetting pull — merge works locally but push fails

---

### LO 3: Open and merge a pull request on GitHub

**Problem:** Mentor cannot see your branch until pushed. PR enables review before merge.

**Steps:**
1. Push feature branch (if not merged locally yet, recreate branch for demo):

```bash
git switch -c feature/footer-update
# edit footer
git commit -am "Update footer with GitHub link"
git push -u origin feature/footer-update
```

2. GitHub → **Compare & pull request**
3. Title: `Update footer with GitHub profile link`
4. Description:

```markdown
## Changes
- Added GitHub icon link in footer

## Test steps
1. Open index.html
2. Scroll to footer
3. Click GitHub link — opens profile
```

5. Request review from mentor
6. After approval → **Merge pull request** → **Confirm**

**Predict:** After GitHub merge, what should you run locally? (`git switch main && git pull origin main`)

**Case study — Linux kernel:** Linus Torvalds receives thousands of PRs per year. Every change reviewed. Your footer PR is the same pattern at beginner scale.

---

### LO 4: Resolve one basic merge conflict with mentor guidance

**Problem:** You and a classmate both edit `styles.css` `body` background on different branches.

**Live conflict lab (instructor orchestrates):**

**Student A branch:**
```css
body { background-color: #f0f4f8; }
```

**Student B branch:**
```css
body { background-color: #1a1a2e; }
```

Both merge to `main` — second merge triggers conflict.

**Walkthrough:**
1. Git prints: `CONFLICT (content): Merge conflict in styles.css`
2. Open file — show markers
3. Discuss: which to keep? combine? (e.g. CSS variable for theming — preview next modules)
4. Remove markers, save
5. `git add styles.css && git commit -m "Resolve background color conflict — keep light theme"`

**[Script:]** "Conflicts are not failures. They are Git saying: 'Humans, decide.' Seniors resolve these calmly every day."

🎯 **Instructor Note:** Pair students — one creates conflict, partner resolves with mentor check.

---

### LO 5: Practise pull-before-push on shared exercise

**Problem:** Class shares one org repo. Student pushes without pull — rejected.

**Shared repo exercise:**
1. Instructor pushes a change to `README.md` on `main`
2. Students run `git pull origin main` before adding their line
3. Each student adds their name to CONTRIBUTORS section on a branch
4. Open PR, merge in order

**Predict:** What happens if Student 3 pushes without pulling after Student 2 merged? (Push rejected or conflict on next pull)

**Explain result:** `pull` integrates others' work first. Collaboration requires sync.

---

## Code Review Mini-Workshop (4 min)

🎯 **Instructor Note:** Show one student PR on screen. Class reviews in 2 minutes:

- [ ] Title clear?
- [ ] Test steps included?
- [ ] No unrelated file changes?
- [ ] Commit message meaningful?

**[Script:]** "As reviewer, be kind and specific: 'Line 42 — consider using a class instead of inline style' beats 'this is wrong.'"

---

## Live Lab Summary (integrated above)

| Lab | Duration | Deliverable |
|-----|----------|-------------|
| Feature branch creation | 12 min | `feature/projects-section` committed |
| Local merge workflow | 10 min | Merged to `main`, pushed |
| GitHub PR open + merge | 15 min | Merged PR visible on GitHub |
| Conflict resolution | 12 min | Resolved `styles.css` conflict |
| Shared repo pull exercise | 11 min | PR merged after pull |

---

## Lecture Summary

- **Branches** isolate feature work — `git switch -c` creates, `git merge` reunites
- **Pull before push** on shared repos prevents overwrite and reduces conflicts
- **Pull Requests** add review step before code hits `main` — industry standard at Stripe, GitHub, Flipkart
- **Merge conflicts** show markers — humans choose, then `add` + `commit`
- **Code review** is brief, kind, and focused on correctness and scope
- **Practical value:** This workflow is non-negotiable for team projects, open source, and your React deployment pipeline via GitHub → Vercel next month

**[Script:]** "Next session we set up Cursor and learn AI hygiene — because after today, you will use AI to build React components, and those changes still flow through branches and PRs. Same discipline, faster typing."

---

## Instructor Prep Checklist

- [ ] Shared GitHub org repo created with all students as collaborators
- [ ] Pre-made conflict scenario branches ready
- [ ] Sample merged PR bookmarked for opening demo
- [ ] Backup: pair students if GitHub access issues
- [ ] Slide or board diagram: branch → PR → merge flow
