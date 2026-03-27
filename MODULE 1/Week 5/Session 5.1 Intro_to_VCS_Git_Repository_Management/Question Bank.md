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

### **Q1. When should you use `git restore --staged file.txt`?**

(Write 2–3 lines explaining the correct scenario.)

---

### **Q2. Why is it unsafe to use `git commit --amend` after pushing your commit to a shared remote?**

(Explain in one short paragraph.)

---

### **Q3. You edited `home.js`, `nav.js`, and `styles.css`.**

- You want to discard changes only in `nav.js` but keep edits in other files.
  Write the exact Git command(s).

---

### **Q4. You accidentally staged 10 files using `git add .`, but you want to unstage all of them without deleting your actual edits.**

- Write the command(s) to fix this.

---

### **Q5. Complete the following workflow:**

1. Modify `dashboard.js`
2. Stage the file
3. Realize you need to change the commit message of your last commit
4. Fix the commit message to: **"Update dashboard logic"**
5. Add a missing file `auth.js` into that same commit

Write all the commands in order.

---

### **Q6. Full Workflow: Create, Commit, Modify, Restore**

**Task:**
Perform the following steps with exact Git commands:

1. Create a new file named **profile.js**
2. Add a simple console log inside it
3. Stage and commit it with message **"Add profile module"**
4. Modify the file by adding another console log
5. Decide you want to discard the new changes and return the file to the last committed version
6. Write all the Git commands in order

---

### **Q7. Workflow with Unstage + Amend**

**Task:**
Follow these steps and write all required commands:

1. Create a file **settings.js**
2. Add some settings-related code
3. Stage the file
4. Realize you staged it too early and need to edit it again — unstage the file
5. Make additional edits
6. Stage it again and commit
7. Immediately realize the commit message is wrong — update the commit message to **"Add settings file"**
8. Add a missing file **defaults.js** into the same commit using amend

Write the full sequence of Git commands from start to finish.


---

