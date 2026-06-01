---
name: MCP Servers — Sentinal workspace
description: Connection patterns and gotchas for MCP servers used in this workspace
metadata:
  type: reference
---

## mcp__supabase__* — Supabase MCP (set up per SETUP.md)

Direct DB access. Use for running migrations, inspecting tables, executing SQL.

**Tools include:** execute_sql, apply_migration, list_tables, list_migrations, generate_typescript_types

**Project ref:** `wuoasccjyqkdstsxuudt`

**Load schema:** `ToolSearch select:mcp__supabase__execute_sql,mcp__supabase__apply_migration,mcp__supabase__list_tables`

**Preferred migration flow:** Use `mcp__supabase__apply_migration` directly — cleaner than running scripts manually.

---

## mcp__sheets-mcp__* — Direct Google Sheets API

Talks directly to Google Sheets API. Used to read/write the Sentinal Feature Tracker.

**Load schema:**
```
ToolSearch select:mcp__sheets-mcp__sheets_get_values,mcp__sheets-mcp__sheets_update_values,mcp__sheets-mcp__sheets_append_values,mcp__sheets-mcp__sheets_get_info
```

**Pattern to update a specific row:**
1. `sheets_get_values` to find the row number where Module ID = target
2. `sheets_update_values` with range `Tasks!A{row}:M{row}` and full row values array

**Gotcha:** Range must include the sheet name (e.g. `Tasks!A1:M56`). GID 0 = first tab.

---

## SUPABASE_DB_PASSWORD

Stored in `[YOUR_REPO_PATH]/.env.local` as `SUPABASE_DB_PASSWORD`.
Get this from Jonathan — required for the migration script fallback if Supabase MCP is unavailable.
Connection pooler: `postgresql://postgres.wuoasccjyqkdstsxuudt@aws-1-ap-northeast-1.pooler.supabase.com:5432/postgres`
