# Sentinal WHS — Git Workflow for Craig

## Overview

You have a **fork** of the Sentinal WHS repo. This means you have your own copy on GitHub to work on freely. When you're ready, you submit a **Pull Request (PR)** for Jon to review and merge.

---

## Your Daily Workflow

### 1. Sync your fork before starting work

Always pull the latest code from Jon's repo before you start anything new.

On GitHub: go to your fork → click **"Sync fork"** → **"Update branch"**

Or from your terminal:
```bash
git fetch upstream
git merge upstream/master
git push origin master
```

(First time only — add upstream once):
```bash
git remote add upstream https://github.com/Jonathonjw/WHS-Sentinal.git
```

---

### 2. Create a branch for your work

Never work directly on `master`. Always create a named branch:

```bash
git checkout -b craig/what-youre-doing
# examples:
# craig/timesheets-logic
# craig/checklist-fix
# craig/swms-update
```

---

### 3. Make your changes

Work as normal. Save files, test things, etc.

---

### 4. Commit your changes

```bash
git add .
git commit -m "Short description of what you changed"
```

---

### 5. Push to your fork

```bash
git push origin craig/what-youre-doing
```

---

### 6. Open a Pull Request

1. Go to your fork on GitHub: `github.com/[your-username]/WHS-Sentinal`
2. You'll see a banner: **"craig/your-branch had recent pushes"** → click **"Compare & pull request"**
3. Make sure the base is set to `Jonathonjw/WHS-Sentinal` → `master`
4. Write a short description of what you changed and why
5. Click **"Create pull request"**

Jon gets notified, reviews it, and merges it. Once merged, Railway auto-deploys.

---

## Rules

- **Never push directly to master** — always use a branch
- **Sync before starting** — avoids conflicts with Jon's changes
- **One thing per PR** — keep PRs focused (one feature, one fix)
- **Describe your PR** — a sentence or two about what changed helps Jon review faster

---

## If You Hit a Merge Conflict

This happens when you and Jon both edited the same file. GitHub will flag it on the PR. Message Jon — you'll sort it together rather than guessing.

---

## Quick Reference

| Task | Command |
|------|---------|
| Sync latest from Jon | `git fetch upstream && git merge upstream/master` |
| New branch | `git checkout -b craig/branch-name` |
| Stage all changes | `git add .` |
| Commit | `git commit -m "description"` |
| Push branch | `git push origin craig/branch-name` |
| Switch back to master | `git checkout master` |
