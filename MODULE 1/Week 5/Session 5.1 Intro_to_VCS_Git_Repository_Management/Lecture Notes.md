## 1. Introduction: The Real-World Need for Version Control

Software development involves continuous changes, improvements, and fixes. Without a system to manage these changes, many issues arise. Two common examples are:

### a) Reverting to an Older Working Version

A developer adds new features and overwrites the existing code. Later, that feature causes bugs or is no longer required. Without historical records of older code:

* It becomes impossible to restore the previous working state
* Debugging becomes difficult and time-consuming
* Project timelines are affected

Key Questions:

* How do we get back the older version?
* Is copying files manually a professional and reliable method?

### b) Collaborative Development Challenges

Modern software is built by teams. Multiple developers may need to modify the same file.

Problems with simple IDE/file setups:

* High chance of overwriting each other’s work
* No tracking of who made specific changes
* No conflict management
* Local-only storage leading to risk of loss

Key Questions:

![Version Control](https://coding-platform.s3.amazonaws.com/dev/lms/tickets/8791fbc9-33ca-421b-ad68-a07d204fca84/qDCpbqCh13XUa0wH.png)

* How can multiple developers safely work on the same codebase?
* How can we synchronize contributions efficiently?

Both scenarios demonstrate the essential need for **Version Control Systems (VCS)** to ensure code safety, proper history, and reliable teamwork.


---

## 2. What is Version Control?


A Version Control System is a software tool that helps manage changes to project files over time. It provides the ability to:

* Track and store every modification made to the code
* Review change history along with author and reasoning
* Restore previous working versions when needed
* Support multiple contributors without overwriting work
* Maintain secure backups of the codebase

VCS enables systematic evolution of the project while preventing errors from becoming permanent.


---

## 3. How Version Control Works

VCS records project history in the form of versions or snapshots. Each saved change creates a new version.

Example:

```
Version 1 → Version 2 → Version 3 → Version 4 → ...
(Stable)     (Bug introduced) (Fix)     (Feature added)
```

![How Version Contraol Works](https://coding-platform.s3.amazonaws.com/dev/lms/tickets/ea91bbca-3caf-4d8e-b228-6f4b8380d69b/y2WUAoqPHCZheOkq.png)

Capabilities enabled by VCS:

* Move to any past stable version if needed
* Compare versions to identify the source of issues
* Selectively restore changes rather than redoing work

Thus, Version Control acts as a timeline for the project, keeping complete control over the evolution of code.

---

## 4. Version Control Analogy

Writing a report or research paper involves multiple drafts (e.g., draft1, draft2, draft3). We keep older drafts because:

* They may contain important content
* They can be referenced again for corrections or improvements

Version Control Systems automate this multi-draft process for code in a structured and safe manner:

* All versions are stored permanently
* Changes can be reviewed and reversed with accuracy
* Full traceability is maintained

Unlike common document editors such as Google Docs, VCS tools are engineered specifically for large-scale, multi-developer, and offline software engineering workflows.

---

## 5. What Problems that Version Control Solves

Writing a report or research paper typically involves multiple drafts (draft1, draft2, draft3, etc.).
We keep earlier drafts because:

* They may contain important information that might be needed later
* We may need to refer back, compare changes, or correct mistakes

Version Control Systems follow this same principle but in a more advanced and automated way:

* Every change in code is recorded as a new version
* Previous versions are preserved permanently and can be restored at any time
* Detailed history is maintained, including what changed and why

Unlike basic document collaboration tools like Google Docs, Version Control Systems are specifically designed for software development — providing reliability, technical traceability, offline support, and scalable collaboration for large and complex projects.


| Problem                                   | Impact                                 |
| ----------------------------------------- | -------------------------------------- |
| Multiple disorganized file copies         | Confusion and wasted time              |
| Overwriting another teammate’s changes    | Lost work and conflicts                |
| No record of reasons behind modifications | Difficult maintenance and debugging    |
| System failure or accidental deletion     | Permanent loss of code                 |
| Work stored only on a single machine      | Hard to continue if a developer leaves |

Manual tracking methods are not suitable for professional development.

---

## 6. Types of Version Control Systems

Version Control Systems have evolved in three major forms:

### a) Local Version Control Systems

* Entire history stored on a single computer
* Suitable only for individual projects
* High risk of total data loss
* Example: RCS

### b) Centralized Version Control Systems (CVCS)

* One central server stores the project history
* Developers pull/push code from this server
* Network dependency for most operations
* Examples: SVN, Perforce

### c) Distributed Version Control Systems (DVCS)

* Every developer has a full copy of the code and its history
* Work continues even without internet
* Powerful collaboration through branching and merging
* Examples: Git, Mercurial

---

## 7. Which VCS is Most Widely Used?

Today, the standard across almost all companies and open-source communities is:

**Git – A Distributed Version Control System**

Reasons for its popularity:

* Faster and more flexible
* Full offline capability
* Efficient code collaboration
* Strong branching and merging features
* Excellent security and data protection

---

## 8. What We Will Learn in This Module

This module focuses on:

1. **Git** — the most widely used DVCS
2. **GitHub** — a popular cloud platform for hosting Git repositories and enabling team collaboration


<image src="https://coding-platform.s3.amazonaws.com/dev/lms/tickets/6000a753-18b7-4a8b-bb7c-efd1aa3535dc/OkTPYUmUaszfEXKe.png" width="300px">


Learners will gain hands-on experience with:

* Commit history management
* Branching and merging
* Collaboration workflows
* Remote repository handling

---

## 9. Edge Cases and How VCS Helps

| Scenario                          | Without VCS                 | With VCS                                   |
| --------------------------------- | --------------------------- | ------------------------------------------ |
| Machine crashes                   | Complete loss               | Easy recovery from remote repository       |
| Trying out a new feature          | Risk of failure             | Use branches without affecting stable code |
| Debugging old issues              | No clue where it went wrong | Commit history reveals the cause           |
| Multiple people editing same file | Frequent overwrites         | Safe merging process                       |

Version Control turns risky development into safe experimentation.

---

## 10. Key Takeaways

* Version Control is essential for professional software development
* It safeguards code integrity and ensures productivity
* It supports teamwork, history tracking, and problem recovery
* Git is the global standard for managing source code versions

---

## 11. Mini Reflection Activity

Consider your current development method:

* How do you manage different versions of the same file?
* How do you restore older working code?
* How do you share and combine changes with others?

Version Control provides a clear, reliable, and industry-approved solution to these issues.

---

## Conclusion

Understanding why Version Control exists forms the foundation for learning Git.
Before exploring commands and tools, it is important to recognize how VCS ensures stable progression, proper collaboration, and strong maintainability in software projects.





---

## 1. Tools Available for Source Code Management

Software teams need tools to manage changing code.
Several tools are used in the industry:

| Tool Name          | Description                                  | Nature of System  |
| ------------------ | -------------------------------------------- | ----------------- |
| SVN (Subversion)   | Single central repository for the whole team | Centralized       |
| Perforce           | Enterprise-scale repository system           | Centralized       |
| Mercurial          | Distributed system similar to Git            | Distributed       |
| Git                | Distributed, fast, widely adopted versioning | Distributed       |
| Azure DevOps / TFS | Microsoft ecosystem for repo + project mgmt  | Centralized/Cloud |

While multiple tools exist, real-world usage shows a clear preference.

---

## 2. Why Git is Most Widely Used

Git became the industry standard for the following reasons:

* Works even **without internet**
* Every developer has a **full copy of history**
* Operations like commits and comparisons are **very fast**
* Branching and merging support is **strong**
* More secure with **built-in integrity checks**
* Large open-source ecosystem and community adoption
* Integrates with cloud services like GitHub, GitLab, Bitbucket

Git allows safe experimentation and easy teamwork for any scale of project.

---

## 3. What is Git?
![Git](https://git-scm.com/images/logos/logomark-orange@2x.png)

Git is a **local command-line tool** used for:

* Tracking every change in a project
* Recording versions through commits
* Creating branches to try new ideas safely
* Merging contributions in a structured way

Git manages version control on **your machine**.

---

## 4. What is GitHub?
<image src='https://github.githubassets.com/assets/GitHub-Mark-ea2971cee799.png' width="300px" >

GitHub is a **cloud platform** that stores repositories online and enables teamwork.

Purpose:

* Backup of your project on the server
* Collaboration with teammates
* Pull requests, code review, issue tracking, and CI/CD
* Display contribution history on developer profiles

| Aspect           | Git                        | GitHub                  |
| ---------------- | -------------------------- | ----------------------- |
| Runs where?      | Local system               | Cloud website           |
| Internet needed? | No                         | Yes                     |
| Purpose          | Version history management | Collaboration + Hosting |

Git works independently.
GitHub enhances Git for team use.

---

## 5. Create GitHub Account (Before Git Setup)

Students must create their profile using:

* Full **professional name**
* Same email to be used in Git config

Reason:
Commit authorship must match GitHub identity, otherwise contributions may not reflect properly.

Steps:
[Signup In Git](https://github.com/signup)
* Sign up on GitHub official website
* Verify email
* Choose a clear username (prefer full name if available)

---

## 6. Install Git

Install Git on your operating system.

### Windows

* Download installer from official Git website
* Proceed with **default recommended** options
* Git Bash terminal will be installed

### macOS

Using Homebrew:

```bash
brew install git
```

Or:

```bash
xcode-select --install
```

### Linux (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install git
```

Verify installation:

```bash
git --version
```

![Git Version](https://coding-platform.s3.amazonaws.com/dev/lms/tickets/ee796583-5e7f-48c7-86d6-585278c363f2/CGnV2pb3SKxRvGEh.png)

---

## 7. Basic Git Setup (After GitHub Account)

Configure Git to match your GitHub identity:

```bash
git config --global user.name "Exact GitHub Name"
git config --global user.email "YourGitHubEmail@example.com"
```

Check saved settings:

```bash
git config --list
```

Why required?

| Without Setup               | With Setup                              |
| --------------------------- | --------------------------------------- |
| Commits show unknown author | Profile contributions counted correctly |
| Confusion in team tracking  | Clear ownership                         |
| Harder debugging history    | Proper accountability                   |

This setup is mandatory even for **local-only work**.

---






---

To understand how Git tracks project changes, you must learn how files move through Git's **three core areas** and how commands like `add`, `status`, `commit`, `log`, and `diff` help you manage versions efficiently.

---

# **1. Git File Lifecycle — The 3-Area Model**

<image src="https://coding-platform.s3.amazonaws.com/dev/lms/tickets/07d42e41-6c25-4883-8b81-04fcc2c94420/UMQizQ3zQGscDIJV.png" width="500px">

<image src = "https://coding-platform.s3.amazonaws.com/dev/lms/tickets/a73fee3f-caa3-4940-a088-4c8b9ed67a4f/EYu3AONtxsf2yZtI.png" width="500px" >

Git manages files through **three logical locations**:

## **A. Working Directory (Workspace)**

This is your project folder on your machine.

Contains:

- New files
- Edited files
- Deleted files

Git **does NOT track** anything here until you stage it.

---

## **B. Staging Area (Index)**

Temporary holding area before committing.

Purpose:

- Prepare a clean snapshot
- Select _which_ files should go into the next commit
- Avoid committing unwanted changes

Files enter staging using:

```bash
git add <file>
```

---

## **C. Local Repository**

Stores **commits**, which are _permanent snapshots_ of your project.

Once committed:

- Changes become part of history
- A unique commit ID is generated
- You can revert or compare later

---

# **2. Checking the Current State — `git status`**

Use this command constantly.

```bash
git status
```

It tells you:

- Which files changed
- Which are staged
- Which are unstaged
- Whether your branch is ahead/behind

Example output:

```bash
Changes not staged for commit:
  modified: index.html

Changes to be committed:
  new file: style.css
```

---

# **3. Adding Files to Staging — `git add`**

### **Add a single file**

```bash
git add file.txt
```

### **Add multiple files**

```bash
git add file1.js file2.js
```

### **Add everything in the folder**

```bash
git add .
```

### **Add only modified files (not deleted/new)**

```bash
git add -u
```

Purpose of staging:

- Gives control
- Lets you commit only what you need
- Helps create clean, logical commits

---

# **4. Creating a Commit — `git commit`**

A **commit** is a permanent snapshot of staged changes.

### Basic commit:

```bash
git commit -m "Meaningful commit message"
```

### Commit messages MUST:

- Start with a verb → _Add, Update, Fix, Remove_
- Be short but descriptive
- Use present tense
- Avoid vague words like “changes”, “fixes”, “stuff”

### Examples of good messages:

- `Add navbar component`
- `Fix login validation issue`
- `Update README with setup instructions`

Bad examples:

- `done task`
- `changes made`
- `final commit`

---

# **5. Viewing Commit History — `git log`**

See full commit details:

```bash
git log
```

Shows:

- Commit ID
- Author
- Date
- Commit message

### Condensed view:

```bash
git log --oneline
```

Example:

```bash
a1b2c3d Add user login API
f4e5d6a Setup project structure
```

Perfect for quick history checks.

---

# **6. Comparing Changes — `git diff`**

Git provides powerful comparison tools.

---

## **A. Compare working directory vs staging area**

```bash
git diff
```

Shows **unstaged** edits.

---

## **B. Compare staging area vs last commit**

```bash
git diff --staged
```

Shows what you’re _about to commit_.

---

## **C. Compare two commits**

```bash
git diff <commit1> <commit2>
```

---

## How to read diff output:

```bash
- lines removed
+ lines added
```

Diffs help you review before committing.

---

# **7. Full Workflow Example**

### Step-by-step:

1. Edit a file
2. Check status

   ```bash
   git status
   ```

3. Stage it

   ```bash
   git add app.js
   ```

4. Review staged changes

   ```bash
   git diff --staged
   ```

5. Commit it

   ```bash
   git commit -m "Add route handler for login"
   ```

6. View commit history

   ```bash
   git log --oneline
   ```

---

# **8. Summary Table**

| Area / Command        | Purpose                         | When to Use                  |
| --------------------- | ------------------------------- | ---------------------------- |
| **Working Directory** | Where edits happen              | Anytime you modify files     |
| **Staging Area**      | Prepare changes for commit      | When changes are ready       |
| **Repository**        | Permanent version history       | After commit                 |
| `git status`          | View current file states        | Frequently                   |
| `git add`             | Move changes to staging         | Before committing            |
| `git commit`          | Save staged changes permanently | After reviewing              |
| `git log`             | See history                     | After commits                |
| `git diff`            | Compare changes                 | Before staging or committing |

---


---


To work confidently with Git, you must understand how to safely **undo mistakes** at different stages of the file lifecycle. Git provides commands to revert working directory changes, unstage staged changes, and modify the latest commit without losing control of your project history.

---

# **1. Undoing Working Directory Changes — `git restore`**


![Git Restore](https://coding-platform.s3.amazonaws.com/dev/lms/tickets/a0520db6-384b-4a0e-baf9-7abc56bb0d7b/rRcGtpdhLbJIJfta.png)

The `git restore` command discards uncommitted edits and restores the file back to the version from the last commit.

---

## **A. Restore a single file**

```bash
git restore file.txt
```

### Example

You mistakenly edited `config.js` and want to revert to last committed version:

```bash
git restore config.js
```

This removes all your local (uncommitted) edits in that file.

---

## **B. Restore multiple files**

```bash
git restore file1.js file2.js
```

### Example

You modified multiple component files accidentally:

```bash
git restore Header.js Footer.js
```

---

## **C. Restore everything in the working directory**

```bash
git restore .
```

### Example

You want to discard all uncommitted changes across the project:

```bash
git restore .
```

This resets all files back to the previous commit.

---

### What this does

* Removes uncommitted edits
* Does not change staging area
* Returns file(s) to last committed state

### Use when:

* You made unintended or incorrect edits
* You want a clean working directory

---

# **2. Unstage Changes — `git reset` or `git restore --staged`**



Staged changes can be safely moved back to the working directory without deleting edits.

---

## **A. Unstage a single file using reset**

```bash
git reset file.txt
```

### Example

You accidentally staged a large log file:

```bash
git reset debug.log
```

The file remains modified but is no longer staged.

---

## **B. Unstage a single file using restore**

```bash
git restore --staged file.txt
```

### Example

You added `index.js` but want to remove it from staging:

```bash
git restore --staged index.js
```

Equivalent to `git reset index.js`.

---

## **C. Unstage everything**

```bash
git reset
```

### Example

You staged many files but want a clean slate again:

```bash
git reset
```

All files move back to working directory with edits intact.

---

### What this does

* Removes files from staging
* Does not delete modifications
* Lets you stage again selectively

---

# **3. Amending the Last Commit — `git commit --amend`**

Sometimes corrections are required after making a commit. Git allows modifying the most recent commit.

---

## **A. Change the last commit message**

```bash
git commit --amend -m "Correct message"
```

### Example

You wrote:

```bash
git commit -m "Addd login page"
```

Fix it:

```bash
git commit --amend -m "Add login page"
```

---

## **B. Add missing changes to the last commit**

Stage missing files:

```bash
git add styles.css
```

Then amend:

```bash
git commit --amend
```

### Example

You forgot to include `validation.js` in the latest commit:

```bash
git add validation.js
git commit --amend
```

The previous commit now includes this file.

---

### What this does

* Rewrites the last commit
* Updates commit message or contents
* Generates a new commit ID

### Important

* Safe only before pushing
* Avoid rewriting public/shared history

---

# **4. Understanding File Lifecycle When Undoing Changes**



Undo commands behave differently depending on the file’s position in Git’s lifecycle.

| Stage             | Action Type        | Correct Command                                         |
| ----------------- | ------------------ | ------------------------------------------------------- |
| Working Directory | Discard edits      | `git restore file.txt`                                  |
| Staging Area      | Unstage file       | `git restore --staged file.txt` or `git reset file.txt` |
| Repository        | Modify last commit | `git commit --amend`                                    |

---

# **5. Common Mistake Scenarios and Solutions**

## **A. “I made changes but want to discard them.”**

```bash
git restore file.txt
```

### Example

You accidentally removed important lines in `app.js`:

```bash
git restore app.js
```

---

## **B. “I added files by mistake.”**

```bash
git reset file.txt
```

### Example

You staged temporary backup files:

```bash
git reset backup-old.js
```

---

## **C. “I forgot to include something in the last commit.”**

```bash
git add missing.js
git commit --amend
```

### Example

You forgot to add test cases:

```bash
git add login.test.js
git commit --amend
```

---

## **D. “My commit message is incorrect.”**

```bash
git commit --amend -m "Refactor authentication module"
```

---

## **E. “I want to review my changes before undoing anything.”**

Working directory diff:

```bash
git diff
```

Staged diff:

```bash
git diff --staged
```

### Example

To check what you added before committing:

```bash
git diff --staged
```

---

# **6. Summary Table**

| Command                | Purpose                           | Stage Affected    |
| ---------------------- | --------------------------------- | ----------------- |
| `git restore file`     | Discard working directory changes | Working Directory |
| `git restore --staged` | Unstage changes                   | Staging Area      |
| `git reset file`       | Unstage changes (alternative)     | Staging Area      |
| `git commit --amend`   | Modify last commit                | Repository        |
| `git diff`             | View unstaged changes             | Working Directory |
| `git diff --staged`    | View staged snapshot              | Staging Area      |

---




---

