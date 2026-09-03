# 04 - Remote Repositories and Collaboration

> Understanding how local Git repositories communicate with remote repositories and how GitHub supports collaborative workflows.

## Table of Contents

* [What is a Remote Repository?](#what-is-a-remote-repository)
* [Understanding `origin`](#understanding-origin)
* [Viewing Remotes](#viewing-remotes)
* [Adding a Remote](#adding-a-remote)
* [Cloning a Repository](#cloning-a-repository)
* [Pushing Changes](#pushing-changes)
* [Fetching Changes](#fetching-changes)
* [Pulling Changes](#pulling-changes)
* [Fetch vs Pull vs Push](#fetch-vs-pull-vs-push)
* [Upstream Branches](#upstream-branches)
* [Remote-Tracking Branches](#remote-tracking-branches)
* [Forks and Pull Requests](#forks-and-pull-requests)
* [A Typical Collaboration Workflow](#a-typical-collaboration-workflow)
* [Key Takeaways](#key-takeaways)

---

# What is a Remote Repository?

A **remote repository** is a Git repository hosted somewhere outside your local computer.

It provides a place where Git repositories can be shared and synchronized between different computers or team members.

GitHub is one of the most widely used platforms for hosting Git repositories.

Other Git hosting platforms include services such as GitLab and Bitbucket.

A simple model is:

```text
Local Repository
       │
       │ git push
       ▼
Remote Repository
       │
       │ git fetch / git pull
       ▼
Local Repository
```

A remote repository is not necessarily GitHub. GitHub is simply one platform that can host Git repositories.

---

# Understanding `origin`

When you clone a repository, Git normally gives the remote repository the name:

```text
origin
```

`origin` is a **remote name**, not a special command.

For example:

```text
origin → https://github.com/JudeNwagu/git-learning.git
```

The name could technically be changed, but `origin` is the conventional default name used in many Git workflows.

---

# Viewing Remotes

To see the remote repositories configured for your local repository:

```bash
git remote -v
```

Example:

```text
origin  https://github.com/username/project.git (fetch)
origin  https://github.com/username/project.git (push)
```

This shows the remote name and the URL Git uses for fetching and pushing.

---

# Adding a Remote

If you created a repository locally with:

```bash
git init
```

you can connect it to a remote repository using:

```bash
git remote add origin <repository-url>
```

Example:

```bash
git remote add origin https://github.com/username/project.git
```

You can then verify the connection:

```bash
git remote -v
```

---

# Cloning a Repository

`git clone` is used to create a local copy of an existing repository.

Example:

```bash
git clone https://github.com/username/project.git
```

Cloning typically creates a new directory containing the repository and its history.

### What I did in this project

I cloned my GitHub repository with:

```bash
git clone https://github.com/JudeNwagu/git-learning
```

Git created a local directory:

```text
git-learning/
```

I then entered it with:

```bash
cd git-learning
```

This gave me a local copy of the repository that I could work on with Git.

---

# Pushing Changes

`git push` sends local commits to a remote repository.

The general form is:

```bash
git push <remote> <branch>
```

Example:

```bash
git push origin main
```

If an upstream branch has already been configured, you can often simply use:

```bash
git push
```

### What happens?

```text
Local Repository
      │
      │ git push
      ▼
Remote Repository
```

Important:

> `git push` sends **commits** to the remote. It does not simply upload every file in your working directory.

Your changes normally need to be committed first.

---

# Fetching Changes

`git fetch` downloads information and commits from a remote repository into your local repository's remote-tracking references.

Example:

```bash
git fetch origin
```

Fetching does **not automatically merge those changes into your current branch**.

This makes it useful when you want to inspect what changed remotely before deciding how to integrate those changes.

Think of it as:

```text
Remote Repository
       │
       │ git fetch
       ▼
Local Git Repository
       │
       └── Remote-tracking references
```

---

# Pulling Changes

`git pull` retrieves changes from a remote repository and integrates them into the current branch.

A simplified model is:

```text
git pull
   ≈
git fetch + integrate
```

For example:

```bash
git pull origin main
```

The exact integration behavior can depend on your Git configuration.

The important distinction is:

```text
git fetch → Retrieve remote updates without immediately integrating them

git pull  → Retrieve remote updates and integrate them into the current branch
```

---

# Fetch vs Pull vs Push

| Command     | Direction      | Purpose                                                                  |
| ----------- | -------------- | ------------------------------------------------------------------------ |
| `git fetch` | Remote → Local | Retrieve remote updates without integrating them into the current branch |
| `git pull`  | Remote → Local | Retrieve remote updates and integrate them into the current branch       |
| `git push`  | Local → Remote | Send local commits to the remote repository                              |

### Visual comparison

```text
FETCH / PULL

Remote
  │
  ▼
Local


PUSH

Local
  │
  ▼
Remote
```

---

# Upstream Branches

An **upstream branch** is a remote branch associated with a local branch.

For example:

```text
Local branch:
feature/readme

Upstream:
origin/feature/readme
```

When an upstream relationship exists, Git knows which remote branch should normally be used for operations such as push and pull.

To establish the upstream relationship when pushing a branch for the first time:

```bash
git push -u origin feature/readme
```

After that, you can often use:

```bash
git push
```

instead of specifying the remote and branch every time.

---

# Remote-Tracking Branches

A **remote-tracking branch** is Git's local record of the state of a branch on a remote repository.

For example:

```text
origin/main
origin/feature/readme
```

These are not your local working branches.

They are references Git uses to keep track of where the corresponding branches were last observed on the remote.

You can see local and remote branches with:

```bash
git branch -a
```

Example:

```text
* feature/readme
  main
  remotes/origin/main
  remotes/origin/feature/readme
```

---

# Understanding `origin/main`

Suppose Git shows:

```text
main
origin/main
```

They are different references.

```text
main
```

is your local branch.

```text
origin/main
```

is a remote-tracking reference representing the state Git last recorded for the remote `main` branch.

This distinction becomes important when checking whether your local branch and the remote branch are synchronized.

---

# Forks and Pull Requests

GitHub adds collaboration features on top of Git.

## Fork

A **fork** creates a copy of a repository under your GitHub account.

Forks are particularly useful when you want to contribute to a repository without having direct write access to the original repository.

For example:

```text
Original Repository
        │
        │ Fork
        ▼
Your GitHub Repository
```

---

## Pull Request

A **Pull Request (PR)** is a GitHub collaboration feature used to propose changes from one branch or repository to another.

A typical Pull Request allows people to:

* Review code
* Discuss changes
* Run automated checks
* Suggest modifications
* Approve or request changes
* Merge the work

Git provides the underlying version control concepts such as commits and branches, while GitHub provides the Pull Request collaboration workflow.

---

# A Typical Collaboration Workflow

A common GitHub workflow can look like this:

```text
Clone / Fork Repository
          │
          ▼
Create Feature Branch
          │
          ▼
Make Changes
          │
          ▼
Review Changes
          │
          ▼
git add
          │
          ▼
git commit
          │
          ▼
git push
          │
          ▼
Create Pull Request
          │
          ▼
Code Review
          │
          ▼
Merge
```

Example:

```bash
git clone https://github.com/username/project.git

cd project

git switch -c feature/customer-analysis

# Make changes

git status
git diff

git add .
git commit -m "Add customer analysis"

git push -u origin feature/customer-analysis
```

After pushing the branch, a Pull Request can be opened on GitHub.

---

# Keeping a Local Branch Up to Date

Before starting work, it is common to make sure your local branch has the latest changes.

One approach is:

```bash
git switch main
git pull origin main
```

Another approach, useful when you want more control, is:

```bash
git fetch origin
```

Then inspect the remote updates before deciding how to integrate them.

---

# My GitHub Repository Workflow

In this learning project, I have already practiced the following workflow:

```text
GitHub Repository
        │
        │ git clone
        ▼
Local Repository
        │
        │ create branch
        ▼
feature/readme
        │
        ├── Update README
        ├── Add Git fundamentals notes
        ├── Add Git workflow notes
        └── Add branching and merging notes
        │
        │ git commit
        ▼
Local Branch
        │
        │ git push
        ▼
origin/feature/readme
```

This repository is helping me learn these concepts through actual practice instead of only reading about them.

---

# Common Remote Commands

| Command                       | Purpose                                       |
| ----------------------------- | --------------------------------------------- |
| `git remote -v`               | View configured remotes                       |
| `git remote add origin <url>` | Add a remote                                  |
| `git clone <url>`             | Create a local copy of an existing repository |
| `git fetch origin`            | Download remote updates                       |
| `git pull origin main`        | Fetch and integrate changes from `main`       |
| `git push origin main`        | Push local commits to `main`                  |
| `git push -u origin <branch>` | Push a branch and set its upstream            |
| `git branch -a`               | View local and remote-tracking branches       |

---

# Key Takeaways

* A remote repository is a Git repository hosted somewhere outside your local machine.
* GitHub is a platform that can host remote Git repositories.
* `origin` is the conventional name Git gives the default remote after cloning.
* `git remote -v` shows configured remote URLs.
* `git clone` creates a local copy of an existing repository.
* `git push` sends local commits to a remote.
* `git fetch` retrieves remote updates without automatically integrating them into the current branch.
* `git pull` retrieves remote updates and integrates them into the current branch.
* An upstream branch connects a local branch with a remote-tracking branch.
* A fork is a GitHub feature for creating your own copy of another repository.
* A Pull Request is a GitHub feature for proposing and reviewing changes.
* Understanding local branches, remote-tracking branches, and remotes is essential for collaboration.

---

# Practice Tasks

Before moving to the next note, I will practice:

* [ ] View my configured remote with `git remote -v`
* [ ] View local and remote-tracking branches with `git branch -a`
* [ ] Fetch remote changes with `git fetch`
* [ ] Compare my local branch with its remote-tracking branch
* [ ] Push a new branch to GitHub
* [ ] Understand an upstream branch
* [ ] Create a Pull Request
* [ ] Practice a basic collaboration workflow

---

# What's Next?

The next note will focus on **Undoing Changes and Recovering from Mistakes**.

Topics will include:

* `git restore`
* `git restore --staged`
* `git reset`
* `git revert`
* `git stash`
* Recovering safely from common mistakes
