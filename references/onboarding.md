# Onboarding — starting a new project

Runs when the working directory has **no `project/` folder**. Goal: establish the
mission and generate the initial structure, *without* prematurely declaring
systems.

## Two modes

Ask the user which fits, or infer from scope.

### Quick start (small projects / evaluation)

1. Ask for **project name** + **one-paragraph description**.
2. Ask the **domain** (default `general` if unclear).
3. Generate the minimal structure (below) and **one** starter system folder.
4. Write a first-pass `mission.md` from the description; confirm with the user.
5. Can expand into full onboarding later.

### Full onboarding (complex projects)

Guided interview — **one question at a time**, conversationally. Cover:

- **Idea & mission** — what is this, and why does it matter? What's the goal?
- **Value proposition** — who benefits, and how?
- **Constraints** — budget, deadlines, compliance, tech/material limits, team.
- **Current state** — greenfield or existing? What already exists?
- **Domain** — "what kind of project is this?" Load the matching pack from
  `references/domains/`; fall back to `general/` if no match. If no shipped pack
  fits well, use `general/` for now. A custom pack can be created later once
  enough project context exists (see `references/governance.md`).

Gather enough context to *recommend* systems — **do not create system folders
yet**. Write `mission.md` and the shared scaffold first.

## What to generate (both modes)

Create in the **working directory** from the skill's `templates/` — this is the
**entire day-one footprint**, deliberately small:

```
AGENTS.md                       ← root rules; record `atlas: <version>`
project/DASHBOARD.md            ← templates/dashboard.md (seed headings; Domain: <domain>)
project/mission.md              ← templates/mission.md (fill from interview)
project/overview/TASKS.md       ← templates/overview/TASKS.md (global roll-up; the inline-mirror target)
project/overview/DECISIONS.md   ← templates/overview/DECISIONS.md (decisions index; the pre-DR scan target)
sessions/CURRENT_STATE.md       ← seed with onboarding summary
sessions/week-01/YYYY-MM-DD.md  ← templates/session-log.md; first entry
```

**Everything else emerges** — do NOT create it at onboarding:

- **Container folders** (`project/systems/`, `project/workstreams/`,
  `project/conception/`, `sessions/conflicts/`) appear with their first
  content (first system, first workstream, first pre-system research doc,
  first conflict report). Git doesn't track empty folders anyway — no
  placeholder files, no MOC indexes (see `references/conventions.md` →
  "INDEX.md").
- **Overview registers** (`glossary.md`, `open-questions.md`, `risks.md`,
  `costs.md`, `system-map.md`, `budget.xlsx`) are created on demand: first
  shared term, first cross-cutting question, first risk, first cost, first
  relationship edge. Load the matching template from the skill when that
  moment comes.

Also create the project's own `.gitignore` (ignore `.obsidian/`, `.DS_Store`).

**Stamp the version.** Record the skill version (from `SKILL.md` frontmatter) in
the root `AGENTS.md` and the `DASHBOARD.md` footer (`atlas: <version>`).

## Recommending the first system(s)

When enough context exists (full mode), propose the first system folder(s) and the
rationale. On user confirmation, hand off to `references/system-lifecycle.md` to
scaffold.
Systems **emerge** — expect to add more over time, not all at once. Systems can
also be **hierarchical**: a system may later gain sub-systems (e.g. a `van` system
may spawn `electrical`, `climate`, `insulation` sub-systems). Don't try to predict
the full hierarchy during onboarding; let it emerge as the project develops (see
`references/system-lifecycle.md` → "Hierarchical systems").

## Workstreams emerge as needed

Workstreams (processes you complete — see `references/workstreams.md`) are not
created during onboarding by default. They emerge as the project develops and
processes are identified that need tracking (e.g. "secure financing", "research
vehicle options", "choose a PM tool"). The agent recommends creating a workstream
when a process warrants structured tracking; the user confirms. Don't force
workstream creation during onboarding — let them emerge naturally, like systems.

## Research before system scaffolding

It's normal — and often ideal — to do **general research before any system
exists**. Early research (vehicle selection, site surveys, technology surveys,
market analysis) lives in `project/conception/research/` — the pre-system
exploration space — or in a **workstream's** `research/` folder if the research
is part of a defined process (see `references/workstreams.md`). Don't rush to
scaffold systems just to have a place for research. Systems become worth
scaffolding once a research stream has enough focus to own its own decisions,
design, and tasks.

## Check complementary skills

Atlas works standalone, but companion skills enhance it when available in the
host environment (see the "Complementary skills" table in `SKILL.md`, and your
host file `references/hosts/<host>.md` for what this host supports and how to
check what's installed). For any that are missing, briefly explain what it does
and ask if the user wants to install it. Do not block onboarding if they
decline; the skills are optional enhancements, not dependencies. Record which
complementary skills are available in the root `AGENTS.md` so future sessions
know.

## Set up the audit cadence

If the host supports scheduled/recurring jobs, offer to set up a recurring
`atlas-audit` job (see `references/agents/audit.md` → "Scheduled audit", and
your host file for the concrete recipe). It's optional; manual `audit` works
everywhere.

## Finish

Summarise: mission in a sentence, chosen domain, what was generated, and the
recommended next step. Append a session-log entry
(`references/agents/session-log.md`).

**Save a memory pointer** so future sessions know this project exists even
without the Atlas skill loaded (see `references/session-continuity.md` →
"Memory pointer", and your host file for where to store it — persistent memory
if the host has it, otherwise the root `AGENTS.md`):
```
Atlas project '<name>' at <abs-path>. Phase: plan. Key focus: <one line>.
```
