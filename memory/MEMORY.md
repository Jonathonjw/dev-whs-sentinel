# Sentinal WHS — Project Memory

## Build Status
- [Full module status + architecture](../../../Epsillon-Media-Claude-Work/memory/sentinal-whs.md) — canonical reference in shared base workspace
- 20+ modules/features built, migrations 015-038, ~55 DB tables, ~100+ API endpoints
- Latest session (2026-06-01): Actions moved to own tab; multi-assignee (`action_assignees`); factor chips "Reason for Action" (`action_factors`); new API routes for actions + `/api/users/org`
- Latest migration: **044** (`action_factors` junction table). 043 = `action_assignees`. **Both need manual SQL editor apply** (MCP privileges blocked).
- **Follow-up required:** Add `CRON_SECRET` to Railway env vars; set up daily external cron to hit `/api/cron/consultation-reminders`
- Next to build: Plan 29 (Involvement Tracking — plan locked), Designations System (DES row 70 in tracker), Module 06 (Timesheets)

## Founding Customer
- Kim (Embark Building) — builder, warm lead via Jono's wife
- Was about to sign BuildPass Lite at $297/mo
- Sentinal matching at $297/mo founder rate, locked for life
- Pre-build gating: discovery call with Kim, Craig alignment, Schedule A
- See `shared-memory/sentinel-whs.md` for full context

## Co-Founder
- Craig Fitzgerald Neeson (Kalgoorlie) — WHS domain expert, 50/50 ownership
- Phase 1 (Development): light commitments, 72hr response
- Expense cap >$500 AUD requires joint written agreement
- Has a **fork** of the repo on GitHub; submits changes via PRs to `Jonathonjw/WHS-Sentinal` master. Cheat sheet: `craig-setup/GIT-WORKFLOW.md`

## Supabase
- Project ID: wuoasccjyqkdstsxuudt
- Account: yeaboii14@gmail.com (main account invited for MCP access)
- RLS uses `get_my_organisation_id()` function on heritage tables, direct subquery on new tables
- **Supabase MCP fallback** — when MCP isn't loaded in a session, use Admin REST directly: load `SUPABASE_SERVICE_ROLE_KEY` from `.env.local`, call `/auth/v1/admin/users/{id}` (PUT, with `email_confirm: true` to skip re-verification) for auth changes and `/rest/v1/users?id=eq.{id}` (PATCH) for `public.users`. Single-shot node script avoids needing the MCP at all.

## Email (Resend)
- Wired through `src/lib/email/send-email.ts` — see [project_resend_free_tier.md](../../../.claude/projects/c--Users-RadiumPCs-Claude-Code-Workspaces-Sentinal-WHS/memory/project_resend_free_tier.md) (auto-memory) for full integration details, free-tier policy, verified domain, gotchas
- Verified sending domain: `send.sentinelwhs.com` (subdomain — apex stays free for Workspace mailboxes)
- Free tier (2/day) by choice — do NOT suggest upgrade until first paying customer
- **Production gap:** Railway env vars not pushed (CLI auth expired 2026-05-01). Prod stubs email until `RESEND_API_KEY` + `RESEND_FROM_EMAIL` are set on Railway

## Login Accounts
- Owner: `jonathon@sentinelwhs.com` (was `jono@sentinal.test`)
- Admin: `craig@sentinelwhs.com` (was `craig@sentinal.test`)
- Updated 2026-05-01 in both `auth.users` (email_confirmed) and `public.users`. Passwords unchanged.
- Other 10 seeded users still on `*@sentinal.test` (Sarah Chen, Lisa Park, Marcus Webb, Tom Rickard, Danny Tran, Dave Nguyen, James O'Brien, Ryan Foster, Kelly Tran, Priya Singh)

## Known Gotchas
- Zod v4 in this project — `z.record()` needs 2 args, `useState(null)` needs explicit type
- Supabase timestamps have space separator not T — don't use `z.string().datetime()`
- Agents create malformed directories `(dashboard/)` and `[id/]` — always verify after build
- RLS `FOR ALL` policies need `WITH CHECK` or writes silently fail
- `router.refresh()` after data mutations often doesn't work — use `window.location.reload()`
- Supabase `.select('*', { count: 'exact', head: true })` returns `data: null` — the count lives on `count`, NOT destructured from `data`. Reference-number generation must use MAX-then-increment, not count-then-increment.
- Permit status enum has no `approved` value — it's `draft → issued → active → suspended → closed → cancelled → archived`. "Approval" gate blocks transitions into `issued`/`active`.
- Seeded checklist templates attach questions directly to template with `section_id=NULL` and have zero section rows. Fill-out UI must synthesize a section when `sections === []`.
- Checklist module was built light-mode and had to be converted — any new module should follow design-system.md dark navy palette from day one.
- Verify FK target table names against existing migrations before writing new ones. We hit `ai_usage_log` vs actual `ai_usage_logs` in migration 025 — plan had it wrong too.
- Tailwind breakpoints are shifted up one tier in `src/app/globals.css` @theme block: sm/md/lg/xl/2xl = 768/1024/1280/**1440**/1920. Sidebar (`md:flex`) now hides below 1024. Don't assume Tailwind defaults.
- Tab bars that wrap must use **pill-style active** (bg-blue-500/10) not underline. Underlines sit at the bottom of their own wrapped row and look like a floating rectangle. Tab bar background = page color (navy-950), NOT card color (navy-900).
- `humanize()` from `src/lib/format/display.ts` — always use for snake_case enum display (employment_type, gender, role, etc.). Returns '—' for null/empty. Acronym set: WHS/PPE/JHA/SWMS/HPI/LOTO/IP/PI/RTW/MTI/LTI/FAI.
- Onboarding submission creates auth user first: `supabase.auth.admin.createUser({ email, email_confirm: false })` → then insert `public.users` with that UUID. `users.auth_id` is NOT NULL so this ordering matters. Invitee confirms via magic link on first sign-in (not yet built).
- `onboarding-documents` Storage bucket is private, keyed by org UUID as first path segment. RLS SELECT policy scopes by `((storage.foldername(name))[1])::uuid IN (SELECT organisation_id FROM users WHERE auth_id = auth.uid())`. Service role bypasses for public uploads.
- WHS industry terminology locked for involvement tracking: IP = Injured Person, PI = Person Involved, plus Witness / First Aider / Supervisor / Site Manager / Contractor. Inference is tracked on a separate `source` column, NOT as a role-name suffix.
- `event_attendee_audit_log` is append-only — DB trigger (`trg_event_attendee_audit_log_no_update` + `_no_delete`) THROWS on any UPDATE or DELETE. Do NOT attempt bulk updates via service role; they will fail loudly.
- Consultation investigations with `lifecycle_status` set render a different detail view (attendance panel + ConsultationAttendancePanel) rather than plain polymorphic detail. Unwanted Event rows always get the full investigation tabs.
- Unwanted Event 3-tier taxonomy order: **Incident** (display 10) → Incident — Environmental (20) → Incident — Injury / Illness (30). "Incident" was renamed from "Incident — Other" — don't restore the old name.
- **`createAdminClient` must NOT use `@supabase/ssr`** — `createServerClient` from `@supabase/ssr` reads the user's session JWT from cookies even when initialised with the service role key, so RLS evaluates against the user token rather than bypassing it. Use plain `createClient` from `@supabase/supabase-js` with `{ auth: { autoRefreshToken: false, persistSession: false } }`. This was causing all admin writes to fail with "new row violates row-level security policy". Fixed in `src/lib/supabase/server.ts` (2026-05-29).
- Document Types are org-configurable at `/settings/document-types` (migration 037 `document_type_definitions`). The `slug` field is what gets stored as `user_documents.document_type`. Upload form loads types dynamically — don't hardcode doc type lists in components.
- **Launcher route group** — personal "Welcome back" dashboard lives at `src/app/(launcher)/page.tsx`, NOT inside `(dashboard)`. It has its own `DashboardHeader` (no sidebar). Navigation links inside the launcher must use correct `(dashboard)` paths: `/investigations`, `/actions`, `/events/new` — NOT `/whs/investigations` or `/whs/actions` (those 404).
- **Optional FK fields in Zod**: `z.string().uuid().nullable().optional()` hard-rejects non-UUID strings (e.g. display names, empty strings). Add `.catch(null)` for optional FK fields so a bad value silently becomes null rather than killing the whole request. Also add a UUID regex guard in the client payload for defence-in-depth.
- PostgREST embedded join `.select('*, table(col)')` silently returns `null` (not an error) when FK path is ambiguous — page ignores error and renders empty state. Fix: use `.select('*')` + a separate lookup query keyed by the FK column. Always add `const { data, error } = await query; if (error) console.error(...)` to catch this.
- Event intake form now collects Actual + Potential risk on creation (`actual_likelihood`, `actual_consequence`, `potential_likelihood`, `potential_consequence`). These pre-populate `risk_assessment` JSONB with `source: 'intake'` — check for this before overwriting in the Risk Assessment tab.
- Settings left-sidebar nav lives at `src/components/settings/settings-nav.tsx` (client component, `usePathname` for active state). Layout at `src/app/(dashboard)/settings/layout.tsx` uses sticky 192px aside + flex content. `/settings` redirects to `/settings/org`.
- Per-module settings: dynamic route `src/app/(dashboard)/settings/modules/[slug]/page.tsx` reads `MODULE_CONFIGS` from `module-configs.ts`. Add new modules there — no new pages needed.
- `user_documents` has `notes TEXT` and `source_url TEXT` columns (migration 038). `source_url` = external link (Dropbox/SharePoint); when set, skips AI review and opens directly. `file_url` is the Supabase storage path; use `createSignedUrl` with `{ download: 'Filename.ext' }` for downloads — extract extension from the path, not the `document_name`.
- **RCA Table tab** (`src/components/investigation/rca-table-tab.tsx`) is currently `minDepth: 'comprehensive'` (only shows on Extreme risk investigations). User couldn't see it on first test — needs a decision: change to `null` (all investigations) or keep Extreme-only. ICAM constants in `src/lib/constants/icam-factors.ts`.

## Workspace Config
- `.mcp.json` in Sentinal-WHS workspace root has Supabase (PAT-authed) + Railway + NameSilo. Gitignored.
- **Global `~/.mcp.json`** has `gdrive` + `sheets-mcp` — loads in ALL workspaces automatically. See `mcp-servers.md` for full config + gotchas.
- Workspace IS a git repo with remote `Jonathonjw/dev-whs-sentinel` — secrets MUST stay in `.gitignore`
- Supabase PAT shared with Epsillon workspace — if rotated, update both `.mcp.json` files
- NameSilo MCP endpoint `https://mcp.namesilo.com/rpc` — `X-API-KEY` header auth (NOT `?key=` query param; that's the separate REST API). Details in `../../../Epsillon-Media-Claude-Work/memory/mcp-servers.md`
- **Supabase Management API token:** Workspace `.mcp.json` PAT only reaches Epsillon Media projects. For Sentinal (project `wuoasccjyqkdstsxuudt`), use the PAT from `epsillon-dev/.mcp.json` key `supabase2` (gitignored, not stored here). When MCP isn't loaded in session, apply migrations via temp `.tmp_apply_migration.mjs` Node script using that token + `https://api.supabase.com/v1/projects/{ref}/database/query`.
- **Supabase MCP DDL blocked:** `mcp__supabase__apply_migration` AND `mcp__supabase__execute_sql` BOTH return "Your account does not have the necessary privileges" for this Sentinal project. DDL-only workaround: use the Node script above. For SELECT queries `execute_sql` may work — DDL always fails.
- **Memory lives in workspace dir, NOT in the harness auto-memory path.** Canonical location: `C:\Users\RadiumPCs\Claude Code Workspaces\Sentinal-WHS\memory\` (this file). The system-prompt-configured path `~\.claude\projects\c--Users-RadiumPCs-...-Sentinal-WHS\memory\` is empty by design. Always write/edit memory here, not there — otherwise new entries get orphaned from this index.

## Brand Decision Pending
- **"Sentinal" is a misspelling of "sentinel"** — whole project currently uses the misspelled form. Rebrand to "Sentinel WHS" recommended before domain registration + Kim signing. Details + domain availability snapshot: [branding.md](branding.md)
- `sentinelwhs.com` available $17.29/yr at NameSilo if confirmed. `.com.au` needs VentraIP (NameSilo doesn't sell .au).

## Sessions
- Recent session log: [sessions.md](sessions.md)
