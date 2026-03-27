## **1. Define Version Control System (VCS)**

### **Problem Description**

When multiple changes are made to a project over time, it becomes difficult to track what changed, when it changed, and who made the change.

### **Objective**

Define what a Version Control System is and explain its core purpose.

### **Hint**

Think about tracking changes, history, and collaboration.

### **Short Explanation**

A Version Control System helps manage and track changes to files over time.

### **Detailed Explanation**

A **Version Control System (VCS)** is a software tool that records changes made to files so that developers can review history, compare versions, and revert to earlier states if needed. It enables multiple people to work on the same project simultaneously without overwriting each other’s work. VCS also provides accountability by maintaining a record of who made each change and why.

### **Constraints / Edge Cases (Optional)**

- Without proper usage, history can still become messy
- Learning curve exists for beginners

---

## **2. Why manually copying project folders is not a reliable version-management method**

### **Problem Description**

Many beginners manage versions by duplicating folders like `project_v1`, `project_v2`, or `final_final`, which quickly becomes confusing.

### **Objective**

Explain why manual folder copying is a poor approach to version management.

### **Hint**

Consider scalability, accuracy, and collaboration.

### **Short Explanation**

Manual copying is error-prone and does not provide reliable version tracking.

### **Detailed Explanation**

Manually copying project folders does not maintain a clear history of changes and makes it difficult to identify what was modified between versions. It wastes storage space, increases the risk of losing work, and completely breaks down in collaborative environments. There is no built-in way to merge changes, track authorship, or safely revert to a specific state, making it unsuitable for real-world software development.

### **Constraints / Edge Cases (Optional)**

- May work only for very small, solo, short-term projects
- Becomes unmanageable as the project grows

---

## **3. Why Git has become the industry standard (two strong reasons)**

### **Problem Description**

There are many version control tools available, yet one dominates the software industry.

### **Objective**

Evaluate why **Git** is widely adopted as the industry standard.

### **Hint**

Focus on architecture and workflow efficiency.

### **Short Explanation**

Git is fast, reliable, and designed for modern collaborative development.

### **Detailed Explanation**

Git has become the industry standard primarily because of its **distributed architecture**, where every developer has a complete copy of the repository, enabling offline work and fast operations. Additionally, Git offers **powerful branching and merging**, allowing developers to experiment safely, work in parallel, and integrate changes efficiently. These features make Git highly scalable and ideal for both small teams and large enterprise projects.

### **Constraints / Edge Cases (Optional)**

- Git can be complex for beginners
- Poor branching practices can cause confusion

---


---

## **Q1. Purpose of the Staging Area in Git**

### **Problem Description**

When files are modified, not all changes may be ready to be saved permanently.

### **Objective**

Explain why Git uses a staging area between working directory and commit history.

### **Hint**

Think of selective commits and review before saving history.

### **Short Explanation**

The staging area allows developers to choose which changes should be included in the next commit.

### **Detailed Explanation**

The **staging area** in **Git** acts as an intermediate step between the working directory and the repository history. It enables developers to review, organize, and selectively add changes before committing. This prevents accidental commits and allows precise control over what gets recorded in the project history.

### **Constraints / Edge Cases (Optional)**

- Files not staged will not be committed
- Overusing `git add .` may stage unintended changes

---

## **Q2. Difference between `git diff` and `git diff --staged`**

### **Problem Description**

Developers often need to compare changes at different stages of the Git workflow.

### **Objective**

Differentiate between working directory changes and staged changes.

### **Hint**

One compares unstaged changes, the other staged changes.

### **Short Explanation**

`git diff` shows unstaged changes, while `git diff --staged` shows staged changes.

### **Detailed Explanation**

`git diff` displays differences between the **working directory** and the **staging area**, helping developers see what is modified but not yet staged.
`git diff --staged` (or `git diff --cached`) shows differences between the **staging area** and the **last commit**, allowing developers to review exactly what will be committed.

### **Constraints / Edge Cases (Optional)**

- If nothing is staged, `git diff --staged` shows no output
- Committed changes are not shown by either command

---

## **Q3. Committing Changes from Only One File**

### **Problem Description**

Sometimes only specific file changes should be committed.

### **Objective**

Demonstrate selective staging and committing.

### **Hint**

Stage only the required file.

### **Short Explanation**

Git allows committing changes from individual files selectively.

### **Detailed Explanation**

**Commands:**

```bash
git status
git add index.html
git diff --staged
git commit -m "Update homepage layout"
```

This workflow ensures that only `index.html` is staged and committed, while changes in `style.css` remain uncommitted.

### **Constraints / Edge Cases (Optional)**

- Forgetting to stage the file will result in no changes committed
- `git add .` would stage both files unintentionally

---

## **Q4. Checking Difference Between Two Commits**

### **Problem Description**

Developers may need to compare changes across different points in history.

### **Objective**

Show how to compare two commits directly.

### **Hint**

Use commit hashes with `git diff`.

### **Short Explanation**

Git allows comparison between any two commits using their IDs.

### **Detailed Explanation**

**Command:**

```bash
git diff a1b2c3d f4e5d6a
```

This command shows all differences between the two specified commits, regardless of their order in history.

### **Constraints / Edge Cases (Optional)**

- Large diffs may be difficult to read
- Short commit hashes must still be unique

---

## **Q5. Viewing Compact One-Line Commit History**

### **Problem Description**

Full commit logs can be lengthy and hard to scan quickly.

### **Objective**

Show how to view a concise commit history.

### **Hint**

Use a log option for compact output.

### **Short Explanation**

Git provides a one-line summary view of commits.

### **Detailed Explanation**

**Command:**

```bash
git log --oneline
```

**Expected Output Style:**

```
f4e5d6a Add utils.js and update app.js
a1b2c3d Add initial app.js file
```

Each line shows a short commit hash and commit message.

### **Constraints / Edge Cases (Optional)**

- Does not show author or timestamp
- Best used for quick overview

---

## **Q6. Complete Git Workflow: Folder, Files, Changes, and Commits**

### **Problem Description**

Understanding Git requires practicing the complete workflow from project creation to multiple commits.

### **Objective**

Demonstrate end-to-end Git usage.

### **Hint**

Follow the standard Git lifecycle.

### **Short Explanation**

This workflow covers initialization, staging, and committing multiple changes.

### **Detailed Explanation**

```bash
mkdir project-demo
cd project-demo

echo "console.log('Hello from app');" > app.js

git init
git add app.js
git commit -m "Add initial app.js file"

echo "console.log('Utility file');" > utils.js
echo "console.log('Another log');" >> app.js

git add app.js utils.js
git commit -m "Add utils.js and update app.js"
```

### **Constraints / Edge Cases (Optional)**

- Git must be initialized before staging
- Untracked files must be added explicitly

---

## **Q7. Comparing Changes Across the Git Workflow**

### **Problem Description**

Developers need to compare changes at different stages of Git history.

### **Objective**

Use appropriate diff and log commands for comparison.

### **Hint**

Each comparison uses a different Git command.

### **Short Explanation**

Git provides different diff commands for each workflow stage.

### **Detailed Explanation**

```bash
git diff
git diff --staged
git diff <initial_commit_hash> <second_commit_hash>
git log --oneline
```

- `git diff` → working directory vs staging
- `git diff --staged` → staging vs last commit
- `git diff commit1 commit2` → history comparison
- `git log --oneline` → compact history

### **Constraints / Edge Cases (Optional)**

- Commit hashes must be correct
- Large diffs may need paging tools

---


---

## **Q1. Purpose of the Staging Area in Git**

### **Problem Description**

When files are modified in a project, not all changes may be ready to be permanently saved in version history.

### **Objective**

Explain why Git introduces a staging area between working files and commits.

### **Hint**

Think about control, review, and selective commits.

### **Short Explanation**

The staging area allows developers to select and review changes before committing them.

### **Detailed Explanation**

In **Git**, the staging area acts as a buffer between the working directory and the commit history. It lets developers decide exactly which changes should be included in the next commit. This helps create clean, meaningful commits and avoids accidentally committing incomplete or unrelated changes.

### **Constraints / Edge Cases (Optional)**

- Files not added to the staging area will not be committed
- Using `git add .` may stage unintended changes

---

## **Q2. Difference between `git diff` and `git diff --staged`**

### **Problem Description**

Developers often need to inspect changes at different stages of the Git workflow.

### **Objective**

Differentiate between unstaged changes and staged changes.

### **Hint**

One compares working directory changes, the other compares staged changes.

### **Short Explanation**

`git diff` shows unstaged changes, while `git diff --staged` shows staged changes.

### **Detailed Explanation**

`git diff` displays the differences between the **working directory** and the **staging area**, helping developers see what has been modified but not yet staged.
`git diff --staged` (or `git diff --cached`) shows the differences between the **staging area** and the **last commit**, allowing developers to review exactly what will be committed next.

### **Constraints / Edge Cases (Optional)**

- If nothing is staged, `git diff --staged` produces no output
- These commands do not show already committed changes

---

## **Q3. Committing Changes from Only `index.html`**

### **Problem Description**

Sometimes developers need to commit changes from only one file while leaving other changes uncommitted.

### **Objective**

Demonstrate selective staging and committing in Git.

### **Hint**

Stage only the required file.

### **Short Explanation**

Git allows committing changes from specific files only.

### **Detailed Explanation**

**Commands:**

```bash
git status
git add index.html
git diff --staged
git commit -m "Update homepage layout"
```

This sequence ensures that only `index.html` is staged and committed, while changes in `style.css` remain in the working directory.

### **Constraints / Edge Cases (Optional)**

- Forgetting `git add index.html` results in no changes committed
- Using `git add .` would stage both files

---

## **Q4. Checking Difference Between Two Commits**

### **Problem Description**

Developers may need to compare how the code changed between two specific points in history.

### **Objective**

Show how to compare two commits directly.

### **Hint**

Use commit hashes with the diff command.

### **Short Explanation**

Git can compare any two commits using their IDs.

### **Detailed Explanation**

**Command:**

```bash
git diff a1b2c3d f4e5d6a
```

This command shows all differences between the two specified commits, regardless of their order.

### **Constraints / Edge Cases (Optional)**

- Commit hashes must be valid and unique
- Large diffs may be difficult to read

---

## **Q5. Viewing Compact One-Line Commit History**

### **Problem Description**

Full commit logs can be long and difficult to scan quickly.

### **Objective**

Display a concise summary of commit history.

### **Hint**

Use a log option for compact output.

### **Short Explanation**

Git provides a one-line summary view of commits.

### **Detailed Explanation**

**Command:**

```bash
git log --oneline
```

**Expected Output Style:**

```
f4e5d6a Add utils.js and update app.js
a1b2c3d Add initial app.js file
```

Each line contains a short commit hash and its commit message.

### **Constraints / Edge Cases (Optional)**

- Does not show author or timestamp
- Best for quick overviews

---

## **Q6. Complete Git Workflow: Creating Files and Making Commits**

### **Problem Description**

Understanding Git requires hands-on practice with the complete workflow.

### **Objective**

Demonstrate folder creation, file creation, staging, and committing.

### **Hint**

Follow the standard Git lifecycle step by step.

### **Short Explanation**

This task demonstrates a full Git workflow from initialization to multiple commits.

### **Detailed Explanation**

```bash
mkdir project-demo
cd project-demo

echo "console.log('Hello from app');" > app.js

git init
git add app.js
git commit -m "Add initial app.js file"

echo "console.log('Utility file');" > utils.js
echo "console.log('Another log');" >> app.js

git add app.js utils.js
git commit -m "Add utils.js and update app.js"
```

### **Constraints / Edge Cases (Optional)**

- Git must be initialized before staging
- Untracked files must be added explicitly

---

## **Q7. Comparing Changes Across the Git Workflow**

### **Problem Description**

Git allows comparisons at different stages of development and history.

### **Objective**

Use appropriate Git commands to compare changes.

### **Hint**

Each comparison uses a different Git command.

### **Short Explanation**

Different Git commands compare changes at different workflow stages.

### **Detailed Explanation**

```bash
git diff
git diff --staged
git diff <initial_commit_hash> <second_commit_hash>
git log --oneline
```

- `git diff` → working directory vs staging area
- `git diff --staged` → staging area vs last commit
- `git diff commit1 commit2` → compare two commits
- `git log --oneline` → compact commit history

### **Constraints / Edge Cases (Optional)**

- Commit hashes must be accurate
- Large diffs may require paging

---


---

## **Q1. What is meant by “isolated development” when using Git branches?**

### **Problem Description**

When multiple developers work on the same project, changes can interfere with each other if not properly separated.

### **Objective**

Explain the concept of isolated development in the context of Git branches.

### **Hint**

Think about working independently without affecting the main code.

### **Short Explanation**

Isolated development means working on changes separately without impacting the main branch.

### **Detailed Explanation**

In **Git**, isolated development refers to creating branches where developers can work on features, fixes, or experiments independently. Changes made in a branch do not affect the main codebase until they are explicitly merged. This allows safe experimentation and parallel development.

### **Constraints / Edge Cases (Optional)**

- Poor merge practices can still introduce conflicts
- Long-lived branches may drift from main

---

## **Q2. Why is branch cleanup considered a best practice in Git?**

### **Problem Description**

Over time, repositories can accumulate many unused or merged branches.

### **Objective**

Explain why deleting unused branches is important.

### **Hint**

Think about clarity, maintenance, and confusion.

### **Short Explanation**

Branch cleanup keeps the repository clean and easy to manage.

### **Detailed Explanation**

Branch cleanup is considered a best practice because merged or unused branches no longer serve a purpose. Keeping them can clutter the repository, confuse team members, and make branch management harder. Deleting such branches improves readability, reduces mistakes, and ensures developers focus only on active work.

### **Constraints / Edge Cases (Optional)**

- Branches should be deleted only after successful merge
- Some teams keep long-lived branches for releases

---

## **Q3. Create a project and work using branches**

### **Problem Description**

Understanding Git branching requires hands-on practice with creating, switching, and committing in branches.

### **Objective**

Demonstrate creating a repository, working on a feature branch, and committing changes.

### **Hint**

Follow the standard Git branching workflow.

### **Short Explanation**

This task shows how to create and work in a feature branch.

### **Detailed Explanation**

```bash
mkdir branch-demo
cd branch-demo

git init

echo "Initial content" > app.txt
git add app.txt
git commit -m "Initial commit on main"

git branch feature-update
git switch feature-update

echo "Feature branch update" >> app.txt
git add app.txt
git commit -m "Update app.txt in feature branch"

git switch main
```

### **Constraints / Edge Cases (Optional)**

- Git may use `master` instead of `main` in older versions
- File must be staged before committing

---

## **Q4. Merge a feature branch and clean it up**

### **Problem Description**

After completing feature development, changes must be merged and the branch removed.

### **Objective**

Demonstrate merging a branch and deleting it safely.

### **Hint**

Merge first, then delete.

### **Short Explanation**

Merged branches can be safely deleted to keep the repository clean.

### **Detailed Explanation**

```bash
git merge feature-update
git branch -d feature-update
git branch
```

- `git merge feature-update` merges the feature branch into `main`
- `git branch -d feature-update` deletes the merged branch
- `git branch` verifies the remaining branches

### **Constraints / Edge Cases (Optional)**

- `-d` prevents deletion if branch is not fully merged
- Use `-D` only when you are sure


---

## **Q1. What is a remote repository, and why is it important for team collaboration?**

### **Problem Description**

In team environments, developers need a shared place to store and access the project code.

### **Objective**

Explain what a remote repository is and why it is essential for collaboration.

### **Hint**

Think of a shared source of truth and teamwork.

### **Short Explanation**

A remote repository is a shared, central version of the project used by all team members.

### **Detailed Explanation**

A **remote repository** is a centrally hosted copy of a project that multiple developers can access and synchronize with. It acts as a **shared source of truth**, ensuring everyone works from the same baseline. Remote repositories enable collaboration, code sharing, reviews, backups, and coordinated development across teams, especially in distributed environments.

### **Constraints / Edge Cases (Optional)**

- Requires internet access to sync changes
- Conflicts may occur if multiple changes overlap

---

## **Q2. How local and remote repositories work together in Git**

### **Problem Description**

Git uses both local and remote repositories, which can confuse beginners.

### **Objective**

Explain how local and remote repositories interact using commit, push, and pull.

### **Hint**

Think of local work first, then sharing.

### **Short Explanation**

Git follows a local-first workflow where changes are shared explicitly.

### **Detailed Explanation**

In **Git**, developers first make changes and create **commits locally**, even without internet access. These commits remain private until explicitly shared using `git push` to the remote repository. To receive updates from teammates, developers use `git pull`, which fetches and merges remote changes into the local repository. This controlled synchronization allows flexibility, safety, and better collaboration.

### **Constraints / Edge Cases (Optional)**

- Forgetting to pull can lead to conflicts
- Push may fail if remote has newer commits

---

## **Q3. “I committed my changes, so my teammates can see them.” — Is this correct?**

### **Problem Description**

There is a common misunderstanding about when changes become visible to others.

### **Objective**

Clarify the difference between committing and sharing changes.

### **Hint**

Where does the commit live?

### **Short Explanation**

No, commits are local until they are pushed to a remote repository.

### **Detailed Explanation**

This statement is **incorrect**. A `git commit` saves changes only in the **local repository**. Teammates cannot see these changes until the developer uses `git push` to send the commits to the remote repository. Until then, the work exists only on the developer’s machine and is invisible to others.

### **Constraints / Edge Cases (Optional)**

- Commits on feature branches are also invisible until pushed
- Force-push can overwrite shared history if misused

---

## **Q4. Typical end-to-end collaboration flow using a remote repository**

### **Problem Description**

Understanding collaboration requires seeing the complete workflow from start to finish.

### **Objective**

List the standard steps involved in collaborative Git development.

### **Hint**

Start from remote and end with merge and sync.

### **Short Explanation**

Collaboration follows a structured flow from cloning to merging.

### **Detailed Explanation**

A typical end-to-end collaboration flow is:

1. Start from a **remote repository**
2. Clone or fork the repository locally
3. Create a branch and make local changes
4. Commit changes locally
5. Push commits to the remote repository
6. Create a Pull Request
7. Review and discuss changes
8. Merge the Pull Request into the main branch
9. Pull the latest changes to keep local repository in sync

This workflow ensures safe collaboration, review, and code quality.

### **Constraints / Edge Cases (Optional)**

- Conflicts may occur during merge
- Review delays can slow down integration


---

## **Q1. Purpose of setting an upstream branch using `git push -u`**

### **Problem Description**

When pushing a branch to a remote repository for the first time, Git needs to know which remote branch it should track.

### **Objective**

Explain why the `-u` (upstream) flag is used during the first push.

### **Hint**

Think about future `git push` and `git pull` commands.

### **Short Explanation**

Setting an upstream branch links the local branch to a remote branch.

### **Detailed Explanation**

Using `git push -u origin branch-name` sets an **upstream relationship** between the local branch and the remote branch. After this is done, Git knows where to push and pull from by default. This allows developers to use simple commands like `git push` and `git pull` without specifying the remote and branch every time.

### **Constraints / Edge Cases (Optional)**

- Required only for the first push of a branch
- Upstream can be changed later if needed

---

## **Q2. Difference between `git fetch` and `git pull` (and why fetch is safer)**

### **Problem Description**

Developers often need to update their local repository with changes from the remote repository.

### **Objective**

Differentiate between `git fetch` and `git pull` and explain why fetch is safer.

### **Hint**

One updates references only, the other updates code automatically.

### **Short Explanation**

`git fetch` downloads changes without merging, while `git pull` fetches and merges automatically.

### **Detailed Explanation**

`git fetch` retrieves the latest changes from the remote repository and updates remote-tracking branches, but it **does not modify the working directory**. This allows developers to review changes before merging.
`git pull` performs a fetch followed by an automatic merge, which can introduce conflicts immediately. Fetch is considered safer because it gives developers full control over when and how changes are merged.

### **Constraints / Edge Cases (Optional)**

- Pull can cause unexpected conflicts
- Fetch requires an extra merge step

---

## **Q3. Local to Remote Workflow (First Push)**

### **Problem Description**

A local Git repository must be connected to a remote repository to enable sharing and collaboration.

### **Objective**

Demonstrate the first-time push workflow from local to remote.

### **Hint**

Initialize locally, then link and push to remote.

### **Short Explanation**

The first push connects the local repository to a remote and uploads initial commits.

### **Detailed Explanation**

In this task, a local repository is created and a `README.md` file is committed. A remote repository is then created on **GitHub** and linked using `git remote add origin`. The code is pushed using `git push -u origin main`, which uploads the commit and sets the upstream branch. This establishes the foundation for all future collaboration.

### **Constraints / Edge Cases (Optional)**

- Remote repository must exist before pushing
- Branch name (`main` vs `master`) must match

---

## **Q4. Feature Branch → Push → Pull Request → Merge**

### **Problem Description**

Direct commits to the main branch can introduce unstable code.

### **Objective**

Demonstrate a safe feature-based workflow using branches and Pull Requests.

### **Hint**

Changes flow through a feature branch before reaching main.

### **Short Explanation**

Feature branches isolate work and are merged through Pull Requests.

### **Detailed Explanation**

In this workflow, a new branch `feature-update` is created from `main`. Changes are committed locally and pushed to the remote repository. A Pull Request is then raised to merge the feature branch into `main`. After review and successful merge, the feature branch is deleted to keep the repository clean. This process ensures code review, traceability, and controlled integration.

### **Constraints / Edge Cases (Optional)**

- Merge conflicts may need manual resolution
- Branch deletion should happen only after merge

---

## **Q5. Collaboration Workflow with Pull Requests**

### **Problem Description**

Multiple developers working on the same repository need a structured collaboration process.

### **Objective**

Demonstrate collaboration using Pull Requests, either with a real collaborator or a simulated workflow.

### **Hint**

Think in terms of ownership, review, and controlled merging.

### **Short Explanation**

Pull Requests enable collaboration, review, and safe merging of changes.

### **Detailed Explanation**

In this task, changes are made in a separate branch (`collab-feature`) either by a collaborator or via solo simulation. The branch is pushed to GitHub and a Pull Request is raised against `main`. The Pull Request allows review, discussion, and approval before merging. Once merged, the changes become part of the main branch, demonstrating a real-world collaborative workflow.

### **Constraints / Edge Cases (Optional)**

- Fork-based workflows are used in open-source projects
- Review delays can slow integration

---


---

## **Q1. Why is merging considered a “decision point” rather than a development activity?**

### **Problem Description**

In collaborative development, code written in branches must eventually be integrated into a shared codebase.

### **Objective**

Explain why merging represents a formal decision rather than ongoing development.

### **Hint**

Think about review, approval, and responsibility.

### **Short Explanation**

Merging is a decision to accept changes into the shared codebase, not to create them.

### **Detailed Explanation**

Merging is considered a **decision point** because it happens after development is complete and reviews are done. At this stage, the team decides whether the changes meet quality standards and are ready to be included in the shared branch. Once merged, the code affects all contributors, making the action accountable and deliberate rather than exploratory development.

### **Constraints / Edge Cases (Optional)**

- Emergency merges may skip full review
- Admin overrides can bypass normal approvals

---

## **Q2. Why testing is important after resolving a merge conflict**

### **Problem Description**

Merge conflicts require manual edits, which can unintentionally introduce errors.

### **Objective**

Explain why testing is required after conflict resolution.

### **Hint**

Think about human error during manual edits.

### **Short Explanation**

Testing ensures that conflict resolution did not introduce hidden bugs.

### **Detailed Explanation**

When resolving merge conflicts, developers manually combine code from different branches. This process can accidentally break logic, remove required code, or introduce syntax errors. Testing the application after resolving conflicts validates that the final merged code works as expected and that no regressions were introduced during manual resolution.

### **Constraints / Edge Cases (Optional)**

- Small changes may still cause major issues
- Automated tests may not cover all edge cases

---

## **Q3. Merge Without Conflict (Basic Workflow) – Editorial**

### **Problem Description**

Not all merges result in conflicts; understanding clean merges is essential for beginners.

### **Objective**

Demonstrate a clean feature-branch merge using Pull Requests.

### **Hint**

Ensure changes do not overlap with main branch edits.

### **Short Explanation**

Non-overlapping changes allow Git to merge branches automatically.

### **Detailed Explanation**

In this workflow, a repository is created and a base commit is pushed to `main`. A feature branch (`feature-a`) is created where additional lines are added to `app.txt` without modifying the same lines changed in `main`. When a Pull Request is raised from `feature-a` to `main`, Git automatically merges the changes without conflict. After merging, the feature branch is deleted to keep the repository clean. This demonstrates the ideal and most common merge scenario in team workflows using **GitHub**.

### **Constraints / Edge Cases (Optional)**

- Even small overlapping edits can cause conflicts
- Branch deletion should happen only after merge confirmation

---

## **Q4. Merge With Conflict (Conflict Resolution Workflow) – Editorial**

### **Problem Description**

Conflicts occur when multiple branches modify the same lines of a file.

### **Objective**

Demonstrate how to identify, resolve, and safely complete a conflicting merge.

### **Hint**

Focus on conflict markers and manual resolution.

### **Short Explanation**

Merge conflicts require manual intervention to decide which changes to keep.

### **Detailed Explanation**

In this workflow, a branch (`feature-b`) modifies the same lines in `app.txt` that were also changed in `main`. When a Pull Request is raised, Git detects conflicting changes and blocks automatic merging. The developer must resolve the conflict either using the command line or GitHub’s UI by choosing or combining changes and removing conflict markers. After resolving the conflict and verifying the final content, the merge is completed. This process ensures correctness and intentional integration of competing changes.

### **Constraints / Edge Cases (Optional)**

- Incorrect conflict resolution can break functionality
- Conflict markers must be completely removed before merge

---


---

