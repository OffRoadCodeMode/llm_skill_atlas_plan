# Changelog — Atlas

All notable changes to the Atlas framework itself. Format: reverse chronological.
Bump the `version:` field in `SKILL.md` frontmatter (semver) for convention changes;
a major bump signals breaking convention changes.

## 0.2.0 — 2026-07-25

Hermes integration improvements — surfaced from real usage test-driving the
framework on the VanV2 project.

- **SKILL.md**: Added "How to load skill files in Hermes" section with
  `skill_view(file_path=...)` guidance for on-demand reference loading.
- **session-continuity.md**: Added "Hermes memory pointer" — save a compact
  project signpost to Hermes persistent memory so fresh sessions know the project
  exists before the skill loads. Added "Recalling past sessions" — use
  `session_search` as a fast path to conversation context before reading daily
  logs.
- **orchestrator.md**: Added "Hermes delegate_task patterns" — when to delegate
  vs do inline, parallel batch research, audit delegation, and what context to
  pass to subagents.
- **research.md**: Expanded from stub to full agent spec. Added research
  methodology (when to delegate vs inline, search strategy, handling conflicting
  sources, structuring research docs for scannability) and cost research guidance
  (pricing as first-class output, confidence levels on estimates).
- **audit.md**: Fixed cron guidance — delivery target should match user's
  configured gateway (not hardcoded to telegram), documented `deliver="local"`
  for CLI-only sessions, and added `attach_to_session=true` for conversational
  audit follow-up.
- **general/phases.md**: Added "Phases are not strictly linear" section —
  acknowledges iterative/overlapping phases common in physical builds, phase
  regression is normal, research never fully stops.
- **onboarding.md**: Added "Research before system scaffolding" — blesses the
  pattern of doing general research in `shared/research/` before any system
  exists. Added Hermes memory pointer to the finish step.

## 0.1.0 — 2026-07-25

Initial release. Minimal-but-complete framework (*prove before scaling*).

- Router `SKILL.md` with activation prompt and planning-verb vocabulary.
- Core guides: `references/conventions.md`, `references/onboarding.md`,
  `references/system-lifecycle.md`, `references/session-continuity.md`,
  `references/governance.md`.
- Generic agents: `orchestrator`, `session-log`, `decision-record`, `audit`
  (fully specified); `research`, `diagram`, `scope-check` (stubs, grow as needed).
- Domain packs: `software` (SDLC phases, system types, templates, traceability +
  sync-tasks agents) and `general` (plan → research → design → execute → review).
- Templates: generic system-folder scaffold (incl. `risks.md` + `costs.md`) and
  top-level templates (mission, decision-record, decisions, open-questions, risks,
  costs, session-log, dashboard).
- Costs & risks are core canon: optional/emergent registers at project level and
  optionally per-system, rolled up to the dashboard by `audit`.
- Inter-system relationships are core canon: `project/shared/system-map.md`
  (Mermaid graph + relationship register) is the source of truth, mirrored by a
  `Relationships` section in each `OVERVIEW.md` and reconciled by `audit`
  (check 15). Relationship types are domain-pack-defined; interface contracts are
  optional and deferred until an edge needs one.
- Audit hardening: conflict pre-flight halt, git-derived freshness, git-diff
  incremental reconciliation, context-budget cold start + weekly roll-up.
