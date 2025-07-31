# 📘 Git Merging - Beginner Friendly Guide

## 🔀 What is Git Merging?

Merging in Git means **combining the changes from one branch into another** (usually into `main` or `master`). It’s commonly used when a feature branch is ready, and you want to integrate its changes back into the main codebase.

---

## 💡 Visual Example:

### ➤ On branch `master`:
```
A---B---C (master)
```

### ➤ You create a branch `feature-x` from `C` and make 3 new commits:
```
A---B---C (master)
         \
          D---E---F (feature-x)
```

### ➤ Now if you switch to master and run:
```bash
git merge feature-x
```

### ➤ After a successful merge, the history becomes:
```
A---B---C---------G (master)
         \       /
          D---E---F (feature-x)
```

✅ `G` is the **merge commit** that combines both histories.

---

## 🔄 Types of Git Merge

### 1. ✅ Fast-Forward Merge

- Happens when no new commits exist on `main` after branch creation.
- Git just **moves the branch pointer forward**.

```
A---B---C---D---E (main & feature-x)
```

### 2. ✅ 3-Way Merge

- Happens when **both branches have new commits**.
- Git creates a new **merge commit** to combine changes.

```
A---B---C---G (main)
         \  /
          D---E (feature-x)
```

---

## 🛠️ TASK 1: Fast-Forward Merge

### 🔧 Step-by-Step:

1. **Initialize a Git repo**
```bash
git init merge-practice
cd merge-practice
```

2. **Create a file and make 3 commits**
```bash
echo "Line 1" > file.txt
git add file.txt
git commit -m "Initial commit"

echo "Line 2" >> file.txt
git commit -am "Added Line 2"

echo "Line 3" >> file.txt
git commit -am "Added Line 3"
```

3. **Create and switch to a new branch**
```bash
git checkout -b feature-fastforward
```

4. **Add 2 more lines and commit**
```bash
echo "Feature Line 1" >> file.txt
git commit -am "Added Feature Line 1"

echo "Feature Line 2" >> file.txt
git commit -am "Added Feature Line 2"
```

5. **Switch back to main**
```bash
git checkout master
```

6. **Merge using Fast-Forward**
```bash
git merge feature-fastforward
```

7. **Verify log and contents**
```bash
git log --oneline --graph
cat file.txt
```

---

## 🛠️ TASK 2: 3-Way Merge

### 🔧 Step-by-Step:

1. **Create a fresh repo**
```bash
git init three-way-merge
cd three-way-merge
```

2. **Create a file and make 2 commits**
```bash
echo "Hello" > app.txt
git add app.txt
git commit -m "Base commit"

echo "Common line" >> app.txt
git commit -am "Added common line"
```

3. **Create a branch and add changes**
```bash
git checkout -b feature-branch
echo "Feature changes" >> app.txt
git commit -am "Added feature changes"
```

4. **Switch back to master and make separate commit**
```bash
git checkout master
echo "Main changes" >> app.txt
git commit -am "Added main changes"
```

5. **Now both branches have diverged**

6. **Merge feature branch (will create 3-way merge)**
```bash
git merge feature-branch
```

7. **Check the log and file contents**
```bash
git log --oneline --graph
cat app.txt
```

---

## ✅ Tips:
- Use `git log --graph --oneline --all` to see branch history clearly.
- Use `git status` before merge to ensure working tree is clean.
- Conflicts may appear during 3-way merge — resolve them manually.

---

🎉 Congratulations! You now understand **Git Merging**, the difference between **Fast-Forward** and **3-Way Merges**, and have practiced both types!
