# Lecture Script: Git Fundamentals & Repository Management
**Duration:** 110 minutes | **Tools:** VS Code Terminal, Git, GitHub | **Context:** Python projects from prior sessions

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & hook | 5 min | Zip files vs version control |
| Why Does This Matter? | 15 min | Industry workflow, portfolio |
| What Is the Concept? | 25 min | Git model, commands, GitHub |
| How Do We Apply It? (LOs) | 50 min | Live repo from scratch to push |
| Hygiene workshop | 10 min | .gitignore, README, messages |
| Recap & summary | 5 min | LO review + branching preview |

---

## Session Opening (5 min)

**[Script:]** "You have written Python in One Compiler, then locally with VS Code, venv, modules, and JSON files. Where do those files live? On your laptop. What happens when you delete a folder by mistake, or your teammate needs the same code? Today we learn Git — the system every software team uses to track, share, and recover code."

**Problem hook:** Show folder with `project_v1`, `project_v2_backup`, `project_FINAL`. "Git replaces this chaos with one folder and a time machine."

🎯 **Instructor Note:** Confirm Git is installed: `git --version`. Help installs only if blocking — do not consume whole session.

---

## Why Does This Matter?

🎯 **Instructor Note:** Ask — "Who has used Google Docs version history?" Git is that, purpose-built for code.

**[Script:]** "Your capstone, portfolio, and internship submissions will be on GitHub. Recruiters look at repos. Messy history and missing READMEs signal inexperience. Clean Git habits signal you can join a team on day one."

**Real-world use:**
- Backup and recovery of project states
- Sharing code with mentors and pair partners
- Deployment pipelines that pull from GitHub (later modules)

**Pain if misunderstood:**
- Committing `venv/` or `.env` — leaks secrets or bloats repo
- Vague commit messages — impossible to debug later
- No remote backup — laptop failure = project gone
- `git add .` without checking — accidental large files

---

## What Is the Concept?

### Version Control and Git's Snapshot Model

**Definition:** Git records **commits** — snapshots of your project at points in time, with metadata (author, date, message).

**Mental model:** Photo album of your codebase. Each photo is a commit. You can flip back to any photo.

**Three areas:**
1. **Working directory** — files you edit
2. **Staging area** — files marked for next commit (`git add`)
3. **Repository** — committed history

### Core Local Commands

| Command | Purpose |
|---------|---------|
| `git init` | Create repo in current folder |
| `git status` | See changed/untracked files |
| `git add <file>` | Stage changes |
| `git commit -m "msg"` | Save snapshot |
| `git log` | View history |
| `git log --oneline` | Compact history |

**[Script:]** "Commit often with small logical changes. One commit per finished thought — not one commit per month."

🎯 **Instructor Note:** Live `git status` after every step — students must read status output fluently.

### Local vs Remote and GitHub

**Remote:** Hosted copy on GitHub.

| Command | Purpose |
|---------|---------|
| `git remote add origin URL` | Link remote |
| `git push -u origin main` | Upload commits |
| `git clone URL` | Download repo |
| `git pull` | Fetch and merge remote changes |

**Common mistakes:**
- Pushing before first commit — nothing to push
- Wrong remote URL — authentication or 404 errors
- Creating GitHub repo with README then trying to push unrelated local history without pull/merge (mention — handle if it arises)

### Repository Hygiene

**`.gitignore`:** Patterns for files Git should ignore.

```
venv/
__pycache__/
.env
.DS_Store
*.pyc
data/*.tmp
```

**README.md sections:**
- Project name and description
- Setup (venv, pip install)
- How to run
- Author / course (optional)

**Commit message format:** Imperative mood — "Add", "Fix", "Update", not "Added" or "Fixed stuff".

---

## How Do We Apply It?

### LO 1: Explain why version control matters and how Git tracks changes

**Problem:** Student lost a working function after a bad edit.

**Translate logic:** Without Git — manual copies. With Git — `git log`, find commit, restore file.

**Demo narrative (no full command dump — show concept):**
1. Commit version A
2. Break code in version B
3. `git log` shows both
4. `git checkout <hash> -- file.py` or `git restore` (mention restore as modern command)

**Predict:** Can you name the exact hour you saved version A without Git?

**Explain result:** Commits timestamp and message create recoverable history.

🎯 **Instructor Note:** Do not deep-dive detached HEAD — keep recovery intuitive for beginners.

---

### LO 2: Initialize repo and use init, add, commit, log

**Problem:** Track a small Python utility from Session 10/11.

**Live terminal workflow:**

```bash
cd ~/projects/text-utility
git init
git status
```

Create or confirm `main.py` exists.

```bash
git add main.py
git commit -m "Add text utility entry point"
git log --oneline
```

**Predict before commit:** What does `git status` show after `add` but before `commit`?

**Explain result:** Staged changes ready; after commit, working tree clean for that file.

**Second commit demo:**

```bash
# edit main.py — add comment
git add main.py
git commit -m "Document main function purpose"
git log --oneline
```

**Predict:** How many commits in log?

---

### LO 3: Connect local repository to GitHub and push

**Problem:** Mentor cannot see your code on your laptop only.

**Steps (narrated live):**
1. GitHub → New repository → `text-utility` → no auto README if local exists
2. Copy HTTPS URL
3. Terminal:

```bash
git remote add origin https://github.com/USERNAME/text-utility.git
git branch -M main
git push -u origin main
```

**Predict:** After push, what appears on GitHub web UI?

**Explain result:** Files and commit visible remotely; `-u` sets upstream for future `git push`.

🎯 **Instructor Note:** Authentication — PAT or SSH — have fallback doc ready; pair students who succeed with those stuck.

---

### LO 4: Clone existing remote repository and work locally

**Problem:** Start from a course starter template on GitHub.

**Write/do:**

```bash
cd ~/projects
git clone https://github.com/ORG/starter-template.git
cd starter-template
ls
git log --oneline
```

Edit `README.md`, then:

```bash
git add README.md
git commit -m "Add student name to README"
```

**Predict:** Does clone include `.git` folder and full history?

**Explain result:** Yes — clone is full copy with remote `origin` already configured.

---

### LO 5: Apply repository hygiene — .gitignore, commit messages, README

**Problem:** Accidentally staged `venv/` — 10,000 files.

**Translate logic:** Add `.gitignore` before first push; write README; use clear commits.

**Create `.gitignore`:**

```
venv/
__pycache__/
.env
.DS_Store
```

**README template (fill live):**

```markdown
# Text Utility

Reads and writes plain-text data files.

## Setup
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

## Run
python main.py
```

```bash
git add .gitignore README.md
git commit -m "Add gitignore and project README"
git push
```

**Predict:** Does `venv/` appear in `git status` after gitignore?

**Explain result:** Ignored files hidden from staging — verify with `git status`.

🎯 **Instructor Note:** Show bad commit message slide vs good — class suggests improvement.

---

## Hygiene Workshop (10 min)

Pairs review each other's GitHub repo:
- [ ] README exists and has run instructions
- [ ] `.gitignore` present
- [ ] No `venv/` in file list on GitHub
- [ ] At least 2 commits with clear messages

---

## Recap (5 min)

**[Script:]** "Next session: branches and pull requests — how teams work in parallel without chaos."

---

## Lecture Summary

- **Version control** prevents copy-paste file naming disasters and enables recovery
- **`init`, `add`, `commit`, `log`** are the local daily Git loop
- **GitHub remote** backs up and shares code via `push` and `clone`
- **`.gitignore` and README`** separate professional repos from student accidents
- **Clear commit messages** document intent for your future self and reviewers
- **Practical value:** Git + GitHub is non-negotiable for modern software careers — you now have the foundation
