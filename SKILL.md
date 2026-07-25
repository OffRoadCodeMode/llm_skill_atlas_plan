---
name: atlas
description: Use when the user asks to plan, manage, or work on any project.
  Activates the Atlas planning framework: onboards projects, manages systems,
  logs sessions, captures decisions, and maintains a structured project wiki.
  Works for any domain (software, business, research, personal builds).
version: 0.3.1
tags: [planning, project-management, wiki, framework]
---

# Atlas: Planning Framework

Atlas is a reusable, domain-agnostic planning framework. It turns a plain folder
into a **goal-directed planning wiki**: mission-anchored, organised by
`system x phase`, and maintained by agents. It is *forward-looking*: it drives
what must be done, not just what is known.

This file is the **router**. It tells you (the agent) what to read and when. Load
bundled files on demand; do not read everything up front.

## How to load skill files in Hermes

Use `skill_view(name='atlas', file_path='references/...')` to load reference
files on demand. The `file_path` is relative to the skill directory. Never try
to read all references upfront — that wastes context. Load only what the router
table below says to load for the current task, and only when that task is active.

Templates are read the same way:
`skill_view(name='atlas', file_path='templates/mission.md')`.

---

## Activation prompt (run this first, every session)

```
You are operating under the Atlas planning framework (installed skill).
Read these skill files in order before doing anything else:
  1. SKILL.md                        (this file: what Atlas is + router)
  2. references/conventions.md       (how files are structured)
  3. references/onboarding.md        (how to onboard this project)
Then check the working directory for an existing project/ folder:
  - If NO  -> begin onboarding: generate the project structure here.
  - If YES -> read project/DASHBOARD.md, sessions/CURRENT_STATE.md,
             and project/mission.md to resume where we left off.
Do not create system folders until onboarding says to.
```

**Working directory vs skill directory.** You *read* instructions, templates, and
agent specs from this installed skill (e.g. `~/.hermes/skills/atlas/`). You
*write* generated project content into the user's **working directory** (their
planning repo). Never write project content into the skill folder.

---

## Operation vocabulary (planning verbs, not wiki verbs)

Speak and act in these verbs, whatever the interface:

- **Onboard**: establish `mission.md` and the initial systems. See `references/onboarding.md`
- **Advance**: move a system through a phase gate, with evidence. See `references/domains/<domain>/phases.md`
- **Decide**: capture a Decision Record or resolve a blocking open question. See `references/agents/decision-record.md`
- **Plan**: break work into tasks / milestones for a system. See `templates/system/TASKS.md`
- **Audit**: integrity + gate + mission-alignment check. See `references/agents/audit.md`
- **Resume**: cold-start orientation at the top of a session. See `references/session-continuity.md`

Research/ingest lives *inside* Onboard and the research phase: a means to unblock
requirements -> design -> build, never the product.

---

## Router: read the right file for the task

| The user wants to... | Read |
|----------------------|------|
| Start a new project / no `project/` exists | `references/onboarding.md` |
| Resume after any gap | `references/session-continuity.md`, then `project/DASHBOARD.md`, `sessions/CURRENT_STATE.md`, `project/mission.md` |
| Understand file/naming/link/git rules | `references/conventions.md` |
| Create, evolve, split, or retire a system | `references/system-lifecycle.md` |
| Track how systems relate (APIs, streams, power, adjacency) | `references/system-lifecycle.md` (section "Relating systems"); `project/shared/system-map.md` |
| Know the phases + exit gates for this project | `references/domains/<active-domain>/phases.md` |
| Log what happened this session | `references/agents/session-log.md` |
| Capture a significant decision | `references/agents/decision-record.md` |
| Check integrity / advance a phase / reconcile drift | `references/agents/audit.md` |
| Maintain indexes, suggest new systems, delegate | `references/agents/orchestrator.md` |
| Modify Atlas itself | `references/governance.md` |

Domain-specific agents/templates live under `references/domains/<domain>/`. The
active domain is chosen during onboarding and recorded in `project/DASHBOARD.md`
(and root `AGENTS.md`). If unset or no match, use `general/`.

---

## Core principles (do not violate)

1. **Everything traces to `mission.md`.** If an artifact can't say why it serves
   the mission, flag it (scope-check).
2. **Systems emerge, they're not declared upfront.** Gather context, then
   *recommend* systems; the user confirms.
3. **Gates need evidence.** A system advances a phase only when every exit
   criterion is satisfied by a named artifact (see `references/domains/<domain>/phases.md`).
4. **Audit on demand, honestly.** Atlas does not passively self-heal; it checks
   and repairs when invoked (manually or by cron).
5. **Portable markdown.** Standard relative links (not `[[wikilinks]]`), Mermaid
   fenced blocks, YAML frontmatter. Render well everywhere, shine in Obsidian.
6. **Single active writer.** Git is the reconciliation backstop.
7. **Delegate to sub-agents** to keep the main context clean.

## Presentation (chat interfaces)

- Summarise file contents; don't dump raw markdown.
- For status/dashboard queries, return a formatted summary.
- For audits, report what was found and what was fixed/proposed.
- Onboard conversationally: one question at a time.

## Complementary Hermes skills

Atlas is self-contained, but these Hermes skills enhance project workflows when
available. Load them with `skill_view(name='...')` when the task calls for it.

| Skill | When to use | Atlas integration |
|-------|-------------|-------------------|
| **obsidian** | Reading, searching, creating, or editing notes in an Obsidian vault | Atlas project wikis are Obsidian-optimised (standard relative links, Mermaid, YAML frontmatter). If the user runs Obsidian, use this skill to interact with the vault directly. |
| **xlsx** | Creating or maintaining cost spreadsheets, budget trackers, comparison tables | Use alongside Atlas cost registers — an `.xlsx` for living budgets/formulas, `costs.md` for the durable record (see `references/agents/research.md` → "Cost research" and `templates/costs.md`). |
| **youtube-content** | Extracting knowledge from YouTube tutorials, reviews, build guides | Research often involves video sources (build tutorials, product reviews, owner walkthroughs). Use this to turn transcripts into cited research-doc material (see `references/agents/research.md` → "Research methodology"). |
| **pdf** | Creating, merging, splitting, or filling PDF documents | Useful for certification forms, vehicle registration paperwork, insurance docs, NSAI standards — any formal document in the project. |

These are recommendations, not dependencies — Atlas works fully without them.
Domain packs may recommend additional skills specific to the domain.

## Version

This skill's version is in the `version:` field of this file's frontmatter
(`0.3.1`). On onboarding, stamp it into the project (root `AGENTS.md` and
`project/DASHBOARD.md` footer, e.g. `atlas: 0.3.1`).
See `references/governance.md` for versioning and maintenance rules.
