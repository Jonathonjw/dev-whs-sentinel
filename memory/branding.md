---
name: Brand Name & Domain Decision
description: Sentinal vs Sentinel spelling decision, domain research snapshot, registration pending
type: project
---

## The Spelling Question (2026-04-20 — DECISION PENDING)

The project is currently branded **"Sentinal WHS"** across repo (`WHS-Sentinal`), CLAUDE.md, code comments, and all internal docs. The correct English spelling is **"sentinel"**. This was flagged as a potential typo, not an intentional stylistic choice.

**Why this matters now:**
- Pre-launch, pre-Kim-signing → cheapest possible moment to correct
- About to commit to a domain name (which is effectively permanent branding)
- "Sentinal" has no clear story/meaning as a stylized choice (unlike Lyft/Flickr)
- Customers will silently judge it as a spelling error on WHS compliance docs — bad optics for a safety platform

**User was leaning toward rebranding to "Sentinel WHS" when last asked. Not yet confirmed.**

## Domain Research Snapshot (2026-04-20, checked via NameSilo JSON-RPC)

### With MISSPELLING ("sentinal") — wider availability
Most variants available. Strong candidates:
- `sentinalwhs.com` — $17.29/yr ⭐
- `sentinalwhs.io` — $34.99 first / $69.99 renew
- `sentinalwhs.app` — $16.99/yr
- `sentinalhq.com`, `sentinalsite.com`, `sentinalhse.com`, `sentinalcloud.com` — all $17.29/yr
- `trysentinal.com`, `usesentinal.com`, `gosentinal.com`, `joinsentinal.com` — all $17.29/yr
- Taken at short: `sentinal.com`, `.io`, `.app`, `.co`, `.dev`, `.tech`, `.net`, `.org`

### With CORRECT SPELLING ("sentinel") — much tighter
Only 12 of 54 tested variants available. Strong candidates:
- **`sentinelwhs.com` — $17.29/yr** ⭐ (primary recommendation)
- `sentinelwhs.io` — $34.99 first / $69.99 renew
- `sentinelwhs.app` — $16.99/yr
- `sentinelwhs.co` — $11.99 first / $33.99 renew
- `whssentinel.com` — $17.29/yr (reversed)
- `sentinelsite.io`, `sentinelhse.io`, `sentinelsafety.io`, `sentinelpro.io` — all $34.99/yr
- `sentinelsaas.com`, `sentinelminings.com` — $17.29/yr

Nearly every generic "sentinel + suffix" (hq, app, cloud, platform, ops, ai, pro, get/go/try/use prefixes) is TAKEN because "Sentinel" is a saturated product name (Microsoft Sentinel, security products, etc.). **The WHS suffix is the differentiator**, and it also doubles as category SEO.

## .au / .com.au — NOT available via NameSilo

NameSilo returned `"invalid"` for all Australian TLDs. They don't sell .au. Must use an Australian registrar:
- **VentraIP** (recommended) — https://ventraip.com.au/
- **Synergy Wholesale** — https://synergywholesale.com/
- **Crazy Domains** — https://crazydomains.com.au/

Requires an ABN (Epsillon Media Pty Ltd has one). `.com.au` is the correct AU TLD for a registered business; `.au` (short form) is also available since 2022 but less recognised.

## RDAP Drop Analysis (for reference)

Short `sentinal.*` / `sentinel.*` TLDs checked for drop potential:

| Domain | Expires | Drop odds |
|---|---|---|
| `sentinal.com` (1996, Tucows) | 2026-10-20 | 🔴 Zero — 30yr old, will renew |
| `sentinal.net` (1999, GoDaddy) | 2026-06-18 | 🟡 Low but has `client renew prohibited` flag |
| `sentinal.org` (2000, Netsol) | 2026-08-31 | 🔴 Very low — 26yr old |
| `sentinal.tech` (2025, eNom) | 2026-10-16 | 🟢 Moderate — fresh reg, possible speculator |
| `sentinal.app` (2025, Name.com) | 2026-11-30 | 🟢 Moderate — fresh reg |
| `sentinal.io` (2025) | 2026-12-20 | 🟢 Moderate — fresh reg |

Drop cycle: expiry → 45d renewal grace → 30d redemption grace → 5d pending delete → drops (~80 days after listed expiry). Backorder services (DropCatch, SnapNames, NameJet) charge $60-$500+ per successful catch.

Don't wait — catches are moderate-probability at best and 6-8 months away.

## Next Actions (when brand decision confirmed)

**If rebranding to "Sentinel WHS" (recommended):**
1. Register `sentinelwhs.com` via NameSilo — `mcp__namesilo__registerDomain` once reload completes, or direct curl
2. Register `sentinelwhs.com.au` via VentraIP (separate flow, manual, needs ABN)
3. Optional defensive: `sentinelwhs.io`, `sentinelwhs.app`
4. Rename GitHub repo: `WHS-Sentinal` → `WHS-Sentinel`
5. Find/replace across codebase: `Sentinal` → `Sentinel` (case-preserving). Watch out for the directory name `Sentinal-WHS` (workspace path) — renaming it means updating every reference in .mcp.json, additionalDirectories, hook paths, etc.
6. Update CLAUDE.md, READMEs, memory files, shared-memory references
7. Update Supabase project display name (actual project ref `wuoasccjyqkdstsxuudt` stays)

**If keeping "Sentinal" (not recommended):**
1. Register `sentinalwhs.com` via NameSilo
2. Register `sentinalwhs.com.au` via VentraIP
3. Prepare a one-liner explanation for why it's spelled that way — customers WILL ask

## Related Context

- Kim (Embark Building) founding customer — hasn't signed yet. Rebrand must happen BEFORE signing, or it becomes very painful.
- Competitors: BuildPass ($59-129/user/mo), HazardCo ($119/mo flat) — both use conventional spellings. A misspelled brand would be an outlier in a trust-sensitive category.
