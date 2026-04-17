---
allowed-tools: Bash(npm run build:*), Bash(npx tsc:*), Bash(npm run lint:*), Bash(npm run typecheck:*), Bash(npm test:*), Bash(git diff:*), Bash(git status:*), Grep, Glob, Read
description: Run verification checks before committing or opening a PR
---

## Context

- Current branch: !`git branch --show-current`
- Git status: !`git status --short`
- Recently modified files: !`git diff --name-only HEAD`

## Arguments

`$ARGUMENTS` can be:
- `quick` — build + types only (fastest, good mid-task)
- `pre-commit` — build, types, lint, console.log audit
- `pre-pr` — everything: build, types, lint, tests, console.log, git status
- _(blank)_ — same as `pre-commit`

## Project Detection

Identify which project to verify based on recently modified files:

- Files under `apps/meta-ads-saas/` → verify Adkore
- Files under `apps/clickup-mcp/` → verify ClickUp MCP
- Files under `command-centre/` → verify Command Centre
- Files under `C:/Users/RadiumPCs/repos/Epsillon-Media-Hub/` → verify Epsillon Hub
- If multiple projects touched, verify each one

## Instructions

Run checks in order. Stop and report if a critical check fails.

### Step 1 — Build (all modes)

Run the appropriate build command for the detected project:

| Project | Build command | Run from |
|---------|--------------|----------|
| Adkore | `npm run build` | `apps/meta-ads-saas/` |
| ClickUp MCP | `npm run build` | `apps/clickup-mcp/` |
| Command Centre | `node -e "require('./server.js')" 2>&1 \| head -5` (syntax check only) | `command-centre/` |

If build fails → report errors and STOP (no point running further checks).

### Step 2 — Type Check (all modes)

| Project | Command | Run from |
|---------|---------|----------|
| Adkore | `npx tsc --noEmit` | `apps/meta-ads-saas/` |
| ClickUp MCP | `npx tsc --noEmit` | `apps/clickup-mcp/` |

Report all errors with file:line format.

### Step 3 — Lint (pre-commit, pre-pr only)

| Project | Command | Run from |
|---------|---------|----------|
| Adkore | `npm run lint` (if script exists) | `apps/meta-ads-saas/` |

Skip silently if no lint script is configured.

### Step 4 — Tests (pre-pr only)

Run `npm test` from the project directory. Report pass/fail count and coverage if available.

### Step 5 — Console.log Audit (pre-commit, pre-pr only)

Search git-modified source files for `console.log`. Exclude test files (`*.test.*`, `*.spec.*`).

### Step 6 — Git Status (pre-pr only)

Show uncommitted changes and any untracked files that should be reviewed.

## Output Format

```
VERIFICATION: [PASS/FAIL] — [project name]
Mode: [quick/pre-commit/pre-pr]

Build:    [OK / FAIL — reason]
Types:    [OK / X errors]
Lint:     [OK / X issues / skipped]
Tests:    [X/Y passed, Z% coverage / skipped]
Logs:     [OK / X console.logs found]
Git:      [clean / X uncommitted files]

Ready for PR: [YES / NO]
```

If any critical issues found, list them with specific fix suggestions.
