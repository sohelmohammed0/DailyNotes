
# 📘 Git & Version Control

## ✅ What is Version Control (VCS)?

Version Control is a system that records changes to files over time so you can recall specific versions later. It helps track history, collaborate with teams, and manage source code efficiently.

---

## 🔁 Types of Version Control Systems

### 🧭 CVCS – Centralized Version Control System

- Codebase is stored on a **central server**.
- Developers check out and commit directly to the central server.

**Examples:** SVN, Perforce  
**Limitations:**
- Requires internet to commit.
- Central server is a single point of failure.

---

### 🔄 DVCS – Distributed Version Control System

- Every developer has a **full copy** of the repository (with history).
- Enables **offline work** and faster operations.

**Examples:** Git, GitHub, GitLab, Bitbucket  
**Advantages:**
- Work offline.
- Faster commits, diffs, and logs.
- No single point of failure.

---

## ⚖️ Difference Between CVCS and DVCS

| Feature           | CVCS                        | DVCS                        |
|------------------|-----------------------------|-----------------------------|
| History Location | Central server only         | Every user has full history |
| Offline Work     | ❌ No                        | ✅ Yes                      |
| Speed            | Slower                      | Faster                      |
| Risk             | Single point of failure     | Redundant & robust          |
| Examples         | SVN, Perforce               | Git, GitHub, GitLab         |

---

## 🔧 Git Basics

### 🆕 `git init`
Initialize a new Git repository in your project directory.

```bash
git init
```

---

### ⚙️ Git Configuration

Configure your identity before making commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Check your Git settings:

```bash
git config --list
```

---

## 📂 Basic Git Commands

### 1. 📝 `git add`

Stage changes for the next commit:

```bash
git add filename        # Add specific file
git add .               # Add all files
```

---

### 2. 🔍 `git status`

Show the current state of the working directory:

```bash
git status
```

---

### 3. ✅ `git commit`

Record staged changes with a message:

```bash
git commit -m "Your commit message"
```

---

### 4. 📜 `git log`

View the history of commits:

```bash
git log                 # Full commit history
git log --oneline       # Condensed one-line summary
```

---

## 🎯 Summary

- VCS helps track and manage code changes.
- Git is a powerful DVCS that allows local versioning and offline work.
- Essential Day 1 Git commands:
  - `git init`
  - `git add`
  - `git status`
  - `git commit`
  - `git config`
  - `git log`, `git log --oneline`

---

> 📝 Tip: Always write clear and meaningful commit messages.
