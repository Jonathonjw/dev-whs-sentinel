Push all five repos (Sentinal-WHS, Epsillon-Media-Claude-Work, ceo-brain, epsillon-marketing, epsillon-dev) safely. Follow these steps in order.

Invoked from the Sentinal-WHS workspace — so Sentinal is pushed first.

epsillon-marketing and epsillon-dev may not have remotes configured — if so, check status + commit locally (if there are changes) and skip the push step. This keeps the commit history clean even on repos not yet connected to GitHub.

---

## Repo 1: Sentinal-WHS (current workspace)

1. Run `git status` to see all changes (staged, unstaged, untracked)
2. Run `git diff --stat` to review what changed — look for any deleted files
3. Run `git diff --name-status` to get a clear list of Added (A), Modified (M), and Deleted (D) files
4. **SAFETY CHECK**: If ANY files show as Deleted (D):
   - List every deleted file and USE THE AskUserQuestion TOOL to ask the user to confirm before proceeding
   - This is a MANDATORY user confirmation — even if permissions are set to "edit automatically", you MUST stop and ask. NEVER skip this step.
   - Do NOT continue until the user explicitly approves the deletions
   - If the user says no, run `git checkout -- <deleted-files>` to restore them before continuing
5. If the safety check passes (no deletions, or user approved):
   - Run `git add -A`
   - Generate a commit message summarising the session. Format: `Session: [brief description of Sentinal-WHS workspace changes]`
   - Run the commit
6. Run `git push origin master`  *(Sentinal-WHS uses `master`, not `main`)*
7. Confirm what was pushed

---

## Repo 2: Epsillon-Media-Claude-Work

8. `cd` to the Epsillon workspace. Try both locations — desktop: `C:\Users\RadiumPCs\Claude Code Workspaces\Epsillon-Media-Claude-Work`, laptop: `C:\Users\jonat\Documents\Claude Code Repository\Epsillon-Media-Claude-Work`. If neither exists, report "Epsillon workspace not found on this machine" and skip to the next repo.
9. Run `git status` — if there are no changes, skip to step 14 and report "Epsillon workspace: no changes"
10. Run `git diff --name-status` to check for deletions
11. **SAFETY CHECK**: Same deletion check as above — ask before proceeding if any files are deleted
12. If changes exist:
    - Run `git add -A`
    - Generate a commit message. Format: `Session: [brief description of Epsillon workspace changes]`
    - Run the commit
13. Run `git push origin main`  *(Epsillon uses `main`)*
14. `cd` back to the Sentinal-WHS working directory

---

## Repo 3: CEO Brain

15. `cd` to CEO Brain. Try both locations — desktop: `C:\Users\RadiumPCs\Claude Code Workspaces\ceo-brain`, laptop: `C:\Users\jonat\Documents\Claude Code Repository\ceo-brain`. If neither exists, report "CEO Brain repo not found on this machine" and skip to the next step.
16. Run `git status` — if there are no changes, skip to step 21 and report "CEO Brain: no changes"
17. Run `git diff --name-status` to check for deletions
18. **SAFETY CHECK**: Same deletion check as above — ask before proceeding if any files are deleted
19. If changes exist:
    - Run `git add -A`
    - Generate a commit message. Format: `Session: [brief description of CEO Brain changes]`
    - Run the commit with `-c user.name="Jonathonjw" -c user.email="jono@epsillonmedia.com.au"`
20. Run `git push origin main`
21. `cd` back to the Sentinal-WHS working directory

---

## Repo 4: epsillon-marketing

22. `cd` to epsillon-marketing. Try both locations — desktop: `C:\Users\RadiumPCs\Claude Code Workspaces\marketing workspace`, laptop: `C:\Users\jonat\Documents\Claude Code Repository\epsillon-marketing`. If neither exists, report "epsillon-marketing repo not found on this machine" and skip to the next repo.
23. Run `git status` — if there are no changes, skip to step 29 and report "epsillon-marketing: no changes"
24. Run `git diff --name-status` to check for deletions
25. **SAFETY CHECK**: Same deletion check as above — ask before proceeding if any files are deleted
26. If changes exist:
    - Run `git add -A`
    - Generate a commit message. Format: `Session: [brief description of epsillon-marketing changes]`
    - Run the commit
27. Run `git remote -v` to check if a remote is configured
28. **REMOTE CHECK**: If no remote is configured, report "epsillon-marketing: committed locally, no remote configured — push skipped". Otherwise run `git push origin main`.
29. `cd` back to the Sentinal-WHS working directory

---

## Repo 5: epsillon-dev

30. `cd` to epsillon-dev. Try both locations — desktop: `C:\Users\RadiumPCs\Claude Code Workspaces\epsillon-dev`, laptop: `C:\Users\jonat\Documents\Claude Code Repository\epsillon-dev`. If neither exists, report "epsillon-dev repo not found on this machine" and skip to the next repo.
31. Run `git status` — if there are no changes, skip to step 37 and report "epsillon-dev: no changes"
32. Run `git diff --name-status` to check for deletions
33. **SAFETY CHECK**: Same deletion check as above — ask before proceeding if any files are deleted
34. If changes exist:
    - Run `git add -A`
    - Generate a commit message. Format: `Session: [brief description of epsillon-dev changes]`
    - Run the commit
35. Run `git remote -v` to check if a remote is configured
36. **REMOTE CHECK**: If no remote is configured, report "epsillon-dev: committed locally, no remote configured — push skipped". Otherwise run `git push origin main`.
37. `cd` back to the Sentinal-WHS working directory

---

## Step 6: Backup MCP Configs to Google Drive

38. **Auto-detect Epsillon Media Google Drive (jward@epsillonmedia.com):** Run this to find the BUSINESS drive (not the personal one):
    ```bash
    for drive in G H I J K; do [ -d "$drive:/My Drive/02_Internal Ops" ] && echo "$drive" && break; done
    ```
    The business drive is identified by having `02_Internal Ops` at root level (not inside `[03] Business`).
39. If the business drive was found, copy all `.mcp.json` files where present:
    - Sentinal-WHS: `cp [sentinal]/.mcp.json "[DRIVE]:/My Drive/02_Internal Ops/claude-config/sentinal-whs.mcp.json"`
    - Epsillon: `cp [epsillon]/.mcp.json "[DRIVE]:/My Drive/02_Internal Ops/claude-config/main-repo.mcp.json"`
    - CEO Brain: `cp [ceo-brain]/.mcp.json "[DRIVE]:/My Drive/02_Internal Ops/claude-config/ceo-brain.mcp.json"`
    - epsillon-marketing: `cp [epsillon-marketing]/.mcp.json "[DRIVE]:/My Drive/02_Internal Ops/claude-config/epsillon-marketing.mcp.json"`
    - epsillon-dev: `cp [epsillon-dev]/.mcp.json "[DRIVE]:/My Drive/02_Internal Ops/claude-config/epsillon-dev.mcp.json"`
40. If no business Google Drive found, skip and report "Epsillon Media Google Drive not detected — MCP config backup skipped"
41. `cd` back to the Sentinal-WHS working directory
42. Confirm what was pushed across all five repos with a combined summary

---

## Rules (apply to ALL repos)

- If there are merge conflicts or push errors, explain what happened and suggest a fix
- NEVER force push. NEVER use `--force` or `--force-with-lease`
- If one repo pushes fine but the others fail, report which succeeded and which failed — don't rollback the successful one
- **Sentinal-WHS uses `master` branch. All others use `main`.** Don't mix these up.
- For repos without a remote (epsillon-marketing / epsillon-dev may fall into this), commit locally and skip the push — don't try to configure a remote automatically.
