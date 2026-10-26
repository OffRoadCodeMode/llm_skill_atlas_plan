# Conventions

How Atlas structures files. Read this before generating or editing project content.

## Directory layout (generated in the working directory)

```
my-project-planning/
├── AGENTS.md                 # Root rules; references Atlas; records skill version used
├── project/
│   ├── DASHBOARD.md          # Scoreboard: phase per system, workstreams, next milestones
│   ├── mission.md            # Mission, vision, value prop, principles, constraints
│   ├── systems/
│   │   └── (system folders emerge over time)
│   │       └── <parent-system>/          # A system may contain sub-systems
│   │           ├── INDEX.md              # System entry file: identity + contents
│   │           ├── research/
│   │           ├── design/               # decisions/, diagrams/
│   │           ├── TASKS.md              # Phase-aligned tasks (optional)
│   │           ├── AGENTS.md             # ONLY if the system has custom conventions
│   │           ├── workstreams/          # Optional: nested workstreams owned by this system
│   │           │   └── <workstream>/     # Same structure as project-level workstreams
│   │           └── systems/              # Sub-systems (same structure)
│   │               └── <sub-system>/
│   │                   ├── INDEX.md      # System entry file (parent: <parent-system>)
│   │                   └── ...
│   ├── workstreams/
│   │   └── (workstream folders emerge over time)
│   │       └── <workstream>/
│   │           ├── INDEX.md              # Purpose + links
│   │           ├── TASKS.md              # Simple checklist (Open / In Progress / Done)
│   │           ├── NOTES.md              # Running narrative
│   │           ├── decisions/            # DR-XXX.md (same format as system DRs)
│   │           ├── research/             # Research docs specific to this workstream
│   │           └── (open-questions.md / risks.md — created on demand)
│   ├── overview/
│   │   ├── TASKS.md           # Global task roll-up — full mirror, maintained inline
│   │   ├── DECISIONS.md       # Index of every Decision Record, maintained inline
│   │   └── (glossary.md, open-questions.md, risks.md, costs.md, system-map.md,
│   │       budget.xlsx — created on demand: first term, question, risk,
│   │       cost, or relationship edge)
│   └── conception/
│       └── (research/ appears with the first pre-system research doc)
├── sessions/
│   ├── CURRENT_STATE.md
│   ├── conflicts/              # Auto-generated decision conflict reports (CONFLICT-XXX.md)
│   └── week-01/
│       └── YYYY-MM-DD.md
```

The framework itself (this skill) is **not** copied in. Projects hold generated
content only.

## Overview roll-ups — maintained inline (core rule)

`project/overview/` holds the whole-project picture one level deeper than the
dashboard. Roll-up files are **mirrors, not separate documents** — their content
derives from local registers and system frontmatter.

**The rule: a roll-up is updated in the same action as its source change.** When
you tick a task, log a risk, raise a question, capture a decision, change a cost,
change a system's phase, or relate two systems — update the local file *and* its
roll-up before moving on. Never defer roll-up maintenance to "later" or
end-of-session; deferred maintenance is where drift is born.

Local registers and system files are the **source of truth**; roll-ups are
redundant views. `audit` verifies roll-ups against their sources and flags
drift; it may offer to regenerate a badly drifted roll-up (with the user's
approval) as a repair. The same rule applies to `INDEX.md` files: add the link
when you add the artifact; `orchestrator` can regenerate any INDEX on request.

| Roll-up | Mirrors |
|---------|---------|
| `overview/TASKS.md` | every system and workstream `TASKS.md` (full mirror) |
| `overview/DECISIONS.md` | every Decision Record across systems and workstreams |
| `overview/open-questions.md` | cross-system questions + index of significant per-owner OQs |
| `overview/risks.md` | project-wide risks + index of high-severity per-owner risks |
| `overview/costs.md` | project-wide cost register / budget (rolls up per-system registers) |
| `overview/system-map.md` | top-level system relationships (edges from all systems) |
| `DASHBOARD.md` rows | system/workstream `INDEX.md` frontmatter |

`project/conception/` is the **pre-system space**: exploration that exists
before any system or workstream owns it. Research here is expected to migrate
into a system's (or workstream's) `research/` as the project takes shape — an
empty conception folder later in the project is a sign of health, not neglect.

Only `overview/TASKS.md` and `overview/DECISIONS.md` exist from day one. The
other overview registers are **created on demand** — first shared term
(glossary), first cross-cutting question, first risk, first cost, first
relationship edge (system-map). Empty registers are noise, and noise is what
makes a planning repo unreadable.

## Obsidian

Atlas project wikis are designed to shine in Obsidian: standard relative links
(not `[[wikilinks]]`) render as graph edges, Mermaid blocks render natively, and
YAML frontmatter is parsed. If the user runs Obsidian, point the vault at the
project root. When an `obsidian` companion skill is available (see your host
file), use it to read, search, create, and edit notes directly in the vault —
it understands Obsidian's structure, tags, and frontmatter.

## INDEX.md — the entry file for systems and workstreams

Only **system folders** and **workstream folders** get an `INDEX.md`:
- **System folders**: the system's identity **and** contents in one file.
  Authored frontmatter (`type: system`, `system_type`, `parent`, `status`,
  `phase`) and authored sections (Purpose, Boundaries, Origin, Sub-systems,
  Relationships, External Links), plus a **Contents** section at the end
  (links to artifacts, subfolders, children) — maintained inline as artifacts
  are added, verified by `audit`. The dashboard's system rows derive from this
  frontmatter (updated inline when it changes).
- **Workstream folders**: a lighter entry file (purpose line + task/research/
  decision links). No AGENTS, no phase gates. See `references/workstreams.md`.

**Container folders have no index file** (`project/`, `systems/`, `workstreams/`,
`overview/`, `conception/`, `sessions/`, root): the folder is its own index, and
orientation comes from `DASHBOARD.md` and the root `AGENTS.md`. Do not create
Map-of-Content index files in containers — a bullet list of links to files that
are already visible in the folder listing is permanent maintenance cost for zero
information.

Sub-parts (research files, individual DRs) do not get their own INDEX. When
maintaining or regenerating a system's INDEX, only the **Contents** section is
rewritten — the authored frontmatter and metadata sections are left intact.

## AGENTS.md

The **root** always gets an `AGENTS.md` (house rules + the `atlas: <version>`
stamp + memory pointer). A **system** gets one **only when it has conventions
that genuinely differ from house rules** — don't seed boilerplate copies that
just repeat the core conventions. Subfolders inherit the nearest parent's.
Keep it small (<20 lines). Template:

```markdown
# AGENTS.md — [Folder]

> Governed by Atlas

## Purpose
[What this folder is for]

## Conventions
[Naming/format/structure — or "see Atlas references/conventions.md"]

## What to do here / What NOT to do here
[Typical tasks / guardrails]

## Relevant agents
[Which Atlas agents apply here]
```

Use plain text "Governed by Atlas" (not a link — relative paths break at different
depths). The root `AGENTS.md` may link the skill's `README.md` and must record the
skill version used, e.g. `atlas: 0.1.0`.

## Workstream file structure

Workstreams are **processes you complete**, not things you build. They are
lighter than systems — a light `INDEX.md` (purpose + links, no system metadata),
no AGENTS, no phase gates, no CHANGELOG. See `references/workstreams.md` for the
full lifecycle and when to create one.

Workstreams can live at **project level** (`project/workstreams/<name>/`) or
**nested inside a system** (`project/systems/<system>/workstreams/<name>/`)
when the process belongs to one system. Same structure at both levels.

```
<workstream>/
├── INDEX.md              # Lightweight index (purpose, tasks, research, decisions)
├── TASKS.md              # Simple checklist: Open / In Progress / Done
├── NOTES.md              # Running narrative — findings, summaries, state changes
├── decisions/            # DR-XXX.md (same format as system DRs, numbered per workstream)
├── research/             # Research docs specific to this workstream
├── open-questions.md     # Optional — questions within this workstream's scope
└── risks.md              # Optional — risks within this workstream's scope
```

Workstream artifacts use frontmatter with `type: workstream` and
`status: active | complete | paused`. System-level workstreams also set
`system: <system-name>` in INDEX.md frontmatter so audit can attribute
ownership.

## Frontmatter — quality signals & freshness

Artifacts that carry *claims or judgments* (research docs, DRs, requirements,
comparisons) begin with YAML frontmatter. Workstream artifacts (TASKS, INDEX,
NOTES) also use frontmatter with `type: workstream`:

```yaml
---
title: <artifact title>
created: YYYY-MM-DD
updated: YYYY-MM-DD          # best-effort; git history is the real source of truth
type: research | decision | requirement | comparison | design
status: fresh | stale | superseded
confidence: high | medium | low
contested: true             # only when there are unresolved contradictions
contradicts: [other-slug]
sources: [research/sources/foo.md]
mission_link: <how this traces to mission.md>
---
```

- **`confidence`** — required on research findings and DRs. `low`/`medium` for
  single-source or fast-moving claims; `high` only when corroborated.
- **`contested` / `contradicts`** — never silently overwrite conflicting content.
  Note both positions with dates and let `audit` flag it.
- **`updated` is best-effort, not authoritative.** Git history is the source of
  truth for freshness; never gate anything on the string alone.
- **`mission_link`** — a short note tying the artifact to `mission.md`. If it
  can't be stated, that's a scope-check signal.

**System `INDEX.md`** carries structured frontmatter the dashboard derives from:

```yaml
---
title: <System Name>
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: system
system_type: <domain-pack-defined>   # dashboard "Type" column
parent: none | <parent-system-name>  # dashboard "Parent" column
status: active | pending | not-started | retired
phase: <current phase>               # domain-pack-defined
---
```

**Workstream `INDEX.md`** carries `type: workstream`, `status`, and optional
`system:` (for nested workstreams).

Session logs don't need frontmatter.

## Cross-linking — standard relative markdown links, no orphans

- Use **standard relative links** — `[title](../other/DR-003.md)` — not Obsidian
  `[[wikilinks]]`. They render everywhere and Obsidian still graphs them.
- **Every artifact links to at least 2 others.** `audit` flags zero-inbound
  orphans and broken paths.
- Cross-system references stay fully qualified, e.g.
  `[DR-003](../ai-runtime/design/decisions/DR-003.md)`. For sub-systems, use the
  full path: `van/electrical/DR-001`. For workstream DRs, qualify as
  `workstream-name/DR-001` (project-level) or `system/workstream-name/DR-001`
  (nested), e.g. `[financing/DR-001](../workstreams/financing/decisions/DR-001.md)`,
  `[van/cad-design/DR-001](../systems/van/workstreams/cad-design/decisions/DR-001.md)`.

## Naming & IDs

- Decision Records: `DR-XXX`, numbered **per system** AND **per workstream**
  (each starts at `DR-001`). Qualify across systems: `ai-runtime/DR-003`;
  sub-systems: `van/electrical/DR-001`; project-level workstreams:
  `financing/DR-001`; nested workstreams: `van/cad-design/DR-001`.
- Requirements: `REQ-XXX`. Open questions: `OQ-XXX`. Tasks: `TASK-XXX`.
  Risks: `R-XXX`. All numbered per register/system/workstream; qualify across
  owners when referenced from a roll-up (e.g. `electrical/TASK-002`,
  `financing/TASK-001`).
- Superseded artifacts move to an `_archive/` subfolder; keep the link, mark
  `status: superseded`.

## Open Questions — per-system, per-workstream, and overview index

Each system or workstream may have its own `open-questions.md` for questions
within that scope. The `project/overview/open-questions.md` serves two roles:

1. **Cross-system/workstream questions** that don't belong to any single system
   or workstream.
2. **Index of per-system and per-workstream open questions** — links to blocking
   or significant open questions from each system's or workstream's
   `open-questions.md`, so the full project picture is visible in one place.

When a new question is raised in a system or workstream, log it in that
system's/workstream's `open-questions.md` — and **in the same action**, if it
is blocking or significant, add a link line to the overview index. When a
question is resolved, tick it in the per-system/workstream file and remove it
from the overview index (or move to Resolved with a link), again in the same
action. `audit` (check 6) verifies the overview index against the per-system
and per-workstream files.

## Risks — per-system, per-workstream, and overview index

Each system or workstream may have its own `risks.md` for risks within that
scope. The `project/overview/risks.md` serves two roles:

1. **Project-wide risks** that don't belong to any single system or workstream.
2. **Index of per-system and per-workstream risks** — links to high-severity or
   blocking risks from each system's or workstream's `risks.md`, so the full
   project risk picture is visible in one place. The dashboard's Top Risks roll
   up from both.

When a new risk is identified in a system or workstream, log it in that
system's/workstream's `risks.md` — and **in the same action**, if it is
high-severity or blocking, add a link line to the overview index. When a risk is
closed, update the per-system/workstream file and remove it from the overview
index (or move to Closed with a link). `audit` (check 13) verifies the overview
index against the per-system and per-workstream files.

## Tasks — local registers, global roll-up

Tasks live in local `TASKS.md` files: phase-aligned for systems (template
`templates/system/TASKS.md`), simple checklists for workstreams
(`templates/workstream/TASKS.md`). Local files are the source of truth.

`project/overview/TASKS.md` is a **full mirror** of all of them — every owner
(systems, project-level and nested workstreams), every status — plus a per-owner
summary table. Like every roll-up it is **maintained inline**: ticking a task
locally without mirroring the tick in the same action is a convention violation
`audit` will flag. See `templates/overview/TASKS.md`.

## Decisions — per-owner files, global index

Every decision is a DR file in its owner's decisions folder (see
`references/agents/decision-record.md`). `project/overview/DECISIONS.md` is the
**index of every DR** — Active and Superseded tables, updated inline at capture
time and on any status change (see `templates/overview/DECISIONS.md`). It serves
two jobs: a single browsable decision history, and the **pre-DR conflict scan** —
before writing a new DR, check the Active table for a decision on the same
topic. If the new DR disagrees, either supersede the old one explicitly or stop
and flag the conflict to the user (`audit` check 16 is the backstop, not the
primary detector).

## Costs — per-system registers, project-wide register

Costs live in local `costs.md` registers (optional per system). The
`project/overview/costs.md` is the project-wide register / budget, rolling up
per-system registers and feeding the dashboard's Costs section. Updated inline
when any register changes; `audit` (check 14) verifies.

## System maps — hierarchical

System maps show how systems relate. They follow a hierarchical structure:

- **Top-level** (`project/overview/system-map.md`): shows all top-level systems as
  nodes. Related systems have edges; unrelated neighbours appear as disconnected
  nodes (their presence is informational). Systems with sub-systems are marked
  with `(has sub-systems)` or a subgraph cluster.
- **Per-system** (`<system>/system-map.md`): for systems with sub-systems, shows
  the children and how they relate to each other. Not every system needs one —
  only systems with sub-systems or enough internal relationships to warrant a
  map.

Each system's `INDEX.md` Relationships table remains the local view. Map and
relationship changes are made **inline**: when two systems relate, record the
edge in the map(s) and both systems' INDEX Relationship tables in the same
action. `audit` (check 15) verifies consistency across all levels: top-level
map, per-system maps, and per-system INDEX relationships. When a new sub-system
is created, add it to the parent's `system-map.md` (if one exists) and to the
top-level map as a child of its parent.

## Research — three homes

Research is a folder that can live in three places:

- **Workstream research** (`<workstream>/research/`): research done as part of a
  process you're completing.
- **System research** (`<system>/research/`): research for a specific system's
  design, architecture, or technology choices.
- **Pre-system / whole-project research** (`project/conception/research/`): early
  exploration, technology surveys, domain background that exists before — or
  outside of — any system or workstream. Expect docs to migrate to a system or
  workstream as the project takes shape.

`audit` (check 17) governs placement so `conception/research/` doesn't become a
dumping ground — it flags docs that would be better placed in a specific system
or workstream. See `references/agents/research.md` for the full research agent
spec.

## Git conventions

- **Commit prefixes:** `docs:` (documentation), `dr:` (Decision Records),
  `system:` (system structure), `workstream:` (workstream structure),
  `atlas:` (framework), `domain:` (domain pack).
- Examples:
  - `docs: add research doc for orchestration patterns`
  - `dr: DR-003 chose event-driven architecture for data ingestion`
  - `system: scaffold data-ingestion system`
  - `workstream: scaffold financing workstream`
- **DRs are committed individually** — one commit per DR for clean history.
- **Branching:** work on `main` for solo planning; feature branches when others
  are involved.
- Git is the reconciliation backstop. **Single active writer at a time.**
