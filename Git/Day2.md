
# 📚 Git Branching Cheat Sheet with Real-World Examples

This guide summarizes essential Git branching operations with clear examples useful for DevOps and software development.

---

## 🔀 1. `git branch`

### ✅ Purpose:
List all branches in the current repository.

### 💡 Example:
```bash
git branch
```

**Output Example:**
```
* main
  docker-cleanup-feature
  jenkins-pipeline-fix
```

---

## 🌿 2. Create a New Branch

### ✅ Command:
```bash
git branch <branch-name>
```

### 💡 Real-World Example:
You're creating a new script for AWS EC2 monitoring:
```bash
git branch ec2-monitoring-script
```

---

## 🔄 3. Switch to a Branch

### ✅ Command:
```bash
git switch <branch-name>
```

### 💡 Real-World Example:
Switch from `main` to work on an existing feature:
```bash
git switch docker-cleanup-feature
```

---

## 🌱🔄 4. Create and Switch to a New Branch

### ✅ Commands:
```bash
git switch -c <branch-name>
```

### 💡 Real-World Example:
You got a Jira ticket to add logging to Jenkins:
```bash
git switch -c jenkins-logging-feature
```

---

## ✏️ 5. Edit Last Commit Message

### ✅ Command:
```bash
git commit --amend -m "New commit message"
```

### 💡 Real-World Example:
Fix typo in last commit:
```bash
git commit --amend -m "Added Dockerfile for Nginx container"
```

> ⚠️ Only use before pushing to remote to avoid conflicts.

---

## 🏷 6. Rename a Branch

### ✅ Command:
```bash
git branch -m <new-name>
```

### 💡 Real-World Example:
Rename incorrect branch name:
```bash
git branch -m nginx-auto-script
```

---

## ❌ 7. Delete a Branch

### ✅ Command (safe delete):
```bash
git branch -d <branch-name>
```

### ✅ Command (force delete):
```bash
git branch -D <branch-name>
```

### 💡 Real-World Example:
After merging:
```bash
git branch -d temp-testing-branch
```

---

## 📌 Summary Table

| Task                     | Command                             | Description                              |
|--------------------------|--------------------------------------|------------------------------------------|
| View branches            | `git branch`                         | See all branches                          |
| Create branch            | `git branch <name>`                  | Create new branch                         |
| Switch branch            | `git switch <name>`                  | Switch to existing branch                 |
| Create & switch branch   | `git switch -c <name>`               | Create and move to new branch            |
| Edit last commit         | `git commit --amend -m "..."`        | Fix last commit message                  |
| Rename branch            | `git branch -m <new-name>`           | Change branch name                        |
| Delete branch            | `git branch -d <name>` or `-D`       | Remove branch (safe or forcefully)       |

---

✅ Keep this as a reference during hands-on Git usage or interviews.
