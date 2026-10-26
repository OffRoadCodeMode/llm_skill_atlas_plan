# Host: Cursor

How Atlas's generic capabilities map to the Cursor IDE (and its CLI).
Read this file once at the start of a session when running under Cursor.
Written from Cursor's published docs — verify flags in your local version when
the recipe matters.

## Capability map

| Capability | On Cursor |
|---|---|
| Loading skill files | Skills are auto-discovered from `.cursor/skills/` or `.agents/skills/` (project) and `~/.cursor/skills/` or `~/.agents/skills/` (user). Type `/atlas` in Agent chat to invoke (attaches to one message). **For planning work, prefer Custom Mode**: pick atlas from the `/` menu and press Option+Enter (Mac) / Alt+Enter (Windows) — the skill stays in context for the whole session, which suits a long-lived planning conversation. Bundled files resolve relative to the skill folder |
| Memory pointer | No persistent cross-repo memory tool — keep the pointer in the planning repo's root `AGENTS.md` (Cursor reads AGENTS.md as an agent rule) |
| Scheduled audits | No built-in scheduler — use CI schedules or system cron invoking the Cursor CLI headless (`cursor-agent`; check `-p`/`--print` support in your version). Mirror the recipe pattern in `devin.md`; verify flags before scheduling |
| Delegation / sub-agents | Built-in Subagents (each runs in its own context); Background Agents for parallel/remote streams |
| Session search | Past chats are not searchable via a tool — rely on `sessions/CURRENT_STATE.md` and daily logs |
| Companion skills | Any skill in the skill directories above; ask the user or list the directories to check what's installed. Optional enhancements, never dependencies |

## Notes

- **Rules vs skills:** Cursor also supports project rules (`.cursor/rules/*.mdc`)
  and `AGENTS.md`. The planning repo's root `AGENTS.md` doubles as a rule
  Cursor loads automatically — one reason the memory pointer lives there.
- **Cloud Agents** don't receive unsynced local skills — if the user plans via
  Cloud Agents, install Atlas at project level (`.cursor/skills/` or
  `.agents/skills/`) instead of user level.

## Scheduled audit

Same shape as other CLI hosts: CI schedule or system cron, headless print
mode, the canonical audit prompt from `references/agents/audit.md` stored as
`.atlas-audit-prompt.txt` in the planning repo, report committed to
`sessions/audits/<date>.md`. Verify the headless flags and auth in your Cursor
version before scheduling.
