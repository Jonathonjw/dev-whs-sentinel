---
name: Mock-first workflow on UI redesigns
description: For non-trivial UI work, build an interactive HTML mock for review BEFORE editing the real codebase
metadata:
  type: feedback
---

For non-trivial UI redesigns (multi-section pages, layout overhauls, new components), build a self-contained HTML mock at `mocks/[feature].html` in the repo root for review BEFORE touching real source. Use Tailwind CDN + the project's actual colour palette (navy palette per `.claude/rules/design-system.md`) so it lands close to production fidelity.

**Why:** Mocks prevent building the wrong layout into multiple files (can be 1000+ LOC changes). Getting approval on the mock first means the real code change is one clean pass.

**How to apply:**
- Trigger when: user says "design feels off", "not sure about the layout", "review this UI", or asks for design judgment on an existing page.
- Skip for tiny tweaks (rename a button, change a colour, add one field) — full mock is overkill. Just edit.
- Skip for greenfield where no existing UI exists to compare against — go straight to spec/build.
- After approval: port to real code with the mock as the visual reference.
- Keep mock files committed in `mocks/` dir at repo root.
