# Atlas — a planning framework, delivered as a Hermes skill

**Atlas** turns a plain folder into a **goal-directed planning wiki**: it onboards
a project, defines its mission, lets *systems* emerge as you understand the work,
moves them through phase gates with evidence, captures decisions, logs sessions
for continuity, and audits itself for integrity. It works for **any project** —
software, a business launch, a research paper, a van conversion — via swappable
**domain packs**.

Output is **portable markdown**: standard relative links, Mermaid diagrams, YAML
frontmatter. It renders well everywhere and shines in Obsidian, but requires no
particular viewer.

> Atlas descends from the **LLM Wiki** lineage (Karpathy; Nous `llm-wiki`;
> `llm-wiki-compiler`) but diverges: it is *forward-looking* and organised by
> `system × phase`, where everything traces to `mission.md`. It records **choices
> and blocking unknowns**, not just facts.

## Install

Atlas is a **Hermes skill** (But could be used in any LLM context) — install it once and share it across all your
projects. Do **not** copy it into each project.

```bash
# Hermes
git clone <this-repo> ~/.hermes/skills/atlas
# (or: npx create-atlas — clones here and checks for updates)
```

**IDE (secondary):** clone anywhere and tell your LLM:
*"Read the Atlas skill folder at `<path>` and follow it."*

## Use

1. Open (or create) the planning repo you want to work in — it can be empty.
2. Ask Hermes to plan your project (the skill loads on matching intent), or point
   your IDE's LLM at the skill folder.
3. Atlas reads `SKILL.md` → `conventions.md` → `onboarding.md`, then **onboards**
   (no `project/` yet) or **resumes** (reads `DASHBOARD.md`, `CURRENT_STATE.md`,
   `mission.md`).

Atlas **generates** the `project/` and `sessions/` structure at runtime from the
templates in this skill. Your planning repo contains only that generated content —
never a copy of Atlas.

## What's in the skill

```
SKILL.md              Machine entry point (router + activation prompt)
conventions.md        INDEX/AGENTS/frontmatter/links/git rules
onboarding.md         Quick-start and full onboarding flows
system-lifecycle.md   How systems are created, evolved, split, retired
session-continuity.md Session logging, cold-start, weekly roll-up
GOVERNANCE.md         Rules for modifying Atlas itself
VERSION / CHANGELOG   Framework version + change log
domains/              Pluggable domain packs (software, general)
agents/               Generic agents (orchestrator, session-log, decision-record, audit, …)
templates/            Generic document + system-folder templates
devin/                Optional IDE-specific bindings (placeholder)
```

## Status

`v0.1.0` — first release. Built minimal-but-complete (principle: *prove before
scaling*). The three critical agents (`session-log`, `decision-record`, `audit`)
are fully specified; others are lighter and grow as pain is felt.

## Maintenance

Atlas is plain markdown under git and is maintained **manually** — edit a file,
commit with the `atlas:` prefix, bump `VERSION`/`CHANGELOG` for convention
changes. There is no autonomous self-editing. See `GOVERNANCE.md`.
