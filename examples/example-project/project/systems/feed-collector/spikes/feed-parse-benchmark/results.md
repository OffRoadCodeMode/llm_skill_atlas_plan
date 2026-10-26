---
title: Spike results — feed-parse-benchmark
created: 2026-10-22
updated: 2026-10-22
type: spike-results
status: fresh
verdict: supports
related_spec: spec.md
mission_link: Parser choice locked with evidence — unblocks implementation planning.
---

# Spike results — feed-parse-benchmark

## Verdict
Hypothesis **supported**: `feedparser` handled 39/40 feeds with usable fields.

## Outcome vs success criteria

| Criterion (from spec) | Measured result | Met? |
|-----------------------|-----------------|------|
| ≥ 38/40 feeds parse with usable fields | 39/40 ✓ | yes |
| Broken feeds fail gracefully | 1 parsed despite bad XML header; 1 (an old podcast feed) crashed on encoding — caught, logged | yes |
| No per-feed custom handling | None needed | yes |

## Findings
- `feedparser`'s lenient XML mode eats malformed headers fine.
- Published-date handling varies by feed; needs a `published_parsed or
  updated_parsed` fallback in code, not in the parser.
- One feed crashed on a UTF-16 BOM — a try/except in the fetch wrapper covers it.

## Surprises / new risks
- Two feeds publish items with future dates; digest must sort/filter by
  "new since last run", not by published date alone.

## Reproduce
`python code/bench.py --opml my-feeds.opml` — prints the per-feed table.

## Next steps
- Capture DR-002: use `feedparser`; date fallback rule as above.

## Links
- Spec: [spec.md](spec.md)
- Informs: feed-collector TASK-001 (entry schema)
- Belongs to: [feed-collector](../../INDEX.md)
