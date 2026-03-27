
### **Why Version Control Matters — Pre-Read**

Software projects constantly evolve — new features, bug fixes, enhancements. Without a system to track and manage these changes, it becomes difficult to restore older working versions or collaborate efficiently with teammates. Version Control provides a secure way to protect history, avoid conflicts, and work together without breaking each other’s code.

---

### **Real-World Problems Without Version Control**

Manual file backups like *final_v3_fixed_reallyfinal.cpp* often fail.
Multiple people editing the same file can overwrite work.
Losing code due to accidental deletion or system crash is common.

Reflective question:
How do you recover your project if a new change breaks everything?

---

### **What Version Control Does**

Version Control acts like a timeline for your project.

```bash
v1 → v2 → v3 → v4
(works) (bug) (fix) (new feature)
```

* Stores every version safely
* Compares and restores previous versions
* Keeps authorship and reasoning for changes
* Enables smooth teamwork

![Version Control](https://coding-platform.s3.amazonaws.com/dev/lms/tickets/8791fbc9-33ca-421b-ad68-a07d204fca84/qDCpbqCh13XUa0wH.png)

---

### **Simple Analogy**

Writing a report with multiple drafts:

Older drafts are saved because they might be useful later.

Version Control does the same automatically for code, but with more structured control:

* Permanent history
* Offline support
* Safe collaboration

---

### **Types of Version Control Systems**

| Type               | Where history lives       | Best suited for           | Example        |
| ------------------ | ------------------------- | ------------------------- | -------------- |
| Local              | One machine               | Solo development          | RCS            |
| Centralized (CVCS) | One server                | Team collaboration        | SVN            |
| Distributed (DVCS) | Every developer’s machine | Modern, scalable projects | Git, Mercurial |

---

### **The Industry Standard: Git**

Git is the most widely used Version Control tool today because it is:

* Fast and flexible
* Fully offline compatible
* Excellent at branching and merging
* Secure and reliable

GitHub hosts Git repositories online for collaboration:

<image src="https://coding-platform.s3.amazonaws.com/dev/lms/tickets/6000a753-18b7-4a8b-bb7c-efd1aa3535dc/OkTPYUmUaszfEXKe.png" width="320px">

---

### **How VCS Supports Development**

| Situation                   | Without VCS              | With VCS                        |
| --------------------------- | ------------------------ | ------------------------------- |
| Machine failure             | Code lost permanently    | Restore from repository         |
| Trying new features         | Risky                    | Use branches to isolate changes |
| Debugging issues            | Hard to trace            | Check commit history            |
| Multiple edits on same file | Conflicts and overwrites | Safe merging workflow           |

---

### **Quick Self-Check**

* Do you keep older versions of your project files?
* Can you revert code confidently if a mistake happens?
* Can you work on the same files with teammates safely?

These challenges highlight why Version Control is essential.

---

### **Useful Resources**

Videos:

* Why Version Control? — [https://youtu.be/zbKdDsNNOhg](https://youtu.be/zbKdDsNNOhg)
* What is Git & GitHub? — [https://youtu.be/RGOj5yH7evk](https://youtu.be/RGOj5yH7evk)

Blog:

* Atlassian: Introduction to Version Control
  [https://www.atlassian.com/git/tutorials/what-is-version-control](https://www.atlassian.com/git/tutorials/what-is-version-control)

---




---

Software development requires continuous updates and collaboration among multiple contributors. To prevent accidental overwrites, data loss, or confusion during teamwork, organizations use Source Code Management tools. These tools maintain a reliable record of changes and ensure smooth coordination within development teams.

---

## Tools Used in the Software Industry

Source code can be managed using different system architectures.

| Tool Name          | Nature of System  | Primary Usage Context                            |
| ------------------ | ----------------- | ------------------------------------------------ |
| SVN, Perforce      | Centralized       | Managed access through one shared server         |
| Mercurial, Git     | Distributed       | Full history stored on every developer’s machine |
| Azure DevOps / TFS | Centralized/Cloud | Repository plus project planning and automation  |

Distributed models like Git have become the preferred standard due to flexibility, performance, and reliability.

---

## Why Git Became the Standard

Git offers strong technical advantages:

- Development continues even without internet access
- Every contributor maintains a full version history
- Faster save and comparison operations
- Safe branching to explore new ideas without breaking the main code
- Secure change tracking and integrity
- Broad adoption in both open-source and enterprise projects
- Integrates with collaboration platforms used globally

These benefits support productivity for both small and large teams.

---

## What Git Provides

Git is installed on the developer’s machine and manages all versioning tasks locally.
It records every change, preserves historical versions, and organizes contributions while keeping a project’s structure stable over time.

---

## GitHub as a Cloud Collaboration Platform

GitHub hosts Git repositories online so teams can:

- Store work securely on remote servers
- Share contributions and provide feedback
- Track issues and improvements
- Show visible contribution history for professional credibility

| Aspect          | Git              | GitHub                            |
| --------------- | ---------------- | --------------------------------- |
| Location        | Local system     | Cloud platform                    |
| Internet needed | Not required     | Required                          |
| Main purpose    | Version tracking | Collaboration, backup, visibility |

Git operates independently. GitHub extends Git for teamwork and distribution.

---

## Getting Ready Before the Session

To participate in collaboration exercises smoothly:

- Create a GitHub account with a **professional name**
- Use an email that will later match your Git configuration
- Ensure your laptop supports installation of Git software

This alignment ensures proper authorship of contributions.


---

Git manages project changes through a clear flow of how files move between different areas before becoming part of the project history.

---

## **1. The Three-Area Model**

Git organizes files across three logical areas that represent their state.

![Image](https://coding-platform.s3.amazonaws.com/dev/lms/tickets/07d42e41-6c25-4883-8b81-04fcc2c94420/UMQizQ3zQGscDIJV.png)

**Working Directory**
Where files are created, edited, or deleted. Changes here are not tracked until staged.

**Staging Area (Index)**
A preparation zone that collects selected changes. This allows forming intentional and clean snapshots.

**Local Repository**
Stores committed snapshots permanently with unique commit IDs.

---

## **2. Checking File States**

`git status` displays which files are modified, which are staged, and which remain unstaged.
Example (conceptually):

- A modified file may appear under “changes not staged.”
- A newly added file may appear under “changes to be committed.”

---

## **3. Staging Changes**

`git add` moves selected files into the staging area.
Examples:

- Staging a single file
- Staging multiple files
- Staging all tracked and modified files

Staging allows grouping related updates into meaningful commits.

---

## **4. Creating Commits**

A commit acts as a snapshot of all staged changes.
Commit messages typically start with verbs such as _Add_, _Fix_, _Update_, _Remove_ and describe the purpose of the change clearly.
Example messages:

- Add navbar component
- Fix validation issue

---

## **5. Viewing Project History**

`git log` lists past commits with details like ID, author, and timestamp.
A condensed format shows just commit IDs and short messages to provide a quick view of progression.

---

## **6. Comparing Versions**

`git diff` highlights differences between two states:

- Working directory vs staging area
- Staging area vs last commit
- One commit vs another

Conceptually, added lines appear with a plus symbol and removed lines with a minus symbol, allowing careful review before saving work.

---

## **7. Workflow Overview**

A typical flow involves:

1. Editing files
2. Checking their state
3. Staging selected updates
4. Reviewing staged content
5. Creating a commit
6. Inspecting history afterward

---


---

Working with Git involves making edits, staging them, and committing them. Mistakes can occur at any of these stages, and Git provides safe ways to undo or adjust those changes without losing work. The preread introduces how Git handles undo operations in the working directory, staging area, and repository.

---

## **1. Undoing Changes in the Working Directory**

The working directory contains uncommitted edits. These changes can be reverted back to the last committed version.
Concept example:

- A configuration file was edited incorrectly and needs to be reset
- Multiple files were changed accidentally and should be restored to a clean state

Restoring affects only uncommitted changes and does not touch the staging area.

---

## **2. Removing Files from the Staging Area**

Staging determines what goes into the next commit. If the wrong files were staged, they can be safely moved back to the working directory while keeping their edits intact.
Concept examples:

- A large log file was staged unintentionally
- Several files were added to staging, but only one of them should remain
- A clean staging area is needed before grouping changes logically

Unstaging affects only the staging area and preserves file modifications.

---

## **3. Modifying the Most Recent Commit**

Sometimes a commit contains a mistake—either in its message or its content. Git allows updating the most recent commit before it is shared.
Concept examples:

- A spelling error in the commit message
- A missing file that should have been part of the last commit
- A small correction that fits the previous change

Amending rewrites the last commit and updates its ID, but is safe as long as the commit has not been pushed.

---

## **4. Undoing Based on the File Lifecycle**

Actions differ depending on where the file currently sits:

- Working directory: discard changes
- Staging area: unstage changes
- Repository: adjust latest commit

This lifecycle helps determine the correct command for the correct situation.

---

## **5. Common Mistake Scenarios**

Concept examples covered in the session include:

1. Changes made incorrectly and needing discard
2. Files staged unintentionally
3. Missing updates that belong in the previous commit
4. Incorrect commit messages
5. Reviewing differences before undoing anything

These situations illustrate how undo operations keep the workflow safe and controlled.

---


---

