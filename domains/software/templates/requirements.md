---
title: Requirements — <system>
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: requirement
status: fresh
mission_link: <how these requirements serve mission.md>
---

# Requirements — <system>

Each requirement has a stable ID (`REQ-XXX`) and is reviewed against the mission.

## Functional
| ID | Requirement | Priority | Rationale / links |
|----|-------------|----------|-------------------|
| REQ-001 | The system shall … | must / should / could | serves: [mission element] |

## Non-functional
| ID | Category | Requirement | Target |
|----|----------|-------------|--------|
| REQ-050 | performance | p95 latency under load | < 200ms |
| REQ-051 | security | … | SOC2 control … |
| REQ-052 | scalability | … | horizontal |

## Out of scope
- [Explicitly excluded, to prevent scope creep.]

## Links
- Traces to: [mission.md]
- Realised by: [DRs / design docs / code modules]
