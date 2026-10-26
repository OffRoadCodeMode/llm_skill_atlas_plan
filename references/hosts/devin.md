# Host: Devin CLI / Windsurf

How Atlas's generic capabilities map to Devin (CLI or Windsurf IDE extension).
Read this file once at the start of a session when running under Devin;
everything else in the skill refers to capabilities, not tool names.

## Capability map

| Capability | On Devin |
|---|---|
| Loading skill files | The `read` tool. When the skill is invoked, its install directory is reported as the skill's base directory — resolve `references/...` and `templates/...` paths against it. If running from a plain clone (not installed), resolve paths against the clone root |
| Memory pointer | No persistent-memory tool — keep the pointer in the planning repo's root `AGENTS.md` (agents read it first). Format: `Atlas project '<name>'. Phase: <phase>. Key focus: <one line>. Active systems: <list>. Active workstreams: <list>.` |
| Scheduled audits | No built-in agent cron — use a CI scheduled pipeline (GitLab/GitHub cron) or system cron invoking headless Devin. Recipe below |
| Delegation / sub-agents | `run_subagent` tool (`subagent_explore` read-only, `subagent_general` full access); background subagents for parallel streams |
| Session search | Not available — rely on `sessions/CURRENT_STATE.md` and daily logs (this is why the written record matters) |
| Companion skills | Skills installed in `~/.config/devin/skills/`, `.devin/skills/`, or `.windsurf/skills/` are listed in the session's available skills; invoke with the `skill` tool or `/name` |

## Companion skills

Devin ships few companion skills by default. Useful ones to install alongside
Atlas (or equivalents the user already has):

| Capability | Notes / fallback |
|------------|------------------|
| Obsidian vault access | Not needed — the wiki is plain markdown and Devin reads the planning repo directly. An Obsidian skill only adds vault-native conveniences |
| Spreadsheets (.xlsx) | Living cost spreadsheets alongside `costs.md`. Fallback: markdown tables in `costs.md` |
| YouTube transcripts | Turn tutorials/reviews into cited research docs. Fallback: `webfetch` on a transcript page |
| Excalidraw | Spatial diagrams (floor plans, layouts). Fallback: Mermaid, or an ASCII sketch in markdown |
| PDF tooling | Create/fill PDFs for formal documents. Fallback: ask the user to handle PDFs manually |

Check the session's available-skills list; ask the user before assuming any are
installed. They are optional enhancements, never dependencies.

## Scheduled audit (CI / system cron)

Devin has no built-in scheduler, so run the audit on infrastructure you control.
Non-interactive runs use `--print` / `-p` (there is no `--workdir` flag — `cd`
into the planning repo first).

**CI scheduled pipeline** (recommended — repo already lives there):

```yaml
# e.g. .gitlab-ci.yml or GitHub Actions — nightly audit
atlas-audit:
  script:
    - cd "$CI_PROJECT_DIR"
    - devin -p "$(cat .atlas-audit-prompt.txt)"
        --permission-mode accept-edits
        --respect-workspace-trust false
  rules:
    - if: $CI_PIPELINE_SOURCE == "schedule"
```

**System cron** (local dev machine):

```cron
0 * * * * cd /path/to/planning-repo && devin -p "$(cat .atlas-audit-prompt.txt)" --permission-mode accept-edits --respect-workspace-trust false
```

- **The prompt** — use the canonical audit prompt from
  `references/agents/audit.md` → "Canonical audit prompt". Storing it in a file
  (e.g. `.atlas-audit-prompt.txt` in the planning repo) keeps the scheduler
  entry readable; an inline string works too.
- **`--respect-workspace-trust false` is required** in scripts/CI — `--print`
  mode cannot show the workspace trust prompt and will otherwise fail.
- **`--permission-mode`** — the audit is read-only except for its own records
  (conflict reports in `sessions/conflicts/`, the `.atlas-audit-state` file).
  Unattended runs therefore need minimal write access: `accept-edits` is the
  least-privilege option that still lets it write those; use the default
  `normal` mode (report-only) if you prefer to review conflict files manually.
- **Delivery:** have the job write its report to `sessions/audits/<date>.md`
  and commit it; the user sees it next session. If the CI supports chat
  notifications, wire those in too — ask the user what they prefer during
  onboarding.
- Unattended runs need credentials/model configured in that environment
  (`devin auth`, optionally `--model`); verify a manual `-p` run works before
  scheduling.

## Delegation patterns (`run_subagent`)

- **Research delegation** — dispatch a `subagent_explore` with a specific goal,
  relevant context (file paths, prior findings, constraints), and a request for
  structured output. Its summary re-enters the main context cleanly.
- **Parallel research** — launch 2-3 independent research streams as background
  subagents in parallel (`is_background=true`), then read results as they
  complete.
- **Audit delegation** — dispatch the audit checklist to a subagent if the
  project is large, keeping intermediate reads out of the main context.

**When NOT to delegate:** single tool calls, tasks needing user interaction
(background subagents cannot prompt the user), or quick lookups. Front-load all
context in the delegation prompt — subagents are stateless and cannot see your
conversation.
