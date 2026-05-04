
### **Question 11 (Hard Subjective - Practical Task):**
A small team is starting a new shared Python project. The team lead has set up a central repository on GitHub and given two developers access. The developers will work on a small calculator module together. You are asked to write a step-by-step playbook that the two developers can follow so that a feature is built on a separate branch, integrated cleanly with the main branch, and shared safely with the rest of the team.

**Scenario details to use in your answer:**

- The repository on GitHub already exists and has a `main` branch.
- Developer 1 will create a Python file named `calc.py` that defines an `add` function.
- Developer 2 will create a function named `subtract` in the same file on a separate branch.
- Both developers must end with their work merged back into `main` and visible to the rest of the team.
- The team has agreed that direct commits to `main` are not allowed; every change must be made on a feature branch first.

**Requirements:**

- Show the exact Git commands each developer would run, in order, from cloning the repository to pushing the final merged result.
- Address what happens if both developers edit the same line of `calc.py` and a merge conflict appears, including how to resolve it and remove the conflict markers.
- Explain when a developer would use `git rebase main` on their feature branch and why that is useful when `main` has moved forward.
- Show a brief import example using `import calc` to demonstrate that the merged module can be reused.

**Constraints:**

- Use only the Git commands and the Python module concepts covered in this lecture (for example: `git clone`, `git branch`, `git checkout`, `git add`, `git commit`, `git push`, `git pull`, `git rebase`, `git diff`, `git apply`, `import`).
- Do not introduce extra tooling that was not part of this lecture.
- Keep commit messages short but explain the reason for the change.

**Submission Format:**

Paste your playbook inline using the skeleton below. No file uploads are required.

**Submission Format Example (use this as your skeleton):**

## Problem Restatement
*One short paragraph showing you understood the problem and any assumptions you are making.*

## Approach
*Explain your high-level approach before showing commands. Why this branching workflow?*

## Solution (Code)
*Paste the exact Git commands for both developers in order, inside fenced code blocks. Specify the language as `bash`. Also paste the final `calc.py` and the `import calc` example as Python.*

```bash
# developer 1 commands
```

```bash
# developer 2 commands
```

```python
# final calc.py
```

## Walkthrough
*Step through the key parts of your playbook in plain English. Explain when a feature branch is created, when a rebase is used, and how a merge conflict gets resolved.*

## Test Cases & Verification
*Show the inputs you would test against and the outputs you would expect, including at least one merge-conflict situation and how the conflict markers are removed before the final commit.*

## Trade-offs & Limitations
*What does this workflow do well? Where would it struggle if the team grew larger or if the code base grew to a million lines?*

**Answer Explanation (Model Answer):**

- **Ideal Approach:**
  1. Both developers start by cloning the central repository so each one has a local copy of the code base. This avoids the risk of working directly on the central repo and matches the rule that every change must come from a feature branch first.
  2. Each developer creates a separate feature branch with `git branch <name>` and switches to it with `git checkout <name>`. This isolates their work from `main` so the shared branch stays stable while the feature is in progress.
  3. Each developer creates or edits `calc.py` on their own branch, stages the change with `git add calc.py`, commits with `git commit -m "<reason>"` so the commit message captures the reason, and pushes the branch with `git push` so the feature branch is also visible on the central repo.
  4. Before merging back into `main`, each developer runs `git pull` on `main` and then `git rebase main` from their feature branch so the latest main commits sit underneath the feature work. This keeps the history clean and resolves any drift before the merge.
  5. If the same line of `calc.py` was edited by both developers, Git surfaces a merge conflict. The developer opens the file, decides the right content, removes the conflict markers (`<<<<<<`, `======`, `>>>>>>`), stages the resolved file, and commits the resolution.
  6. After the feature branches are merged into `main`, anyone on the team can run `import calc` and use both the `add` and `subtract` functions.

- **Complete Code:**

```bash
# developer 1
git clone <repo-url>
git checkout main
git pull
git branch feature
git checkout feature
# create calc.py with the add function
git add calc.py
git commit -m "add: implement add function in calc module"
git push
# bring main updates in before merging back
git checkout main
git pull
git checkout feature
git rebase main
git checkout main
# merge feature back into main
git push
```

```bash
# developer 2
git clone <repo-url>
git checkout main
git pull
git branch feature
git checkout feature
# add the subtract function to calc.py
git add calc.py
git commit -m "add: implement subtract function in calc module"
git push
# main has moved on because developer 1 already merged
git checkout main
git pull
git checkout feature
git rebase main
# if calc.py shows a conflict, open the file, remove the conflict markers, keep both functions
git add calc.py
git commit -m "fix: resolve merge conflict in calc module"
git checkout main
git push
```

```python
# final calc.py
def add(x, y):
    return x + y

def subtract(x, y):
    return x - y
```

```python
# usage example after the merged module is shared
import calc
print(calc.add(10, 2))
print(calc.subtract(10, 2))
```

- **Walkthrough of Conflict Resolution:** When developer 2 rebases on top of main, Git reports a conflict on the line that both developers touched. The file will contain a block bounded by `<<<<<<`, `======`, and `>>>>>>`. The developer keeps both function definitions (the merged file should expose both `add` and `subtract`), removes the three marker lines, saves the file, runs `git add calc.py`, and commits the resolution with a short message explaining why.

- **Test Cases & Verification:**
  - After developer 1's push and merge, run `import calc` and `calc.add(10, 2)` should print `12`.
  - After developer 2's resolved merge, run `import calc` and both `calc.add(10, 2)` and `calc.subtract(10, 2)` should work, the second returning the difference.
  - Run `git log` on `main` after both merges to verify both commit messages appear in the history with their reasons.

- **Alternative Approaches:**
  - Instead of `git rebase main`, the team could choose a plain merge of `main` into the feature branch. The lecture covers rebase as the preferred way to keep the feature branch on top of the latest main; merge would also integrate the changes but would create a merge commit instead of a linear history.
  - For sharing a small fix without merging an entire branch, a developer could capture the change as a patch with `git diff` and let a teammate apply it with `git apply`. This is useful when the change is tiny and the team wants to review the patch lines directly.

---