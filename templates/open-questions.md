# Open Questions — <Project | System>

`OQ-XXX`, numbered per register. Tag `blocks:` with the phase it gates (or `—`).
Resolving a question should link to the DR/decision/research that settled it.
`audit` surfaces unresolved questions, especially those blocking a phase.

## Per-system file (`<system>/open-questions.md`)

Each system maintains its own open-questions file. Questions live here when they
belong to a single system's scope.

```
## Open
- [ ] OQ-001: [question] | raised: YYYY-MM-DD | blocks: [phase or —]

## Resolved
- [x] OQ-000: [question] → Resolved YYYY-MM-DD: [answer] (see DR-XXX)
```

## Overview file (`project/overview/open-questions.md`)

The overview file is a **cross-system index**, maintained inline. It holds:

1. **Cross-system questions** that don't belong to any single system.
2. **Links to per-system open questions** that are blocking or significant, so
   you can see all outstanding questions across the project in one place.

```
## Open

### Cross-system
- [ ] OQ-001: [question] | raised: YYYY-MM-DD | blocks: [phase or —]

### Per-system index
- [ ] electrical/OQ-002: [question] → [electrical/open-questions.md](../systems/electrical/open-questions.md)
- [ ] van/OQ-004: [question] → [van/open-questions.md](../systems/van/open-questions.md)

## Resolved
- [x] OQ-000: [question] → Resolved YYYY-MM-DD: [answer] (see DR-XXX)
```

When a per-system question is resolved, remove its line from the overview index
(or move it to Resolved with a link) in the same action — roll-ups are never
left to lag. `audit` verifies the index against the per-system files (check 6).
