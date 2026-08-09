# Host: generic / any other agent

How Atlas's generic capabilities map to an arbitrary LLM host (IDE assistant,
other CLI agents, a plain chat model with file access). Use this when no
specific host file matches. Everything here is a *capability description* —
translate it to whatever your environment provides.

## Capability map

| Capability | Generic guidance |
|---|---|
| Loading skill files | Read files with whatever file-reading tool you have. All paths in `SKILL.md` are relative to this skill's install/clone directory. If you cannot read files at all, ask the user to paste `SKILL.md` and then each referenced file as needed |
| Memory pointer | If the host has persistent memory, store the pointer there. Otherwise (or if unsure) keep it in the planning repo's root `AGENTS.md` — agents conventionally read that file first |
| Scheduled audits | If the host has a scheduler (agent cron, CI schedules, system cron), wire the audit prompt (see below) into it. If not, audits are manual — say so during onboarding and move on |
| Delegation / sub-agents | If the host can spawn sub-agents or parallel tasks, use them for research-heavy and audit work to keep the main context clean. If not, do the work inline but stay disciplined about loading files on demand |
| Session search | If the host can search past conversations, use it for "why did we decide X?" before scanning daily logs. Otherwise the written record (`CURRENT_STATE.md`, daily logs, DRs) is the only memory — keep it accurate |
| Companion skills | If the host supports additional skills/plugins/tools, check for the companion skills listed below. If not supported, skip — they are never dependencies |

## Companion skills (any host)

| Capability | What it adds |
|------------|--------------|
| Obsidian vault access | Read/search/edit notes directly in the user's vault |
| Spreadsheet creation (.xlsx) | Living cost spreadsheets with formulas alongside `costs.md` |
| YouTube transcript extraction | Turn tutorials/reviews into cited research docs |
| Excalidraw | Spatial diagrams (floor plans, layouts, schematics) |
| PDF tooling | Create/fill PDFs for formal documents |

Ask the user which of these are available. Record the answer in the root
`AGENTS.md` so future sessions know.

## The scheduled-audit prompt (portable)

Whatever the scheduler, the audit prompt is the same: use the **canonical audit
prompt** in `references/agents/audit.md` → "Canonical audit prompt" verbatim.
Have the job run in the planning repo's working directory, load this skill, and
write its report somewhere durable (a committed file under `sessions/` is the
portable choice).
