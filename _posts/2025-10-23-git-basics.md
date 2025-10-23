**quick, practical Git basics tutorial** — ideal if you just want to start working productively right away.

---

## 🧩 1. What Git Is
Git is a **version control system** — it tracks changes in code so you can:
- Revert to previous versions
- Work in parallel (branches)
- Collaborate safely

---

## ⚙️ 2. Initial Setup
Run these once on a new machine:
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
You can check settings with:
```bash
git config --list
```

---

## 📁 3. Starting a Repo
### Create a new repo
```bash
git init
```

### Or clone an existing one
```bash
git clone https://github.com/user/repo.git
```

---

## ✍️ 4. Making Changes
Stage and commit your work:

```bash
git add .           # Add all changed files
git commit -m "Describe what you changed"
```

View your changes:
```bash
git status          # See modified/untracked files
git diff            # Show unstaged changes
```

---

## 🌿 5. Branching & Merging
Create and switch branches:
```bash
git branch feature-x       # Create branch
git checkout feature-x     # Switch to it
# or shortcut:
git switch -c feature-x
```

Merge changes into main:
```bash
git checkout main
git merge feature-x
```

Delete branch after merge:
```bash
git branch -d feature-x
```

---

## ☁️ 6. Working with Remote
Push your work:
```bash
git push origin main
```

Pull latest updates:
```bash
git pull origin main
```

Add remote manually:
```bash
git remote add origin https://github.com/user/repo.git
```

---

## 🕰️ 7. Undoing Mistakes
Undo last commit but keep changes:
```bash
git reset --soft HEAD~1
```

Discard all uncommitted changes:
```bash
git checkout -- .
```

Revert a specific commit:
```bash
git revert <commit-hash>
```

---

## 🔍 8. Inspecting History
```bash
git log --oneline --graph --decorate
```

See who changed what:
```bash
git blame file.txt
```

---

## 🚀 Quick Workflow Summary
1. `git pull` – update local copy
2. `git switch -c feature-branch` – create new branch
3. edit files
4. `git add .`
5. `git commit -m "message"`
6. `git push origin feature-branch`
7. open pull request / merge

---
