---
name: Build status snapshot — 2026-05-29
description: What's been built, what's schema-only, what's UI stubs, and what's next as of 2026-05-29
metadata:
  type: project
---

## Built & Deployed (migrations 001–036)

| Module | Notes |
|--------|-------|
| Investigations (H1) | Incident workflow, evidence upload, AI summary, PDF export |
| Actions (H2) | Corrective/preventive action tracking |
| Permits (H3) | Permit-to-work (hot work, confined space, heights, electrical) |
| JHA (H4) | Job Hazard Analysis |
| People (H5) | People profiles, invite flow, 6-step onboarding wizard |
| Dashboard (H6) | WHS dashboard with incident trend chart |
| Module System (01) | Modular feature flags per org |
| Contractor Management (02) | Contractor onboarding, compliance tracking |
| SWMS Reg 291 (03) | Legal SWMS document workflow |
| Compliance Gate (04) | 13 parallel compliance checks, block/warn/override |
| Geotracking & Check-in (05) | GPS check-in, geofence, site attendance |
| WHS Checklists (13) | Take 5s, inspection checklists |
| Platform Roles & Permissions (022) | worker/supervisor/whs_advisor/site_manager/admin/owner |
| Org Hierarchy & Escalation (023) | Department/position trees, escalation chains |
| Work Packs + Notifications (27) | Work pack bundles, notification system |
| People Module Phase 1a+1b (28) | Profile read-only, correction requests, onboarding wizard |
| Password Reset | Self-service via Supabase PKCE |
| Address Autocomplete | Google Maps autocomplete for geofence setup |
| Rosters Phase 0–2.5 (34) | Roster grid, shift types, pattern wizard — migrations 031/033-036 |

## Schema Complete, UI Not Built

| Module | Notes |
|--------|-------|
| Event Form Framework (30) | Polymorphic events (Unwanted/FLI/Assurance/Regulatory/Mine Record) — migrations 026-030 |
| Consultation Events (31) | Plan/run/confirm + attendance — migration 032 |
| Roster Phases 3–5 | Conflict detector, swap RLS, fatigue bridge — deferred |
| Event Taxonomy Admin (36) | Schema seeded on org creation; no CRUD UI yet |

## UI Stubs Only (no API/DB)

- Document Library (`/documents`) — ModuleGuard key `documents` not in EnabledModules, fix before build
- Training Matrix (`/training`)
- Workforce sub-views: Flights, Accommodation, Travel Requests

## Next to Build (priority order)

1. **People Module Phase 1c** — AI doc review (Claude vision: licence expiry extraction)
2. **Involvement Tracking (29)** — IP/PI/Witness/First Aider, 3-tier privacy — plan at `plans/modules/29-involvement-tracking.md`
3. **Timesheets (06)** — HIGH priority
4. **AI Investigation Assist** — AI PEEPO plan + evidence extraction (Craig's ideas #1 + #4 from tracker rows 60-69)
5. **Real-Time Dashboard (07)**
6. **Mobile PWA (08)**

## Latest Migration

`036` — check `supabase/migrations/` for current highest number before writing new migrations.
