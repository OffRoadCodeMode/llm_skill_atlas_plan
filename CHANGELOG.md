# Changelog — Atlas

All notable changes to the Atlas framework itself. Format: reverse chronological.
Bump `VERSION` (semver) for convention changes; a major bump signals breaking
convention changes.

## 0.1.0 — 2026-07-25

Initial release. Minimal-but-complete framework (*prove before scaling*).

- Router `SKILL.md` with activation prompt and planning-verb vocabulary.
- Core guides: `conventions.md`, `onboarding.md`, `system-lifecycle.md`,
  `session-continuity.md`, `GOVERNANCE.md`.
- Generic agents: `orchestrator`, `session-log`, `decision-record`, `audit`
  (fully specified); `research`, `diagram`, `scope-check` (stubs, grow as needed).
- Domain packs: `software` (SDLC phases, system types, templates, traceability +
  sync-tasks agents) and `general` (plan → research → design → execute → review).
- Templates: generic system-folder scaffold (incl. `risks.md` + `costs.md`) and
  top-level templates (mission, decision-record, decisions, open-questions, risks,
  costs, session-log, dashboard).
- Costs & risks are core canon: optional/emergent registers at project level and
  optionally per-system, rolled up to the dashboard by `audit`.
- Audit hardening: conflict pre-flight halt, git-derived freshness, git-diff
  incremental reconciliation, context-budget cold start + weekly roll-up.
