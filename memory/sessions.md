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
