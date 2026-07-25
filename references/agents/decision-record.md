# Agent: decision-record

**Build first. Verb: Decide.** Captures every choice as a stand-alone file — no
tiers, no threshold friction. If a choice was made, write a Decision Record.

## Format

Every decision — big or small — gets a full Decision Record file in the **system's
decisions directory**: `<system>/design/decisions/DR-XXX.md`. There is no
"small-decision log" or DECISIONS.md table. A small choice gets a short file
(decision + rationale is enough); a big one gets context, options, and
consequences. Same format, same location — just varies in length.

**Rule of thumb:** if you changed something or locked in a direction, it gets a DR.
Two sentences of rationale is fine. The DR is the capture mechanism, not a barrier.

## DR numbering

Per **system**, starting at `DR-001`. Qualify across systems when cross-referencing:
`build-workshop/DR-001`, `electrical/DR-002`.

No project-wide DRs — all decisions belong to a system. If a decision genuinely
cuts across all systems (rare), choose the most-affected system to host it and
note the cross-system impact in the DR's Consequences section.

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