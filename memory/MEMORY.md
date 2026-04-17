# Sentinal WHS — Project Memory

## Build Status
- [Full module status + architecture](../../../Epsillon-Media-Claude-Work/memory/sentinal-whs.md) — canonical reference in shared base workspace
- 10 modules built, migrations 015-023, ~30 DB tables, ~50 API endpoints
- Next: Timesheets (06)

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
