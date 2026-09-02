# 01 - Git Fundamentals

> Building a strong foundation in Git before moving into workflows, branching, remote repositories, collaboration, and more advanced concepts.

## Table of Contents

* [What is Git?](#what-is-git)
* [Why Version Control Matters](#why-version-control-matters)
* [Centralized vs Distributed Version Control](#centralized-vs-distributed-version-control)
* [Git vs GitHub](#git-vs-github)
* [How Git Works](#how-git-works)
* [The `.git` Directory](#the-git-directory)
* [Git's Main Areas](#gits-main-areas)
* [File States in Git](#file-states-in-git)
* [Git Configuration](#git-configuration)
* [Line Endings](#line-endings)
* [Git Interfaces](#git-interfaces)
* [Key Takeaways](#key-takeaways)

---

## What is Git?

**Git is a distributed version control system that tracks changes to files and project history over time.**

Git allows you to:

* Track changes to a project
* Compare different versions of files
* Review project history
* Create separate lines of development
* Combine changes from different branches
* Work locally without always needing an internet connection
* Synchronize work with remote repositories
* Collaborate with other developers

Git was created by **Linus Torvalds** in 2005 to support the development of the Linux kernel.

---

## Why Version Control Matters

Without version control, managing different versions of a project can become difficult.

For example:

```text
project/
├── report_final
├── report_final_v2
├── report_final_revised
└── report_final_final
```

This approach makes it difficult to answer questions such as:

* What changed?
* When did the change happen?
* Who made the change?
* Which version is the latest?
* Can I return to an earlier version?

Git provides a structured way to track project history.

Instead of creating multiple copies manually, Git records changes through **commits**.

A commit represents a recorded snapshot of a project's history.

---

## Centralized vs Distributed Version Control

There are different approaches to version control.

### Centralized Version Control

In a centralized version control system, the main project history is stored on a central server.

```text
Developer A ─┐
Developer B ─┼──► Central Server
Developer C ─┘
```

Example:

* Apache Subversion (SVN)

### Distributed Version Control

In a distributed version control system such as Git, each developer can have a local repository containing project history.

```text
Remote Repository
       │
       ▼
Developer A ─── Local Repository

Developer B ─── Local Repository

Developer C ─── Local Repository
```

This allows many Git operations to be performed locally.

### Comparison

| Centralized VCS                                         | Distributed VCS                                        |
| ------------------------------------------------------- | ------------------------------------------------------ |
| Main history is primarily stored on a central server    | Developers can have local copies of repository history |
| Users depend more heavily on the central server         | Many operations can be performed locally               |
| Often requires server communication for more operations | Commits and history can be managed locally             |
| Example: SVN                                            | Example: Git                                           |

> Git is a **distributed version control system (DVCS)**.

---

## Git vs GitHub

Git and GitHub are related, but they are not the same thing.

| Git                                   | GitHub                                                       |
| ------------------------------------- | ------------------------------------------------------------ |
| A distributed version control system  | A platform for hosting and collaborating on Git repositories |
| Tracks changes and project history    | Hosts repositories online                                    |
| Primarily runs on your local computer | Provides remote hosting and collaboration features           |
| Can be used without GitHub            | Works with Git repositories                                  |

### Simple way to remember

```text
Git      → Tracks your project's history
GitHub   → Hosts Git repositories and supports collaboration
```

GitHub also provides collaboration features such as:

* Pull Requests
* Issues
* Code review
* Discussions
* Project management tools

Git can be used without GitHub.

---

## How Git Works

A common Git workflow looks like this:

```text
Working Directory
      │
      │ git add
      ▼
Staging Area (Index)
      │
      │ git commit
      ▼
Local Repository
      │
      │ git push
      ▼
Remote Repository
```

### Typical workflow

1. Create or modify files in the **working directory**.
2. Select changes for the next commit using `git add`.
3. The selected changes are placed in the **staging area**.
4. Create a commit using `git commit`.
5. The commit is stored in the **local repository**.
6. Use `git push` to send local commits to a **remote repository**.

> **Important:** `git commit` creates a commit in your local repository. `git push` sends commits to a remote repository.

---

## The `.git` Directory

When you run:

```bash
git init
```

Git creates a hidden directory called:

```text
.git
```

This directory contains important information Git uses to manage the repository.

Conceptually, Git stores information about:

* Commits
* Branches
* Tags
* Configuration
* References
* Other repository metadata

### Important

Do not manually delete or modify the `.git` directory unless you understand the consequences.

If the `.git` directory is removed, the folder may no longer function as the same Git repository.

---

## Git's Main Areas

Git is commonly explained using three main areas.

### 1. Working Directory

The **working directory** is where you create, edit, and delete files.

For example:

```text
git-learning/
├── README.md
└── notes/
    └── 01-git-fundamentals.md
```

These are the files you are actively working with.

---

### 2. Staging Area

The **staging area**, also called the **index**, is where you select changes for the next commit.

For example:

```bash
git add README.md
```

This tells Git to prepare the current changes in `README.md` for the next commit.

The staging area gives you control over exactly what goes into a commit.

---

### 3. Local Repository

The **local repository** stores the commit history on your computer.

When you run:

```bash
git commit -m "Add Git fundamentals notes"
```

Git creates a commit in your local repository.

You can view commit history using:

```bash
git log
```

---

## File States in Git

A file can move through different states during a Git workflow.

| State          | Meaning                                                |
| -------------- | ------------------------------------------------------ |
| **Untracked**  | Git has not started tracking the file                  |
| **Unmodified** | A tracked file matches the latest committed version    |
| **Modified**   | A tracked file has changes that are not yet staged     |
| **Staged**     | Selected changes are prepared for the next commit      |
| **Committed**  | The changes have been recorded in the local repository |

### Example

```text
Create README.md
      ↓
Untracked
      ↓ git add README.md
Staged
      ↓ git commit
Committed
      ↓ edit README.md
Modified
      ↓ git add README.md
Staged
      ↓ git commit
Committed
```

A command I will use regularly is:

```bash
git status
```

It shows the current state of files in the repository.

---

## Git Configuration

Git uses configuration settings to control how Git behaves.

### Configure Your Name

```bash
git config --global user.name "Your Name"
```

### Configure Your Email

```bash
git config --global user.email "your.email@example.com"
```

Git uses this information to identify the author of commits.

---

## Configuration Levels

Git configuration can be applied at different levels.

| Level  | Scope                                            |
| ------ | ------------------------------------------------ |
| System | Applies across the system                        |
| Global | Applies to repositories used by the current user |
| Local  | Applies only to the current repository           |

View configuration:

```bash
git config --list
```

Get help:

```bash
git config --help
```

---

## Line Endings

Different operating systems use different line-ending conventions.

* **Windows:** CRLF (`\r\n`)
* **Linux and Unix-like systems:** LF (`\n`)

Git can help manage line-ending conversion between environments.

For example, on Windows:

```bash
git config --global core.autocrlf true
```

This is particularly useful when a project is shared across different operating systems.

---

## Git Interfaces

Git can be used through different interfaces.

### Command Line Interface (CLI)

Examples:

* Git Bash
* Terminal
* PowerShell

The command line provides direct access to Git commands.

### Code Editors and IDEs

Examples:

* Visual Studio Code
* IntelliJ IDEA

These tools can provide graphical interfaces for Git operations.

### Graphical User Interfaces (GUIs)

Examples:

* GitHub Desktop
* Sourcetree

These tools provide visual interfaces for Git operations.

### My Learning Environment

I am currently learning Git using:

* **Git Bash** on Windows for Git commands and terminal practice
* **Visual Studio Code** for creating and editing files
* **GitHub** for hosting and sharing my repositories

My primary goal is to understand the Git command line. VS Code is used mainly for creating, editing, and organizing project files.

---

## Key Takeaways

* Git is a distributed version control system.
* Git tracks project history through commits.
* Git and GitHub are not the same thing.
* A Git repository can exist locally without GitHub.
* The working directory is where files are created and modified.
* The staging area is where changes are selected for the next commit.
* A commit records changes in the local repository.
* `git push` sends local commits to a remote repository.
* Git uses the `.git` directory to store repository metadata and history.
* `git status` is an essential command for understanding the current state of a repository.

---

## What's Next?

The next note in this learning journey will focus on the **core Git workflow**, including:

* `git init`
* `git status`
* `git add`
* `git diff`
* `git commit`
* `git log`

The focus will be on understanding how changes move from the working directory to the staging area and finally into the local repository.

---

## Learning Resources

Resources I am using to learn and practice Git:

* [GitByBit](https://gitbybit.com/)
* [Official Git Documentation](https://git-scm.com/doc)
* [Pro Git Book](https://git-scm.com/book/en/v2)
* [GitHub Docs](https://docs.github.com/)

I am also learning through hands-on practice using Git Bash, Visual Studio Code, and GitHub.
