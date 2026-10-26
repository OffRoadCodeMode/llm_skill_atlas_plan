# Atlas: a planning framework, delivered as a portable LLM skill

**Atlas** turns a plain folder into a **goal-directed planning wiki**: it onboards
a project, defines its mission, lets *systems* emerge as you understand the work,
moves them through phase gates with evidence, captures decisions, logs sessions
for continuity, and audits itself for integrity. It works for **any project**
(software, a business launch, a research paper, a van conversion) via swappable
**domain packs**.

**Systems** are things you build (with phases, design, artifacts). **Workstreams**
are processes you complete (with tasks, research, decisions, notes, but no phase
gates). Workstreams can be project-level or nested inside a system. See
`references/workstreams.md`.

Output is **portable markdown**: standard relative links, Mermaid diagrams, YAML
frontmatter. It renders well everywhere and shines in Obsidian, but requires no
particular viewer.

> Atlas descends from the **LLM Wiki** lineage (Karpathy; Nous `llm-wiki`;
> `llm-wiki-compiler`) but diverges: it is *forward-looking* and organised by
> `system × phase`, where everything traces to `mission.md`. It records **choices
> and blocking unknowns**, not just facts.

## Install

Atlas is a **host-agnostic skill**: a `SKILL.md` plus reference/template files.
It works with any LLM agent that can read files — host-specific tooling
(Hermes, Devin CLI/Windsurf, anything else) is adapted via
`references/hosts/<host>.md`. Install it once into your agent's skills directory
and share it across all your projects. Do **not** copy it into each project.

```bash
# Hermes
git clone <this-repo> ~/.hermes/skills/atlas
# (or just ask Hermes to install the skill from this repo URL)

# Devin CLI / Windsurf (global, all projects)
git clone <this-repo> ~/.config/devin/skills/atlas

# Devin CLI / Windsurf (single project, committed with the repo)
git clone <this-repo> <project>/.devin/skills/atlas
```

**Any other agent / IDE:** clone anywhere and tell your LLM:
*"Read the Atlas skill folder at `<path>` and follow it."*
Skill file paths in `SKILL.md` are relative to the skill's install directory.

## Use

1. Open (or create) the planning repo you want to work in; it can be empty.
2. Invoke the skill (`/atlas` where slash commands are supported), ask your agent
   to plan your project (the skill loads on matching intent), or point your
   IDE's LLM at the skill folder.
3. Atlas loads its **host file** (`references/hosts/<host>.md`) to learn what
   tools this environment offers, then reads `SKILL.md` →
   `references/conventions.md` → `references/onboarding.md`, and **onboards**
   (no `project/` yet) or **resumes** (reads `DASHBOARD.md`, `CURRENT_STATE.md`,
   `mission.md`).

Atlas **generates** the `project/` and `sessions/` structure at runtime from the
templates in this skill. Your planning repo contains only that generated content,
never a copy of Atlas.

## What it outputs

Atlas writes plain markdown into your planning repo — and keeps it **sparse**:
day one is ~7 files, everything else emerges on demand. A software project a
little way into planning might look like this:

```
my-project-planning/
├── AGENTS.md                     # Root rules (records the Atlas version used)
├── project/
│   ├── DASHBOARD.md              # Scoreboard: phase per system, workstreams, next milestones
│   ├── mission.md                # The anchor: everything traces back here
│   ├── systems/
│   │   ├── your-system-1/
│   │   │   ├── INDEX.md           # System entry file: identity (type, purpose, phase) + contents
│   │   │   ├── open-questions.md  # Optional register, created on demand
│   │   │   ├── risks.md           # Optional register, created on demand
│   │   │   ├── costs.md           # Optional register, created on demand
│   │   │   ├── research/
│   │   │   │   └── orchestration-patterns.md
│   │   │   ├── design/
│   │   │   │   ├── decisions/
│   │   │   │   │   └── DR-001.md   # A Decision Record with context + consequences
│   │   │   │   └── diagrams/
│   │   │   │       └── architecture.md   # Mermaid diagram
│   │   │   ├── TASKS.md
│   │   │   └── workstreams/        # Optional: nested workstreams owned by this system
│   │   │       └── cad-design/
│   │   │           ├── INDEX.md
│   │   │           └── TASKS.md
│   │   └── your-system-2/         # A second system, lighter so far
│   │       ├── INDEX.md
│   │       └── research/
│   │           └── approach-options.md
│   ├── workstreams/               # Project-level workstreams (processes to complete)
│   │   └── financing/
│   │       ├── INDEX.md
│   │       ├── TASKS.md
│   │       └── decisions/
│   │           └── DR-001.md
│   ├── overview/                  # Cross-project roll-ups — maintained inline, verified by audit
│   │   ├── TASKS.md               # Global task roll-up: full mirror of every system/workstream TASKS.md
│   │   ├── DECISIONS.md           # Index of every Decision Record (+ pre-DR conflict scan)
│   │   ├── open-questions.md      #   …created on demand: cross-system questions + per-system index
│   │   ├── risks.md               #   …created on demand: project-wide risks + per-system index
│   │   ├── costs.md               #   …created on demand: project-wide cost register / budget
│   │   ├── glossary.md            #   …created on demand: shared terminology
│   │   └── system-map.md          #   …created on demand: Mermaid graph + relationship register
│   └── conception/                # Pre-system space
│       └── research/              # Early / whole-project exploration, before systems exist
└── sessions/
    ├── CURRENT_STATE.md           # Rolling "where are we now" for cold starts
    ├── conflicts/                 # Auto-generated decision conflict reports (CONFLICT-XXX.md)
    └── week-01/
        └── 2026-07-25.md          # Incremental daily log
```

Notes on the output:

- **Day one is ~7 files** (`AGENTS.md`, `DASHBOARD.md`, `mission.md`,
  `overview/TASKS.md`, `overview/DECISIONS.md`, `CURRENT_STATE.md`, one session
  log). There are **no placeholder files** — no empty register templates, no
  map-of-content indexes; container folders appear with their first content.
- **Systems are folders that emerge**, each tracking its own phase and decisions.
  A non-software project would have different systems (e.g. `electrical`,
  `plumbing` for a van conversion) driven by its **domain pack**.
- **Only what's needed exists.** Subfolders like `research/`, `design/`, and
  registers like `risks.md`/`costs.md` appear when a system actually needs them;
  overview registers appear with the first term, question, risk, cost, or edge.
- **Systems can relate.** `overview/system-map.md` holds a Mermaid graph +
  register of how systems connect (APIs, event streams, shared data; or power,
  physical adjacency for a physical build), and each system mirrors its own
  edges. So you can answer "if I change X, what's affected?"
- **Roll-ups are maintained inline, not regenerated.** Any change to a local
  register (task tick, risk, decision, cost) is mirrored into its `overview/`
  roll-up in the same action — so the whole-project picture is always current.
  `audit` verifies the mirrors and reports drift; it never rewrites content.
- **Everything cross-links** with standard relative markdown links, so it browses
  cleanly in GitHub, VS Code, Obsidian, or any markdown viewer.

## What's in the skill

```
SKILL.md              Machine entry point (router + activation prompt)
references/           All support files (guides, agents, domain packs, changelog)
  hosts/              Host adapters (hermes, devin, generic) — tool mappings per environment
  conventions.md      INDEX/AGENTS/frontmatter/links/git rules
  onboarding.md       Quick-start and full onboarding flows
  system-lifecycle.md How systems are created, evolved, split, retired
  session-continuity.md Session logging, cold-start, weekly roll-up
  governance.md       Rules for modifying Atlas itself
  changelog.md        Framework change log
  agents/             Generic agents (orchestrator, session-log, decision-record, audit, ...)
  domains/            Pluggable domain packs (software, general)
templates/            Generic document + system-folder templates
```

## Status

`v1.0.0`. Built minimal-but-complete (principle: *prove before scaling*). The
critical agents (`session-log`, `decision-record`, `audit`, `diagram`) are fully
specified; others are lighter and grow as pain is felt.

## Contributing

**Domain packs** and **host adapters** are the two extension points:

- A domain pack tailors Atlas to a kind of project (phases, system types,
  templates) — add it under `references/domains/<name>/`.
- A host adapter teaches Atlas one agent environment's tools — add it under
  `references/hosts/<host>.md` and register it in the `SKILL.md` host table.

If either could help others, submit a PR. See `references/governance.md` for
both processes.

## License

MIT. See [LICENSE](LICENSE).
