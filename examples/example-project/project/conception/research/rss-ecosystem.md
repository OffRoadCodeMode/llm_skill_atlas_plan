---
title: RSS/Atom ecosystem survey (2026)
created: 2026-10-20
updated: 2026-10-20
type: research
status: fresh
confidence: medium
contested:
sources: []
mission_link: Early survey before any system existed — informed the feed-collector system and its parsing spike.
---

# Research — RSS/Atom ecosystem survey

## Question
What's the sane way to consume RSS in Python in 2026 — and is RSS even "alive
enough" that a digest CLI is worth building?

## Findings
- RSS is a stable niche: ~100% of my target blogs/podcasts still publish a
  feed, even when the UI hides the link.
- `feedparser` remains the de-facto library; actively maintained, lenient XML
  handling. (medium confidence: mostly confirmed by my own spike, see below.)
- Alternatives (`listparser`-adjacent projects, JS ecosystems) add no benefit
  for a Python CLI.

## Options / approaches
| Approach | Pros | Cons | Notes |
|----------|------|------|-------|
| feedparser (Python) | mature, lenient, stdlib-adjacent | dated API | validated by spike |
| FreshRSS/Miniflux server | ready-made | violates "no dashboard" non-goal | rejected |

## Recommendation
`feedparser` — subsequently validated empirically by the
[feed-parse-benchmark spike](../../systems/feed-collector/spikes/feed-parse-benchmark/results.md).

## Links
- Informs: [feed-collector](../../systems/feed-collector/INDEX.md) (origin)
- Belongs to: conception/ (pre-system survey)
