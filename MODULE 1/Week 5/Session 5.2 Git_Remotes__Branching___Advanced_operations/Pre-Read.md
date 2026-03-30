As projects grow, multiple types of work often happen at the same time—new features, bug fixes, experiments, and urgent changes. Git branching provides a structured way to manage this parallel work without affecting the stability of the main codebase.

## 1. Why Branching Exists

![Image](https://nvie.com/img/git-model%402x.png?utm_source=chatgpt.com)

![Image](https://itknowledgeexchange.techtarget.com/coffee-talk/files/2021/01/gitflow-hotfix-branch-diagram.jpg?utm_source=chatgpt.com)

A stable version of the application must remain reliable while new changes are developed.

- Incomplete features can break the application.
- Critical bugs need immediate fixes.
- Experimental ideas should not affect production code.

Without branches, all changes mix, increasing risk and confusion.

## 2. What a Branch Represents

A branch is an independent line of development pointing to a specific commit.

- Each branch tracks its own sequence of commits.
- Work done in one branch does not impact others.
- The `main` branch usually represents stable code.

Branches are lightweight and easy to create or remove.

## 3. Isolated Development

![Image](https://anarsolutions.com/wp-content/uploads/2019/11/Gitflow.png?utm_source=chatgpt.com)

![Image](https://miro.medium.com/0%2AG2z5FjDvIsQRfKvM.png?utm_source=chatgpt.com)

Isolation is the core benefit of branching.

- Feature work continues without breaking stable code.
- Bug fixes do not interfere with feature development.
- Experiments can be discarded safely.

Each branch acts like a focused workspace for a specific purpose.

## 4. Managing Multiple Branches

Developers need ways to view, identify the active, and switch between branches. Clear visibility prevents working on the wrong branch.

## 5. Creating and Switching Branches

Branch creation allows starting new work without disturbing existing code (e.g., for a login feature or a header bug fix). Switching branches changes the active workspace so new changes apply only to that specific branch.

## 6. Branch Naming Practices

Consistent naming improves readability and collaboration. Branch names should indicate purpose (feature, bugfix, hotfix) to reduce confusion and make repositories easier to manage long-term.

## 7. Deleting and Cleaning Up Branches

![Image](https://git-scm.com/book/en/v2/images/basic-merging-1.png?utm_source=chatgpt.com)

![Image](https://wac-cdn.atlassian.com/dam/jcr%3Ac6db91c1-1343-4d45-8c93-bdba910b9506/02%20Branch-1%20kopiera.png?cdnVersion=3129&utm_source=chatgpt.com)

Branches are temporary. A feature branch is removed after merging, an experimental branch deleted when abandoned, and old branches are cleaned to avoid clutter. Regular cleanup keeps the repository organized and readable.

## 8. Branch Lifecycle Overview

A typical branch follows a simple lifecycle:
1. Creation for a specific purpose.
2. Active development in isolation.
3. Integration back into stable code (merge covered later).
4. Deletion after use.

This lifecycle reflects how professional teams structure development work.

---

Git manages local code, but effective collaboration requires a shared **remote repository**. Remotes enable synchronization, reviews, and control over project changes.

## 1. Git vs GitHub
*   **Git**: Local version control (code management).
*   **GitHub**: Remote hosting (teamwork, collaboration).

## 2. Remote Repository
A Git repository stored off-machine (e.g., GitHub).
*   Shared source of truth.
*   Holds full project history.
*   Enables collaboration & backup.

## 3. Local-Remote Relationship
*   Work happens locally first.
*   Changes are shared explicitly (push/pull).

## 4. Meaning of `origin`
*   Default name for the primary remote.
*   A simple shortcut, not a special keyword.

## 5. Repository Basics
A repo holds code, history, branches.
*   **Local**: on your machine.
*   **Remote**: on GitHub (for teamwork).

## 6. Cloning
*   Creates a local copy of a remote repo.
*   Includes full history and links to the remote.

## 7. Push and Pull
*   **Push**: Local changes → Remote.
*   **Pull**: Remote changes → Local.
Synchronization is always manual.

## 8. Collaboration Models
*   **Collaborators**: Direct access to remote.
*   **Forking**: Independent copy (for restricted access).

## 9. Pull Requests (PRs)
*   Propose changes to a remote.
*   Enable review, discussion, and controlled merging.

## 10. Review Decisions
*   **Approve**: Allows merge.
*   **Request changes**: Blocks merge.

## 11. Typical Collaboration Flow
1.  Remote repo exists.
2.  Local work, then push changes.
3.  Open Pull Request.
4.  Review and merge.
5.  Local copies sync.

---

Teams need a shared place for code storage, review, and synchronization. Remote repositories provide this, allowing independent local work.

### What a Remote Repository Provides
- Centralized reference point for a project.
- Stores code and full history online.
- Allows multiple developers to sync work.
- Enforces permissions and review rules.
- Acts as a reliable backup.

### Git and GitHub — Different Responsibilities
- Git: Manages local code history (commits, branches, versions).
- GitHub: Hosts online repositories, manages team collaboration.

### Local and Remote Working Together
- Development occurs locally first.
- Changes shared when pushed to remote; updates received when pulled or fetched.
- Allows offline work, controls sharing.

### Cloning and Connecting Repositories
- Cloning: Creates a complete local copy of a remote repository, including its full history.
- Connecting: Links a local repository to a remote later, without immediate code sharing.

### Sharing and Receiving Changes
- Pushing: Sends committed changes from local to remote.
- Pulling: Brings remote changes into the local branch.
- Fetching: Retrieves updates without merging them immediately.

### Collaboration Using Branches and Pull Requests
- Teams use feature branches, not direct pushes to stable branches.
- Work shared via Pull Requests (PRs) for review.
- PRs enable discussion, approval, and controlled merging on the remote platform.

### Why Repository Access Control Matters
- Defines who can view, contribute, review, merge, or manage settings on shared remote repositories.
- Protects stable branches, enforces reviews, and maintains accountability.

### Typical Collaboration Flow
1.  Remote repository created.
2.  Access permissions assigned.
3.  Developers clone the repository.
4.  Work done on feature branches.
5.  Changes pushed to remote.
6.  Pull Requests reviewed.
7.  Approved changes merged.
8.  Team members sync updates.

---

In collaborative development, merging integrates reviewed and approved work into the main branch, marking an official integration point.

### Real-World Problems Without a Clear Merge Process
Unreviewed code enters the main branch, parallel changes overwrite each other, there's no clear record of why or when changes were accepted, and bugs can enter production without accountability.

### What a Merge Represents
A merge integrates one branch’s commit history into another (typically `main`), accepting completed work after review. It integrates approved changes, preserves collaboration history, and marks a stable checkpoint. Merges usually happen through Pull Requests for review and traceability.

### Why Merges Sometimes Fail Automatically
When two branches evolve independently, Git must reconcile differences. If the same part of a file is changed incompatibly, Git stops the merge; this is called a **merge conflict**.

### What a Merge Conflict Means
A merge conflict indicates ambiguity. Git detected overlapping changes, automatic merge was unsafe, human decision is required, and integration is paused until resolved. Conflicts protect the codebase from unintended data loss.

### Common Causes of Merge Conflicts
Conflicts typically occur when parallel work overlaps, such as editing the same lines in a file, deleting a file in one branch while modifying it in another, renaming files during active development, or long-running branches falling behind `main`. The longer branches remain separate, the higher the risk.

### What Happens During a Conflict
When a conflict occurs, Git pauses the merge and marks the affected files. These files are temporarily modified to show both versions of the conflicting changes:

![Merge Conflict](https://coding-platform.s3.amazonaws.com/dev/lms/tickets/29216d01-1968-4352-9d6f-606c74dc553d/vkbLoaViDCX0pzbJ.png)

The repository enters a conflicted state until a decision is made.

### Resolving Conflicts Conceptually
Resolving a conflict involves reviewing both versions, deciding the correct final code, and removing all conflict markers. Once resolved, the merge can continue and be completed. Conflict resolution is a reasoning task, not a mechanical one.

### CLI vs GitHub UI Resolution
Conflicts can be resolved locally using Git (CLI) or directly on GitHub. Local resolution is preferred for complex changes requiring testing. GitHub’s interface is useful for small, straightforward conflicts with visual comparison. Both approaches achieve the same outcome.

### Why Testing After Resolution Matters
Conflict resolution can unintentionally introduce bugs. Testing ensures that the integrated code still works as expected before final acceptance. A merge is complete only after validation.

### Quick Self-Check
Merges must be intentional and safe.

---

### **Rebase & Commit History — Pre-Read**

Commit history explains code evolution; a clean history improves debugging, reviews, and maintenance.

### **Why Commit History Becomes Hard to Read**

Feature branches often become outdated as `main` evolves. Merging these carelessly clutters history with merge commits and parallel paths. The question is: How can changes be integrated without cluttering history?

### **What Rebase Is Conceptually**

Rebase is a Git operation that changes a branch's starting point. It replays commits onto another branch for a cleaner, more linear history. The final code remains the same; only the commit history is reorganized.

### **Rebase vs Merge — The Core Difference**

Merge preserves the complete branching story, including parallel paths. Rebase reshapes commits to appear as if developed on top of the latest `main`. Rebase favors readability, while merge favors historical accuracy.

### **A Common Real-World Rebase Scenario**

When a feature branch becomes outdated due to updates in `main`, rebasing updates the feature branch onto the latest `main`, preparing it for clean integration. This is its safest and most common use.

### **What Happens During a Rebase**

Git temporarily removes feature commits, moves the branch to the latest base, then reapplies each commit. If conflicts occur, Git pauses for manual resolution.

### **What Changes After Rebase**

After rebase, the branch starts from the latest `main`, commit IDs change, and history becomes linear. This enables fast-forward merges, keeping the `main` branch clean.

### **Conflict Handling in Rebase**

Conflicts during rebase are resolved one commit at a time. This often makes understanding them easier. You can `continue` after resolution or `abort` to restore the original branch state.

### **Rebase and Remote Repositories**

Rebase rewrites history, so do it *before pushing* to remote. Rebasing pushed branches can disrupt collaborators and typically requires careful force pushing. Rule: rebase *only branches you own*.

### **Choosing Between Rebase and Merge**

Merge preserves the full development story and is safer for shared branches. Rebase produces a clean, linear history that's easier to read and debug. Most teams rebase feature branches locally and then merge them into `main` after review, following agreed team practices.