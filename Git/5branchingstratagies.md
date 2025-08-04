# 🌿 Git Branching Strategy and Pull Requests

---

## 🌿 What is a Branching Strategy?

A **branching strategy** defines how developers organize and manage branches in a Git repository to streamline collaboration, testing, and delivery.

---

### 🔰 Common Branching Strategies

#### 1. Git Flow

**Main branches**:
- `main` (production-ready code)
- `develop` (latest development version)

**Supporting branches**:
- `feature/*` → for new features
- `release/*` → for preparing releases
- `hotfix/*` → for emergency fixes on production

**Use Case**:
- Large teams with strict release cycles
- Teams using continuous delivery

Diagram:
```
main <—— hotfix
      ↕
 develop <—— release
        ↕
   feature/login
```

---

#### 2. GitHub Flow

Very simple: create feature branches from `main`, then open PRs

**Use Case**:
- Startups or small agile teams
- Continuous deployment environments

Diagram:
```
main
  ↳ feature/add-user-form → PR → merge to main → deploy
```

---

#### 3. Trunk-Based Development

Only one long-lived branch (usually `main`), with short-lived branches merged back quickly

**Use Case**:
- DevOps, CI/CD pipelines
- Google, Facebook-style engineering culture

---

## 🔄 What is a Pull Request (PR)?

A **Pull Request** is a request to merge one branch into another (usually into `main` or `develop`).

---

### 🔧 PR Workflow Example

**Scenario**: You’re working on a login feature in a large team

1. Create a branch:
```bash
git checkout -b feature/login
```

2. Write code, commit, push:
```bash
git push origin feature/login
```

3. Create PR: `feature/login` → `develop`

4. Code review, CI tests, suggested changes

5. Push updates based on feedback

6. Merge when approved

---

### ✅ Benefits of PRs

- Code Review for quality
- PR description documents context
- CI pipelines test changes
- PRs serve as an audit trail

---

## 🧠 Real-World Summary

| Scenario                            | Strategy Used         | Why?                             |
|-------------------------------------|------------------------|----------------------------------|
| New feature for next release        | `feature/*` branch     | Isolated development             |
| Bug fix in production               | `hotfix/*` branch      | Quick fix, urgent deploy         |
| Long release prep                   | `release/*` branch     | For staging and final testing    |
| Small team with quick changes       | GitHub Flow            | Simplicity and speed             |
| Fast-moving CI/CD environment       | Trunk-Based Dev        | Merge frequently, deploy fast    |
