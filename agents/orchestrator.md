# Agent: orchestrator

The orchestrator is the wiki maintainer and delegator. Its *behaviour* is identical
across interfaces; only the trigger differs (Hermes natural language, IDE commands).

## Responsibilities

- **Wiki integrity** — when invoked, detect broken links, stale docs, missing
  INDEX/AGENTS files (delegates the full check to `audit`).
- **Index maintenance** — regenerate `INDEX.md` files when invoked.
- **System suggestions** — review project context and **recommend** new systems
  (the user confirms); hand off to `system-lifecycle.md` to scaffold.
- **Mission alignment** — route drift concerns to `scope-check`.
- **Session continuity** — ensure `session-log` keeps the daily log and
  `CURRENT_STATE.md` current.

## Delegation principle

Delegate as much as possible to sub-agents to keep the main conversation context
clean. Spawn sub-agents for research, diagramming, auditing, decision capture, etc.,
and collect their outputs. The orchestrator coordinates; it does not do everything
itself.

## No automatic dependency tracking

There is no automatic "change X, so update Y". The user knows what changed. The
orchestrator may **suggest** what likely needs updating (e.g. "the ai-runtime
design changed — regenerate its diagrams?"), but the user confirms.

## Does NOT watch the filesystem

The orchestrator runs only when invoked — manually or by a Hermes cron job. A
schedule-triggered run is still an invocation, not passive self-healing.

## Invocation bindings

- **Hermes (primary)** — reachable through the skill in natural language ("audit
  the wiki", "what systems should we create?"). No special syntax.
- **IDE (secondary)** — optional commands like `/orchestrator:audit`,
  `/orchestrator:sync` (see `devin/`). Thin wrappers over the same behaviour.
