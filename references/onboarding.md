# Onboarding — starting a new project

Runs when the working directory has **no `project/` folder**. Goal: establish the
mission and generate the initial structure, *without* prematurely declaring
systems.

## Session-one intro (runs once, before any questions)

The user has never seen Atlas work. Before the first interview question, deliver
a short orientation so their answers land in the right vocabulary. Keep it to
one message — roughly four short paragraphs plus one tree — then the interview
begins (one question at a time). Adapt the wording; cover these beats:

1. **Welcome + what this is.** *"Welcome to Atlas, the plan-anything framework.
   We'll start with a few questions to define your project's mission statement —
   the anchor everything else traces back to."*
2. **The nouns.** **Mission** (the goal; every file must trace to it).
   **Systems**: things you build — software, a web platform, a physical product,
   a room conversion. Each system moves through phases (typically research →
   design → build/execute, from the project's domain pack) with exit gates that
   need named evidence. Systems are not declared upfront: some are obvious on
   day one, others emerge over time — the agent recommends, the user confirms.
   Systems can nest (a `van` system can spawn `electrical`, `climate`).
   **Workstreams**: processes you finish rather than build — sourcing, research,
   certification, choosing a tool — with tasks, notes, and decisions but no
   phase gates. **Decisions** are captured as Decision Records the moment
   they're made, indexed, and checked for conflicts with earlier decisions.
3. **What keeps it alive.** A compact state file + daily logs mean a new session
   resumes in three file reads. Global roll-ups (`overview/TASKS.md`,
   `overview/DECISIONS.md`) stay current because they're updated in the same
   action as any change. An optional scheduled audit verifies integrity and
   reports — it never rewrites content without approval.
4. **What a mature project looks like** — show this generic tree (placeholder
   identifiers, nothing project-specific):

   ```
   planning/
   ├── AGENTS.md                  # house rules for the agent
   ├── project/
   │   ├── mission.md             # the anchor; confirmed on day one
   │   ├── DASHBOARD.md           # scoreboard: phase/status per system, milestones
   │   ├── systems/
   │   │   ├── system-a/          # in design: research/, design/decisions/, TASKS.md
   │   │   └── system-b/          # still in research; lighter so far
   │   ├── workstreams/
   │   │   └── workstream-a/      # a process being completed: tasks, decisions, notes
   │   ├── overview/              # always-current roll-ups: all tasks, all decisions,
   │   │                          # open questions, costs, risks
   │   └── conception/research/   # exploration that predates any system
   └── sessions/                  # CURRENT_STATE.md + dated logs
   ```

5. **The ask.** End with the first interview question, e.g. *"So — tell me about
   your project. What's the mission, and who's it for?"*

Frame expectations honestly: the day-one structure is about 7 files and the tree
above is what it *grows into*; nothing is created before it's real.

## Location & version control (confirm *before* generating anything)

Never write into a folder without confirming placement and git first. Ask both
as a single question during the interview:

1. **Where the planning repo lives.** Default: the working directory. Offer
   alternatives — a new subfolder (e.g. `planning/`) or another absolute path.
   **If the chosen folder already contains a code project or an unrelated git
   repo, recommend a separate planning folder** — the planning repo is the
   project's map, not its source tree — unless the user explicitly wants them
   together. (Plan↔code crossing references come later; see the domain pack.)
2. **Git.** Recommended: it's the reconciliation backstop ("single active
   writer"), powers the audit's incremental diff, and is the real source of
   freshness truth. If yes and the folder isn't a repo: `git init` and make the
   onboarding commit. If the folder is already inside a git repo: reuse it,
   don't re-init, follow the commit-prefix conventions. If the user declines
   git (e.g. a plain Obsidian vault): that works — record `version control:
   none` in the root AGENTS.md so future sessions know; note that audits fall
   back to full read-and-compare passes and freshness is tracked via the
   frontmatter `updated:` field instead of git history.

Only then generate the structure. The location is then fixed for the project's
life; the memory pointer and (if scheduled) the audit job record the path.

## Two modes

Ask the user which fits, or infer from scope.

### Quick start (small projects / evaluation)

1. Ask for **project name** + **one-paragraph description**.
2. Ask the **domain** (default `general` if unclear).
3. Confirm **location & git** (see above).
4. Generate the minimal structure (below) and **one** starter system folder.
5. Write a first-pass `mission.md` from the description; confirm with the user.
6. Can expand into full onboarding later.

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

**Show the result — don't just say it happened.** The user's first impression of
Atlas is this response; concrete output is the trust signal. Present:

1. **The generated structure as a tree**, with a one-line purpose per
   file/folder (the day-one footprint from "What to generate").
2. **The mission file content** (in full if short, else the key sections) —
   mission.md is the anchor; ask the user to confirm or adjust it before
   anything else is built on top.
3. **What was deliberately NOT created**: no empty registers, no system folders
   yet — state that systems and registers emerge as the project develops, so
   the repo never fills with placeholder files.
4. **The recommended next step**: typically early research (into
   `conception/research/`) or proposing the first system from the interview
   context.

Then append a session-log entry
(`references/agents/session-log.md`).

**Save a memory pointer** so future sessions know this project exists even
without the Atlas skill loaded (see `references/session-continuity.md` →
"Memory pointer", and your host file for where to store it — persistent memory
if the host has it, otherwise the root `AGENTS.md`):
```
Atlas project '<name>' at <abs-path>. Phase: plan. Key focus: <one line>.
```
