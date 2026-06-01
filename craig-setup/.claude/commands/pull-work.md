Pull the Sentinal WHS repo safely before starting work. Follow these steps:

1. Run `git status` to check for uncommitted local changes
2. **LOCAL CHANGES CHECK**: If there are uncommitted changes:
   - USE THE AskUserQuestion TOOL to ask: "You have uncommitted changes. Stash them before pulling, or abort?"
   - Options: "Stash and pull" / "Abort pull"
   - If stash: run `git stash push -m "auto-stash before pull-work"`
   - If abort: stop here and report why
3. Run `git fetch origin` to see what's on remote
4. Report any new commits on `master` that you don't have locally
5. Run `git pull origin master`
6. If there was a stash in step 2, run `git stash pop` and check for conflicts
7. Run `npm install` if `package.json` changed in the pull
8. Report: branch status, commits pulled (or "already up to date"), any conflicts

## Rules

- If there are merge conflicts, explain what happened and list the conflicting files — do NOT auto-resolve
- NEVER run `git reset --hard` or `git checkout .` — these destroy local work
- NEVER force pull or overwrite local changes without explicit user approval
- Sentinal WHS uses the `master` branch
