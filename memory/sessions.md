### 2026-04-19 — NameSilo MCP setup

- Confirmed NameSilo has an official MCP server at `https://mcp.namesilo.com/rpc`
- Added `namesilo` HTTP MCP to `.mcp.json` in both Sentinal-WHS and Epsillon-Media-Claude-Work (auth via `X-API-KEY` header, key `3a2e9a67d625264d1a35d817` — user's live key, rotated once mid-session)
- Added `mcp__namesilo__*` to permission allow-list in both workspaces' `.claude/settings.json`
- NameSilo keys are ~23 chars (not 64) — that's just their format, confirmed by second regenerated key being identical length
- NameSilo **MCP** auth = `X-API-KEY` header. NameSilo **REST API** (`www.namesilo.com/api/...`) uses `?key=` query param — different surface, easy to confuse
- User hit VS Code updater error (os error 5, Code.exe locking files) when trying to reload — blocked MCP activation; session ended before NameSilo tools could be tested
- Direct curl to `https://mcp.namesilo.com/rpc` with `tools/list` returned `{"error":{"code":-32600,"message":"Invalid Request"}}` — probe was incomplete JSON-RPC; not a confirmed failure but worth re-testing once session reloads
- **Root cause found:** NameSilo's "MCP" isn't a real MCP server — it's a JSON-RPC wrapper that only responds to `namesilo.<method>` calls, not MCP spec methods. Direct curl to `namesilo.listDomains` returned all 30 of the user's domains cleanly
- **Fix shipped:** Built local stdio MCP proxy at `C:\Users\RadiumPCs\mcp-servers\namesilo-mcp\server.py` (Python + `mcp[cli]` + httpx via uv). Fetches the 51-method catalog from `/.well-known/mcp.json` at startup and proxies tool calls to NameSilo with the `X-API-KEY` header. Smoke-tested: catalog fetch + `getAccountBalance` both 200. All 51 tools register in FastMCP without collision.
- Updated both workspaces' `.mcp.json` to point at the local stdio proxy instead of the broken HTTP endpoint. Awaiting VS Code reload to pick up
- Memory trail: [mcp-servers.md](../../../Epsillon-Media-Claude-Work/memory/mcp-servers.md) has the full NameSilo MCP section with config + rationale

### 2026-04-20 — Sentinel brand spelling + domain research

- User flagged: "isn't sentinal really spelt sentinel?" — yes, whole project is using the misspelled **Sentinal** (repo name, code, CLAUDE.md, etc.)
- Checked domain availability via NameSilo JSON-RPC (bypassing MCP since session reload still pending):
  - **`sentinalwhs.com` available $17.29/yr** + `.io $34.99` + `.app $16.99`
  - **`sentinelwhs.com` available $17.29/yr** (correct spelling) + `.io $34.99` + `.app $16.99` + `.co $11.99`
  - Short single-word `sentinel.*` (com/io/app/co/net/org/dev/tech) all TAKEN — "sentinel" is saturated (Microsoft Sentinel, security products)
  - Misspelled `sentinal.*` short TLDs also all taken, likely typo-squatters
  - Most generic "sentinel + suffix" variants (hq, cloud, platform, ops, ai, pro, get/go/try/use) also TAKEN with correct spelling — WHS suffix is what differentiates
- **NameSilo doesn't sell .au / .com.au** — all AU TLDs returned "invalid". Need separate AU registrar (VentraIP, Synergy Wholesale, Crazy Domains). Requires ABN (Epsillon has one).
- RDAP analysis of unavailable `sentinal.*` short TLDs: `.com` (1996, Tucows) and `.net` (1999, GoDaddy) effectively permanent. Fresh specs like `.tech` (2025), `.app` (2025), `.io` (2025) have moderate drop potential 80 days after expiry but not worth waiting for.
- **Recommendation given to user: rebrand from Sentinal → Sentinel WHS, grab `sentinelwhs.com` immediately** — correct spelling, available, WHS suffix gives category SEO, much cheaper to fix pre-Kim-signing than post-launch
- **DECISION PENDING:** User has not yet confirmed rebrand. Next session: confirm spelling decision and register domain(s). If confirmed, needs find/replace across repo (Sentinal → Sentinel in all docs/code), rename GitHub repo, update Supabase project name reference
- User's 30 existing NameSilo domains logged via `namesilo.listDomains` — full list in context if needed. Closest expiring: `veegainz.com` 2026-05-23

### 2026-05-01 — Login email rebrand + Resend wired

- Updated Jonathon (owner) and Craig (admin) login emails in Supabase from `*@sentinal.test` → `jonathon@sentinelwhs.com` / `craig@sentinelwhs.com`. Patched both `auth.users` (with `email_confirm: true`) and `public.users` via Admin REST + service role from `.env.local`. Passwords unchanged.
- Wired Resend into `src/lib/email/send-email.ts` (was a console-log stub). Installed `resend` pkg. Behaviour: dispatches when `RESEND_API_KEY` set, falls back to stub otherwise, errors caught so call sites stay fire-and-forget. Default FROM = `Sentinal <noreply@send.sentinelwhs.com>` (configurable via `RESEND_FROM_EMAIL`). Build green. Commits `cc60e86` + `5077239` pushed to master.
- Resend domain `send.sentinelwhs.com` verified by user (subdomain choice — keeps apex SPF/MX free for Workspace mailboxes). Free-tier account (2/day quota); decision: stay on free tier until first paying customer.
- Test sends accepted by Resend (ids `8f657f4a`, `4e11f727`, `60a9343e`) but **user reports not seeing the email** — could be spam or bounced. Send-only API key can't query status from code; user needs to check https://resend.com/emails to see delivered/bounced state and confirm whether `jonathon@sentinelwhs.com` / `craig@sentinelwhs.com` are real Workspace mailboxes (apex MX records).
- **Unfinished:** (a) Railway `RESEND_API_KEY` + `RESEND_FROM_EMAIL` not pushed — `railway login` expired, blocking. Production app still stubs email to console. (b) Confirm test-email delivery via Resend dashboard.

### 2026-05-01 — Sentinal memory audit + sync

- Audited all Sentinal-related memory across 3 locations: local workspace `Sentinal-WHS\memory\`, cross-workspace `Epsillon-Media-Claude-Work\memory\`, and the harness-configured auto-memory path under `~/.claude/projects/`
- **Found auto-memory path mismatch:** harness expects writes at `C:\Users\RadiumPCs\.claude\projects\c--Users-RadiumPCs-Claude-Code-Workspaces-Sentinal-WHS\memory\` but it's empty — actual memory lives in workspace `memory\` dir. New saves via the system-prompt-default would be orphaned from MEMORY.md
- **Found 2 stale-memory drift points and fixed both:**
  - Epsillon `memory\MEMORY.md` Sentinel section claimed "5 modules built (01-05), migrations 015-019, next Timesheets" — reality is 12+ modules through migration 025, Module 28 Phase 1b shipped. Updated.
  - `CLAUDE.md` Built & Deployed table stopped at Module 27 / migration 024. Added rows for Module 28 Phase 1a+1b (migration 025) and `/people` directory rebuild. Pushed Phase 1c + Module 29 above Timesheets in Next-to-Build.
- Confirmed local `Sentinal-WHS\memory\MEMORY.md`, Epsillon `sentinal-whs.md` (architecture), `branding.md`, and `sessions.md` were already current — no edits needed
- Flagged near-duplicate filenames: `sentinal-whs.md` (architecture, misspelled) vs `sentinel-whs.md` (product/market, correct spelling) sit side-by-side in Epsillon memory. Distinct topics, but spelling collision is a footgun — especially given the rebrand decision still pending
- Activity gap: 11 days quiet between last commit (2026-04-20) and today (2026-05-01) — nothing new in the repo to capture

### 2026-05-02 — Plan 33 written: event location pin-drop

- User asked feasibility/effort for adding map pin-drop to Location Details field on Unwanted Event form (`src/components/events/forms/unwanted-event-form.tsx:434`)
- Confirmed Google Maps JS API is already wired up (`NEXT_PUBLIC_GOOGLE_MAPS_API_KEY`) and used by `src/components/address-autocomplete.tsx` for the Plan 05 / Geofence module — incremental cost only
- `location_details` currently TEXT-only on `investigations` (added in migration 026 line 148); used by unwanted-event-form, report-tab, polymorphic-event-detail, events POST + investigations PATCH APIs
- Wrote `plans/modules/33-event-location-pin-drop.md` — migration 031 adds `location_latitude` + `location_longitude` NUMERIC(10,7) with paired-or-null + range CHECK constraints + partial index for "events with a pin" lookups
- Plan covers: extract shared Google Maps loader (`src/lib/maps/load-google-maps.ts`) refactoring autocomplete to use it before adding the second consumer; new `<LocationPicker>` (text input + "Pin on map" button + lazy-loaded `next/dynamic` modal with click-to-drop draggable marker + reverse-geocode as opt-in suggestion, never auto-overwrite); read-only Google Maps Embed iframe on detail page
- Estimated 3-5 hrs. **Awaiting user approval to build** — no code written this session
- Latest migration is 030 (consultation_event_type), so pin-drop migration takes 031

### 2026-05-01 — Unwanted Event page redesign

- User flagged desktop layout of `/events/new/unwanted_event` felt off — single-column 768px form with 7 stacked sections wasted real estate on wide screens
- Ran `web-design-guidelines` skill review against page.tsx + unwanted-event-form.tsx + common.tsx — flagged real compliance issues (missing `name` attrs, hydration drift on `useMemo(() => new Date(), [])`, ARIA gaps, ellipsis chars) alongside root-cause layout density
- Built interactive HTML mock at `mocks/unwanted-event-redesign.html` (Tailwind CDN, navy palette, scroll-spy + severity callout JS) — 12-col layout, sticky anchor rail, paired fields, accent-bar lead sections, sticky bottom action bar, severity preview when Notifiable=Yes (submit switches to red "Submit & Notify SafeWork")
- User approved ("that looks way better") — delegated build to `dev-frontend` agent. Extended shared `Section` with optional `accent` + `badge` props (additive, safe for other 5 event form templates); fixed `YesNoRadio` ARIA (replaced `aria-pressed` with `role="radiogroup"` + `aria-checked`); restructured `unwanted-event-form.tsx` to match mock; bundled compliance fixes (name attrs, autocomplete, aria-required, aria-describedby, role="alert", focus-first-invalid, hydration-safe timestamps, ellipsis chars)
- Notable agent decision: bypassed shared `Section` component for the 7 redesigned sections because its `space-y-4` children wrapper blocks grid layouts as immediate parent of fields. Each section is a raw `<section>` with chrome replicated inline. Shared `Section` stays unchanged for the other 5 forms.
- Build green, commit `9cfde6a` pushed to master → Railway auto-deploy. Mock file committed alongside code for future reference.
- Pattern validated: HTML mock first → user approval → then code. Saved ~30+ min vs. building the wrong layout. Saved as feedback memory.

### 2026-05-02 — Native Google Drive + Sheets MCP (no n8n)

- Replaced n8n-brokered `mcp__google-sheets__` with direct native MCP servers
- Set up `@modelcontextprotocol/server-gdrive` as stdio server — needed env vars `GDRIVE_OAUTH_PATH` + `GDRIVE_CREDENTIALS_PATH` to work around broken Windows path resolution in `new URL(import.meta.url).pathname`
- OAuth keys at `~/.claude/gcp-oauth.keys.json`, token at `~/.claude/.gdrive-server-credentials.json` — Desktop app OAuth (not service account)
- `server-gdrive` only exposes `mcp__gdrive__search` — no write, no structured read
- Built custom `~/.claude/sheets-mcp-server.js` (~150 lines Node.js CJS) — stdio MCP wrapping Google Sheets API v4 via `googleapis` installed locally at `~/.claude/node_modules/`; reuses same Desktop app OAuth client with separate Sheets scope token at `~/.claude/.sheets-token.json`
- Tools live: `sheets_get_values`, `sheets_update_values`, `sheets_append_values`, `sheets_get_info`, `sheets_clear_values`
- Both servers in `~/.mcp.json` (user-level) → available in ALL workspaces automatically
- Removed stale n8n `google-sheets` SSE entry from `Sentinal-WHS/.mcp.json`
- Google Cloud project: `1061954019899` (Claude Code Google Stuff) — Drive + Sheets APIs enabled

### 2026-05-02 — Event system, polymorphic forms + consultation lifecycle (long session w/ Craig)

- Reviewed epsillon-dev toolset → copied `railway-deploy` skill. Set autonomous mode + removed file-deletion confirmation gate.
- Viewed PeopleTray screenshots — Craig built that WHS taxonomy himself; used as IP-clean canonical reference for Craig's WHS event framework.
- Built **WHS dashboard incident trend chart** — chip filters (All Events + HiPo live, 7 stubbed), period dropdown, dashed in-progress current month. Commit `776fca2`.
- Shipped **Plan 30** — Submit New Event flow: 15-field form, 2-tier taxonomy tables (mig 026), worker self-report, AI chat gated on investigation_level. Commits `a19799a`, `b382e0d`, `6dc43d6`.
- Shipped **Plan 31** — Polymorphic event forms: Craig's 5→6 type taxonomy, type-specific templates, 3-tier classification (FAI/MTI/LTI etc. under Unwanted Event/Injury), Consultation 6th type. Migs 027-030. Commits `8fb7742`, `c39491f`, `3c9366a`, `e265e38`, `91da432`.
- Shipped **Plan 32** — Consultation lifecycle Plan→Run→Confirm: attendee register, tokenised public confirmation page, 2-step acknowledgement, Day 0/3/5/7 cron, append-only audit log with throwing DB trigger. Migs 031/032. Commit `3f9090a`.
- Wrote plan docs 30/31/32 (commit `79fb2b8`). Updated Feature Tracker (rows 31/32 → Built & Deployed; added H6a + EF-LP).
- **Follow-up:** Add `CRON_SECRET` to Railway env. Set up daily external cron scheduler for `/api/cron/consultation-reminders`.
- Token note: Sentinal Management API — use the PAT from `epsillon-dev/.mcp.json` key `supabase2` (gitignored). Workspace `.mcp.json` PAT only reaches Epsillon projects.

### 2026-05-02 — Modules dropdown in top-right header

- Built `src/components/layout/module-switcher.tsx` (`'use client'`, lucide `Grid3x3` + `ChevronDown`) listing WHS, People, Documents, Training, Workforce, Reports — highlights current module by pathname, closes on outside click. Wired into `header.tsx` desktop bar between `IndustrySwitcher` and the email/profile link
- User reported "didn't appear" + screenshot showing no dropdown — **cause: I edited locally and never pushed.** They view the Railway deploy, not localhost. Committed `db58071` and pushed to origin/master to trigger auto-deploy
- Saved as feedback memory: after editing `C:\Users\RadiumPCs\repos\Sentinal WHS\` files, commit+push immediately rather than suggesting "hard refresh" — the user's surface is the Railway deploy, local edits are invisible to them
- Tooling note: Bash `cd` resets between calls in this session; used `node -e "execSync(..., { cwd })"` to run commands in the Sentinal repo
