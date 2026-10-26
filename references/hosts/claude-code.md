# Host: Claude Code

How Atlas's generic capabilities map to Anthropic's Claude Code (CLI/desktop).
Read this file once at the start of a session when running under Claude Code.
Written from Claude Code's published docs — verify flags in your local version
when the recipe matters.

## Capability map

| Capability | On Claude Code |
|---|---|
| Loading skill files | Skills are auto-discovered from `~/.claude/skills/atlas/` (personal, all projects) or `.claude/skills/` (project). The description triggers automatically, or invoke `/atlas` directly. Bundled files (`references/...`, `templates/...`) resolve relative to the skill folder — read them with the file tools from the skill's install directory |
| Memory pointer | No project-agnostic store the user controls — keep the pointer in the planning repo's root `AGENTS.md` (Claude Code reads `AGENTS.md` in place of `CLAUDE.md`). Claude's *auto memory* (`~/.claude/projects/<repo>/memory/`) is agent-managed and may also absorb the pointer, but AGENTS.md is the durable, user-owned spot |
| Scheduled audits | No built-in scheduler — use CI schedules or system cron invoking headless Claude Code. Recipe below |
| Delegation / sub-agents | Built-in subagents (Task delegation), custom subagent definitions in `.claude/agents/`; parallel background delegation supported |
| Session search | Session resume/continue exists for picking up a specific past session, but there is no full-text search tool across past sessions — rely on `sessions/CURRENT_STATE.md` and daily logs |
| Companion skills | Any skill in `~/.claude/skills/` or `.claude/skills/`; list those directories (or ask the user) to check what's installed. Optional enhancements, never dependencies |

## Scheduled audit (CI / system cron)

Claude Code runs headless with `claude -p "<prompt>"`. Non-interactive runs
need authentication configured in that environment — verify a manual `-p` run
works before scheduling.

**CI scheduled pipeline** (recommended — repo already lives there):

```yaml
# e.g. GitHub Actions / GitLab CI — nightly audit
atlas-audit:
  script:
    - cd "$CI_PROJECT_DIR"
    - claude -p "$(cat .atlas-audit-prompt.txt)"
  rules:
    - if: scheduled
```

**System cron:**

```cron
0 * * * * cd /path/to/planning-repo && claude -p "$(cat .atlas-audit-prompt.txt)"
```

- **The prompt** — the canonical audit prompt from `references/agents/audit.md`
  → "Canonical audit prompt", stored in the repo as `.atlas-audit-prompt.txt`.
- **Permissions** — the audit writes only its own records (conflict reports,
  state file). Give headless runs the minimal permission mode that allows file
  edits; use read-only/report-only if you prefer manual review.
- **Delivery** — have the job write its report to `sessions/audits/<date>.md`
  and commit it; the user sees it next session.

## Delegation patterns

- **Research delegation** — dispatch a subagent with a specific goal and file
  paths; its summary re-enters the main context cleanly.
- **Parallel research** — run 2-3 independent research subagents concurrently.
- **Audit delegation** — for large projects, dispatch the audit checklist to a
  subagent to keep intermediate reads out of the main context.

**When NOT to delegate:** single tool calls, tasks needing user interaction, or
quick lookups. Front-load context — subagents don't see the conversation.
