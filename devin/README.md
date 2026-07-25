# Devin / IDE bindings (optional)

Thin, interface-specific wrappers over the **generic** agent specs in `../agents/`.
The generic specs are the source of truth; bindings only change how an agent is
*invoked*, never its behaviour.

- `skills/` — `SKILL.md` files exposing agents as IDE slash-commands
  (e.g. `/orchestrator:audit`, `/orchestrator:sync`).
- `agents/` — deeper agent profiles (e.g. `orchestrator/AGENT.md`) for maintenance
  sessions.

**Status:** placeholder. Not required for Hermes (primary) or for pointing any IDE
LLM at the skill folder. Populate these only if/when you want first-class IDE
command bindings — see principle *prove before scaling*.
