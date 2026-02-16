# 📌 Team Git Workflow Guide

This project uses a **branch-based workflow**.  
All team members work on their own branches and merge into `main` using Pull Requests.

---

## 🌱 Branch Strategy

- `main` → stable production branch  
- `feature-*` → each feature has its own branch  
  - example: `feature-login`, `feature-ui`, `feature-api`

❌ Do NOT push directly to `main`  
✅ Always work on your own branch

---

## 🚀 Initial Setup

Clone the repository:
```bash
git clone <repo-url>
cd <project-folder>
```

Get latest `main`:
```bash
git checkout main
git pull origin main
```

---

## 🌿 Create a New Branch

```bash
git checkout -b feature-yourname
```

or:
```bash
git switch -c feature-yourname
```

Example:
```bash
git checkout -b feature-auth
```

---

## 💾 Save Your Work (Commit)

```bash
git add .
git commit -m "Add authentication feature"
```

---

## ⬆️ Push Your Branch

First time push:
```bash
git push -u origin feature-yourname
```

Next pushes:
```bash
git push origin feature-yourname
```

---

## ⬇️ Pull Latest Changes from Main

While on your branch:
```bash
git pull origin main
```

---

## 🔀 Merge into Main (via Pull Request)

1. Push your branch  
2. Open Pull Request on GitHub  
3. Team reviews code  
4. Merge into `main`

---

## 🧹 Delete Branch After Merge

Delete locally:
```bash
git branch -d feature-yourname
```

Delete remotely:
```bash
git push origin --delete feature-yourname
```

---

## 📋 Useful Commands

Check status:
```bash
git status
```

Check branches:
```bash
git branch
```

Switch branches:
```bash
git checkout branch-name
```

---

## 👥 Team Rules

- One feature = one branch  
- Pull before pushing  
- Clear commit messages  
- No force push  
- No direct commits to `main`

---

## 📊 Workflow Summary

```
main
  ↑
Pull Request
  ↑
feature-branch → commit → push
```
