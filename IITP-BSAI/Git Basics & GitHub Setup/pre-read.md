# Pre-Read: Git Basics & GitHub Setup

## 1. What You'll Learn

In this pre-read, you'll discover:

- **Why every professional developer** uses version control — with real team scenarios
- How Git **snapshots your project** so you can recover, compare, and collaborate
- How to run **`git init`, `add`, `commit`, and `log`** on a real web project folder
- How to **push your portfolio or DOM lab** to GitHub for mentors and recruiters to see
- How to **clone** a starter repo and work from a template
- How **`.gitignore`, README, and commit messages`** separate beginners from job-ready developers

---

## 2. Detailed Explanation

### Why Version Control Exists

Imagine you and a teammate both edit `app.js` on Friday night. You email files back and forth:

```
app_v3.js
app_v3_ayaz.js
app_v3_ayaz_FINAL.js
app_v3_ayaz_FINAL_use_this.js
```

Someone will overwrite someone else. Someone will lose work. No one knows what changed.

**Version control** (a system that records every change to your codebase over time) fixes this. **Git** is the industry standard. **GitHub** hosts Git repos online.

> **In the Real World:** At companies like **Flipkart**, **Razorpay**, and **Swiggy**, every line of production code lives in Git. Deployments are triggered from specific commits on `main`. You cannot intern or join a team without Git fluency.

**Analogy:** Git is **Google Docs version history**, but designed for code — with branches, merges, and team workflows you will learn next session.

### The Three States of Git

| State | What it means | Command |
|-------|---------------|---------|
| **Working directory** | Files you are editing right now | — |
| **Staging area** | Files marked for next snapshot | `git add` |
| **Repository** | Committed history | `git commit` |

**Mental model:** Packing a shipment. You choose items (`add`), seal the box (`commit`), ship it (`push`).

### Essential Local Commands

```bash
git init              # Start tracking this folder (once)
git status            # What changed? What's staged?
git add index.html    # Stage one file
git add .             # Stage all changes (check status first!)
git commit -m "Add portfolio HTML structure"
git log               # History of commits
git log --oneline     # Compact history
```

**Good commit message rule:** Complete this sentence — *"This commit will ___"*

- ✅ `Add responsive nav with Flexbox`
- ✅ `Fix broken contact form label`
- ❌ `update`
- ❌ `changes`

### Local vs Remote

| Term | Where | Purpose |
|------|-------|---------|
| **Local repo** | Your laptop | Fast commits, offline work |
| **Remote repo** | GitHub servers | Backup, sharing, collaboration |
| **push** | Local → Remote | Upload your commits |
| **clone** | Remote → Local | Download full project + history |

### Connecting to GitHub (Step by Step)

1. Create account at [github.com](https://github.com)
2. Click **New repository** — name it `my-portfolio` (example)
3. Do **not** add README if you already have local files
4. Copy HTTPS URL: `https://github.com/YOUR_USERNAME/my-portfolio.git`
5. In your project folder:

```bash
git remote add origin https://github.com/YOUR_USERNAME/my-portfolio.git
git branch -M main
git push -u origin main
```

After this, refresh GitHub — your files appear online.

### Cloning

When a company or course gives you a starter template:

```bash
git clone https://github.com/org/starter-template.git
cd starter-template
```

You get the full project and all history. This is how new hires join existing codebases.

### Repository Hygiene

**`.gitignore`** — tells Git what **never** to track:

```
node_modules/
.DS_Store
.env
*.log
dist/
```

> **In the Real World:** Committing `.env` with API keys has caused real data breaches. Teams use `.gitignore` from day one. **Never commit secrets.**

**`README.md`** — first file recruiters open:

```markdown
# My Portfolio

Personal portfolio built in IITP Web Fundamentals module.

## Live Demo
https://your-vercel-url.vercel.app (add later)

## Tech Stack
HTML, CSS, JavaScript

## Run Locally
Open index.html in browser, or use Live Server in VS Code.
```

### Why It Matters for Your Web Projects

You built HTML, CSS, and DOM labs locally. Without GitHub:

- Laptop dies → project gone
- Mentor cannot review your code
- Recruiter asks for GitHub link → you have nothing

With GitHub:

- Cloud backup
- Shareable portfolio URL
- Proof of steady progress through commit history

**Benefits:**
- Recover any previous version
- Show work in interviews
- Prepare for team PR workflow next session

### Messy to Clear

**Messy repo on GitHub:**
- 1 commit: "initial"
- `node_modules/` folder uploaded (10,000 files)
- No README
- File named `project (1).zip`

**Professional repo:**
- 15+ commits with clear messages
- `.gitignore` from first commit
- README with setup steps
- Clean folder structure

### Building Blocks Checklist

- [ ] I can explain version control in one sentence
- [ ] I know difference between `add` and `commit`
- [ ] I have a GitHub account
- [ ] I know what `.gitignore` protects
- [ ] I can write a README with run instructions

---

## 3. Practice Exercises

**Exercise 1 — Local Git**
Initialize Git in your portfolio folder. Commit `index.html` with message `Add portfolio HTML skeleton`. Run `git log`.

**Exercise 2 — .gitignore**
Create `.gitignore` ignoring `.DS_Store` and any `*.log` files. Verify with `git status` that ignored files do not appear.

**Exercise 3 — GitHub push**
Create GitHub repo and push your portfolio. Open in browser and confirm files match local.

**Exercise 4 — README**
Write README with: project title, 2-sentence description, tech stack list, how to open locally.

**Exercise 5 — Clone**
Clone any public educational repo (e.g. a simple HTML template). Run `git log --oneline` and note how many commits exist.
