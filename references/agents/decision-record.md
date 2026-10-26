# Agent: decision-record

**Build first. Verb: Decide.** Captures every choice as a stand-alone file — no
tiers, no threshold friction. If a choice was made, write a Decision Record.

## Format

Every decision — big or small — gets a full Decision Record file in the
**system's decisions directory** (`<system>/design/decisions/DR-XXX.md`) or the
**workstream's decisions directory** (`<workstream>/decisions/DR-XXX.md`). There
is no "small-decision log" or DECISIONS.md table. A small choice gets a short
file (decision + rationale is enough); a big one gets context, options, and
consequences. Same format, same location — just varies in length.

**Rule of thumb:** if you changed something or locked in a direction, it gets a DR.
Two sentences of rationale is fine. The DR is the capture mechanism, not a barrier.

## DR numbering

Per **system** or **per workstream**, starting at `DR-001`. Qualify across the
project when cross-referencing: `build-workshop/DR-001`, `electrical/DR-002`,
`financing/DR-001` (project-level workstream), `van/cad-design/DR-001` (nested
workstream).

No project-wide DRs — all decisions belong to a system or workstream. If a
decision genuinely cuts across all systems (rare), choose the most-affected
system to host it and note the cross-system impact in the DR's Consequences
section.

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

## Conflict scan — BEFORE writing the DR

Scan `project/overview/DECISIONS.md` (the Active table) for a decision on the
same topic. If one is active:

- **The new DR replaces it** → mark the old DR `status: superseded` and note
  `supersedes: <owner>/DR-XXX` in the new one; update both rows in DECISIONS.md,
  in the same action as creating the DR.
- **The new DR disagrees without replacing** → STOP. This is a decision
  conflict: flag it to the user before writing (see
  `references/agents/audit.md` → check 16 for how conflicts are formalised).

## After capturing

- **Add the DR's row to `project/overview/DECISIONS.md` in the same action** —
  the decisions index never lags (see `templates/overview/DECISIONS.md`).
- Link the DR (min 2 links): to the requirement/mission it serves and its system
  or workstream.
- If the DR belongs to a workstream, note it in the workstream's `NOTES.md`
  (systems rely on git history + DRs for their record — no CHANGELOG).
- If it resolves an open question, tick it in `open-questions.md` and link back —
  removing it from the overview OQ index if it was listed there.
- Commit individually: `dr: DR-XXX <short title>`.