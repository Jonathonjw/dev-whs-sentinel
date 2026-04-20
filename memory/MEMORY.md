# Sentinal WHS — Project Memory

## Build Status
- [Full module status + architecture](../../../Epsillon-Media-Claude-Work/memory/sentinal-whs.md) — canonical reference in shared base workspace
- 12 modules built, migrations 015-025, ~40 DB tables, ~75 API endpoints
- Latest: Module 28 Phase 1b shipped (commit a3425c0, 2026-04-20) — admin invite + public 6-step onboarding wizard + file upload to `onboarding-documents` bucket
- Pending on 28: Phase 1c AI doc review (Claude vision extracts licence expiry, flags discrepancies)
- Plan 29 Involvement Tracking written (`plans/modules/29-involvement-tracking.md`) + locked — IP/PI/Witness/First Aider terminology, 3-tier privacy, inferred-links-show-by-default with Confirm-with-role-picker
- Next (after 28 Phase 1c): Module 29 build, then Timesheets (06)

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

## Workspace Config
- `.mcp.json` in Sentinal-WHS workspace root has Supabase (PAT-authed) + Railway + NameSilo (HTTP, X-API-KEY header). Gitignored.
- Workspace IS a git repo with remote `Jonathonjw/dev-whs-sentinel` — secrets MUST stay in `.gitignore`
- Supabase PAT shared with Epsillon workspace — if rotated, update both `.mcp.json` files
- NameSilo MCP endpoint `https://mcp.namesilo.com/rpc` — `X-API-KEY` header auth (NOT `?key=` query param; that's the separate REST API). Details in `../../../Epsillon-Media-Claude-Work/memory/mcp-servers.md`

## Brand Decision Pending
- **"Sentinal" is a misspelling of "sentinel"** — whole project currently uses the misspelled form. Rebrand to "Sentinel WHS" recommended before domain registration + Kim signing. Details + domain availability snapshot: [branding.md](branding.md)
- `sentinelwhs.com` available $17.29/yr at NameSilo if confirmed. `.com.au` needs VentraIP (NameSilo doesn't sell .au).

## Sessions
- Recent session log: [sessions.md](sessions.md)
