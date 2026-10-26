---
title: Spike — feed-parse-benchmark
created: 2026-10-21
updated: 2026-10-22
type: spike-spec
status: concluded
timebox: 1 day
code_location: in-repo
code_ref:
mission_link: Picking the feed parser is the first irreversible code decision; get empirical data before locking it.
---

# Spike spec — feed-parse-benchmark

## Question
Is `feedparser` robust enough on my 40 real feeds (including two known-broken
ones), or do I need a heavier parser?

## Hypothesis
`feedparser` parses 95%+ of my feeds correctly, including the malformed ones —
it's the pragmatic choice.

## Success criteria
- ≥ 38 of 40 feeds parse with usable title/url/published fields.
- The two broken feeds either parse or fail gracefully with a logged warning.
- No per-feed custom handling required.

## Approach
Script iterates over my exported OPML list of feed URLs, fetches and parses
each, logs field-level failures. Nothing persisted; output is a table.

## Code location
in-repo — code lives in this folder under `code/`.

## Timebox
1 day. If I can't get clean comparisons by then, conclude with the partial
table and escalate.

## Unblocks
Library choice DR for feed parsing (follows DR-001's store decision).

## Links
- Results: [results.md](results.md)
- Informs: parser library decision (to be captured as DR-002)
- Belongs to: [feed-collector](../../INDEX.md)
