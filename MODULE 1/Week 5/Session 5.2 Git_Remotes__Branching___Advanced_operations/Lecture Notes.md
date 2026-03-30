# 1. Why Branching is Needed

In development, multiple types of work (new features, bug fixes, experiments) often happen simultaneously. If all work occurs directly on `main`, incomplete features, bugs, and experimental changes can easily break the application, making parallel work risky and history hard to manage. Branching isolates work to prevent these issues.

# 2. What is a Branch in Git

A **branch** is a lightweight pointer to a commit, representing an independent line of development.
- Each branch has its own commit history.
- Changes in one branch do not affect others.
- `main` is the default stable branch.

# 3. Isolated Development Using Branches

Branches provide isolation between different types of work, leading to:
- Feature development without risk.
- Bug fixes without blocking others.
- Experiments without fear.
- A stable and clean `main` branch.
Each branch acts like a separate workspace.

# 4. Viewing Existing Branches — `git branch`

List all local branches:
```bash
git branch
```
Output shows `*` for the current branch:
```bash
* main
  feature-login
  bugfix-header
```

# 5. Creating Branches — `git branch`

Create a new branch without switching to it:
```bash
git branch feature-login
```
You remain on `main`.

# 6. Switching Branches — `git checkout` / `git switch`

## A. Switch to an existing branch
```bash
git checkout feature-login
# or
git switch feature-login
```
All new work now belongs to `feature-login`.

## B. Create and switch in one command
```bash
git checkout -b feature-signup
# or
git switch -c feature-signup
```

# 7. Working in an Isolated Branch

Once inside a branch, you edit, stage, and commit changes:
```bash
git switch feature-login
git add login.js
git commit -m "Add login form UI"
```
These changes are isolated from `main`.

# 8. Branch Naming Conventions

Clear naming improves collaboration.

| Purpose    | Example Branch Name    |
| ---------- | ---------------------- |
| Feature    | `feature-login`        |
| Bug Fix    | `bugfix-navbar`        |
| Hotfix     | `hotfix-payment-issue` |
| Experiment | `experiment-ui-layout` |

Rules:
- Lowercase letters
- Hyphen-separated
- Avoid generic names (`test`, `new`, `temp`)

# 9. Deleting Branches — `git branch -d`

Delete branches after use.

## A. Delete a merged branch (safe)
```bash
git branch -d feature-login
```
Deletes only if fully merged.

## B. Force delete an unmerged branch
```bash
git branch -D experiment-old-ui
```
Use when no longer needed, even if unmerged.

# 10. Branch Cleanup (Best Practices)

Cleanup keeps the repository manageable:
- Avoids clutter and confusion.
- Prevents working on outdated branches.

Recommended cleanup process:
1. Merge branch into `main`.
2. Delete the local branch.

Example flow:
```bash
git switch main
git branch -d feature-login
```

# 11. Understanding Branch Lifecycle

| Stage              | Description                    |
| ------------------ | ------------------------------ |
| Branch creation    | New line of development begins |
| Active development | Commits added in isolation     |
| Merge (next topic) | Changes integrated into main   |
| Cleanup            | Branch deleted after use       |

This lifecycle is standard in professional projects.

# 12. Summary Table

| Command                  | Purpose                    |
| ------------------------ | -------------------------- |
| `git branch`             | List all local branches    |
| `git branch <name>`      | Create a branch            |
| `git checkout <name>`    | Switch to a branch         |
| `git checkout -b <name>` | Create and switch branch   |
| `git switch <name>`      | Switch branch (modern)     |
| `git switch -c <name>`   | Create and switch (modern) |
| `git branch -d <name>`   | Delete merged branch       |
| `git branch -D <name>`   | Force delete branch        |

# 13. Key Takeaway

Branches enable safe, parallel, and structured development. Branch cleanup is crucial for maintaining a clean and readable repository.

---

Effective Git collaboration relies on **remote repositories**. They provide a shared, centralized location to synchronize, review, and control changes. This guide covers remotes, GitHub, and team collaboration workflows.

## 1. Git vs. GitHub
*   **Git**: Local version control (tracks changes, commits, branches, offline).
*   **GitHub**: Remote hosting for Git repos (online shared platform for collaboration, permissions, PRs).

## 2. Remote Repository
A **remote repository** is a Git repo stored externally (e.g., GitHub).
Its purpose:
*   Shared source of truth
*   Synchronize multiple developers' work
*   Project history backup
*   Enable collaboration workflows
Developers have local repos, but the remote is the central hub.

## 3. Local-Remote Interaction
Work is always local first (edit, commit).
*   **Push**: sends local commits to remote.
*   **Pull/Fetch**: brings remote changes locally.
This explicit control allows offline work, controlled sharing, and isolated local mistakes.

## 4. Understanding 'origin'
`origin` is the conventional name for the **primary remote repository**. It's a shortcut, not a keyword.
`git remote -v` shows connected remotes.

## 5. What is a Repository
A repository (`repo`) is a container for: source code, commit history, branches, tags, config.
*   **Local repo**: on your machine.
*   **Remote repo**: on GitHub (enables teamwork).

## 6. Creating a GitHub Repository
Establishes:
*   Remote project location
*   Ownership & access rules
*   Collaboration entry point
Initially, it exists only in the cloud, awaiting local connections.

## 7. Cloning a Repository
`git clone <repository-url>`
Creates a local copy of a remote repo, including:
*   Project files
*   Full commit history
*   Automatic connection to the remote (as `origin`)

## 8. Synchronizing with Remote
*   `git pull origin main`: brings changes *from* remote *to* local.
*   `git push origin main`: sends changes *from* local *to* remote.
Synchronization is always explicit and controlled.

## 9. Direct Collaboration (Collaborators)
**Collaborators** have direct push access to a remote repo.
*   Push branches/commits
*   Participate in reviews
Common for internal teams or trusted contributors.

## 10. Remote Repository Permissions
GitHub defines who can:
*   Push code
*   Review/approve changes
*   Manage repo settings
Ensures code safety and accountability; enforced on GitHub.

## 11. Forking
Creates a **separate remote repository** under the contributor's account when direct push access isn't granted.
*   Original repo remains protected.
*   Contributor works independently on their fork.

## 12. Pull Requests (PRs)
A PR is a request to merge changes into a remote repo.
*   GitHub-specific feature.
*   Facilitates code review, discussion, and controlled merging.
*   Connects branches or forks.

## 13. Pull Request Review
Reviewers examine changes, provide feedback, and discuss before merging. Ensures quality and intentional changes.

## 14. Review Decisions
*   **Approve**: Code is ready to merge.
*   **Request Changes**: Blocks merge until issues are resolved.
Ensures quality control.

## 15. End-to-End Collaboration Flow
1.  Remote repo exists on GitHub.
2.  Developers **clone** (or fork).
3.  Work **locally**.
4.  **Push** changes to remote branch.
5.  Create **Pull Request**.
6.  Code **reviewed** & discussed.
7.  **Approved** changes **merged**.
8.  Remote updates.
9.  Local repos **sync** (pull) again.
This cycle repeats for ongoing teamwork.

---

## **1. Creating a Remote Repository**
- Collaboration requires a **remote repository** (e.g., GitHub).
- Centralized for code, ownership, rules.
- Initially, exists only on GitHub; no local connection.
- Becomes the **single source of truth**.

## **2. Cloning a Remote Repository**
- Cloning creates a **local working copy**.
- `git clone <remote-repository-url>`
- Downloads files, copies full commit history.
- Configures `origin` remote automatically.

## **3. Connecting an Existing Local Repository to a Remote — `git remote add`**
- For local projects adding a remote later.
- `git remote add origin <remote-repository-url>`
- Registers remote, names it `origin`.
- Local and remote are **connected but not synchronized**.

## **4. Pushing Code to a Remote Repository**
- Transfers local commits to remote.
- `git push origin main`
- Uploads commits, updates remote branch.
- Makes changes visible. Explicit and intentional.

## **5. Upstream Branches and `git push -u`**
- First push often sets **upstream branch**.
- `git push -u origin main`
- Links local branch to `origin/main`.
- Simplifies future `git push` and `git pull`.

## **6. Pull vs Fetch — Understanding Synchronization**
### **git fetch**
- `git fetch origin`
- Downloads remote changes.
- Does not alter local branches (updates remote-tracking refs).
- **Safe inspection step**.

### **git pull**
- `git pull origin main`
- Fetches changes, then immediately merges them.
- `git pull = git fetch + git merge`
- Prevents unintended merges.

## **7. Collaboration Through Branches and Pull Requests**
- Avoid pushing directly to `main`.
- Work on feature branches, push to remote.
- Open Pull Requests (PRs) for review.
- Ensures stable, reviewed shared branches.

## **8. Pull Requests as a Collaboration Contract**
- A PR is a **formal request to integrate changes**.
- Explains changes, enables review/discussion, records decisions.
- Controls merge timing.
- Exists only on the **remote platform**, not local Git.

## **9. Repository Access Control and Why It Matters**
- Remote repository is a shared resource.
- **Access control** defines who interacts and how (push, PRs, review, merge, settings).
- Enforced **only on remote platform**.

### **Why Essential**
- Protects main branch, ensures mandatory reviews, maintains accountability.
- Foundation of trust and safety.

### **Types of Repository Access (Role-Based)**
- GitHub roles: Read, Write, Maintain, Admin.
- Each role allows specific actions, restricting risk.

### **How Access Control Is Enforced**
- Checks user identity, role, and action permission.
- Action rejected if permission missing (e.g., push blocked without write access).

### **Adding/Managing Collaborators**
- Process: Invite, assign permission, acceptance.
- Access can be modified/revoked instantly.

### **Impact on PRs**
- Permissions dictate who opens, approves, merges PRs.
- Separates contribution from decision-making, stabilizes shared branches.

## **10. Typical End-to-End Collaboration Flow**
1.  Remote repo created.
2.  Access roles assigned.
3.  Developers clone repo.
4.  Feature branches created.
5.  Changes committed locally.
6.  Branches pushed to remote.
7.  Pull Requests opened.
8.  Reviews performed.
9.  Approved PRs merged.
10. Team synchronizes updates.
- All steps governed by remote access rules.

---

## 1. Merge — How Approved Changes Enter the Main Codebase

Merging is the final step where approved changes integrate from one branch (e.g., a feature branch) into another (commonly `main`). This stage is about **acceptance and integration** of completed, reviewed work, not new development.

Git's merge operation combines **commit history**, not just files, recording how independent lines of work converge. Modern workflows typically perform merges via Pull Requests on remote platforms to ensure review, access control, and traceability.

A merge is a **decision point**, confirming the proposed changes meet quality standards and align with project goals. Merge behavior varies: straightforward if no parallel changes, complex if both branches evolved independently, leading to a merge conflict. Merging integrates work, preserves collaboration history, and marks a stable checkpoint.

## 2. What Is a Merge Conflict

A merge conflict occurs when two branches modify the same part of the same file in incompatible ways, and Git cannot automatically decide which version is correct. Git is conservative; if there's ambiguity, it refuses to guess. A conflict means: Git detected overlapping changes, human intervention is required, and the merge is paused until resolved.

## 3. Common Causes of Merge Conflicts

Conflicts arise from timing and parallel work. Common scenarios:
*   Two branches edit the same lines of a file.
*   One branch deletes a file while another modifies it.
*   A file is renamed in one branch and edited in another.
*   Long-running branches fall far behind `main`.
*   Large refactors overlap with feature development.

The longer branches stay unmerged, the higher the probability of conflicts.

## 4. What Happens Internally When a Conflict Occurs

When Git encounters a conflict, it pauses the merge, marks the conflicted files, and inserts **conflict markers** into them. The repository enters a **conflicted state**, blocking normal commits until the conflict is resolved or the merge is aborted.

## 5. Understanding Conflict Markers

Git modifies conflicted files to show both versions of the conflicting code:

![Conflict Indicator](https://coding-platform.s3.amazonaws.com/dev/lms/tickets/29216d01-1968-4352-9d6f-606c74dc553d/vkbLoaViDCX0pzbJ.png)

*   `<<<<<<< HEAD` → current branch version
*   `=======` → separator
*   `>>>>>>> branch-name` → incoming branch version

These markers are **not valid code** and must be removed after deciding which changes to keep.

## 6. Resolving Merge Conflicts Using CLI (Conceptual Flow)

Via the command line, the developer's conceptual steps are:
1.  Identify conflicted files.
2.  Open files, review both versions.
3.  Decide the final correct code.
4.  Remove conflict markers, save.
5.  Mark conflict as resolved by staging the file (`git add`).
6.  Complete the merge with a commit (`git commit`).

Conflict markers should never remain in the final code.

## 7. Resolving Merge Conflicts Using GitHub UI

GitHub provides a web-based interface for in-browser conflict resolution, useful for small, easy-to-review conflicts where visual comparison is preferred. The GitHub UI shows both versions side-by-side, allows editing, automatically removes conflict markers, and commits the resolution. GitHub performs the same Git steps internally.

## 8. Choosing Between CLI and GitHub UI

Both approaches yield the same Git result. CLI resolution is preferred for complex, multi-file conflicts requiring local testing. GitHub UI is suitable for small, straightforward conflicts needing quick resolution. Professional teams use both depending on the situation.

## 9. Testing After Conflict Resolution

Resolving a conflict is incomplete until the code is **validated**. After resolution, ensure the application compiles/runs, tests pass, and core functionality works. Conflicts can introduce subtle bugs if resolved carelessly; testing ensures correctness before final integration.

## 10. Finalizing the Merge

Once conflicts are resolved and validated, the merge process is completed, the Pull Request merged, and the main branch updated. All collaborators can then synchronize the changes.

## 11. Common Mistakes During Conflict Resolution

Frequent errors include:
*   Leaving conflict markers in files.
*   Accidentally deleting required logic.
*   Resolving conflicts without understanding the code.
*   Skipping testing after resolution.

Conflict resolution is a **decision-making process**, not a mechanical task.

---

## **1. Why Commit History Quality Matters**

Commit history is crucial for debugging, auditing, and understanding past decisions. In projects, feature branches and an evolving `main` branch can lead to cluttered history from careless merges. This section explores how to keep history clean and linear using rebase.

## **2. What is Rebase in Git**

Rebase is a Git operation that **changes the base of a branch**. It re-applies commits from one branch on top of another. Merge combines histories; Rebase rearranges history. The final code remains unchanged, only the commit history is altered for clarity.

## **3. Rebase as an Alternative to Merge**

Direct merges create merge commits and show visible branching, making history harder to follow. Rebase replays feature commits on top of the latest `main`, resulting in a linear history with no extra merge commit. Rebase prioritizes **history clarity** over preserving exact branch structure.

## **4. Real-World Scenario for Using Rebase**

**Scenario**: A feature branch becomes outdated as `main` receives updates. 
*   **Without rebase**: A merge commit and messy history.
*   **With rebase**: The feature branch is cleanly updated, appearing as continuous development. This is the most common and safest use of rebase.

## **5. Basic Rebase Workflow (Safe Usage)**

Always perform rebase on the **feature branch**.

1.  `git switch feature-login`
2.  `git rebase main`

Git temporarily removes feature commits, updates the branch to the latest `main`, and then replays commits. It completes automatically if no conflicts occur.

## **6. What Changes After Rebase**

After rebase:
*   Feature branch starts from the latest `main`.
*   Commit IDs are regenerated.
*   History becomes linear.

This often enables a **fast-forward merge** later, keeping the `main` branch history clean. Rebase intentionally reshapes history.

## **7. Handling Conflicts During Rebase (Basic Level)**

If conflicts occur during rebase:
*   Git pauses at the conflicting commit.
*   Resolve conflicts manually and stage changes.
*   Continue with `git rebase --continue`.
*   To cancel: `git rebase --abort` (restores original branch state).

Resolving conflicts one commit at a time often clarifies issues.

## **8. Rebase and Pushing Changes**

Rebase rewrites commit history, so it should be done **before pushing** to a remote. Rebasing already-pushed branches can disrupt collaborators and might require `git push --force-with-lease`, which must be used carefully. **Rule:** Rebase only branches you own.

## **9. Rebase vs Merge: Choosing the Right Approach**

*   **Merge**: Preserves the true story, including parallel work.
*   **Rebase**: Produces a clean, linear history, easier to read and debug.

Teams often rebase feature branches for clean history, then merge into `main` after review, based on team standards.

## **10. Key Takeaways**

*   Rebase improves commit history readability.
*   It's a safe alternative to merge when used correctly.
*   Basic rebase helps keep history linear.
*   Avoid rebase on shared branches.
*   Clean history eases debugging and maintenance.

## **11. Mini Reflection Activity**

Consider your workflow:
*   Do merge commits clutter your history?
*   Could a linear history improve understanding?
*   Are you only rebasing your own branches?

Understanding when and why to rebase is essential.