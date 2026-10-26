# Host: OpenClaw

How Atlas's generic capabilities map to a self-hosted OpenClaw agent.
Read this file once at the start of a session when running under OpenClaw.
OpenClaw ships first-class memory and scheduling, which suits Atlas well —
verify exact command flags against your deployment's docs.

## Capability map

| Capability | On OpenClaw |
|---|---|
| Loading skill files | Clone the Atlas repo anywhere and point the agent at it via workspace instructions (its `AGENTS.md`/persona files), e.g. "Read the Atlas skill folder at `<path>` and follow it." If your OpenClaw version has a native skills directory, install it there instead — verify layout in your deployment. Bundled files resolve relative to the skill folder |
| Memory pointer | Strong built-in memory: workspace `MEMORY.md` (durable curated facts) plus dated `memory/YYYY-MM-DD.md` notes. Write the pointer into `MEMORY.md` ("Remember that Atlas project '<name>' lives at <abs-path>..."), or keep it in the planning repo's root `AGENTS.md` for project-scoped clarity — either works; `MEMORY.md` is better when one agent serves several projects |
| Scheduled audits | **First-class** — OpenClaw has a built-in scheduler (Automations). Recipe below; it persists the job, wakes the agent, and can deliver output to a chat channel |
| Delegation / sub-agents | OpenClaw supports spawning sub-agents/sessions in recent versions — verify the exact tool name in your deployment; otherwise keep work inline and load files on demand |
| Session search | Supported: memory search over transcripts and notes (with the memory-search capability). Good path for "why did we decide X?" before reading daily logs; falls back to the written record if the capability is absent |
| Companion skills | OpenClaw's community skill ecosystem — verify the install location in your deployment; skills are optional enhancements, never dependencies |

## Scheduled audit via Automations

```bash
openclaw automations add \
  --name atlas-audit \
  --every 1h \
  --workdir /path/to/planning-repo \
  --session isolated \
  --prompt "$(cat .atlas-audit-prompt.txt)" \
  --announce --channel <your-chat-channel>
```

- **The prompt** — the canonical audit prompt from `references/agents/audit.md`
  → "Canonical audit prompt", stored as `.atlas-audit-prompt.txt` in the
  planning repo.
- **`--session isolated`** keeps the audit out of the main conversation
  context. For all schedule kinds (`at`, `every`, `cron`, and event triggers)
  see OpenClaw's Automations docs.
- **Delivery** — `--announce --channel` posts findings to a chat channel the
  user sees; alternatively have the job commit its report to
  `sessions/audits/<date>.md`.
- The audit writes only its own records (conflict reports, state file); grant
  file-write access accordingly.

## Notes

- OpenClaw's tiered memory means the memory pointer genuinely survives: it's in
  the curated tier loaded at session start, not the episodic tier.
- If one OpenClaw agent tends several Atlas projects, keep each project's
  pointer in its own root `AGENTS.md` and a single "Atlas exists at these
  repos" line in `MEMORY.md`.
