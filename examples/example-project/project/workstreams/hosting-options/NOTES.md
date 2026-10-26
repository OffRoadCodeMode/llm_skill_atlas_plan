---
title: Notes — hosting-options
created: 2026-10-20
updated: 2026-10-24
type: workstream
status: active
---

# Notes — hosting-options

## 2026-10-21 — candidates listed

Four candidates: fly.io, Railway, Hetzner, Oracle free tier. Railway ruled out
fast — its free tier needs card verification, and the budget is effectively
zero until proven. Remaining three look viable for a daily cron + email job.

## 2026-10-24 — the email question is the real one

Outbound email is the likely blocker, not compute. Free tiers often block port
25; will need either a provider that allows it or a transactional email API
with a free allowance. Comparing in research/hosting-comparison.md next.
