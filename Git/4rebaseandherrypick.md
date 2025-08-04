
# 🚀 Git Rebase vs Git Cherry-Pick – Explained Clearly with Real Examples & Tasks

---

## 🔄 What is `git rebase`?

### 📘 Definition:
`git rebase` is used to **move or rebase one branch onto another branch**.  
It **rewrites commit history** by replaying commits from your current branch onto a new base branch.

---

### 💼 Real-World DevOps Example:
You’re working on a feature branch `feature/update-prometheus`, and the `main` branch has received new commits.  
Before merging, you rebase your branch on top of the updated `main` to keep a **linear and clean commit history**.

---

### 🧠 Visual Example:

**Before Rebase:**

```
main:       A---B---C
feature:             D---E
```

**After Rebase:**

```
main:       A---B---C
feature:                D'---E'
```

`D` and `E` are replayed as new commits on top of `C`.

---

### ✅ Use Cases:
- Avoid merge conflicts
- Keep a clean linear history
- Sync your branch with updated main before creating a pull request

---

### 💻 Rebase Commands:

```bash
# Switch to your feature branch
git checkout feature/update-prometheus

# Rebase onto the latest main
git rebase main

# If conflicts occur
git add .
git rebase --continue
```

---

### 🧪 Hands-On Task: Try Git Rebase

1. Create a repo with a `main` branch and add 3 commits.
2. Create a branch `feature-1` from commit 2 and add 2 commits.
3. Add 1 more commit to `main`.
4. Rebase `feature-1` onto updated `main`.

---

## 🍒 What is `git cherry-pick`?

### 📘 Definition:
`git cherry-pick` is used to **apply a specific commit (or commits) from one branch to another**, without merging the entire branch.

---

### 💼 Real-World DevOps Example:
Your team fixed a bug in the `develop` branch. The fix also needs to go into `main`, but you don't want to merge all of `develop`.

---

### 🧠 Visual Example:

```
develop: A---B---C (C = bug fix)
main:    A---B
```

After cherry-picking commit `C` into `main`:

```
main:    A---B---C'
```

Commit `C` is now also in `main` as `C'` (a copy of C).

---

### ✅ Use Cases:
- Hotfixes: Apply a fix from one branch to another without merging
- Feature isolation: Pick specific changes only
- Release Management: Apply select fixes to both `dev` and `release` branches

---

### 💻 Cherry-Pick Commands:

```bash
# Switch to target branch
git checkout main

# Cherry-pick a specific commit
git cherry-pick <commit-hash>

# Pick multiple commits
git cherry-pick <hash1> <hash2>
```

To find commit hashes:
```bash
git log --oneline
```

---

### 🧪 Hands-On Task: Try Git Cherry-Pick

1. Create a repo and commit 3 changes on the `develop` branch.
2. Create a `main` branch from the first commit.
3. Cherry-pick the third commit from `develop` into `main`.

---

## ⚠️ Key Differences Summary

| Feature              | `git rebase`                             | `git cherry-pick`                        |
|----------------------|-------------------------------------------|------------------------------------------|
| Purpose              | Move all commits from one branch to another | Copy specific commits to another branch  |
| Rewrites History     | ✅ Yes                                      | ✅ Yes (for those commits only)           |
| Use Case             | Sync branches, keep linear history         | Apply fixes selectively                   |
| Best Used When       | Before merge/pull request                  | For hotfixes or picking isolated commits  |

---

## 📌 Pro Tips

- Never rebase **shared branches** unless you coordinate with teammates.
- Cherry-picking is useful but can cause **duplicate commit issues** if not tracked properly.



# Git Interactive Rebase, Restore, and Reset

---

## 🔄 What is Interactive Rebase?

**`git rebase -i`** allows you to interactively modify commit history.

### 🧠 Why use it?
- ✅ Squash multiple commits into one
- 🔄 Reorder commits
- 📝 Edit commit messages
- ❌ Remove unnecessary commits
- 🔪 Split a commit into smaller ones

---

### ✅ Example: Interactive Rebase

Assume commit history:
```
A---B---C---D (feature)
```

You want to:
- Squash C & D into one
- Edit message of B

Run:
```bash
git rebase -i HEAD~3
```

You’ll see:
```
pick B some message
pick C added feature 1
pick D added more to feature 1
```

Change to:
```
pick B some message
squash C added feature 1
squash D added more to feature 1
```

Then edit the final commit message. Done!

---

## 🧽 git restore

Restores file(s) in the working directory from:
- Last commit
- A specific branch or stash

### ✍️ Example:
```bash
git restore file.txt
```
→ Restores `file.txt` to last committed version.

```bash
git restore --source=feature file.txt
```
→ Restores `file.txt` from `feature` branch.

```bash
git restore --staged file.txt
```
→ Unstages a file (same as `git reset HEAD file.txt`)

---

## 🔙 git reset

Changes your current HEAD and can modify **staging area** and/or **working directory**.

---

### 1️⃣ git reset --soft <commit>
- Moves HEAD to `<commit>`
- Keeps changes staged

```bash
git reset --soft HEAD~1
```
→ Undo last commit, but keep changes staged.

---

### 2️⃣ git reset --mixed <commit> (default)
- Moves HEAD to `<commit>`
- Unstages changes (files stay in working dir)

```bash
git reset --mixed HEAD~1
```
→ Undo last commit, unstage the changes.

---

### 3️⃣ git reset --hard <commit>
- Moves HEAD to `<commit>`
- Discards **everything** (staging + working directory)

```bash
git reset --hard HEAD~1
```
→ Completely remove last commit & changes.

⚠️ **Use with caution!**

---

## 🧠 Summary Table

| Command                     | Affects Commit | Affects Staging | Affects Working Dir | Use Case                             |
|----------------------------|----------------|------------------|---------------------|--------------------------------------|
| `git reset --soft`         | ✅              | ✅               | ❌                  | Undo commit, keep changes staged     |
| `git reset --mixed`        | ✅              | ✅ (unstage)     | ❌                  | Unstage files after undoing commit   |
| `git reset --hard`         | ✅              | ✅               | ✅                  | Wipe commit & local changes          |
| `git restore`              | ❌              | ✅ or ❌         | ✅                  | Restore specific files               |


    
