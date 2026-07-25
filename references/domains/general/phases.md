# General domain pack — phases & exit criteria

Fallback pack for non-software projects (business launch, research paper, van
conversion, event, personal build). Each **system** tracks its own phase in its
`OVERVIEW.md`; the `DASHBOARD.md` shows the aggregate.

| Phase | Description | Exit criteria (the gate) |
|-------|-------------|--------------------------|
| **plan** | Defining the project, its mission and scope | Mission written, initial systems identified, `DASHBOARD.md` created |
| **research** | Investigating options, approaches, constraints | Research docs exist for all identified streams, each reviewed, open questions logged |
| **design** | Designing the solution | Design docs created, DRs for key decisions, diagrams where useful |
| **execute** | Building / doing the work | Work in progress, tasks tracked, progress visible on dashboard |
| **review** | Testing, validating, refining | Validation complete, issues logged, ready for completion |

## Phases are not strictly linear

Real projects — especially physical builds — rarely follow a clean
plan -> research -> design -> execute -> review sequence. Expect iteration:

- **Build discoveries send you back to research.** You start building and find
  an unexpected constraint (e.g. a structural member where you planned to run
  wiring). That triggers a research spike, then a design revision, then back to
  building. This is normal, not a process failure.
- **Phases overlap.** You may be executing the electrical system while still
  researching the plumbing system. Each system tracks its own phase
  independently — the dashboard shows the aggregate.
- **Research never fully stops.** Even in execute/review, ad-hoc research
  happens (sourcing a replacement part, checking a spec, comparing a late
  alternative). Log it in the relevant system's `research/` folder.

The gate still matters — a system shouldn't advance to `execute` until its
`design` is substantively complete — but don't treat phase transitions as
one-way doors. If new information invalidates a design, move the system back to
`research` or `design` and note the regression in its `CHANGELOG.md`.

## Gates need evidence

A system advances only when every exit criterion is satisfied by a **named
artifact**. `audit` verifies the gate; `scope-check` confirms alignment with
`mission.md`. Unmet criteria or unresolved blocking open questions hold the gate.
