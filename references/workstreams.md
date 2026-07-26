# Workstreams — processes you complete

A **workstream** is a process you work through and complete — not a thing you
build. It needs tasks, research, decisions, and notes, but it has no phase
gates and no physical artifacts. Workstreams are the companion to systems
(`references/system-lifecycle.md`): **systems are things you build;
workstreams are processes you complete.**

## When to create a workstream

Create one when a process needs structured tracking but doesn't involve
building a physical or software artifact with its own design lifecycle.
Signals:

- A chunk of work has decisions, research, and tasks.
- It's a process to *complete* (e.g. "secure financing", "research vehicle
  options", "choose a project management tool", "get electrical certification").
- It doesn't need phase gates, a rich system INDEX, or AGENTS files.

The `orchestrator` **recommends**; the user **confirms**. Workstreams emerge
as needed — don't pre-create them "just in case".

## Where workstreams live

Workstreams can live at two levels:

- **Project-level** (`project/workstreams/<name>/`): processes not owned by any
  single system (e.g. "secure financing", "choose PM tool", "research vehicle
  options").
- **System-level / nested** (`project/systems/<system>/workstreams/<name>/`):
  processes owned by one system (e.g. "complete 3D CAD design" under the van
  system, "get electrical certification" under the electrical system).

Same structure at both levels. When to nest: the process clearly belongs to
one system. Routine internal work (a few tasks, some research) can stay in the
system's existing `TASKS.md` and `research/` folder — a nested workstream is for
a named process with its own bundle of tasks, research, decisions, and notes.

The agent **recommends** the appropriate level; the user **confirms**.

## Structure

```
<workstream>/
├── INDEX.md              # Lightweight index — purpose, tasks, research, decisions
├── TASKS.md              # Simple checklist (Open / In Progress / Done)
├── NOTES.md              # Running narrative — findings, summaries, state changes
├── decisions/            # DR-XXX.md files (same format as system DRs)
├── research/             # Research docs specific to this workstream
├── open-questions.md     # Optional — questions within this workstream's scope
└── risks.md              # Optional — risks within this workstream's scope
```

Optional files (`open-questions.md`, `risks.md`) reuse the existing templates
from `templates/system/open-questions.md` and `templates/system/risks.md` —
no separate workstream templates needed.

### What workstreams DON'T have

- **Light `INDEX.md`** — a workstream's INDEX is just a purpose line plus links
  (no system metadata: no `system_type`, `phase`, boundaries, or relationships).
- **No `AGENTS.md`** — workstreams inherit project-level rules.
- **No phase gates** — workstreams don't have phases. Tasks move from Open to
  Done; that's it.
- **No `CHANGELOG.md`** — NOTES.md serves the same purpose in a lighter form.

## Decision Records

Workstreams get their own `decisions/` folder. DRs use the **same format** as
system DRs (`templates/decision-record.md`). Numbered **per workstream**,
starting at `DR-001`. Qualify across the project: `financing/DR-001`,
`base-vehicle/DR-002` (project-level); `van/cad-design/DR-001` (nested).

See `references/agents/decision-record.md` for the DR format and capture rules.

## Lifecycle

```
created → active → complete
                      ↓
                 (may spawn a system)
```

1. **Created** — agent recommends, user confirms. Scaffold from
   `templates/workstream/`.
2. **Active** — tasks progress, research accumulates, decisions captured.
3. **Complete** — all tasks done, decisions recorded, outcome documented in
   NOTES.md. Set frontmatter `status: complete`.

Not all workstreams lead to systems. "Decide PM tool" completes with a DR and
that's it — the outcome is a decision, not an artifact to build.

## Workstream → System transitions

When one or more completed workstreams produce research that references
building something physical (or a software system), the agent should
**recommend** creating a system. The user confirms. Systems are **never
auto-created**.

### Many-to-one spawning

Multiple workstreams can feed into a single system. Example:

```
financing (complete) ─┐
van-research (complete) ─┼──→ base-vehicle (system)
build-workshop (complete) ─┘
```

When recommending a system, the agent should cite which completed workstreams
informed it and link their research. Record the provenance in the new system's
`INDEX.md` ("Origin / informed by" section). The new system's `research/`
folder can reference or migrate relevant docs from the workstreams.

### Not all workstreams lead to systems

Some workstreams complete with just a decision (e.g. "decide PM tool" → DR-001:
chose Notion). No system is needed. The workstream is done.

## Relationship to shared/

Workstreams participate in the same shared registers as systems:

- **Open questions** — workstream OQs are linked to
  `project/shared/open-questions.md` (the cross-system index), same as system
  OQs. Blocking or significant workstream questions appear in the shared index.
  See `references/conventions.md` → "Open Questions".
- **Risks** — workstream risks are linked to `project/shared/risks.md`, same
  as system risks. High-severity or blocking workstream risks appear in the
  shared index. See `references/conventions.md` → "Risks".
- **Provenance** — when a completed workstream feeds into a new system, record
  the link in that system's `INDEX.md` ("Origin / informed by" section), not
  on the system map. See "Workstream → System transitions" below.

## Research

Research is a folder that can live in three places: a workstream's
`research/`, a system's `research/`, or `project/shared/research/` (general /
cross-cutting). See `references/agents/research.md` for the full guide. Audit
(check 17) flags misplaced research docs.

## Workstreams vs systems — quick reference

| | Systems | Workstreams |
|---|---|---|
| **Nature** | Things you build | Processes you complete |
| **Phase gates** | Yes | No |
| **INDEX.md** | Rich (identity + metadata + contents) | Light (purpose + links) |
| **AGENTS.md** | Yes | No |
| **CHANGELOG.md** | Yes | No |
| **NOTES.md** | No | Yes |
| **DRs** | `design/decisions/` | `decisions/` |
| **Research** | `research/` | `research/` |
| **Tasks** | `TASKS.md` (phase-aligned) | `TASKS.md` (simple checklist) |
| **Lifecycle** | phases: research → design → build → ... | created → active → complete |
| **May spawn** | Sub-systems | Systems (optional) |
