# Sentinal WHS — Claude Code Workspace

Development workspace for [Sentinal WHS](https://github.com/Jonathonjw/WHS-Sentinal), a modular Work Health & Safety platform built on Next.js 16 + Supabase.

## What This Repo Is

This is the **Claude Code workspace** — not the application code. It contains the AI tooling, rules, skills, agents, and memory that power development of Sentinal WHS. The actual app lives at [`C:\Users\RadiumPCs\repos\Sentinal WHS`](https://github.com/Jonathonjw/WHS-Sentinal).

## Quick Start

1. Open this workspace in VS Code
2. Add the Sentinal WHS repo as an additional working directory
3. Start Claude Code — it will load CLAUDE.md with all architecture rules, design system, and build patterns

## Workspace Contents

```
.claude/
  agents/          21 specialist agents (dev-brain, code-reviewer, etc.)
  commands/        16 slash commands (/commit, /plan, /verify, etc.)
  rules/
    build-patterns.md    Reference files + server/client component rules
    design-system.md     Full UI spec: colours, cards, buttons, tables, forms
  skills/          33 skills (Next.js, TypeScript, React, Postgres, Supabase,
                   Tailwind, security, testing, design, UI/UX)
  settings.json    Supabase MCP configured

memory/
  MEMORY.md        Gotchas, founding customer context, Supabase config

CLAUDE.md          Architecture corrections, build checklist, module status
```

## What's Built (App Repo)

| Module | Description |
|--------|-------------|
| Heritage | ICAM Investigations, Actions, Permits, JHA, People, Dashboard |
| 01 Module System | Per-org module enable/disable + terminology |
| 02 Contractors | Companies, workers, compliance docs, pre-qualification |
| 03 SWMS | Reg 291 compliance, 18 HRCW categories, version control |
| 04 Compliance Gate | 13 parallel checks, configurable per-site, override workflow |
| 05 Geotracking | GPS check-in, QR codes, geofencing, activity log |
| 13 Checklists | Take 5, pre-start, inspections, auto-escalation on fail |
| RBAC | Platform roles & permissions (granular JSONB per module) |
| Org Hierarchy | Departments, positions, reporting lines, escalation chains |
| Industry Switcher | 6 presets: Builder, Mining, Construction, Trades, Manufacturing, Corporate |

## What's Next

| Module | Priority |
|--------|----------|
| 06 Timesheets | HIGH (Kim MVP) |
| 07 Real-Time Dashboard | MEDIUM |
| 08 Mobile PWA | MEDIUM |
| 09 Reporting | MEDIUM |

## Architecture Rules

Every module build must follow 12 correction rules documented in [CLAUDE.md](CLAUDE.md). Key ones:

- RLS uses `auth_id = auth.uid()` (not `id = auth.uid()`)
- App Router only (not Pages Router)
- `TIMESTAMPTZ` not `TIMESTAMP`
- Zod v4: `z.record(z.string(), z.any())`
- No client `<ModuleGuard>` wrapping server components
- No event handlers in server components
- Always verify directory names after agent builds (`(dashboard)` not `(dashboard/)`)

## Design System

Full dark navy UI spec in [.claude/rules/design-system.md](.claude/rules/design-system.md):

- **Dark theme only** — `bg-navy-950` page, `bg-navy-900` cards, never `bg-white`
- **Consistent components** — cards, buttons, badges, tables, forms all specified
- **Mobile-first** — responsive grids, 44px min tap targets
- **Status colours** — emerald (pass), amber (warn), red (block), blue (info)

## Related Repos

| Repo | Purpose |
|------|---------|
| [WHS-Sentinal](https://github.com/Jonathonjw/WHS-Sentinal) | Application code (Next.js + Supabase) |
| [Epsillon-Media-Claude-Work](https://github.com/Jonathonjw/Epsillon-Media-Claude-Work) | Shared base workspace (orchestration, memory) |

## Infrastructure

- **Supabase:** Project `wuoasccjyqkdstsxuudt` (ap-northeast-1)
- **Railway:** Auto-deploys from WHS-Sentinal master branch
- **Migrations:** 001-023 applied, next is 024
