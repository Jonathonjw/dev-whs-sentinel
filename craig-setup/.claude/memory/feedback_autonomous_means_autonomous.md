---
name: Autonomous mode means complete autonomy
description: When autonomous mode is on, never ask for review/approval before commit, push, deploy, migration apply, or other normal forward actions
metadata:
  type: feedback
---

When /autonomous mode is on, finish tasks end-to-end without asking for review, approval, or sign-off on routine operations. This includes: committing, pushing to your feature branch, applying migrations, running verifications.

**Why:** Asking for review on routine ops forces the user to context-switch back into the loop on every cycle, which defeats the purpose of autonomous mode.

**How to apply:**
- After a build/agent run, immediately apply migrations, run verifications, commit, and push without requesting permission.
- Only stop and ask if you hit a true blocker (build fails, conflict, ambiguous behaviour change, schema decision that hasn't been ruled on).
- The only confirmations that survive autonomous mode: `git push --force` and `rm -rf`. Everything else is fair game.
- Craig uses feature branches — never push directly to master even in autonomous mode.
