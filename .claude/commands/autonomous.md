---
description: Toggle autonomous mode on/off (bypasses all permission prompts except destructive actions)
argument-hint: [on|off|status]
allowed-tools: Read, Edit, Bash(cat:*)
---

Toggle autonomous mode in ~/.claude/settings.json by setting or removing `permissions.defaultMode`.

The argument is: $ARGUMENTS

## Rules

- If argument is `on` → set `"defaultMode": "bypassPermissions"` inside the `permissions` object in `~/.claude/settings.json`
- If argument is `off` → remove the `defaultMode` field from the `permissions` object in `~/.claude/settings.json`
- If argument is `status` or empty → read the file and report whether autonomous mode is currently on or off

## How to check current state
Read `C:/Users/RadiumPCs/.claude/settings.json` and look for `"defaultMode": "bypassPermissions"` inside the `permissions` object.

## Important
- Always read the file first before editing
- Preserve ALL other existing settings — only add/remove the `defaultMode` field
- After making the change, confirm the new state clearly:
  - ON: "Autonomous mode ON — all tool confirmations bypassed. Destructive actions (rm, force push) still protected by deny rules."
  - OFF: "Autonomous mode OFF — normal permission prompts restored."
  - STATUS (when on): "Autonomous mode is currently ON"
  - STATUS (when off): "Autonomous mode is currently OFF"
