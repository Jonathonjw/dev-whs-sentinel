# Sentinal WHS — Project Memory

## Build Status
- [Full module status + architecture](../../../Epsillon-Media-Claude-Work/memory/sentinal-whs.md) — canonical reference in shared base workspace
- 20+ modules/features built, migrations 015-032, ~55 DB tables, ~100+ API endpoints
- Latest session (2026-05-02): Plans 30+31+32 shipped — Submit New Event, polymorphic event forms (6 types, 3-tier taxonomy), consultation lifecycle Plan→Run→Confirm
- Latest migration: **032** (event_attendees, event_attendee_audit_log, lifecycle_status on investigations)
- Plan 30+31 (event system + polymorphic forms): Built & Deployed — migrations 026-030
- Plan 32 (consultation lifecycle): Built & Deployed — migrations 031-032
- **Follow-up required:** Add `CRON_SECRET` to Railway env vars; set up daily external cron to hit `/api/cron/consultation-reminders`
- Next to build: Plan 28c (AI doc review), Plan 29 (Involvement Tracking), Plan 06 (Timesheets)

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

## Workspace Config
- `.mcp.json` in Sentinal-WHS workspace root has Supabase (PAT-authed) + Railway + NameSilo. Gitignored.
- **Global `~/.mcp.json`** has `gdrive` + `sheets-mcp` — loads in ALL workspaces automatically. See `mcp-servers.md` for full config + gotchas.
- Workspace IS a git repo with remote `Jonathonjw/dev-whs-sentinel` — secrets MUST stay in `.gitignore`
- Supabase PAT shared with Epsillon workspace — if rotated, update both `.mcp.json` files
- NameSilo MCP endpoint `https://mcp.namesilo.com/rpc` — `X-API-KEY` header auth (NOT `?key=` query param; that's the separate REST API). Details in `../../../Epsillon-Media-Claude-Work/memory/mcp-servers.md`
- **Supabase Management API token:** Workspace `.mcp.json` PAT only reaches Epsillon Media projects. For Sentinal (project `wuoasccjyqkdstsxuudt`), use the PAT from `epsillon-dev/.mcp.json` key `supabase2` (gitignored, not stored here). When MCP isn't loaded in session, apply migrations via temp `.tmp_apply_migration.mjs` Node script using that token + `https://api.supabase.com/v1/projects/{ref}/database/query`.
- **Memory lives in workspace dir, NOT in the harness auto-memory path.** Canonical location: `C:\Users\RadiumPCs\Claude Code Workspaces\Sentinal-WHS\memory\` (this file). The system-prompt-configured path `~\.claude\projects\c--Users-RadiumPCs-...-Sentinal-WHS\memory\` is empty by design. Always write/edit memory here, not there — otherwise new entries get orphaned from this index.

## Brand Decision Pending
- **"Sentinal" is a misspelling of "sentinel"** — whole project currently uses the misspelled form. Rebrand to "Sentinel WHS" recommended before domain registration + Kim signing. Details + domain availability snapshot: [branding.md](branding.md)
- `sentinelwhs.com` available $17.29/yr at NameSilo if confirmed. `.com.au` needs VentraIP (NameSilo doesn't sell .au).

## Sessions
- Recent session log: [sessions.md](sessions.md)
