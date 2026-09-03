# 03 - Branching and Merging

> Understanding how Git branches provide separate lines of development and how changes are brought together through merging.

## Table of Contents

* [What is a Branch?](#what-is-a-branch)
* [Why Use Branches?](#why-use-branches)
* [Understanding `HEAD`](#understanding-head)
* [Viewing Branches](#viewing-branches)
* [Creating a Branch](#creating-a-branch)
* [Switching Branches](#switching-branches)
* [Creating and Switching in One Command](#creating-and-switching-in-one-command)
* [My Current Branch](#my-current-branch)
* [Merging Branches](#merging-branches)
* [Merge Conflicts](#merge-conflicts)
* [Deleting a Branch](#deleting-a-branch)
* [Branching Workflow](#branching-workflow)
* [Key Takeaways](#key-takeaways)

---

# What is a Branch?

A **branch** in Git is a lightweight, movable pointer to a commit that represents a line of development.

Branches allow you to work on changes separately from another line of development.

For example:

```text
main
  │
  ├── feature/readme
  │
  └── feature/data-pipeline
```

A branch is not best thought of as a completely separate copy of the project. Instead, it provides a separate line of development that points to commits.

This distinction becomes important when working with larger projects.

---

# Why Use Branches?

Branches allow you to work on new ideas, features, fixes, or documentation without immediately changing another branch.

Common reasons to create a branch include:

| Purpose       | Example                         |
| ------------- | ------------------------------- |
| New feature   | Add a new data pipeline         |
| Bug fix       | Fix a broken SQL script         |
| Documentation | Update the README               |
| Experiment    | Test a different implementation |

For example:

```text
main
  │
  └── feature/readme
         │
         ├── Update README
         └── Add Git notes
```

The work can later be merged into `main`.

---

# Understanding `HEAD`

`HEAD` is a reference that tells Git where you are currently working.

In normal branch-based work, `HEAD` points to the current branch.

For example:

```text
HEAD
 │
 ▼
feature/readme
 │
 ▼
latest commit
```

You can see `HEAD` in commands such as:

```bash
git log --oneline
```

For example:

```text
2c03998 (HEAD -> feature/readme, origin/feature/readme) Add core Git workflow notes
```

Here:

* `HEAD` means this is the commit currently checked out.
* `feature/readme` is the current local branch.
* `origin/feature/readme` is the remote-tracking branch at the same commit.

---

# Viewing Branches

To list your local branches:

```bash
git branch
```

Example:

```text
* feature/readme
  main
```

The `*` shows the branch you are currently on.

To see remote branches as well:

```bash
git branch -a
```

---

# Creating a Branch

You can create a branch with:

```bash
git branch <branch-name>
```

Example:

```bash
git branch feature/data-pipeline
```

This creates the branch but does not switch you to it.

---

# Switching Branches

A modern command for switching branches is:

```bash
git switch <branch-name>
```

Example:

```bash
git switch main
```

You can also encounter:

```bash
git checkout <branch-name>
```

`git checkout` is still supported, but `git switch` is specifically designed for branch switching and is easier to understand when learning modern Git workflows.

---

# Creating and Switching in One Command

Instead of creating a branch and switching separately:

```bash
git branch feature/data-pipeline
git switch feature/data-pipeline
```

you can do both at once:

```bash
git switch -c feature/data-pipeline
```

An older but still common equivalent is:

```bash
git checkout -b feature/data-pipeline
```

---

# My Current Branch

During this learning project, I created:

```bash
git checkout -b feature/readme
```

This created the `feature/readme` branch and switched me to it.

My work on the README and the first two notes was therefore developed on:

```text
feature/readme
```

This is an example of using a feature branch instead of making every change directly on `main`.

---

# Merging Branches

**Merging** combines the history of one branch into another branch.

Suppose I have:

```text
main
  │
  A
  │
  B

feature/readme
  │
  C
  │
  D
```

After finishing the work on `feature/readme`, I may want those changes in `main`.

First switch to the branch that should receive the changes:

```bash
git switch main
```

Then merge:

```bash
git merge feature/readme
```

The important rule is:

> The branch you are currently on is the branch that receives the merge.

So:

```bash
git switch main
git merge feature/readme
```

means:

> Merge `feature/readme` into `main`.

---

# Merge Conflicts

A **merge conflict** occurs when Git cannot automatically determine how competing changes should be combined.

For example, two branches may modify the same part of a file in incompatible ways.

Git will mark the conflicting section in the file.

A simplified example looks like:

```text
<<<<<<< HEAD
Content from current branch
=======
Content from the branch being merged
>>>>>>> feature/readme
```

I must manually decide what the final content should be.

After resolving the conflict:

```bash
git status
```

Then stage the resolved file:

```bash
git add <file-name>
```

Finally complete the merge:

```bash
git commit
```

Merge conflicts are a normal part of collaborative Git workflows.

---

# Deleting a Branch

After a branch has been merged and is no longer needed, it can be deleted.

```bash
git branch -d feature/readme
```

The `-d` option is the safer form because Git generally prevents deletion if it believes the branch contains unmerged work.

A force deletion uses:

```bash
git branch -D feature/readme
```

Use `-D` carefully because it can delete a branch even when its work has not been merged.

---

# A Practical Branching Workflow

A common feature-branch workflow is:

```text
main
  │
  │
  └── Create feature branch
             │
             ▼
      feature/my-feature
             │
             ├── Make changes
             ├── git status
             ├── git add
             └── git commit
             │
             ▼
          Review work
             │
             ▼
      Merge into main
```

Example:

```bash
git switch main
git switch -c feature/example

# Make changes

git status
git add .
git commit -m "Add example feature"

git switch main
git merge feature/example
```

---

# Branch Naming

Clear branch names make repositories easier to understand.

Examples:

```text
feature/readme
feature/data-pipeline
feature/customer-analysis
fix/login-error
docs/git-notes
```

A branch name should communicate the purpose of the work.

---

# Key Takeaways

* A branch represents a separate line of development.
* Branches are lightweight references to commits.
* `HEAD` indicates where you are currently working.
* `git branch` lists local branches.
* `git switch` changes branches.
* `git switch -c` creates and switches to a new branch.
* `git merge` combines another branch into the current branch.
* A merge conflict happens when Git cannot automatically combine changes.
* `git branch -d` safely deletes a merged branch.
* `git branch -D` force-deletes a branch and should be used carefully.

---

# Practice Tasks

Before moving to the next topic, I want to practice the following:

* [ ] Create a test branch
* [ ] Switch between `main` and the test branch
* [ ] Make a change on the test branch
* [ ] Commit the change
* [ ] Merge the branch into `main`
* [ ] Create a merge conflict intentionally
* [ ] Resolve the conflict
* [ ] Delete the test branch

These exercises will be documented in the `exercises/` directory later.

---

# What's Next?

The next note will cover **Remote Repositories and Collaboration**.

Topics will include:

* Remote repositories
* `origin`
* `git remote`
* `git clone`
* `git fetch`
* `git pull`
* `git push`
* Upstream branches
* Forks
* Pull Requests
* Collaboration workflows
