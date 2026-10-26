---
title: feed-collector
created: 2026-10-20
updated: 2026-10-24
type: system
system_type: backend
parent: none
status: active
phase: design
---

# System: feed-collector

> Identity + contents. Frontmatter changes are mirrored to the dashboard in the
> same action.

Fetches RSS/Atom feeds on a schedule, deduplicates entries against a local
store, and composes the daily digest email.

## Purpose
The only system the project has so far. Everything else (email sending,
scheduling) is expected to emerge as sub-systems or separate systems once the
fetch/parse core proves itself.

## Boundaries
- Owns: feed fetching, parsing, dedup storage, digest composition.
- Delegates: hosting/scheduling choice → `hosting-options` workstream; email
  delivery → future system (not yet created).

## Origin / informed by
Early feed-library survey: [rss-ecosystem](../../conception/research/rss-ecosystem.md).
Spike: [feed-parse-benchmark](spikes/feed-parse-benchmark/spec.md).

## Relationships
None yet — only system in the project.

## External Links
Code repo: https://github.com/offroadcodemode/nightowl (created, empty)

## Contents
> Maintained inline; verified by audit.

- [Design](design/) — [decisions](design/decisions/)
- [Spikes](spikes/) — [feed-parse-benchmark](spikes/feed-parse-benchmark/spec.md)
- [Tasks](TASKS.md)
