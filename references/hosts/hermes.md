# Host: Hermes

How Atlas's generic capabilities map to Hermes tools. Read this file once at the
start of a session when running under Hermes; everything else in the skill
refers to capabilities, not tool names.

## Capability map

| Capability | On Hermes |
|---|---|
| Loading skill files | `skill_view(name='atlas', file_path='references/...')` — path relative to the skill directory (e.g. `~/.hermes/skills/atlas/`). Templates likewise: `skill_view(name='atlas', file_path='templates/mission.md')` |
| Memory pointer | `memory(action="add", target="memory", content="Atlas project '<name>' at <abs-path>. Phase: <phase>. Key focus: <one line>. Active systems: <list>. Active workstreams: <list>.")` — update when phase/focus meaningfully changes |
| Scheduled audits | `cronjob` tool — full recipe below |
| Delegation / sub-agents | `delegate_task`; batch mode via `tasks` for 2-3 parallel independent streams |
| Session search | `session_search(query="...")` — fast path for "why did we decide X?" before reading daily logs |
| Companion skills | `skill_view(name='<skill>')` to check availability / load specs |

## Companion skills (Hermes)

| Skill | When to use |
|-------|-------------|
| **obsidian** | Reading, searching, creating, or editing notes in an Obsidian vault |
| **xlsx** | Living cost spreadsheets with formulas alongside `costs.md` |
| **youtube-content** | Turn YouTube tutorials/reviews into cited research docs |
| **excalidraw** | Spatial diagrams (floor plans, layouts, schematics) |
| **pdf** | Create/fill PDFs for certification forms, permits, insurance docs |

Check which are installed with `skill_view(name='<skill>')` or by asking the
user. They are optional enhancements, never dependencies.

## Scheduled audit via `cronjob`

During onboarding, offer to create a recurring, skill-backed job. Use the
**canonical audit prompt** from `references/agents/audit.md` → "Canonical audit
prompt" as the `prompt` value:

```
cronjob(
  action="create",
  name="atlas-audit",
  schedule="every 1h",
  workdir="<absolute-path-to-planning-repo>",   # REQUIRED — cron runs detached
  skills=["atlas"],
  prompt="<canonical audit prompt — see references/agents/audit.md>",
  deliver="telegram",          # adjust to user's configured gateway — see below
)
```

- **`workdir` is required** — cron runs detached; set it to the planning repo so
  file tools and `AGENTS.md` resolve correctly.
- **Pin provider/model** for unattended runs (an unpinned job fails closed on
  default change), or use `hermes setup --portal`. The gateway must be running.
- **Delivery target** — set `deliver` to a gateway the user has configured
  (e.g. `"telegram"`, `"discord"`, `"all"`). On CLI-only sessions there is no
  live delivery channel — use `deliver="local"` to save results viewable via
  `cronjob action='list'`, or omit `deliver` to save silently. Ask the user
  which gateway they want during onboarding.
- **Conversational audits** — if the user may want to follow up on audit
  findings conversationally, set `attach_to_session=true` so the delivery is
  continuable (the user can reply and the agent has the audit brief in context).

## Delegation patterns (`delegate_task`)

- **Research delegation** — dispatch a subagent with a specific research goal,
  relevant context (file paths, prior findings, constraints), and a request for
  a structured output. The subagent's summary re-enters the main context cleanly.
- **Parallel research** — use `tasks` (batch mode) to dispatch 2-3 independent
  research streams simultaneously (e.g. "research electrics cert", "research
  solar panel options", "research insulation materials" in one batch).
- **Audit delegation** — the audit checklist can be dispatched to a subagent if
  the project is large, keeping the audit's intermediate reads out of context.

**When NOT to delegate:** single tool calls, tasks needing user interaction
(subagents cannot use `clarify`), or quick lookups. Pass all relevant context in
the `context` field — subagents have no memory of your conversation. If the user
works in a non-English language, note it in context so subagents respond in the
right language.
