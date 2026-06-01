> **Craig's setup note:** Replace `[YOUR_REPO_PATH]` with the path where you cloned the repo (e.g. `C:\Users\Craig\repos\Sentinal WHS`), and `[YOUR_WORKSPACE_PATH]` with the path to this workspace folder (e.g. `C:\Users\Craig\Claude Code Workspaces\Sentinal-WHS`).

# Claude Code — Sentinal WHS

## Workspace Purpose

Dedicated workspace for **Sentinal WHS** — a modular Work Health & Safety SaaS platform. All Sentinal development, debugging, and feature builds happen here.

**Repo:** `[YOUR_REPO_PATH]`
**GitHub:** `https://github.com/Jonathonjw/WHS-Sentinal.git`
**Stack:** Next.js 16 + TypeScript + Tailwind CSS v4 + Supabase (App Router)
**Supabase project:** `wuoasccjyqkdstsxuudt` (account: yeaboii14@gmail.com)
**Deploy:** git push to origin/master → Railway auto-deploy
**Collaboration:** Jonathan works on `master`. Craig uses feature branches (e.g. `feature/craig-xyz`) and opens PRs to master. Never push directly to master.

---

## Execution Mode

**Default: EXECUTE.** Act immediately. Only confirm before destructive or irreversible actions.

**Full auto:** "mighty morphin claude code go" — switch to full auto mode, invoke `dev-brain` to plan and orchestrate with specialist agents in parallel.

**Always confirm:** git force push (never)

## Context Window Protection (CRITICAL)

**Main context = discussion only. Every task spawns an agent.**

All file modifications, data fetches, and tool executions go to subagents. The main context stays lean for planning, orchestration, and communication.

---

## Architecture Corrections (MUST APPLY TO EVERY MODULE BUILD)

The plan documents in `plans/modules/` all contain these errors. Correct them EVERY time:

1. **RLS pattern:** Plans reference `user_organisations` — DOES NOT EXIST. Use: `organisation_id IN (SELECT organisation_id FROM users WHERE auth_id = auth.uid())`
2. **RLS WITH CHECK:** Every `FOR ALL` policy MUST include `WITH CHECK (...)` or writes silently fail.
3. **Role names:** Plans use `safety_manager` — actual enum: `worker`, `supervisor`, `whs_advisor`, `site_manager`, `admin`, `owner`
4. **API pattern:** Plans show Pages Router (`NextApiRequest`) — codebase is App Router: `export async function GET/POST/PATCH(request: Request)`
5. **Timestamps:** Use `TIMESTAMPTZ` not `TIMESTAMP`
6. **Module guard:** All API routes call `checkModuleAccess()` from `src/lib/modules/module-guard.ts`
7. **Directory names:** Agent bug creates `(dashboard/)` instead of `(dashboard)` and `[id/]` instead of `[id]` — ALWAYS verify after agent builds
8. **No client wrappers on server pages:** `<ModuleGuard>` (client component) CANNOT wrap server component content. Sidebar gates visibility. Server pages don't need ModuleGuard.
9. **No event handlers in server components:** `onChange`, `onClick` etc. can't be serialized. Extract interactive elements into `'use client'` child components.
10. **Zod v4 compat:** `z.record(z.string(), z.any())` NOT `z.record(z.any())`. `useState(null)` infers `never` — type it explicitly.
11. **Zod vs Supabase timestamps:** Supabase returns `2026-03-27 12:03:19+00` (space separator). Zod `.datetime()` rejects it. Don't strict-parse org config timestamps.
12. **Migration FK ordering:** If table A references table B, create B first. The compliance gate migration had `logs` referencing `overrides` before `overrides` existed.

---

## Module Build Checklist

When building ANY module, follow this sequence:

1. Read the plan from `plans/modules/XX-name.md`
2. Read existing patterns: latest API route, latest page, latest migration
3. Apply ALL architecture corrections above
4. Migration → Types → Validators → API routes → UI pages → Sidebar nav
5. `npm run build` — must pass
6. Commit and push to your feature branch, open PR
7. Apply migration to Supabase via MCP (`mcp__supabase__apply_migration` project `wuoasccjyqkdstsxuudt`)
8. Verify directory names (no `(dashboard/)` or `[id/]` artifacts)

---

## Build Status

### Built & Deployed
| # | Module | Migration |
|---|--------|-----------|
| — | Heritage (Investigations, Actions, Permits, JHA, People, Dashboard) | 001-014 |
| 01 | Module System Foundation | 015 |
| 02 | Contractor Management | 016 |
| 03 | SWMS (Reg 291) | 017 |
| 04 | Compliance Gate Engine | 018 |
| 05 | Geotracking & Check-in | 019 |
| — | Address Autocomplete (geofence) | 020 |
| 13 | WHS Checklists | 021 |
| — | Platform Roles & Permissions | 022 |
| — | Org Hierarchy & Escalation | 023 |
| — | Industry Preset Switcher | — |
| 27 | Work Packs + Notifications Phase 1 | 024 |
| 28 | People Module Phase 1a + 1b (profile read-only, correction requests, admin invite + 6-step public onboarding wizard, `onboarding-documents` Storage bucket) | 025 |
| — | `/people` directory rebuild (filter sidebar, A-Z bar, URL-driven pagination) + Investigation People Involved → UserAutocomplete | — |

### In Progress (schema complete, UI not built)
| # | Module | Notes |
|---|--------|-------|
| 30 | Event Form Framework (polymorphic events: Unwanted Event / Field Leadership / Assurance / Regulatory / Mine Record) | Schema in migrations 026-030. Depends on 29 for full value. |
| 31 | Consultation Event Lifecycle (plan/run/confirm + attendance) | 6th event type; tables in 030, 032. |
| 34 | Rosters & Workforce Scheduling (FIFO patterns, swap requests, conflict detection, audit) | Phases 0-2.5 schema in migrations 031, 033-036. Phases 3-5 deferred (conflict detector, swap RLS, geotracking fatigue bridge). |
| 36 | Event Taxonomy Admin UI | Schema seeded auto on org creation (migration 026); no admin CRUD UI. |

### Stub UI Only (page exists, no API/DB)
| Tag | Module | Notes |
|-----|--------|-------|
| DL | Document Library + Expiry Tracking | `src/app/(dashboard)/documents/`. ModuleGuard references `documents` key not in `EnabledModules` — fix before build. |
| TM | Training Matrix (cross-cuts Modules 10 + 11) | `src/app/(dashboard)/training/`. |
| WF | Workforce sub-views: Flights, Accommodation, Travel Requests | `src/app/(dashboard)/workforce/layout.tsx`. Mining-targeted. |
| RP-FR | Reports: Frequency Rates (TRIFR/LTIFR) | Tender-unlocker. Needs Module 06 Timesheets denominator. |
| RP-CR | Reports: Compliance Sub-view | Built on Module 04 data (already deployed). |

### Feature-flagged but unbuilt
| Tag | Module | Priority |
|-----|--------|----------|
| ER | Emergency Roll Call / Muster (life-safety, big sales moment) | HIGH — flag `emergency_roll_call_enabled` true in 4 presets |
| 08a | Offline-first Sync (PWA sub-feature, distinct from shell) | MEDIUM — flag `offline_mode_enabled` true in 4 presets |

### Next to Build
| # | Module | Priority |
|---|--------|----------|
| 28 | People Module Phase 1c — AI doc review (Claude vision: licence expiry extraction, discrepancy flagging) | HIGH — finishes module 28 |
| 29 | Involvement Tracking (IP/PI/Witness/First Aider, 3-tier privacy, inferred-links UX) — plan locked at `plans/modules/29-involvement-tracking.md` | HIGH |
| 06 | Timesheets | HIGH |
| 32 | AI Investigation Blame Detection (contractor portal layer) | MEDIUM |
| 07 | Real-Time Dashboard | MEDIUM |
| 08 | Mobile PWA (shell — pair with 08a offline-sync) | MEDIUM |
| 09 | Reporting | MEDIUM |
| 33 | Data Migration Tooling (BuildPass/HazardCo importers — sales accelerator) | LOW |
| 10 | Inductions | LOW |
| 11 | Qualifications | LOW |
| 12 | Inspections | LOW |
| 14 | Plant & Equipment | LOW |

> **Granular tracking** — sub-features, phases, feature flags and code TODOs (54 rows total) live in the Sentinal Feature Tracker:
> https://docs.google.com/spreadsheets/d/1HFKe5OZE5Q1nU1z0gcqSrlWkRzHhJ8GePA1QnSfyD-M/edit

---

## Key Architecture Decisions

- **SWMS ≠ JHA** — SWMS is a legal document (Reg 291). JHA is a planning tool. Separate modules.
- **Platform permissions ≠ Org hierarchy** — Platform roles (Admin/Editor/Viewer) control app access. Org positions (Manager/Superintendent/Worker) drive escalation chains. Two separate systems.
- **Org defines own positions** — Seeds are industry examples. Organisations create/customise their own departments and positions.
- **Contractors complete checklists, don't create templates** — PCBU creates Take 5s, inspections. Contractor workers fill them in. If contractor uploads their own → AI reads for data, tracked against that contractor.
- **Terminology per org** — Builder: "Projects"/"Subbies". Mining: "Mine Sites"/"Crew". Configured via industry presets.
- **Industry presets** — 6 types, switchable by owner in header. Each configures modules + terminology + features.
- **Compliance gate** — 13 parallel checks (<500ms), configurable per-site (block/warn), override workflow.

---

## Competitor Benchmarks

- **BuildPass** ($59-129/user/mo): geofencing, rotating QR codes, timesheets, 80K+ workers
- **HazardCo** ($119/mo flat): QR check-in only (no GPS), safety-only, simpler
- **Sentinal differentiator**: mining-grade safety depth + modular industry flexibility + full compliance gate chain
