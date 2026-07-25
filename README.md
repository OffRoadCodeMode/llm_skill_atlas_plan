# Atlas: a planning framework, delivered as a Hermes skill

**Atlas** turns a plain folder into a **goal-directed planning wiki**: it onboards
a project, defines its mission, lets *systems* emerge as you understand the work,
moves them through phase gates with evidence, captures decisions, logs sessions
for continuity, and audits itself for integrity. It works for **any project**
(software, a business launch, a research paper, a van conversion) via swappable
**domain packs**.

Output is **portable markdown**: standard relative links, Mermaid diagrams, YAML
frontmatter. It renders well everywhere and shines in Obsidian, but requires no
particular viewer.

> Atlas descends from the **LLM Wiki** lineage (Karpathy; Nous `llm-wiki`;
> `llm-wiki-compiler`) but diverges: it is *forward-looking* and organised by
> `system × phase`, where everything traces to `mission.md`. It records **choices
> and blocking unknowns**, not just facts.

## Install

Atlas is a **Hermes skill** (but could be used in any LLM context). Install it once and share it across all your
projects. Do **not** copy it into each project.

```bash
# Hermes
git clone <this-repo> ~/.hermes/skills/atlas
# (or: npx create-atlas, which clones here and checks for updates)
```

**IDE (secondary):** clone anywhere and tell your LLM:
*"Read the Atlas skill folder at `<path>` and follow it."*

## Use

1. Open (or create) the planning repo you want to work in; it can be empty.
2. Ask Hermes to plan your project (the skill loads on matching intent), or point
   your IDE's LLM at the skill folder.
3. Atlas reads `SKILL.md` → `references/conventions.md` → `references/onboarding.md`,
   then **onboards** (no `project/` yet) or **resumes** (reads `DASHBOARD.md`,
   `CURRENT_STATE.md`, `mission.md`).

Atlas **generates** the `project/` and `sessions/` structure at runtime from the
templates in this skill. Your planning repo contains only that generated content,
never a copy of Atlas.

## What it outputs

Atlas writes plain markdown into your planning repo. A software project a little
way into planning might look like this (systems **emerge** over time, so early on
you will have fewer):

```
my-project-planning/
├── INDEX.md                      # Master map of content
├── AGENTS.md                     # Root rules (records the Atlas version used)
├── project/
│   ├── DASHBOARD.md              # Quick-scan status: phase per system, risks, costs, next steps
│   ├── mission.md                # The anchor: everything traces back here
│   ├── systems/
│   │   ├── INDEX.md
│   │   ├── your-system-1/
│   │   │   ├── OVERVIEW.md        # Type, purpose, boundaries, current phase
│   │   │   ├── AGENTS.md
│   │   │   ├── CHANGELOG.md
│   │   │   ├── open-questions.md
│   │   │   ├── DECISIONS.md       # Small decisions (one line each)
│   │   │   ├── risks.md           # Optional register (rolls up to the dashboard)
│   │   │   ├── costs.md           # Optional register (rolls up to the dashboard)
│   │   │   ├── research/
│   │   │   │   └── orchestration-patterns.md
│   │   │   ├── design/
│   │   │   │   ├── decisions/
│   │   │   │   │   └── DR-001.md   # A Decision Record with context + consequences
│   │   │   │   └── diagrams/
│   │   │   │       └── architecture.md   # Mermaid diagram
│   │   │   └── TASKS.md
│   │   └── your-system-2/         # A second system, lighter so far
│   │       ├── OVERVIEW.md
│   │       ├── AGENTS.md
│   │       ├── open-questions.md
│   │       └── research/
│   │           └── approach-options.md
│   └── shared/                    # Cross-system content
│       ├── glossary.md
│       ├── open-questions.md
│       ├── DECISIONS.md
│       ├── risks.md               # Project-wide risk register
│       ├── costs.md               # Project-wide cost register / budget
│       ├── system-map.md          # How systems relate: Mermaid graph + register
│       └── decisions/             # Project-wide Decision Records (shared/DR-XXX)
└── sessions/
    ├── CURRENT_STATE.md           # Rolling "where are we now" for cold starts
    └── week-01/
        └── 2026-07-25.md          # Incremental daily log
```

Notes on the output:

- **Systems are folders that emerge**, each tracking its own phase and decisions.
  A non-software project would have different systems (e.g. `electrical`,
  `plumbing` for a van conversion) driven by its **domain pack**.
- **Only what's needed exists.** Subfolders like `research/`, `design/`, and
  registers like `risks.md`/`costs.md` appear when a system actually needs them.
- **Systems can relate.** `shared/system-map.md` holds a Mermaid graph + register
  of how systems connect (APIs, event streams, shared data; or power, physical
  adjacency for a physical build), and each system mirrors its own edges. So you
  can answer "if I change X, what's affected?"
- **Everything cross-links** with standard relative markdown links, so it browses
  cleanly in GitHub, VS Code, Obsidian, or any markdown viewer.

## What's in the skill

```
SKILL.md              Machine entry point (router + activation prompt)
references/           All support files (guides, agents, domain packs, changelog)
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

`v0.1.0`, first release. Built minimal-but-complete (principle: *prove before
scaling*). The three critical agents (`session-log`, `decision-record`, `audit`)
are fully specified; others are lighter and grow as pain is felt.

## License

MIT. See [LICENSE](LICENSE).