# Git Stash and Tags

This README file explains the Git commands: `git stash` and `git tag`, including their real-world use cases, examples, and practical tasks.

---

## 📌 Git Stash

### 🔍 What is `git stash`?
`git stash` temporarily shelves (or stashes) changes you've made to your working directory so you can work on something else, then come back and re-apply them later.

### 💡 Real-world Use Case:
You’re working on a new feature but are asked to urgently fix a bug on the main branch. You don’t want to commit your unfinished work. Instead, use `git stash` to save your changes, switch branches, fix the bug, and then re-apply your work.

### 🧪 Example:
```bash
# Step 1: Start working on a feature
git checkout -b feature/login
echo "new feature code" > login.js

# Step 2: Stash the changes
git stash

# Step 3: Switch to the main branch to fix a bug
git checkout main

# Step 4: Fix the bug, commit, and push

# Step 5: Go back to the feature branch
git checkout feature/login

# Step 6: Re-apply the stashed changes
git stash pop
```

### ✅ Task:
1. Create a new branch and modify a file.
2. Stash the changes.
3. Switch to `main` and modify another file.
4. Commit the second change.
5. Switch back and apply the stashed changes.

---

## 🔖 Git Tags

### 🔍 What is a Tag?
A tag in Git is a reference to a specific point in Git history (usually a commit). It’s commonly used to mark release versions.

### 📂 Types of Tags:

#### 1. **Lightweight Tag**
A simple name for a commit – like a bookmark.

**Example:**
```bash
git tag v1.0
```

#### 2. **Annotated Tag**
Includes the tagger name, email, date, and a message. Stored as a full object in the Git database.

**Example:**
```bash
git tag -a v1.0 -m "Initial release version 1.0"
```

#### 3. **Tagging a Specific Commit**
You can tag a past commit (not just HEAD).

**Example:**
```bash
git log  # find the commit hash
git tag -a v0.9 <commit-hash> -m "Pre-release version 0.9"
```

### 📤 Pushing Tags to Remote
```bash
# Push a single tag
git push origin v1.0

# Push all tags
git push origin --tags
```

### ✅ Task:
1. Create a tag (both lightweight and annotated).
2. Push tags to GitHub.
3. Clone the repo elsewhere and check the tags using `git tag`.

---

### 📚 Summary

| Command | Purpose |
|--------|---------|
| `git stash` | Temporarily save changes without committing |
| `git stash pop` | Reapply stashed changes and remove stash |
| `git tag <tagname>` | Create lightweight tag |
| `git tag -a <tagname> -m "message"` | Create annotated tag |
| `git push origin <tag>` | Push tag to remote |
| `git push origin --tags` | Push all tags to remote |

---

Happy Git-ing! 🚀