---
name: Push to deploy — feature branch edits are invisible
description: After editing repo files, commit AND push to your feature branch immediately — the app is viewed on Railway deploy, not localhost
metadata:
  type: feedback
---

After editing any file in the Sentinal WHS repo, commit and push to your feature branch immediately as part of the same task. Don't stop at "edited + tsc passes". The deployed app on Railway is what anyone testing will see — local edits are completely invisible until pushed.

**Why:** The app is deployed on Railway (auto-deploys from master). Local changes in the repo are invisible to anyone viewing the app. Always push so changes are visible.

**How to apply:**
- Treat "edited + typechecks" as ~50% done. The other 50% is `git add` → `git commit` → `git push origin feature/[your-branch]`.
- Craig uses feature branches — NEVER push directly to master. Always push to your feature branch and open a PR.
- If someone reports "I don't see it", FIRST check whether the change is on your branch and a PR was opened. Don't troubleshoot caching/dev-server issues until you've confirmed the push landed.
- Autonomous mode means push without asking permission — but still to a feature branch, not master.
