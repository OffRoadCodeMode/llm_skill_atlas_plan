# System lifecycle — create, evolve, split, retire

A **system** is a coherent area of the project that owns its own phase, decisions,
and artifacts (e.g. `ai-runtime`, `data-ingestion`, `electrical` for a van). Systems
**emerge** from gathered context — they are not declared upfront.

Systems can be **hierarchical**: a system may contain sub-systems, each with the
same structure (OVERVIEW, research, design, decisions, etc.) and its own
independent phase. See "Hierarchical systems" below.

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
`references/domains/<domain>/system-types.md`). Always include `INDEX.md`, `AGENTS.md`, `OVERVIEW.md`,
`CHANGELOG.md`. Add `research/`, `design/` (with `decisions/`, `diagrams/`),
`open-questions.md`, `DECISIONS.md`, `TASKS.md`, `risks.md`, `costs.md` as needed.

> **Domain overrides.** A domain pack may override the generic template. The
> `software` pack, for example, adds a `features/` subfolder for `feature-spec.md`.

Fill `OVERVIEW.md` (type, purpose, boundaries, relationships, current phase). Set
the initial phase to the domain pack's first phase. If this is a sub-system, set
the `parent:` field in `OVERVIEW.md` frontmatter to the parent system name. Add the
system to `project/systems/INDEX.md` (or the parent system's `INDEX.md` if nested)
and let `audit` add its `DASHBOARD.md` row from `OVERVIEW.md`. Commit:
`system: scaffold <name> system`.

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
  named artifacts (see `references/domains/<domain>/phases.md`); `audit` verifies
  the gate, `scope-check` confirms mission alignment. Record the transition in the system
  `CHANGELOG.md`.
- **Add artifacts** — research docs, DRs, diagrams, requirements — in the relevant
  subfolder, linked (min 2 links) and (if claim-bearing) with frontmatter.
  System-specific research goes in `<system>/research/`; cross-system or
  general research goes in `project/shared/research/`.
- **Track risk/cost** — add `risks.md`/`costs.md` when the system has material
  risk or cost; these roll up to the project registers and dashboard.

## Hierarchical systems

Systems can contain sub-systems. A sub-system is a regular system folder nested
inside a parent system's `systems/` subfolder. It has the same structure
(OVERVIEW, research, design, decisions, etc.) and tracks its own phase
independently.

### When to create a sub-system

**Not upfront.** Start with the parent system and let sub-systems emerge when:

1. A component within the parent has its **own distinct research stream** (e.g.
   van electrical has different research from van insulation).
2. A component has **its own design decisions** that don't belong at the parent
   level (e.g. battery bank sizing is an electrical decision, not a van-level
   one).
3. A component has **its own phase progression** (e.g. electrical is in design
   while insulation is still in research).
4. The parent system's artifacts would become **too large to scan** if everything
   stayed at one level.

The agent **recommends** splitting; the user **confirms**. This follows the same
*emergence* principle as top-level systems.

### How to create a sub-system

1. Create `project/systems/<parent>/systems/<name>/` using the same
   `templates/system/` scaffold.
2. Set `parent: <parent-name>` in the sub-system's `OVERVIEW.md` frontmatter.
3. Add the sub-system to the parent system's `INDEX.md`.
4. Add the sub-system to `project/shared/system-map.md` (inside a `subgraph`
   cluster for the parent).
5. Commit: `system: scaffold <parent>/<name> sub-system`.

### What stays at the parent level

- The parent system's own research, design, and decisions (e.g. van vehicle
  selection is a parent-level concern).
- The parent's `OVERVIEW.md` lists its sub-systems in a **Sub-systems** section
  with links to each.
- The parent tracks its own phase for its own direct work.
- Sub-system phases are **independent** — the parent does not auto-advance when
  all children complete. The dashboard shows both parent and child phases.

### Depth guidance

No fixed limit, but 2-3 levels is plenty in practice. If you feel the need for a
4th level, the parent is probably too broad and should be split into peer
top-level systems instead.

## Splitting a system

If a system's sub-parts start wanting their **own changelogs**, or artifacts grow
too large to scan, that's the signal to **split**. Two options:

- **Extract as a peer** — create a new top-level system, move artifacts, fix
  relative links. Use when the split-off area is independent.
- **Extract as a sub-system** — create a nested system under the current one.
  Use when the split-off area is a component of the current system (see
  "Hierarchical systems" above).

In both cases: move artifacts, fix relative links, note the split in both
changelogs, and let `audit` reconcile indexes.

## Retiring a system

Mark the system `status` retired in its `OVERVIEW.md`, move it (or its artifacts)
to an `_archive/` location, keep inbound links resolvable, and note it on the
dashboard. Never hard-delete decision history.
