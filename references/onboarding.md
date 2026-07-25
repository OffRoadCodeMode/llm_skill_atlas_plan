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

Create in the **working directory** from the skill's `templates/`:

```
INDEX.md                      ← templates: master MOC
AGENTS.md                     ← root rules; record `atlas: <version>`
project/INDEX.md
project/DASHBOARD.md          ← templates/dashboard.md (seed headings; Domain: <domain>)
project/mission.md            ← templates/mission.md (fill from interview)
project/systems/INDEX.md
project/shared/INDEX.md
project/shared/glossary.md
project/shared/open-questions.md   ← templates/open-questions.md
project/shared/DECISIONS.md        ← templates/decisions.md
project/shared/risks.md            ← templates/risks.md
project/shared/costs.md            ← templates/costs.md
project/shared/system-map.md       ← templates/system-map.md (starts empty; fills as systems relate)
project/shared/decisions/          ← (empty; shared DRs land here)
project/shared/research/           ← (empty)
sessions/INDEX.md
sessions/CURRENT_STATE.md          ← seed with onboarding summary
sessions/week-01/YYYY-MM-DD.md     ← templates/session-log.md; first entry
```

Also create the project's own `.gitignore` (ignore `.obsidian/`, `.DS_Store`).

**Stamp the version.** Record the skill version (from `SKILL.md` frontmatter) in
the root `AGENTS.md` and the `DASHBOARD.md` footer (`atlas: <version>`).

## Recommending the first system(s)

When enough context exists (full mode), propose the first system folder(s) and the
rationale. On user confirmation, hand off to `references/system-lifecycle.md` to
scaffold.
Systems **emerge** — expect to add more over time, not all at once.

## Research before system scaffolding

It's normal — and often ideal — to do **general research before any system
exists**. Early research (vehicle selection, site surveys, technology surveys,
market analysis) lives in `project/shared/research/` and informs which systems
should emerge. Don't rush to scaffold systems just to have a place for research;
`shared/research/` exists precisely for this pre-system phase. Systems become
worth scaffolding once a research stream has enough focus to own its own
decisions, design, and tasks.

## Check complementary skills (Hermes)

Atlas works standalone, but these Hermes skills enhance it when available (see
the "Complementary Hermes skills" table in `SKILL.md`):

| Skill | What it adds |
|-------|-------------|
| **obsidian** | Read, search, and edit notes directly in an Obsidian vault |
| **xlsx** | Living cost spreadsheet with formulas alongside `costs.md` |
| **youtube-content** | Turn YouTube tutorials/reviews into cited research docs |
| **excalidraw** | Spatial diagrams (floor plans, layouts, schematics) |
| **pdf** | Create/fill PDFs for certification forms, permits, insurance docs |

Check which are installed (`skill_view(name='<skill>')` or ask the user). For
any that are missing, briefly explain what it does and ask if the user wants to
install it. Do not block onboarding if they decline; the skills are optional
enhancements, not dependencies. Record which complementary skills are available
in the root `AGENTS.md` so future sessions know.

## Set up the audit cadence (Hermes)

Offer to create the recurring `atlas-audit` cron job (see
`references/agents/audit.md` → "Scheduled audit"). It's optional; manual `audit`
works everywhere.

## Finish

Summarise: mission in a sentence, chosen domain, what was generated, and the
recommended next step. Append a session-log entry
(`references/agents/session-log.md`).

**Save a Hermes memory pointer** so future sessions know this project exists
even without the Atlas skill loaded (see `references/session-continuity.md` →
"Hermes memory pointer"):
```
memory(
  action="add",
  target="memory",
  content="Atlas project '<name>' at <abs-path>. Phase: plan. Key focus: <one line>."
)
```
