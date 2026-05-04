In this pre-read, you'll discover:

- How **version control** tracks every change you make to your code
- How **branches** let you work on features without breaking the main project
- How **commits** save snapshots of your progress
- How **pull requests** propose your changes for review
- How **code reviews** keep shared code clean and working

---

## Detailed Explanation

### What is Version Control?

**Version control** is a system that records every change to your files. Think of it like a detailed save system in a video game. You can revisit any earlier point, compare versions, and even undo mistakes.

**Git** is the most popular version control tool. It runs on your computer and tracks your code history locally.

**GitHub** is a website that stores your Git projects online. It makes sharing code and working with others easy.

### Why It Matters

Version control protects your work. If you break something, you can always go back. It also lets multiple people work on the same project without overwriting each other's changes.

Benefits:
- Never lose your work
- See exactly what changed and when
- Work with others without conflicts

### Commits: Your Save Points

A **commit** is a snapshot of your code at a specific moment. Each commit has a message describing what changed.

Think of commits like checkpoints in a game. You save your progress, add a note about what you accomplished, and can always return to that spot.

```bash
git add myfile.txt
git commit -m "Added a new feature"
```

### Branches: Parallel Worlds

A **branch** is a separate line of development. The main branch holds the working, stable code. You create branches to add new features or fix bugs without touching the stable code.

Imagine you have a printed book (the main branch). You don't write directly in it. Instead, you use sticky notes (branches) to add content. When ready, you transfer the notes into the book.

```bash
git branch feature-login
git checkout feature-login
```

### Pull Requests: Proposing Changes

A **pull request** (or PR) is a proposal to merge your branch into the main project. It shows exactly what you changed and asks others to review before merging.

This is where **code reviews** happen. Someone else looks at your changes, leaves feedback, and approves or requests changes.

### Collaborating Safely

Working on shared code requires rules. Here are key practices:

- Pull the latest changes before you start
- Create a new branch for each feature
- Commit often with clear messages
- Test your code before creating a pull request
- Respond kindly to review feedback

---

## Practice Exercises

1. Create a new folder on your computer and initialize it as a Git repository using `git init`.

2. Create a text file called `hello.txt`, add the text "Hello World", and commit your changes.

3. Create a new branch called `feature-notes` and switch to it.

4. On your `feature-notes` branch, add a new file and make two separate commits.

5. View your commit history using `git log`.

6. Switch back to your main branch and view the difference between the two branches.
