### **1. Define Version Control System (VCS).**

---

### **2. Explain why manually copying project folders is not a reliable version-management method.\_**

---

### **3. Evaluate why Git has become the industry standard. Provide two strong reasons.\_**

---


---

### **Q1. Define what Git is in the context of version control.**

---

### **Q2. Describe the main difference between Git and GitHub in terms of purpose.**

---

### **Q3. Summarise why having the same name and email in Git and GitHub matters.**

---

### **Q4. Differentiate between centralized and distributed version control systems with one real-world advantage of each.**

---

### **Q5. GitHub requires internet while Git does not. Evaluate whether it is still necessary to use GitHub in a software team and justify your answer.**

---

### **Q6. You installed Git but commits show “Unknown Author” in GitHub. Apply your knowledge to solve the issue. (What command(s) should be executed?)**


---

### **Q1. What is the purpose of the Staging Area in Git?**

---

### **Q2. Explain the difference between `git diff` and `git diff --staged`.**

---

### Q3. You modified two files: `index.html` and `style.css`. You want to commit only the changes in `index.html`.

- Write the exact Git commands to:

1. Check the status
2. Stage only `index.html`
3. Review staged changes
4. Commit with message **“Update homepage layout”**

---

### Q4. You want to check the difference between two commits with IDs `a1b2c3d` and `f4e5d6a`.

- Write the Git command.

---

### Q5. A teammate asks you to show a compact one-line history of commits.

- Which command do you use? Provide the expected output style.

---

## Q6. Complete Git Workflow: Creating a Folder, Files, Making Changes, and Committing (Full Scenario)

**Task:** Perform the following steps using Git commands and write the code you would run:

1. Create a folder named **project-demo**
2. Inside it, create a file **app.js** and write any simple JS code
3. Initialize Git in the folder
4. Stage and commit the file with the message **“Add initial app.js file”**
5. Create another file **utils.js** and write some code
6. Modify **app.js** by adding one more `console.log` line
7. Stage and commit **both files** with the message **“Add utils.js and update app.js”**

---

## Q7. Compare Changes Across Workflow (Diff Analysis)

**Task:** After completing the workflow above:

1. Compare changes between the **working directory and staging area**
2. Compare staged changes with the **last commit**
3. Compare the **initial commit** with the second commit
4. View a compact commit history

---


---

### **Q1. What is meant by “isolated development” when using Git branches?**

(Answer in 2–3 lines.)

---

### **Q2. Why is branch cleanup considered a best practice in Git?**

(Explain briefly.)

---

### **Q3. Create a project and work using branches**

**Task:**
Write the Git commands for the following steps:

1. Create a folder named **branch-demo**
2. Initialize a Git repository inside it
3. Create a file **app.txt** and add any text
4. Stage and commit the file with message **"Initial commit on main"**
5. Create a new branch named **feature-update**
6. Switch to the **feature-update** branch
7. Add more text to **app.txt**
8. Stage and commit the changes with message **"Update app.txt in feature branch"**
9. Switch back to **main**

---

### **Q4. Merge a feature branch and clean it up**

**Task:**
Continuing from the previous question, write the Git commands to:

1. Merge **feature-update** into **main**
2. Delete the **feature-update** branch after successful merge
3. Verify that the branch has been deleted

_(Assume there are no merge conflicts.)_


---

### **Q1. What is a remote repository, and why is it important for team collaboration?**

(Answer in 3–4 lines.)

_Expected focus:_
Shared source of truth, synchronization, collaboration, backup.

---

### **Q2. Explain how local and remote repositories work together in Git.**

(Include the role of commit, push, and pull.)

_Expected focus:_
Local-first workflow, explicit sharing, controlled synchronization.

---

### \*\*Q3. A developer says: “I committed my changes, so my teammates can see them.”

Is this statement correct? Justify your answer.\*\*

_Expected focus:_
Difference between local commits and pushing to remote.

---

### **Q4. Describe a typical end-to-end collaboration flow starting from a remote repository to merging changes back.**

(List the steps in order.)

_Expected focus:_
Remote repo → clone/fork → local work → push → pull request → review → merge → sync.

---


---


### **Q1. What is the purpose of setting an upstream branch using `git push -u`?**

(Answer in 2–3 lines.)

---

### **Q2. Explain the difference between `git fetch` and `git pull`. Why is `fetch` considered safer?**

(Answer briefly.)

---

## **Implementation-Based Questions (Submission Required)**

> ⚠️ **Submission Instruction (for Q6, Q7, Q8):**
> Submit the **GitHub repository link** showing commit history, branches, and merged Pull Requests.

---

### **Q3. Local to Remote Workflow (First Push)**

**Task:**
Perform the following steps and submit the repository link:

1. Create a new local repository named **remote-demo**
2. Create a file **README.md** and add some content
3. Commit the file with message **"Initial commit"**
4. Create a remote repository on GitHub
5. Connect the local repository to GitHub using `origin`
6. Push the code to GitHub and set the upstream branch

**Expected Evidence:**

- GitHub repo exists
- `main` branch contains at least one commit

---

### **Q4. Feature Branch → Push → Pull Request → Merge**

**Task:**
Using the repository from Q6, perform the following:

1. Clone the repository (if not already cloned)
2. Create a new branch named **feature-update**
3. Modify **README.md** by adding more content
4. Commit the changes with a meaningful message
5. Push the branch to GitHub
6. Raise a Pull Request from **feature-update → main**
7. Merge the Pull Request
8. Delete the feature branch after merge

**Submission:**

- GitHub repository link
- Pull Request must be visible and merged

---

### **Q5. Collaboration Workflow with Pull Requests**

**Task:**
Perform **one of the following two approaches** (based on availability):

---

#### **Option A: With Collaborator (Preferred)**

1. Add a collaborator to your GitHub repository (Write access)
2. Collaborator clones the repository
3. Collaborator creates a new branch **collab-feature**
4. Collaborator adds a file **collab.txt**, commits, and pushes
5. Collaborator raises a Pull Request to `main`
6. You review and merge the collaborator’s Pull Request

---

#### **Option B: Solo Simulation (If No Collaborator Available)**

1. Create a new branch **collab-feature**
2. Add a file **collab.txt** and commit changes
3. Push the branch to GitHub
4. Raise a Pull Request from **collab-feature → main**
5. Merge the Pull Request yourself

---

**Submission:**

- GitHub repository link
- At least one merged Pull Request must be visible


---

### **Q1. Why is merging considered a “decision point” rather than a development activity?**

(Answer in 3–4 lines.)

_Expected focus:_
Approval, review completion, acceptance into shared codebase, accountability.

---

### **Q2. After resolving a merge conflict, why is testing the application important before finalizing the merge?**

(Explain briefly.)

_Expected focus:_
Avoiding hidden bugs, validation after manual changes.

---

### **Q3. Merge Without Conflict (Basic Workflow)**

**Task:**
Perform the following steps and submit the GitHub repository link:

1. Create a new GitHub repository
2. Clone it locally
3. Add a file **app.txt** with some content
4. Commit and push to `main`
5. Create a branch **feature-a**
6. Modify **app.txt** by adding new lines (no overlapping changes)
7. Commit and push the branch
8. Raise a Pull Request from **feature-a → main**
9. Merge the Pull Request
10. Delete the feature branch after merge

> ⚠️ **Submission Instruction:**
> Submit the **GitHub repository link** clearly showing:
>
> - Branches
> - Commits
> - Pull Requests
> - Merge (with or without conflict)

---

### **Q4. Merge With Conflict (Conflict Resolution Workflow)**

**Task:**
Using the same repository or a new one:

1. Create a branch **feature-b**
2. Modify the **same lines** in **app.txt** that were modified in `main`
3. Commit and push the branch
4. Raise a Pull Request from **feature-b → main**
5. Encounter a merge conflict
6. Resolve the conflict **(CLI or GitHub UI)**
7. Ensure conflict markers are removed
8. Complete the merge
9. Verify the final content is correct

**Submission:**

- GitHub repository link
- Pull Request showing conflict and resolution

---


---

