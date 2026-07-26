# Risks — <Project Name>

Project-wide risk register. `R-XXX` per register. `Severity` derives from
Likelihood x Impact. `Status` in open | mitigating | accepted | closed. Every
active risk should name an Owner and a Mitigation (audit flags high-severity
risks missing either). Risks that block a phase gate cross-link to the relevant
open-questions.md / phase.

The shared file holds:

1. **Project-wide risks** that don't belong to any single system.
2. **Index of per-system risks** — links to high-severity or blocking risks from
   each system's `risks.md`, so the full project risk picture is visible in one
   place. The dashboard's Top Risks are rolled up from both.

```
## Active

### Project-wide
| ID | Risk | Likelihood | Impact | Severity | Owner | Mitigation | Status |
|------|------|-----------|--------|----------|-------|-----------|--------|
| R-001 | [risk] | high/med/low | high/med/low | high/med/low | [owner] | [mitigation] | open |

### Per-system index
- electrical/R-002: [risk] (severity: high) -> [electrical/risks.md](../systems/electrical/risks.md)
- van/R-005: [risk] (severity: high, blocks: design) -> [van/risks.md](../systems/van/risks.md)

## Closed
- R-000: [risk] -> resolved YYYY-MM-DD ([how / link to DR])
```

When a per-system risk is closed, remove its line from the shared index (or move
to Closed with a link). `audit` (check 13) reconciles the index against the
per-system files.
