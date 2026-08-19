# Lecture Script: Git Basics & GitHub Setup
**Duration:** 110 minutes | **Tools:** VS Code Terminal, Git, GitHub | **Project:** Student portfolio from Module 2

---

## Session Timeline Overview

| Block | Time | Focus |
|-------|------|-------|
| Opening & real-world hook | 8 min | Production deploys from Git |
| Why Does This Matter? | 15 min | Careers, teams, disasters |
| What Is the Concept? | 25 min | Git model, commands, GitHub |
| How Do We Apply It? (LOs) | 48 min | Portfolio repo live on GitHub |
| Hygiene clinic | 10 min | .gitignore, README, messages |
| Recap & branching preview | 4 min | Next session teaser |

---

## Session Opening (8 min)

**[Script:]** "You built a real webpage — HTML structure, CSS layout, JavaScript interactivity. It lives on your laptop. Today we answer: how do professionals **save**, **share**, and **prove** their work? The answer is Git and GitHub. This is not a side topic. This is how code ships at every company you admire."

**Real-world hook:** "When Netflix deploys a fix at 2 AM, they are not copying files over USB. They merge a reviewed commit on GitHub, CI runs tests, and production updates. You start with the same foundation today — one folder, one repo, one push."

🎯 **Instructor Note:** Poll — "Who has GitHub account?" Pair account-setup helpers with ready students.

---

## Why Does This Matter?

🎯 **Instructor Note:** Show a real open-source repo on GitHub (e.g. `facebook/react` or a smaller educational repo) — point at commit history, README, contributors.

**[Script:]** "Recruiters ask for GitHub within the first five minutes of many tech interviews. Not because they will read every line — but because your repos show: Do you commit regularly? Do you write READMEs? Do you understand hygiene? Empty GitHub after six months of learning is a red flag. Active repos with clear history is a green flag."

**Real-world scenarios:**

| Scenario | Without Git | With Git |
|----------|-------------|----------|
| Laptop stolen | Project lost | Clone from GitHub |
| Mentor review | Screenshot hell | Share repo URL |
| Broke working code | Panic, rewrite | `git log`, restore file |
| Team of 3 devs | Email chaos | Branches + PRs (next session) |

**Pain if misunderstood:**
- Pushing `node_modules/` — repo bloat, slow clones
- Committing `.env` with API keys — security incident
- Vague commits — cannot bisect bugs in real teams
- No remote — single point of failure on one machine

---

## What Is the Concept?

### Version Control Mental Model

**Definition:** Git stores **commits** — snapshots with author, timestamp, message, and parent commit(s).

**Analogy:** Chapter bookmarks in a textbook. Each commit is a bookmark you can return to.

### Core Commands (Live Board)

```bash
git init
git status
git add <file>
git commit -m "message"
git log --oneline
```

**[Script:]** "`status` is your best friend. Run it when confused. It tells you what Git sees."

### Local vs Remote

> **In the Real World:** **Vercel** and **Netlify** (which you will use later) connect to GitHub. Push to `main` → site redeploys. Your GitHub repo becomes the **source of truth** for live products.

```bash
git remote add origin URL
git push -u origin main
git clone URL
```

### Hygiene Files

**`.gitignore`** — patterns excluded from tracking

**`README.md`** — project front door for humans

🎯 **Instructor Note:** Show real `.gitignore` from a popular repo. Point out `node_modules`, `.env`.

---

## How Do We Apply It?

### LO 1: Explain why version control matters

**Real-world case study (narrate):** A startup founder edits production CSS directly on server. No Git. New hire breaks homepage. No way to roll back. Downtime costs sales. **Lesson:** version control is insurance.

**Discussion (5 min):** Groups list 3 disasters Git prevents.

**Predict:** Can you undo a bad edit without version control easily? (Only manual backups.)

---

### LO 2: Initialize repo and use init, add, commit, log

**Problem:** Track portfolio project from Module 2.

**Live terminal:**

```bash
cd ~/projects/portfolio
git init
git status
git add index.html styles.css app.js
git commit -m "Add portfolio HTML, CSS, and DOM script"
git log --oneline
```

**Predict:** After commit, what does `git status` show?

**Explain result:** Clean working tree for committed files.

**Second commit demo:**

```bash
# edit README.md
git add README.md
git commit -m "Add project README with setup steps"
```

> **In the Real World:** Engineers at **Google** and **Microsoft** make dozens of small commits per day. Small commits = easier reviews and debugging.

---

### LO 3: Connect local repo to GitHub and push

**Problem:** Mentor needs URL, not ZIP file.

**Steps:**
1. GitHub → New repo `iitp-portfolio-username`
2. Copy HTTPS URL
3. Terminal:

```bash
git remote add origin https://github.com/USERNAME/iitp-portfolio-username.git
git branch -M main
git push -u origin main
```

**Predict:** What appears on GitHub after push?

**Verify:** Refresh browser — files visible, commit message shown.

🎯 **Instructor Note:** Auth issues — have PAT/SSH troubleshooting doc ready.

---

### LO 4: Clone existing remote repository

**Problem:** Course provides `masai-web-starter` template.

```bash
git clone https://github.com/example/masai-web-starter.git
cd masai-web-starter
ls
git log --oneline
```

**Predict:** Does clone include `.git` folder?

**Explain result:** Yes — full history + `origin` remote configured.

**Exercise:** Add your name to README, commit locally (push optional if no write access).

---

### LO 5: Apply hygiene — .gitignore, commit messages, README

**Problem:** Student accidentally stages `.DS_Store` and `secrets.env`.

**Create `.gitignore`:**

```
.DS_Store
.env
*.log
node_modules/
```

**README template (fill live):**

```markdown
# [Your Name] — Web Foundations Portfolio

Built during IITP BSAI Module 2.

## Features
- Semantic HTML portfolio
- CSS Flexbox/Grid layout
- DOM interactivity (theme toggle / counter)

## Run
Open index.html or use Live Server in VS Code.
```

```bash
git add .gitignore README.md
git commit -m "Add gitignore and README for portfolio repo"
git push
```

**Predict:** Will `.env` appear on GitHub if listed in `.gitignore`?

**Explain result:** No — Git never tracks ignored files.

---

## Hygiene Clinic (10 min)

**Bad vs good commit messages** — class rewrites 5 bad messages.

**Repo audit pairs:**
- [ ] README exists
- [ ] No secrets in file list
- [ ] 3+ meaningful commits
- [ ] Portfolio files present

---

## Recap (4 min)

**[Script:]** "Next session: branches and pull requests — how **Razorpay-scale teams** work in parallel without breaking `main`."

---

## Lecture Summary

- **Version control** is industry infrastructure, not optional tooling
- **`init`, `add`, `commit`, `log`** form the daily local Git loop
- **GitHub remote** backs up and shares your web portfolio publicly
- **`clone`** downloads templates and team projects with full history
- **`.gitignore`, README, clear commits** signal professional habits to recruiters
- **Practical value:** Your Module 2 project becomes a shareable, recoverable, hireable artifact today
