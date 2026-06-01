Push Sentinal WHS changes. Craig should use feature branches — push to your feature branch and open a PR, NOT directly to master. Follow these steps:

---

## Step 1: Safety checks

1. Run `git status` to see all changes (staged, unstaged, untracked)
2. Run `git diff --stat` to review what changed
3. Run `git diff --name-status` to get a clear list of Added (A), Modified (M), and Deleted (D) files
4. **DELETION SAFETY CHECK**: If ANY files show as Deleted (D):
   - List every deleted file and USE THE AskUserQuestion TOOL to ask Craig to confirm before proceeding
   - This is MANDATORY — NEVER skip this step
   - If Craig says no, run `git checkout -- <deleted-files>` to restore them

---

## Step 2: Branch check

5. Run `git branch --show-current` to see which branch you're on
6. **MASTER GUARD**: If you're on `master`:
   - USE THE AskUserQuestion TOOL to ask: "You're on master. Craig should use a feature branch. Create one now, or push directly (risk of conflict with Jonathan)?"
   - Options: "Create feature branch" / "Push to master anyway"
   - If feature branch: ask Craig for a branch name, run `git checkout -b [name]`
7. If already on a feature branch: continue

---

## Step 3: Commit and push

8. Run `git add -A`
9. Generate a commit message summarising what changed. Format: `Session: [brief description]`
10. Run the commit
11. Run `git push origin [current-branch]`
12. If this is a NEW branch (no upstream): run `git push --set-upstream origin [branch-name]`
13. If on a feature branch: suggest opening a PR with `gh pr create --base master --title "[branch-name]"` (if gh CLI is available)
14. Confirm what was pushed

---

## Rules

- NEVER force push. NEVER use `--force` or `--force-with-lease`
- If there are push errors (e.g. rejected because master has new commits), explain what happened and suggest `git pull origin master --rebase` then retry
- Jonathan reviews and merges PRs to master — Railway auto-deploys from master
