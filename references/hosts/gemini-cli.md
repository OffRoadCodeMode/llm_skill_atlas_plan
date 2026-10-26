# Host: Gemini CLI

How Atlas's generic capabilities map to Google's Gemini CLI.
Read this file once at the start of a session when running under Gemini CLI.
Written from Gemini CLI's published docs — verify flags in your local version
when the recipe matters.

## Capability map

| Capability | On Gemini CLI |
|---|---|
| Loading skill files | Skills are discovered from `~/.gemini/skills/` or `~/.agents/skills/` (user) and workspace skill directories. Activation is **consent-gated**: expect a confirmation prompt the first time the `atlas` skill activates; approving it grants the skill folder's assets (references/templates) to the agent. Bundled files resolve relative to the skill folder |
| Memory pointer | Gemini's instructional memory is hierarchical `GEMINI.md` context, with `/memory add <text>` for additions. Write the pointer with `/memory add "Atlas project '<name>' at <abs-path>. Phase: <phase>. Focus: <one line>. Active systems: <list>. Active workstreams: <list>."` — or keep it in the project's GEMINI.md by hand. (If the repo root uses `AGENTS.md` instead, set `context.fileName` accordingly or duplicate the pointer line into `GEMINI.md`) |
| Scheduled audits | No built-in scheduler — use system cron or CI invoking headless Gemini CLI (`gemini -p "<prompt>"`). Recipe below |
| Delegation / sub-agents | No stable documented sub-agent API at time of writing — keep work inline and load files on demand; verify in your version |
| Session search | Checkpoints/`/restore` allow resuming specific sessions; there is no full-text search tool across sessions — rely on `sessions/CURRENT_STATE.md` and daily logs |
| Companion skills | Any skill in `~/.gemini/skills/` or bundled in extensions; `/memory list` and the skill directories show what's installed. Optional enhancements, never dependencies |

## Scheduled audit (system cron / CI)

```cron
0 * * * * cd /path/to/planning-repo && gemini -p "$(cat .atlas-audit-prompt.txt)"
```

- **The prompt** — the canonical audit prompt from `references/agents/audit.md`
  → "Canonical audit prompt", stored as `.atlas-audit-prompt.txt` in the
  planning repo.
- **Consent** — headless runs can't show interactive approvals; configure the
  narrowest auto-approve/file-access policy that allows session starts, or run
  the audit manually instead.
- **Delivery** — report committed to `sessions/audits/<date>.md`; seen next
  session.
- Verify a manual headless run authenticates and succeeds before scheduling.
