# Host: OpenAI Codex CLI

How Atlas's generic capabilities map to OpenAI's Codex CLI.
Read this file once at the start of a session when running under Codex.
Written from OpenAI's published docs — verify flags in your local version when
the recipe matters.

## Capability map

| Capability | On Codex CLI |
|---|---|
| Loading skill files | Skills are discovered from `.agents/skills/<name>/` in the project (Codex also walks up toward the project root) and `$HOME/.agents/skills/` globally. A skill is a `SKILL.md` plus optional `references/`/`scripts/`/`assets/` — Atlas's layout fits directly. Bundled files resolve relative to the skill folder |
| Memory pointer | Codex reads the `AGENTS.md` chain (global `~/.codex/AGENTS.md`, then project files toward the cwd). Keep the pointer in the planning repo's root `AGENTS.md` — it is loaded every run automatically |
| Scheduled audits | No built-in scheduler — use CI schedules or system cron invoking headless Codex (`codex exec "<prompt>"`). Recipe below |
| Delegation / sub-agents | Codex documents subagents as a customization layer; the exact mechanism evolves — verify in your version, otherwise keep work inline and load files on demand |
| Session search | `codex resume` picks up specific past sessions; there is no full-text search tool across sessions — rely on `sessions/CURRENT_STATE.md` and daily logs |
| Companion skills | Any skill in `.agents/skills/` or `$HOME/.agents/skills/` (plugins may bundle more); ask the user or list directories. Optional enhancements, never dependencies |

## Scheduled audit (CI / system cron)

```cron
0 * * * * cd /path/to/planning-repo && codex exec "$(cat .atlas-audit-prompt.txt)"
```

- **The prompt** — the canonical audit prompt from `references/agents/audit.md`
  → "Canonical audit prompt", stored as `.atlas-audit-prompt.txt` in the
  planning repo.
- **Permissions** — the audit writes only its own records (conflict reports,
  state file); choose the least-permissive sandbox/approval mode that allows
  those writes, or run read-only for manual review.
- **Delivery** — report committed to `sessions/audits/<date>.md`; seen next
  session.
- Verify a manual headless run authenticates and succeeds before scheduling.

## Note on AGENTS.md

Codex already treats `AGENTS.md` as first-class project guidance, so Atlas's
root `AGENTS.md` (house rules + version stamp + memory pointer) works without
any adaptation on this host.
