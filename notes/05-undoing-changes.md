# 05 - Undoing Changes and Recovery

> Understanding how to safely undo, reverse, or temporarily set aside changes in Git.

Undoing changes is an important part of working with Git.

Mistakes happen. I may edit the wrong file, stage something accidentally, commit something I did not intend to commit, or need to temporarily put unfinished work aside.

Git provides several commands for handling these situations, but they are **not interchangeable**.

---

## Table of Contents

* [The First Question to Ask](#the-first-question-to-ask)
* [Quick Decision Guide](#quick-decision-guide)
* [`git restore`](#git-restore)
* [`git restore --staged`](#git-restore---staged)
* [`git reset`](#git-reset)
* [`git revert`](#git-revert)
* [`git stash`](#git-stash)
* [Practical Scenarios](#practical-scenarios)
* [Important Safety Rules](#important-safety-rules)
* [Key Takeaways](#key-takeaways)

---

# The First Question to Ask

Before undoing anything, I should ask:

> **Where is the change right now?**

Is it:

```text
Working Directory
        ↓
Staging Area
        ↓
Local Repository
        ↓
Remote Repository
```

The answer determines which Git command is appropriate.

---

# Quick Decision Guide

| Situation                                       | Command                       | What it does                                                                               |
| ----------------------------------------------- | ----------------------------- | ------------------------------------------------------------------------------------------ |
| I changed a file but have not staged it         | `git restore <file>`          | Discards the file's working-directory changes                                              |
| I staged a change by mistake                    | `git restore --staged <file>` | Removes the change from the staging area but keeps the file edits                          |
| I need to move `HEAD` to another commit         | `git reset`                   | Moves the current branch reference and may change the index/worktree depending on the mode |
| I need to undo an existing commit safely        | `git revert <commit>`         | Creates a new commit that reverses an earlier commit                                       |
| I need to temporarily set aside unfinished work | `git stash`                   | Stores eligible uncommitted changes for later                                              |

---

# `git restore`

`git restore` is used to restore files in the working tree.

## Discard Unstaged Changes

Suppose I edit:

```text
README.md
```

but decide I do not want those edits.

I can use:

```bash
git restore README.md
```

This restores the file in the working directory to the version from the appropriate source, which by default is the index for a working-tree restore.

### Important

This can discard uncommitted changes.

I should review my changes before running it:

```bash
git diff
```

---

# `git restore --staged`

Suppose I run:

```bash
git add README.md
```

and then realize I staged the wrong file.

I can unstage it with:

```bash
git restore --staged README.md
```

This removes the file's changes from the staging area but **keeps my edits in the working directory**.

The workflow becomes:

```text
Modified
   ↓ git add
Staged
   ↓ git restore --staged
Modified
```

This is useful when I want to correct what will be included in my next commit without losing my work.

---

# `git reset`

`git reset` is more powerful and can change the position of `HEAD`.

It can also affect the staging area and working directory depending on the mode used.

Common modes include:

| Mode      | General effect                                                              |
| --------- | --------------------------------------------------------------------------- |
| `--soft`  | Moves `HEAD` while keeping changes staged                                   |
| `--mixed` | Moves `HEAD` and resets the staging area while keeping working-tree changes |
| `--hard`  | Moves `HEAD` and resets the index and working tree to match it              |

Example:

```bash
git reset --soft HEAD~1
```

This moves `HEAD` back one commit while keeping the changes staged.

Another example:

```bash
git reset HEAD~1
```

The default mode is `--mixed`.

### Why I should be careful

`git reset --hard` can discard uncommitted changes.

For example:

```bash
git reset --hard HEAD
```

can make the working tree and staging area match the current commit, discarding local changes.

I should understand exactly what I am doing before using destructive reset options.

---

# `git revert`

`git revert` is used to undo the effect of an existing commit.

Example:

```bash
git revert <commit-hash>
```

Instead of removing the old commit, Git creates a **new commit** that reverses the changes.

Conceptually:

```text
Original history

A → B → C
        ↑
      mistake


After revert

A → B → C → D
            ↑
     reverses C
```

This preserves the existing history.

### Why this matters

When a commit has already been shared with other people or pushed to a remote repository, `git revert` is generally safer than rewriting the shared history with `git reset`.

---

# `git stash`

Sometimes I have unfinished work but need to switch branches or work on something else.

I may not want to create a commit yet.

Git provides:

```bash
git stash
```

This temporarily stores eligible local changes and returns the working tree to a cleaner state.

Example:

```text
Working on feature A
       ↓
Need to switch tasks
       ↓
git stash
       ↓
Work can be resumed later
```

---

## Restore Stashed Work

### `git stash apply`

```bash
git stash apply
```

Restores the stashed changes while keeping the stash entry.

### `git stash pop`

```bash
git stash pop
```

Restores the stashed changes and removes the stash entry if the application succeeds.

---

## View Stashes

```bash
git stash list
```

Example:

```text
stash@{0}: WIP on feature/readme
stash@{1}: WIP on feature/data-pipeline
```

---

# Practical Scenarios

## Scenario 1: I changed a file and want to discard the changes

First review:

```bash
git diff
```

Then:

```bash
git restore README.md
```

---

## Scenario 2: I staged the wrong file

Check:

```bash
git status
```

Then:

```bash
git restore --staged README.md
```

My edits remain in the working directory.

---

## Scenario 3: I want to edit my most recent commit before sharing it

A common approach is:

```bash
git reset --soft HEAD~1
```

This moves `HEAD` back one commit while keeping the changes staged.

I can then modify the files and create a new commit.

> This changes local history, so I should be careful when the commit has already been pushed and shared.

---

## Scenario 4: A previously committed change introduced a problem

Use:

```bash
git revert <commit-hash>
```

Git creates a new commit that reverses the earlier change.

This preserves the existing history.

---

## Scenario 5: I need to switch tasks but my current work is unfinished

Use:

```bash
git stash
```

Switch branches:

```bash
git switch another-branch
```

Return later:

```bash
git stash pop
```

---

# A Simple Mental Model

I can remember the commands like this:

```text
Not committed yet?
        │
        ├── Wrong edits?
        │       └── git restore
        │
        ├── Wrongly staged?
        │       └── git restore --staged
        │
        └── Need to put work aside?
                └── git stash


Already committed?
        │
        ├── Want to move local history?
        │       └── git reset
        │
        └── Want to undo while preserving history?
                └── git revert
```

---

# Important Safety Rules

### 1. Review before destroying

Use:

```bash
git status
git diff
```

before running commands that may discard changes.

### 2. Be careful with `git reset --hard`

It can permanently discard changes that are not safely stored elsewhere.

### 3. Be careful rewriting shared history

Commands such as reset and rebasing can change commit history.

If other people are already working with those commits, changing the shared history can create problems.

### 4. Prefer `git revert` for shared commits

When a problematic commit has already been pushed and shared, creating a new reversing commit is usually safer than rewriting the existing history.

---

# Key Takeaways

* `git restore` is primarily used to restore files in the working tree.
* `git restore --staged` removes changes from the staging area without removing the working-directory edits.
* `git reset` changes where `HEAD` points and can affect the index and working tree depending on the mode.
* `git revert` creates a new commit that reverses an earlier commit.
* `git stash` temporarily stores eligible uncommitted changes.
* `git stash apply` restores a stash while keeping it in the stash list.
* `git stash pop` restores a stash and removes the stash entry when successful.
* `git status` and `git diff` should be used before performing potentially destructive operations.
* Understanding the state of your changes is more important than memorizing commands.

---

# Practice Tasks

Before moving on, I will practice these scenarios in a safe test repository:

* [ ] Modify a file and restore it
* [ ] Stage a file and unstage it
* [ ] Create a commit and inspect it with `git log`
* [ ] Practice `git reset --soft`
* [ ] Practice `git revert`
* [ ] Stash unfinished work
* [ ] Restore a stash with `apply`
* [ ] Restore a stash with `pop`

> I will practice destructive commands on test files rather than using them carelessly on important work.

---

# What's Next?

The next stage of this learning project will move from **notes into hands-on exercises**.

I will create the `exercises/` directory and use it to practice Git commands and workflows in realistic scenarios.

The first exercises will cover:

* Repository initialization
* Working with commits
* Branch creation and switching
* Merging
* Merge conflicts
* Remote repositories
* Undoing changes
* Stashing work
