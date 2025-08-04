
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


# what is interactive Rebase?
- Interactive rebase (rebase -i) allows you to edit, reorder, squah or delete commits before integrating them into the current branch history.
- Its extremely useful for cleaning up commit history before merging a feature branch or sharing code

# Why it is used:
- To squash multiple commits in to one
- To reorder commits logically
- To edit commit messages for clarity
- To remove unnecessary commits
- To split one commit in to multiple
