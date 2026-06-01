---
name: Sentinal Feature Tracker (Google Sheet)
description: Living register of all Sentinal features/modules — built, in-progress, planned, schema-only, UI stubs, feature flags, and code TODOs.
metadata:
  type: project
---

**Sheet:** https://docs.google.com/spreadsheets/d/1HFKe5OZE5Q1nU1z0gcqSrlWkRzHhJ8GePA1QnSfyD-M/edit
**Sheet ID:** `1HFKe5OZE5Q1nU1z0gcqSrlWkRzHhJ8GePA1QnSfyD-M`, first sheet tab = `Tasks`, GID = `0`.

**Why:** CLAUDE.md tracks top-level modules only. The sheet captures sub-features, phases, feature flags, and UI stubs. It's the source of truth for feature status across all contributors.

**How to read/write (preferred — direct Google Sheets API):**
Use `mcp__sheets-mcp__*` tools:
- Load schema: `ToolSearch select:mcp__sheets-mcp__sheets_get_values,mcp__sheets-mcp__sheets_update_values,mcp__sheets-mcp__sheets_append_values`
- Read: `sheets_get_values` with spreadsheetId + range (e.g. `Tasks!A:M`)
- Update cells: `sheets_update_values` with spreadsheetId + range + values array
- Append row: `sheets_append_values`
- Match row to update: read first to find row number, then `sheets_update_values` with the specific row range (e.g. `Tasks!A35:M35`)

**Columns (A→M):** Module ID, Feature / Module, Category, Status, Priority, Description, Customer Benefit, Differentiator, Migration #, Date Built, Industry Preset, Notes, By:

**Category values:** Compliance / Site Operations / People / Reporting / Infrastructure / Heritage

**Craig's ideas:** Rows 60–69 added 2026-05-29 (no Module IDs yet — to be formalised).
