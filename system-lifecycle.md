# System lifecycle — create, evolve, split, retire

A **system** is a coherent area of the project that owns its own phase, decisions,
and artifacts (e.g. `ai-runtime`, `data-ingestion`, `electrical` for a van). Systems
**emerge** from gathered context — they are not declared upfront.

## When to create a system

Create one when the user has given enough context that a distinct area of work has
clear **boundaries** and **purpose**. Signals:

- A chunk of work has its own decisions, research, and tasks.
- It could plausibly be owned or advanced independently of other areas.
- Referring to it needs a name.

The `orchestrator` **recommends**; the user **confirms**. Don't pre-create empty
systems "just in case".

## Scaffolding a system

Copy `templates/system/` into `project/systems/<name>/`, keeping **only the
subfolders relevant to that system type** (per the active domain pack's
`system-types.md`). Always include `INDEX.md`, `AGENTS.md`, `OVERVIEW.md`,
`CHANGELOG.md`. Add `research/`, `design/` (with `decisions/`, `diagrams/`),
`open-questions.md`, `DECISIONS.md`, `TASKS.md`, `risks.md`, `costs.md` as needed.

> **Domain overrides.** A domain pack may override the generic template. The
> `software` pack, for example, adds a `features/` subfolder for `feature-spec.md`.

Fill `OVERVIEW.md` (type, purpose, boundaries, relationships, current phase). Set
the initial phase to the domain pack's first phase. Add the system to
`project/systems/INDEX.md` and let `audit` add its `DASHBOARD.md` row from
`OVERVIEW.md`. Commit: `system: scaffold <name> system`.

## Relating systems (the system map)

When a system connects to another (calls its API, shares power/space, feeds it
data), record the edge in **both** places:

- **`project/shared/system-map.md`** — the source-of-truth graph + relationship
  register. Add/label the edge (type from the domain pack's Relationship types,
  direction `→`/`↔`).
- **Each system's `OVERVIEW.md` → Relationships** — the local view.

`audit` (check 15) reconciles the two, flags dangling/asymmetric edges, and warns
if a system in a multi-system project has no relationships (a possible missed link).
Interface contracts (API/event schemas, power-budget calcs) are optional — create
one as its own artifact both systems link to only when an edge needs it.

## Evolving a system

- **Advance a phase** — only when the current phase's exit criteria are met by
  named artifacts (see `domains/<domain>/phases.md`); `audit` verifies the gate,
  `scope-check` confirms mission alignment. Record the transition in the system
  `CHANGELOG.md`.
- **Add artifacts** — research docs, DRs, diagrams, requirements — in the relevant
  subfolder, linked (min 2 links) and (if claim-bearing) with frontmatter.
- **Track risk/cost** — add `risks.md`/`costs.md` when the system has material
  risk or cost; these roll up to the project registers and dashboard.

## Splitting a system

If a system's sub-parts start wanting their **own changelogs**, or artifacts grow
too large to scan, that's the signal to **split** into multiple systems. Create the
new system(s), move artifacts, fix relative links, note the split in both
changelogs, and let `audit` reconcile indexes.

## Retiring a system

Mark the system `status` retired in its `OVERVIEW.md`, move it (or its artifacts)
to an `_archive/` location, keep inbound links resolvable, and note it on the
dashboard. Never hard-delete decision history.
