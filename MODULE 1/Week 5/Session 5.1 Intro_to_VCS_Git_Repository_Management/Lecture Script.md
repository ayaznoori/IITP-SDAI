### 1. Start with an Activity ( 5 minutes)

* Present a scenario: student overwrites old working code with a new feature; needs old version back.
* Ask: how to recover the older code?
* Discuss why manual file copying is not reliable.
* Present a second scenario: multiple developers editing the same file leading to overwrite issues.
* Highlight the need for a systematic solution.

### 2. Introduce Version Control ( 3 minutes)

* Define Version Control as a system that tracks file changes over time.
* Mention capabilities: restoring old versions, collaboration without overwriting, history tracking, backup safety.

### 3. Explain How Version Control Works ( 5 minutes)

* Show the concept of saved snapshots/versions.
* Describe ability to switch between versions and compare changes.

### 4. Give an Analogy and Small Demo ( 5 minutes)

* Use the example of multiple drafts of a written document.
* Open a Google Doc/Sheet, make a few changes, and show:

  * Version History
  * Who edited what
  * Restore previous versions option
* Connect the analogy to coding: need a similar but more powerful system that works offline and supports branching.

### 5. Explain Problems Solved by Version Control ( 3 minutes)

* List practical issues avoided: overwrite conflicts, accidental deletion, no history, no reasoning for changes, local-machine dependency.
* Reinforce suitability for professional development workflows.

### 6. Announce a Short Break (5 minutes)

* Allow learners to mentally absorb the concepts presented so far.

### 7. Types of Version Control Systems (5 minutes)

* Briefly introduce:

  * Local VCS
  * Centralized VCS
  * Distributed VCS
* Emphasize evolution toward distributed systems.

### 8. Most Widely Used VCS ( 3 minutes)
 
* State that Git is the current global standard due to flexibility, offline capability, and collaboration features.

### 9. What Will Be Used in the Module ( 3 minutes)

* Introduce Git for version control.
* Introduce GitHub as the cloud-based remote repository platform.
* Clarify the difference between Git (tool) and GitHub (service).

### 10. Conclude the topic with Reflection ( 2 minutes)

* Ask students to reflect briefly on:

  * How they currently track different file versions
  * How they would restore old code today
  * How they would collaborate safely on shared code
* Reinforce Version Control as the professional solution.




---

### 1. Different Tools Available for Source Code Management ( 3 minutes)

- Display table of popular SCM tools.
- Briefly introduce each tool name and category (centralized or distributed).
- Emphasize that many tools exist in industry.

### 2. Why Git is Most Widely Used ( 3 minutes)

- Present reasons Git dominates current development practices.
- Highlight offline support, fast operations, strong branching, security, and community adoption.

---

### 3. What is Git? ( 3 minutes)

- Define Git as a local version control tool.
- Explain key abilities: tracking changes, commits, branches, merging.

### 4. Short Break (2–3 minutes)

- Allow learners to process what Git is and why it is essential.

### 5. What is GitHub? ( 3 minutes)

- Explain GitHub as an online hosting and collaboration platform.
- Clarify relationship and differences between Git and GitHub using a small comparison table.

---

### 6. Demonstrate GitHub Account Creation ( 5 minutes)

- Show GitHub signup process step-by-step.
- Instruct students to use professional names and email that will also be used in Git configuration.
- Explain importance of doing this before Git installation so contribution mapping works correctly.
- Wait 5 minutes for everyone to create an account.
- If anyone needs more time, ask them to continue later and focus back on the session.

### 7. Install Git ( 10 minutes)

- Guide installation steps based on operating systems:

  - Windows installer with default settings
  - macOS via Homebrew or Xcode tools
  - Linux using apt commands

- Confirm installation with `git --version`.
- Pause for 5–7 minutes for students to finish installation.

### 8. Basic Git Setup (After GitHub Account) ( 5 minutes)

- Configure username and email using Git config commands.
- Show how to verify using `git config --list`.
- Explain the need for matching GitHub identity for proper commit attribution.
- Pause for 10 minutes for students to complete setup.

---


---

## 1. **Set Context (3 minutes)**

- Keep Open VS Code project folder say 'First-Git-Project'. Keep terminal open”

---

## 2. **Demonstrate the 3-Area Model (Working → Staging → Repository) (6 minutes)**

### Show the image, explain how Working logic of Git

### **Step 1 — Show Working Directory**

“Create a new file named `demo.txt` in your project.
Type this line inside it:
`Hello Git`
Save the file.”

“Now go to the terminal and run:”

```bash
git status
```

**Tell them what to observe:**
“Notice Git says this file is _untracked_. That means it’s only in the **working directory**, not tracked by Git yet.”

---

### **Step 2 — Move a File to Staging**

“Now stage the file. Run:”

```bash
git add demo.txt
```

“Check status again:”

```bash
git status
```

“Observe the difference — now the file is in the **staging area**.
Git is ready to take a clean snapshot of it.”

---

### **Step 3 — Commit to Repository**

“Let’s save this snapshot permanently. Run:”

```bash
git commit -m "Add demo.txt with initial text"
```

“Now that snapshot is stored in your **local repository**.”

---

## **Explore `git status` — Live Feedback Loop**

“Make a small edit in `demo.txt`. Add one more line:
`Learning staging`
Save it.”

“Run:”

```bash
git status
```

“Point out what Git shows: unstaged changes.
This is your real-time dashboard — use it constantly.”

---

## 3. **Repeat the process with another example and allow students to practice the same ( 5 minutes)**

## 4. **Practice Staging Variants (3 minutes)**

“Try staging everything with:”

```bash
git add .
```

“Undo that using:”

```bash
git reset
```

“Now stage only updates (no new files):”

```bash
git add -u
```

Explain while demonstrating:
“These variations help you control _what exactly_ goes into a commit.”

---

## 5. **Commit Cleanly — Commanding Good Messages**

“Now commit again. Use a meaningful message:”

```bash
git commit -m "Update demo.txt with learning note"
```

“Short, action-based, present tense. No vague ‘final commit’ messages in this class.”

---

## 6. **View History Using `git log`**

“Run:”

```bash
git log --oneline
```

“Keep the output open. Explain each commit represents a snapshot of your project at a moment in time.”

---

## 7. **Compare Changes Using `git diff` (3 minutes)**

### **A. Compare Working Directory vs Staged**

“Modify the file again.
Add: `Diff practice`
Save it.”

“Now run:”

```bash
git diff
```

“Explain: this shows what is changed but _not yet staged_.”

---

### **B. Compare Staged vs Last Commit**

“Stage it:”

```bash
git add demo.txt
```

“Now run:”

```bash
git diff --staged
```

“This shows what will be committed.”

---

### **C. Compare Two Commits**

“Copy two commit IDs from `git log --oneline` and run something like:”

```bash
git diff a1b2c3d f4e5d6a
```

“Explain how Git highlights additions (+) and deletions (−).”

---

## 8. **Full Workflow Practice — Students Perform (5 minutes)**

Let students tries with another example,
Give the following command sequence as a drill:

1. “Edit any file.”
2. “Run `git status`.”
3. “Stage using `git add <file>`.”
4. “Check with `git diff --staged`.”
5. “Commit with a meaningful message.”
6. “View your commit using `git log --oneline`.”

Walk around (or observe screens) while they perform.


---

# **Undoing Working Directory Changes — `git restore` (3 minutes)**

“Sometimes you make edits you didn’t intend, or you break something and want to go back to the last clean version. Git allows you to safely discard uncommitted changes without affecting your work history.”

“Before we start, modify a file. Open `app.js`, add some random text, and save it.”

“Check what Git sees:”

```bash
git status
```

“You should see `modified: app.js`.
Now watch what happens when we restore it back to the last committed version.”

```bash
git restore app.js
```

“Open the file again — everything is back to the committed state.
That’s how you safely throw away unwanted edits.”

---

### **Now repeat the same flow with two more examples (5 minutes)**

**Example 2 — Another Single File**

1. “Open `index.html`, make an obvious change, save it.”

2. “Run `git status` to see the modification.”

3. “Undo it using:”

   ```bash
   git restore index.html
   ```

4. “Verify the file is back to normal.”

---

**Example 3 — Restore Entire Project**

1. “Modify _two_ different files in the project.”

2. “Run `git status`.”

3. “Now restore everything at once:”

   ```bash
   git restore .
   ```

4. “Observe that all local edits are gone.”

---

# **Unstaging Changes — `git reset` or `git restore --staged` (3 minutes)**

“There are times you add files to staging that shouldn’t be part of the next commit. Git lets you pull them back safely without losing the changes.”

“Let’s try it together. Edit `config.js`, save it, and stage it:”

```bash
git add config.js
```

“Check its status:”

```bash
git status
```

“It’s staged. Now unstage it while keeping the edits intact:”

```bash
git reset config.js
```

OR

```bash
git restore --staged config.js
```

“Run `git status` again — notice the file is now only modified, not staged.”

---

### **Repeat same flow with two more examples (5 minutes)**

**Example 2 — Using reset**

1. “Edit `styles.css`. Save it.”

2. “Stage it: `git add styles.css`”

3. “Unstage it:”

   ```bash
   git reset styles.css
   ```

4. “Confirm edits are still present.”

---

**Example 3 — Unstage Everything**

1. “Stage a bunch of files together:”

   ```bash
   git add .
   ```

2. “Now remove _all_ files from staging:”

   ```bash
   git reset
   ```

3. “Check with `git status` — everything should be back to modified state.”

---

# **Updating the Last Commit — `git commit --amend` (3 minutes)**

“Sometimes you commit too early—maybe you forgot a file or made a typo in the message. Git lets you update the most recent commit before pushing it.”

“Let’s try this. Make a commit with a deliberate mistake in the message:”

```bash
git commit -m "Addd login page"
```

“Now fix the commit message:”

```bash
git commit --amend -m "Add login page"
```

“Notice the new commit replaces the old one with a corrected message.”

---

### **Adding missing files to the last commit (3 minutes)**

1. “Create or edit `validation.js`.”

2. “Stage it:”

   ```bash
   git add validation.js
   ```

3. “Now update the previous commit contents:”

   ```bash
   git commit --amend
   ```

4. “Explain: this rewrites the last commit with both message and files fixed.”

---

### **Repeat same flow with two more examples (5 minutes)**

**Example 2 — Fix wrong commit message**

1. “Make another commit with a bad message.”
2. “Correct it using:”

   ```bash
   git commit --amend -m "Corrected commit message"
   ```

---

**Example 3 — Add a missing test file**

1. “Create or update `login.test.js`.”
2. “Stage it: `git add login.test.js`”
3. “Amend the previous commit:”

   ```bash
   git commit --amend
   ```

---

# **Closing Demonstration — Choosing the Correct Undo Action (7 minutes)**

“Now we will practice selecting the right command based on the situation.
Follow along with this scenario-based flow.”

### **Scenario A — You want to discard edits**

```bash
git restore <file>
```

### **Scenario B — You staged something accidentally**

```bash
git reset <file>
```

### **Scenario C — You want to fix the last commit**

```bash
git commit --amend
```

After describing each, ask students:

**“Repeat each scenario using different files in your project. You must execute all three flows again with your own examples.”**


---

