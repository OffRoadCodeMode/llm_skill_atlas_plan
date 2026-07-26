# Software domain pack — phases & exit criteria

Phases for software projects. Each **system** tracks its own phase in its
`INDEX.md` frontmatter (`phase:`); the `DASHBOARD.md` shows the aggregate.
Systems can be in different phases simultaneously. There is no single
project-level phase.

| Phase | Description | Exit criteria (the gate) |
|-------|-------------|--------------------------|
| **onboarding** | Gathering project context, defining mission | Mission written, initial systems identified, `DASHBOARD.md` created |
| **research** | Investigating approaches, competitors, technology | Research docs exist for all identified streams, each reviewed, open questions logged |
| **requirements** | Defining what the system must do | Functional + non-functional requirements written, each has an ID (REQ-XXX), reviewed against mission |
| **architecture** | Designing system structure | Architecture diagrams created, DRs for key decisions, component decomposition done, data flow mapped |
| **design** | Detailed design | API contracts defined, data models designed, wireframes for user-facing parts, spike prototypes for risky components |
| **implementation** | Building the system | Code repo created, plan↔code links established, CI/CD running, iterative delivery |
| **deployed** | In production | System deployed, monitoring in place, runbook written |

## Gates need evidence

A system advances only when **every exit criterion is satisfied by a named
artifact**. `audit` checks each criterion against real files and reports met /
missing / stale; `scope-check` confirms the move aligns with `mission.md`. A gate
with unmet criteria or unresolved **blocking** open questions does not pass.

## Phases vs folders

`architecture` (system structure, component decomposition, key DRs) and `design`
(API contracts, data models, wireframes) are distinct **phases**, but both produce
artifacts that live in the single `design/` folder (`design/decisions/`,
`design/diagrams/`, plus detailed design docs). The phase is the *stage of work*;
the folder is *where artifacts land*. They don't map 1:1 — a system in the `design`
phase still has its `architecture` DRs sitting in `design/decisions/`.

## Template override

The software pack **overrides** the generic system template to add a `features/`
subfolder (for `feature-spec.md` documents) alongside `research/` and `design/`.
