# Current State — nightowl

> Last updated: 2026-10-24

## Where we are
Project onboarded 2026-10-20 (quick start, software domain). One system
(`feed-collector`, design phase) and one workstream (`hosting-options`,
active).

## Recent decisions
- **DR-001 (feed-collector):** SQLite for the dedup store. Store wrapper stays
  thin so a swap stays possible.
- **Spike concluded:** `feedparser` handles 39/40 real feeds — DR-002 (parser
  choice) pending capture next session.

## Current focus
1. feed-collector: define the entry schema (TASK-001), sketch digest template
   (TASK-002).
2. hosting-options: the real open question is outbound email on free tiers
   (TASK-002), not compute.

## Blockers / open questions
- Does any free tier allow outbound SMTP, or do we need a transactional email
  API? (hosting-options/TASK-002)

## Next steps
- Capture DR-002 (feedparser) — evidence is in the spike results.
- Write hosting comparison doc; complete hosting-options by 2026-11-01.
