# Sentinal WHS Build Patterns

## File Patterns to Follow

When building a new module, read these reference files FIRST:

| Pattern | Reference File |
|---------|---------------|
| API route (list + create) | `src/app/api/geotracking/checkins/route.ts` |
| API route (detail + update) | `src/app/api/contractors/[id]/route.ts` |
| Server list page | `src/app/(dashboard)/contractors/page.tsx` |
| Client interactive page | `src/app/(dashboard)/geotracking/checkin/page.tsx` |
| Client search component | `src/components/contractors/search-bar.tsx` |
| Server auth | `src/lib/auth/get-current-user.ts` |
| Permission check | `src/lib/auth/check-permission.ts` |
| Module guard (API) | `src/lib/modules/module-guard.ts` |
| Types file | `src/types/swms.ts` |
| Validators (Zod) | `src/lib/validators/geotracking.ts` |
| Migration + RLS | `supabase/migrations/019_geotracking.sql` |
| Industry presets | `src/lib/constants/industry-presets.ts` |
| Escalation helper | `src/lib/hierarchy/escalation-chain.ts` |

## Server vs Client Component Rules

**Server components** (async function, no 'use client'):
- CAN: fetch data, use getCurrentUser(), render static content
- CANNOT: have event handlers (onChange, onClick), use useState/useEffect, wrap in client ModuleGuard

**Client components** ('use client' at top):
- CAN: have event handlers, use hooks, use ModuleGuard wrapper
- CANNOT: use async/await at component level, call server-only functions

**Pattern:** Server page fetches data → passes to client child components for interactivity.

## Sidebar Navigation

Add nav items to `src/components/layout/sidebar.tsx`. Module-gated items go in the conditional sections that check `orgConfig?.enabled_modules.X`.

## Migration Numbering

Check `supabase/migrations/` for the latest number. Increment by 1. Current latest: 036.
