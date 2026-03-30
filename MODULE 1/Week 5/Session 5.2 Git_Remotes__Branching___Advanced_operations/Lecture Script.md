## LO–11.1.5: Develop features using branches (30 minutes)

### 1. Ideal Scenario Activity

Imagine a team:
  - Dev 1: New feature
  - Dev 2: Another new feature
  - Dev 3: Critical bug fix
  - Dev 4: Experimental change

All modify the same project, sometimes even the same file.

Guiding questions:
  - Can all push directly to `main`?
  - If pushed together, which code is accepted?
  - What if one feature is incomplete, or an experiment breaks things?
  - Can the bug fix be released alone?

Direct work on `main` mixes unrelated changes, risks breaking stable code, and makes parallel development unsafe.

We need a system for independent work, selective change acceptance, and stable code protection.

### 2. Connect to Branching

In professional development, `main` represents stable, production-ready code. New features, fixes, and experiments must not disturb this stability.

Git **branching** solves this. Isolation is its core idea.

### 3. Concept of a Branch

A branch is an independent line of development. Each branch has its own commit history, and changes in one do not affect others. `main` is the default stable branch. Branching allows parallel work without conflict.

### 4. Isolated Development

Branches allow:
  - Feature development without risk
  - Bug fixes without blocking other work
  - Experiments without fear of failure

Each developer can work on a separate branch. Only completed and approved work is later integrated. Each branch acts like a separate workspace.

### 5. Viewing Existing Branches

List local branches:
```bash
git branch
```
`*` indicates the current branch. Always be aware of your current branch before making changes.

### 6. Creating a Branch

Create a branch without switching:
```bash
git branch feature-login
```
The branch is created, but your current working branch remains unchanged. Verify with `git branch`.

### 7. Switching Between Branches

Switch to an existing branch:
```bash
git switch feature-login
# or
git checkout feature-login
```
All new work now belongs to the selected branch. Your context switches entirely.

### 8. Creating and Switching in One Step

Combine creation and switching:
```bash
git checkout -b feature-signup
# or
git switch -c feature-signup
```
This is a common workflow.

### 9. Working Inside an Isolated Branch

Typical workflow:
  - Switch to a branch
  - Modify or create files
  - Stage changes
  - Commit changes

```bash
git switch feature-login
git add login.js
git commit -m "Add login form UI"
```
Switch back to `main`:
```bash
git switch main
```
Changes made in `feature-login` are not present in `main`. Isolation is preserved.

### 10. Student Practice — Repeat Isolation

For the next 6 minutes, practice this twice:
1. Create a new branch (e.g., `bugfix-header`, `experiment-dark-mode`)
2. Switch to the branch
3. Make a small change (e.g., add a line to a file)
4. Commit the change
5. Switch back to `main`

### 11. Branch Naming Conventions

Recommended patterns:
  - `feature-login`
  - `bugfix-navbar`
  - `hotfix-payment-issue`
  - `experiment-ui-layout`

Rules: Lowercase, hyphen-separated, purpose-driven. Avoid generic names like `test`, `new`, `temp`.

### 12. Deleting Branches

Branches are temporary.

Safe deletion (if merged):
```bash
git branch -d feature-login
```

Force deletion (for unused or abandoned branches, even if unmerged):
```bash
git branch -D experiment-old-ui
```

### 13. Branch Cleanup Best Practices

Cleanup prevents clutter, confusion, and working on outdated code.

Recommended flow:
```bash
git switch main
git branch -d feature-login
```

### 14. Branch Lifecycle

Stages:
  - Branch creation
  - Active development
  - Merge into main (next topic)
  - Branch deletion

This lifecycle is followed in professional teams.

### 15. Key Takeaways

Remember:
  - One task per branch
  - `main` must remain stable
  - Branches enable parallel development
  - Cleanup is mandatory

Next, we'll explore merging these isolated changes.

---

## LO–11.2.1: Configure & synchronize remote repositories (30 minutes)

### 1. Start with a Team Collaboration Scenario
Imagine a team of developers. Everyone works on their local machine, but you need one shared codebase, right? How do you share changes without chaos? Local Git is great for individuals, but for a team, we need a central hub. That's where a **remote repository** comes in.

### 2. Clarify the Roles of Git and GitHub
Let's distinguish Git and GitHub. **Git** is your local version control, tracking changes on your machine. **GitHub** is an online platform that *hosts* Git repositories, enabling team collaboration. Think of it this way: Git controls *how* your code changes, and GitHub controls *how people collaborate* on that code. Git works offline, GitHub is your online shared space.

### 3. Introduce the Concept of a Remote Repository
A **remote repository** is simply a Git repository stored outside your local machine, usually on a server like GitHub. It's the central hub for your project: a shared source of truth, a place for collaboration, and a backup. Each developer has their local copy, but everyone connects to this one remote.

### 4. Explain the Local–Remote Separation
Work with Git happens locally first. You make changes, you commit them – all on your machine. Nothing is shared until you explicitly **push** your changes to the remote. And you only get others' updates when you **pull**. This means you can work offline, you control when you share, and your local mistakes don't affect the team until you push them.

### 5. Explain the Meaning of “origin”
When you connect to a remote, Git gives it a name. By convention, the main remote is called `origin`. It's just a shortcut, not a special keyword. While you *can* have multiple remotes, `origin` typically points to your GitHub repository.

### 6. Define What a Repository Is
A **repository**, or 'repo', is a container for your project. It holds your source code, commit history, branches, and documentation. You have a **local repository** on your machine, and a **remote repository** on a platform like GitHub. Teamwork relies entirely on that remote repository.

### 7. Explain Creating a Repository on GitHub
Creating a repository on GitHub establishes that remote location for your project. It defines who owns it and who has access. At this point, it's just an empty container in the cloud; no local machines are connected yet.

### 8. Explain Cloning Conceptually
To start working on an existing remote project, you **clone** it. Cloning creates a complete local copy of the remote repository on your machine. This includes all the project files, the full commit history, and crucially, it automatically sets up the connection to the remote, usually as `origin`. After cloning, you work locally and then sync with the remote.

### 9. Explain Synchronization with Remote
Synchronization is explicit. You **pull** to bring changes *from* the remote *to* your local repository, and you **push** to send your local changes *to* the remote. Git never syncs automatically. This control prevents accidental overwrites and ensures clarity in your team's workflow.

### 10. Introduce Collaborators
In a direct collaboration model, you add team members as **collaborators**. Collaborators have direct push access to the remote repository, meaning they can push their branches and commits directly. This is common for internal company projects or trusted teams.

### 11. Explain Permission Management
Not all collaborators have the same level of access. GitHub allows you to manage permissions, defining who can push to certain branches, who can approve changes, and who can manage settings. This protects important branches and ensures accountability, with permissions enforced at the GitHub level, not locally.

### 12. Introduce Forking as an Alternative Model
When you don't have direct push access to a repository, like on an open-source project, you use **forking**. Forking creates your *own independent copy* of the remote repository under your account. The original repository remains protected, and you can work freely on your fork without affecting the original.

### 13. Explain Pull Requests Conceptually
A **Pull Request**, or PR, is how you propose changes to be merged into a remote repository, whether from a branch or from a fork. PRs are a GitHub feature, not a Git command. They provide a dedicated space for code review, discussion, approval tracking, and controlled merging.

### 14. Explain Review and Decision Flow
Once a PR is opened, team members review the changes, provide feedback, and discuss. The outcome is either **Approve**, allowing the changes to be merged, or **Request Changes**, which blocks the merge until issues are resolved. This ensures quality control and shared responsibility before code lands in the main branch.

### 15. Walk Through the End-to-End Collaboration Cycle
Let's summarize the end-to-end workflow: You start with a remote repository on GitHub. Developers **clone** it. They work **locally**, commit their changes, then **push** them to a branch on the remote. They open a **Pull Request**. The team **reviews** the code, and once **approved**, it's **merged** into the main branch of the remote. Finally, other developers **pull** these merged changes to update their local copies. This cycle repeats continuously.

### 16. Conclude with Reflection
Think about this: How would a team manage code without a remote repository? GitHub, with its remote repositories, brings order to team development. Remember, Git handles your local history; GitHub handles the collaboration. Remote repositories are the absolute backbone of teamwork. In our next session, we'll get hands-on with these GitHub collaboration workflows.

---

## LO–11.2.2: Collaborate using PRs & repository access control (30 minutes)

### **1. Introduce the Need for a Remote Repository**
Git manages code locally; collaboration requires a shared remote repository. It hosts project code centrally, defines ownership, and enforces rules. This remote is the single source of truth, initially existing only on GitHub with no local connection.

### **2. Demonstrate Creating a Remote Repository (Manual Demo)**
Demo: Create a GitHub repository (name, visibility, description). Explain it exists only on GitHub, contains no local code, and access rules are controlled remotely.

### **3. Demonstrate Cloning a Remote Repository (Manual Demo)**
Cloning creates a local working copy of the remote repository. Demo: Clone using a repository URL. This downloads project files, copies full commit history, and automatically configures a remote named `origin`. Developers work locally, with the remote as the shared reference.

### **4. Demonstrate Connecting an Existing Local Repository to a Remote (Manual Demo)**
Scenario: A project started locally, with a remote created later. Demo: Link the local repository to GitHub using `git remote add origin <url>`. This registers the remote as `origin`. Local and remote are now connected but not yet synchronized.

### **5. Demonstrate Pushing Code to the Remote (Manual Demo)**
Pushing transfers local commits to the remote. Demo: Push the local `main` branch to GitHub (`git push origin main`). This uploads commits, updates the remote branch, and makes changes visible to collaborators. Pushing is explicit.

### **6. Demonstrate Upstream Branch Concept (Manual Demo)**
The first push of a branch usually establishes a long-term relationship. Demo: Push with upstream configuration (`git push -u origin main`). This links the local branch to its remote counterpart, simplifying future `git push` and `git pull` commands and reducing errors.

### **7. Demonstrate Pull vs Fetch (Manual Demo)**
To receive updates from the remote: Demo fetching (`git fetch`). Changes are downloaded, but local branches remain unchanged—it's a safe inspection step. Demo pulling (`git pull`). Changes are fetched and immediately merged into the current branch. Pull = Fetch + Merge. Understanding this distinction prevents unintended merges.

### **8. Demonstrate Collaboration Using Branches (Manual Demo)**
Professional teams do not push directly to `main`. Demo: Create a feature branch, then push it to the remote. Each branch represents isolated work, ensuring shared stable branches. This prepares for Pull Requests.

### **9. Demonstrate Pull Requests (Manual Demo)**
Open GitHub and show the Pull Request (PR) interface. Demo: Selecting source and target branches, writing a PR title and description. A PR requests integration of changes, initiates review and discussion, and controls when code is merged. Hands-on portion concludes; all further concepts are theoretical.

## **Theoretical Explanation (No Live Demo from This Point)**

### **10. Explain Pull Requests as a Collaboration Contract**
Pull Requests are formal agreements to merge changes. PRs document changes and rationale, record discussions permanently, and provide approval control. They exist solely on the remote platform; local Git has no concept of PRs.

### **11. Explain Repository Access Control**
A remote repository is a shared resource. Access control defines who can push, review, merge, and manage settings. These rules are enforced only at the remote level.

### **12. Explain Why Access Control Is Essential**
Without access control: risks include production breakage, accidental deletions, and no accountability. With it: the main branch is protected, mandatory reviews are enforced, and responsibilities are clear. Access control is the foundation of safe collaboration.

### **13. Explain Role-Based Access Types**
GitHub uses conceptual roles: Read, Write, Maintain, and Admin. Each role allows specific actions; restrictions are intentional to minimize risk.

### **14. Explain How Access Control Is Enforced**
Enforcement involves an identity check, role validation, and action authorization. For example, a push is blocked if the user lacks write permission, or a merge is blocked if the role is insufficient.

### **15. Explain Adding and Managing Collaborators**
Collaborator management involves invitation, permission assignment, and acceptance. Access can be modified or revoked, with changes taking effect immediately, protecting against outdated access.

### **16. Explain Access Control Impact on Pull Requests**
Permissions influence who can open PRs, who can approve, and who can merge. This separates contribution from decision-making, ensuring the stability of shared branches.

### **17. Walk Through End-to-End Collaboration Flow**
The complete lifecycle includes: remote repository creation, role assignment, cloning, branch creation, local commits, push to remote, Pull Request creation, review/approval, merge, and synchronization. All steps are governed by remote access rules.

---

## LO–11.3.1: Merge and resolve merge conflicts (30 minutes)

### 1. Merge Context: After Pull Requests

Briefly, code is developed on branches, pushed to remote, and reviewed via Pull Requests. After approval, changes must enter the main codebase. Merging is the final step in collaboration, representing **acceptance and integration** of completed, reviewed work.

### 2. What a Merge Represents

Merging integrates approved work into another branch (usually `main`), combining commit histories, not just files. Git records how independent lines of work come together, reflecting collaboration in project history. Modern practice typically involves merges via Pull Requests for review, access control, and traceability.

### 3. Merging as a Decision Point

A merge is not automatic. By merging, the team confirms code meets quality standards, aligns with project goals, and is ready for shared use. It marks a stable checkpoint in project history.

### 4. When Merging Is Straightforward vs. Complex

Merge behavior depends on branch history. A simple scenario involves one branch progressing with no overlapping changes; Git integrates automatically. A complex scenario involves both branches changing independently with overlapping edits. Git must reconcile differences. Ambiguity results in a **merge conflict**.

### 5. Define a Merge Conflict

A merge conflict occurs when two branches modify the same part of the same file in incompatible ways. Git does not guess; it stops and asks for human input. The merge is paused, and resolution is mandatory before proceeding.

### 6. Common Causes of Merge Conflicts

Typical scenarios include: same lines edited in multiple branches; a file deleted in one branch and edited in another; a file renamed in one and modified in another; long-lived branches falling behind `main`; or large refactors overlapping with active features. The longer branches live independently, the higher the risk.

### 7. What Happens Internally During a Conflict

Git pauses the merge process, marks conflicted files, and the repository enters a conflicted state. Normal commits are blocked until the merge is resolved or aborted.

### 8. Conflict Markers Conceptually

Git modifies conflicted files to show both versions. Conflict markers include `<<<<<<< HEAD` (current branch version), `=======` (separator), and `>>>>>>> branch-name` (incoming branch version). These markers are not valid code and must be removed; the developer must decide the final logic.

### 9. Conflict Resolution via CLI (Conceptual Flow)

The developer's responsibility is to: 1. Identify conflicted files. 2. Open files manually. 3. Review both versions. 4. Decide final correct logic. 5. Remove all conflict markers. 6. Save files. 7. Stage resolved files. 8. Complete the merge. Resolution is a thinking task, not a mechanical action.

### 10. Conflict Resolution via GitHub UI

GitHub offers a web-based resolution option for small conflicts with easy-to-understand changes, preferring a visual comparison. GitHub UI provides side-by-side comparison, editing, automatic marker removal, and commit creation. Internally, GitHub performs the same merge steps.

### 11. Choosing the Right Resolution Approach

CLI resolution is suitable for complex, multi-file conflicts, especially when local testing is required. GitHub UI is ideal for small, simple conflicts. Both produce the same Git result; the choice depends on complexity and testing needs.

### 12. Importance of Testing After Resolution

Resolving conflicts is not the final step. Validation is required: ensure the application builds or runs, tests pass, and core functionality works. Conflicts can introduce subtle bugs, and testing prevents silent failures.

### 13. Finalizing the Merge

After successful resolution and testing, the merge is completed, the Pull Request finalized, and the main branch receives updated code. All collaborators then synchronize changes, and the project history reflects the merge decision.

### 14. Common Mistakes During Conflict Resolution

Frequent errors include: leaving conflict markers in files, accidentally removing required logic, resolving without understanding the code, or skipping testing. Conflict resolution requires understanding and intent; it is a decision-making process.

---

## LO–11.3.2: Improve commit history using basic rebase (20 minutes)

### **1. The Importance of Commit History**

Beyond just correct code, **commit history** explains *how* our code reached its current state. It's crucial for debugging regressions, understanding past decisions, and auditing changes. A common issue arises in feature branches that live for days or weeks while the main branch evolves. Careless merges clutter history, making it hard to read. We need to ask: Why does commit history become hard to read? And is there a way to integrate changes without polluting it?

### **2. Rebase as the Conceptual Solution**

**Rebase** is a Git operation that changes the base of a branch. Instead of merging commits as-is, they are *replayed* on top of another branch. Think of it this way: **Merge** combines histories, while **Rebase** rearranges history. The final code remains the same; only the commit history changes.

### **3. Rebase as an Alternative to Merge**

A normal merge leaves parallel histories visible and introduces merge commits, which can make history harder to follow. Rebase offers an alternative: it places feature commits directly on top of the latest main branch. This creates a linear history and avoids extra merge commits. Rebase prioritizes **readability over historical branching accuracy**.

### **4. Real-World Rebase Scenario**

Imagine you create a feature branch and work on it. Meanwhile, other changes are merged into `main`, making your feature branch outdated. 
*   **Without rebase**: you'd get a merge commit and a messy history when integrating.
*   **With rebase**: your branch is updated cleanly. 

This is the most common and safest use of rebase: preparing your branch for a clean, straightforward merge.

### **5. Basic Rebase Workflow (Conceptual)**

Rebase must always be done on your **feature branch**. First, switch to your feature branch: `git switch feature-login`. Then, rebase it onto the latest `main`: `git rebase main`. Internally, Git temporarily removes your feature commits, updates your branch pointer to the latest `main`, and then replays your commits one by one. If there are no conflicts, rebase completes automatically.

### **6. What Changes After a Rebase**

After a successful rebase, your feature branch now starts from the latest `main`. The commit IDs are regenerated, and the history becomes linear. This linearity is beneficial because when you later merge this branch, Git can often perform a *fast-forward merge*, keeping the main branch history clean. Rebase intentionally reshapes history.

### **7. Conflict Handling During Rebase (Conceptual)**

Rebase may pause if conflicts occur. Git stops at the conflicting commit, and you must resolve the conflicts manually. Once resolved and changes are staged, continue the rebase with `git rebase --continue`. If you need to cancel, `git rebase --abort` restores your branch to its original state. Resolving conflicts one commit at a time can often make them easier to understand.

### **8. Rebase and Remote Repositories**

Because rebase rewrites commit history, it should be done **before pushing** your branch to a remote repository. Rebasing pushed branches changes shared history, which can disrupt collaborators. The safety rule is: **Rebase only branches you own.** If you must rebase a pushed branch, you'll need `git push --force-with-lease`, but this must be done with extreme care and understanding of the impact.

### **9. Compare Rebase vs Merge for Decision-Making**

*   **Merge**: Preserves the full development story, including parallel work, and shows integration points clearly.
*   **Rebase**: Produces a clean, linear history that's often easier to read and debug.

Many teams adopt a common practice: rebase feature branches to clean their history, then merge them into `main` after review. The choice ultimately depends on your team's standards and policies.