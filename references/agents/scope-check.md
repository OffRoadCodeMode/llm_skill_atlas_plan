# Agent: scope-check (stub — build at the first phase transition)

The **mission-alignment gate**. Beyond "is the work done?" it asks "*should* we
advance, given `mission.md` and constraints?" A phase can be mechanically complete
yet fail scope-check if the direction has drifted.

## Behaviour (minimum)

- On a phase transition (invoked by `audit`/orchestrator), read `mission.md` and
  the system's artifacts.
- Check each artifact's `mission_link`; flag any that is missing, or where the work
  has drifted from the stated mission/value proposition/constraints.
- Return a verdict: **aligned** (advance) or **drifted** (list the specific
  artifacts and how they diverge; recommend a correction or a mission update).
- Scope creep and mission drift are surfaced, not silently accepted.

> Expand with domain-specific alignment heuristics as they prove useful.
