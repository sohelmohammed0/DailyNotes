
# 📘 Complete Git Commands Cheat Sheet (Beginner to Advanced)

---

## 🔧 Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global core.editor "code --wait"   # Set VSCode as default editor
git config --list                               # View config
```

---

## 📁 Repository Setup

```bash
git init                          # Initialize a new repository
git clone <url>                   # Clone an existing repo
```

---

## 📄 Basic Snapshotting

```bash
git status                        # Show file status
git add <file>                    # Stage a file
git add .                         # Stage all changes
git commit -m "message"           # Commit with message
git commit -am "msg"              # Stage & commit tracked files
```

---

## 🔍 Viewing History

```bash
git log                           # Commit history
git log --oneline                 # One-line summary
git log --graph --oneline --all   # Visual history
git show <commit>                 # Show specific commit
```

---

## 🔄 Branching & Merging

```bash
git branch                        # List branches
git branch <name>                # Create branch
git switch <name>                # Switch to branch
git checkout <name>              # (Older way to switch)
git merge <branch>               # Merge branch into current
git branch -d <name>             # Delete branch (safe)
git branch -D <name>             # Force delete
```

---

## 🧪 Rewriting History

```bash
git commit --amend               # Edit last commit
git rebase -i HEAD~3             # Interactive rebase last 3 commits
git reset --soft HEAD~1          # Undo commit, keep staged
git reset --mixed HEAD~1         # Undo commit, unstage changes
git reset --hard HEAD~1          # Undo everything (DANGER)
```

---

## 🚀 Remote Repositories

```bash
git remote -v                    # Show remotes
git remote add origin <url>     # Add remote
git push origin main             # Push to remote
git pull origin main             # Pull from remote
git fetch                        # Fetch all branches
git push -u origin <branch>     # Push & track branch
```

---

## 📌 Tags

```bash
git tag                          # List tags
git tag v1.0                     # Create tag
git tag -a v1.0 -m "message"     # Annotated tag
git push origin v1.0             # Push tag
```

---

## 🧹 Cleaning & Stashing

```bash
git stash                        # Save changes temporarily
git stash pop                    # Reapply stashed changes
git clean -f                     # Remove untracked files
git clean -fd                    # Remove untracked files + dirs
```

---

## 📎 Comparing & Diffing

```bash
git diff                         # Show unstaged changes
git diff --staged                # Show staged changes
git diff branchA..branchB        # Compare branches
```

---

## 🔁 Cherry-Pick & Revert

```bash
git cherry-pick <commit>         # Apply specific commit
git revert <commit>              # Undo commit (safe way)
```

---

## 🔐 Ignore & Attributes

```bash
touch .gitignore                 # Create ignore file
touch .gitattributes             # Set custom attributes
```

---

## 🌍 Submodules

```bash
git submodule add <url> path     # Add submodule
git submodule update --init      # Init submodules
```

---

## 📦 Git Aliases (Pro)

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
```

---

## 📈 Git Log Visualization (Pro)

```bash
git log --graph --decorate --oneline --all
```

---

## 🧠 Useful One-Liners

```bash
git diff --name-only HEAD        # Show changed files
git shortlog -sn                 # Contributions by user
git blame <file>                 # Who wrote each line
```

---

## ✅ Best Practices

- Commit often, but meaningfully
- Use branches for features/fixes
- Write clear commit messages
- Rebase before merging if needed
- Never rebase shared branches!
