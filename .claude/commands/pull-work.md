Pull all five repos (Sentinal-WHS, Epsillon-Media-Claude-Work, ceo-brain, epsillon-marketing, epsillon-dev) safely, then restore MCP configs from Google Drive if missing. Follow these steps in order.

Invoked from the Sentinal-WHS workspace — so Sentinal is pulled first.

---

## Repo 1: Sentinal-WHS (current workspace)

1. Run `git status` to check for uncommitted local changes
2. **LOCAL CHANGES CHECK**: If there are uncommitted changes:
   - USE THE AskUserQuestion TOOL to ask the user: "You have uncommitted changes in Sentinal-WHS. Stash them before pulling, or abort?"
   - Options: "Stash and pull" / "Abort pull"
   - If stash: run `git stash push -m "auto-stash before pull-work"`
   - If abort: stop here and report why
3. Run `git pull origin master`  *(Sentinal-WHS uses `master`)*
4. If there was a stash in step 2, run `git stash pop` and check for conflicts
5. Report: branch status, commits pulled (or "already up to date"), any conflicts

---

## Repo 2: Epsillon-Media-Claude-Work

6. `cd` to the Epsillon workspace. Try both locations — desktop: `C:\Users\RadiumPCs\Claude Code Workspaces\Epsillon-Media-Claude-Work`, laptop: `C:\Users\jonat\Documents\Claude Code Repository\Epsillon-Media-Claude-Work`. If neither exists, report "Epsillon workspace not found on this machine" and skip to the next repo.
7. Run `git status` to check for uncommitted local changes
8. **LOCAL CHANGES CHECK**: Same as above — ask before stashing if uncommitted changes exist
9. Run `git pull origin main`  *(Epsillon uses `main`)*
10. If there was a stash, run `git stash pop` and check for conflicts
11. Report: branch status, commits pulled (or "already up to date"), any conflicts
12. `cd` back to the Sentinal-WHS working directory

---

## Repo 3: CEO Brain

13. `cd` to CEO Brain. Try both locations — desktop: `C:\Users\RadiumPCs\Claude Code Workspaces\ceo-brain`, laptop: `C:\Users\jonat\Documents\Claude Code Repository\ceo-brain`. If neither exists, report "CEO Brain repo not found on this machine" and skip to the next repo.
14. Run `git status` to check for uncommitted local changes
15. **LOCAL CHANGES CHECK**: Same as above — ask before stashing if uncommitted changes exist
16. Run `git pull origin main`
17. If there was a stash, run `git stash pop` and check for conflicts
18. Report: branch status, commits pulled (or "already up to date"), any conflicts
19. `cd` back to the Sentinal-WHS working directory

---

## Repo 4: epsillon-marketing

20. `cd` to epsillon-marketing. Try both locations — desktop: `C:\Users\RadiumPCs\Claude Code Workspaces\marketing workspace`, laptop: `C:\Users\jonat\Documents\Claude Code Repository\epsillon-marketing`. If neither exists, report "epsillon-marketing repo not found on this machine" and skip to the next repo.
21. Run `git status` to check for uncommitted local changes
22. **LOCAL CHANGES CHECK**: Same as above — ask before stashing if uncommitted changes exist
23. Run `git remote -v` to check if a remote is configured
24. **REMOTE CHECK**: If no remote is configured, report "No remote configured — pull skipped" and proceed to step 26
25. If remote exists, run `git pull origin main`
26. If there was a stash, run `git stash pop` and check for conflicts
27. Report: branch status, commits pulled (or "already up to date"), any conflicts
28. `cd` back to the Sentinal-WHS working directory

---

## Repo 5: epsillon-dev

29. `cd` to epsillon-dev. Try both locations — desktop: `C:\Users\RadiumPCs\Claude Code Workspaces\epsillon-dev`, laptop: `C:\Users\jonat\Documents\Claude Code Repository\epsillon-dev`. If neither exists, report "epsillon-dev repo not found on this machine" and skip to the next repo.
30. Run `git status` to check for uncommitted local changes
31. **LOCAL CHANGES CHECK**: Same as above — ask before stashing if uncommitted changes exist
32. Run `git remote -v` to check if a remote is configured
33. **REMOTE CHECK**: If no remote is configured, report "No remote configured — pull skipped" and proceed to step 35
34. If remote exists, run `git pull origin main`
35. If there was a stash, run `git stash pop` and check for conflicts
36. Report: branch status, commits pulled (or "already up to date"), any conflicts
37. `cd` back to the Sentinal-WHS working directory

---

## Step 6: Restore MCP Configs from Google Drive (if missing)

38. Check if `.mcp.json` exists in ALL repos (Sentinal-WHS, Epsillon, ceo-brain, epsillon-marketing, epsillon-dev). If all exist, skip to step 42.
39. **Auto-detect Epsillon Media Google Drive (jward@epsillonmedia.com):** Run this to find the BUSINESS drive (not the personal one). Use a 2-second timeout per drive:
    ```bash
    for drive in G H I J K; do timeout 2 bash -c "[ -d \"$drive:/My Drive/02_Internal Ops\" ]" 2>/dev/null && echo "$drive" && break; done
    ```
    The business drive has `02_Internal Ops` at root (not inside `[03] Business`).
    If `timeout` isn't available, fall back to checking J: only: `[ -d "J:/My Drive/02_Internal Ops" ] && echo "J"`
40. If the business drive was found and any `.mcp.json` files are missing:
    - Sentinal-WHS missing: `cp "[DRIVE]:/My Drive/02_Internal Ops/claude-config/sentinal-whs.mcp.json" [sentinal]/.mcp.json`
    - Main repo missing: `cp "[DRIVE]:/My Drive/02_Internal Ops/claude-config/main-repo.mcp.json" [epsillon]/.mcp.json`
    - CEO Brain missing: `cp "[DRIVE]:/My Drive/02_Internal Ops/claude-config/ceo-brain.mcp.json" [ceo-brain]/.mcp.json`
    - epsillon-marketing missing: `cp "[DRIVE]:/My Drive/02_Internal Ops/claude-config/epsillon-marketing.mcp.json" [epsillon-marketing]/.mcp.json`
    - epsillon-dev missing: `cp "[DRIVE]:/My Drive/02_Internal Ops/claude-config/epsillon-dev.mcp.json" [epsillon-dev]/.mcp.json`
    - Report which configs were restored
41. If no business Google Drive found AND any configs are missing, report "MCP config missing and Google Drive not detected — you'll need to restore .mcp.json manually"
42. Confirm final status across all repos with a combined summary

---

## Rules (apply to ALL repos)

- If there are merge conflicts, explain what happened and list the conflicting files — do NOT auto-resolve
- NEVER run `git reset --hard` or `git checkout .` — these destroy local work
- NEVER force pull or overwrite local changes without explicit user approval
- If any repo pulls fine but others fail, report which succeeded and which failed
- The MCP config restore is only for missing files (e.g. fresh clone) — NEVER overwrite an existing `.mcp.json` with the backup
- If a repo has no remote configured, skip the pull (do NOT try to configure a remote automatically)
- **Sentinal-WHS uses `master` branch. All others use `main`.** Don't mix these up.
