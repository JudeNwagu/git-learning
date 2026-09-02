# 02 - Core Git Workflow

> Understanding how changes move from the working directory to the staging area and into the local repository.

## Table of Contents

* [Overview](#overview)
* [The Core Workflow](#the-core-workflow)
* [Checking Repository Status](#checking-repository-status)
* [Reviewing Changes](#reviewing-changes)
* [Staging Changes](#staging-changes)
* [Creating Commits](#creating-commits)
* [Viewing Commit History](#viewing-commit-history)
* [A Complete Workflow Example](#a-complete-workflow-example)
* [Key Takeaways](#key-takeaways)
* [What's Next](#whats-next)

---

## Overview

The core Git workflow is the foundation of everyday Git usage.

When working on a project, I typically:

1. Create or modify files.
2. Check what has changed.
3. Review my changes.
4. Stage the changes I want in the next commit.
5. Create a commit.
6. Review the commit history when necessary.

The basic workflow is:

```text
Working Directory
      │
      │ git add
      ▼
Staging Area
      │
      │ git commit
      ▼
Local Repository
```

This note focuses on understanding each stage and the commands used to move changes through the workflow.

---

# Initializing a Repository

To turn an existing directory into a Git repository, use:

```bash
git init
```

This creates the Git metadata needed to manage the repository.

Conceptually:

```text
Project Folder
      │
      │ git init
      ▼
Git Repository
```

Git stores repository metadata inside the hidden `.git` directory.

---

## Checking Repository Status

The command I use most frequently is:

```bash
git status
```

It helps answer:

* Which branch am I currently on?
* Are there modified files?
* Are there untracked files?
* Which changes are staged?
* Is my branch ahead of or behind its upstream branch?

A good habit is to run:

```bash
git status
```

before and after important Git operations.

---

## Understanding Common `git status` Output

### Clean Working Tree

```text
nothing to commit, working tree clean
```

This means there are no uncommitted changes in your working directory or staging area.

---

### Untracked Files

```text
Untracked files:
  README.md
```

Git can see the file, but it is not currently being tracked.

To begin tracking it:

```bash
git add README.md
```

---

### Modified Files

```text
Changes not staged for commit:
  modified: README.md
```

This means Git is already tracking the file, but the file has changed since the last commit and the current changes have not been staged.

---

### Staged Changes

```text
Changes to be committed:
  modified: README.md
```

This means the changes have been added to the staging area and are ready to be included in the next commit.

---

## Reviewing Changes with `git diff`

Before staging changes, I can review them using:

```bash
git diff
```

This shows the difference between the **working directory** and the **staging area**.

In a typical workflow:

```text
Edit file
    ↓
git diff
    ↓
Review unstaged changes
```

After staging changes, I can review what is staged using:

```bash
git diff --staged
```

This shows the difference between the **staging area** and the latest commit.

Another useful command is:

```bash
git diff HEAD
```

This compares the current working state with the latest commit.

---

## Understanding `git diff`

Git commonly displays changes using:

```text
- Removed line
+ Added line
```

For example:

```diff
- Learning Git
+ Learning Git and GitHub
```

The `-` indicates a line removed from the previous version.

The `+` indicates a line added in the current version.

Reviewing changes before staging and committing helps prevent mistakes from being added to the repository history.

---

# Staging Changes

The staging area allows me to choose which changes should be included in my next commit.

## Stage a Specific File

```bash
git add README.md
```

This stages changes from only `README.md`.

---

## Stage Multiple Specific Files

```bash
git add README.md notes/01-git-fundamentals.md
```

This allows me to choose multiple files explicitly.

---

## Stage Changes Under the Current Directory

```bash
git add .
```

This stages changes under the current directory.

---

## Stage Changes Across the Repository

```bash
git add -A
```

This stages changes across the repository, including file additions, modifications, and deletions.

> A good practice is to check `git status` before and after staging changes.

---

## Why the Staging Area Matters

The staging area allows a commit to contain selected changes rather than automatically including everything in the working directory.

For example:

```text
Working Directory

README.md                       → Modified
notes/01-git-fundamentals.md    → Modified
notes/02-git-workflow.md        → New
```

I may decide to stage only:

```text
README.md
notes/02-git-workflow.md
```

while leaving changes in `01-git-fundamentals.md` unstaged.

This allows commits to represent logical units of work.

---

# Creating Commits

After staging changes, I can create a commit:

```bash
git commit -m "Add Git workflow notes"
```

A commit records the staged changes in the local repository.

The workflow is:

```text
Working Directory
      │
      │ git add
      ▼
Staging Area
      │
      │ git commit
      ▼
Local Repository
```

> `git commit` does not automatically send changes to GitHub or another remote repository.

---

## Writing Good Commit Messages

A commit message should clearly describe the change.

Good examples:

```text
Add Git workflow notes
Update README learning objectives
Fix typo in Git fundamentals note
Add staging area example
```

Weak examples:

```text
update
changes
done
fix
```

Clear commit messages make the repository history easier to understand.

---

## Viewing Commit History

To view commit history:

```bash
git log
```

For a more compact view:

```bash
git log --oneline
```

Example:

```text
ebdd71e Add initial README
4071cce Revert "Introduced a bug"
af71e81 Added HTML5 doctype
```

Each line contains:

```text
Short Commit ID + Commit Message
```

For example:

```text
ebdd71e Add initial README
```

| Part                 | Meaning             |
| -------------------- | ------------------- |
| `ebdd71e`            | Shortened commit ID |
| `Add initial README` | Commit message      |

---

# A Complete Workflow Example

Suppose I update `README.md`.

### 1. Check the repository

```bash
git status
```

### 2. Review the changes

```bash
git diff
```

### 3. Stage the file

```bash
git add README.md
```

### 4. Review staged changes

```bash
git diff --staged
```

### 5. Commit the changes

```bash
git commit -m "Update README documentation"
```

### 6. Verify the repository

```bash
git status
```

### 7. View recent history

```bash
git log --oneline
```

The complete flow is:

```text
Edit File
    ↓
git status
    ↓
git diff
    ↓
git add
    ↓
git diff --staged
    ↓
git commit
    ↓
git status
```

---

# Key Takeaways

* `git init` initializes a Git repository.
* `git status` shows the current repository state.
* `git diff` shows unstaged changes.
* `git diff --staged` shows staged changes.
* `git add` moves selected changes to the staging area.
* `git commit` records staged changes in the local repository.
* `git log` shows commit history.
* `git log --oneline` provides a compact history.
* The staging area allows commits to contain selected, logical changes.
* A clean Git history starts with meaningful commits.

---

# What's Next?

The next note will focus on **branching and merging**.

Topics will include:

* What branches are
* Creating and switching branches
* The role of `HEAD`
* Merging branches
* Merge conflicts
* Branch management

---

## Learning Resources

Resources I am using to learn and practice Git:

* GitByBit
* Official Git Documentation
* Pro Git Book
* GitHub Docs
* Hands-on practice using Git Bash and GitHub
