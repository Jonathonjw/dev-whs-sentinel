---
name: Resend email integration — free tier until first paying customer
description: Sentinal email goes via Resend through verified send.sentinelwhs.com subdomain. Account is free tier by deliberate choice.
metadata:
  type: project
---

**Setup:**
- Provider: Resend, key in `.env.local` as `RESEND_API_KEY`
- Verified domain: `send.sentinelwhs.com`
- Default FROM: `Sentinal <noreply@send.sentinelwhs.com>`
- Wired in: `src/lib/email/send-email.ts`

**Free tier quotas:** 2 emails/day. Real onboarding traffic will silently fail above quota — app does not crash, just `console.error`s.

**Why on free tier:** Pre-revenue. Decision is to not pay for tooling until there's a paying customer. Jonathan is aware production email won't work at any meaningful volume.

**How to apply:**
- Don't suggest upgrading the Resend plan proactively.
- If a feature depends on reliable email at volume (e.g. bulk worker invites), flag it as blocking and propose upgrade then — not before.
