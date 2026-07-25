# Agent: decision-record

**Build first. Verb: Decide.** Captures decisions consistently. Two tiers keep
Decision Records meaningful without losing small rationales.

## Which tier?

- **Decision Record (DR)** — a significant decision with lasting consequences
  (e.g. "event-driven architecture for data ingestion", "lithium over AGM
  batteries"). Gets a full file in `<system>/design/decisions/DR-XXX.md`.
- **`DECISIONS.md` line** — a small decision that still deserves a recorded
  rationale (e.g. "use pnpm", "timestamps in UTC"). One row in the system's (or
  `project/shared/`) `DECISIONS.md`.

**Rule of thumb:** if you'd write more than a couple of sentences of *why*, or it
shapes the project significantly, it's a DR. A `DECISIONS.md` line can be promoted
to a DR later.

## DR numbering

Per **system**, starting at `DR-001`. Qualify across systems: `ai-runtime/DR-003`;
project-wide DRs live in `project/shared/decisions/` and use `shared/DR-XXX`.

## DR format (use templates/decision-record.md)

```markdown
---
title: DR-XXX — <decision title>
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: decision
status: fresh
confidence: high | medium | low
mission_link: <how this serves mission.md>
---

# DR-XXX — <decision title>

## Status
Proposed | Accepted | Superseded by [DR-YYY](DR-YYY.md)

## Context
[The forces at play — requirements, constraints, the problem being solved.]

## Options considered
1. **[Option A]** — pros / cons
2. **[Option B]** — pros / cons

## Decision
[What was chosen.]

## Consequences
[Positive, negative, and follow-on work. What this locks in or rules out.]

## Links
- Serves: [requirement / mission element]
- Belongs to: [system]
```

## After capturing

- Link the DR (min 2 links): to the requirement/mission it serves and its system.
- Add a line to the system `CHANGELOG.md`.
- If it resolves an open question, tick it in `open-questions.md` and link back.
- Commit individually: `dr: DR-XXX <short title>`.
