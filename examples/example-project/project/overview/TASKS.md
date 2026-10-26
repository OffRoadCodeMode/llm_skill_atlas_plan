# Tasks Roll-up — nightowl

> **Full mirror** of every task across the project, maintained inline. Local
> `TASKS.md` files are the source of truth; `audit` verifies this file against
> them and flags drift.

## Summary

| Owner | Type | Todo | In progress | Blocked | Done |
|-------|------|------|-------------|---------|------|
| feed-collector | system | 1 | 1 | 0 | 1 |
| hosting-options | workstream | 2 | 1 | 0 | 2 |

## Systems

### feed-collector
> Mirror of [feed-collector/TASKS.md](../systems/feed-collector/TASKS.md)

- [ ] TASK-001: Define entry schema (title, url, source, published, guid) | priority: high | estimate: — | status: todo
- [ ] TASK-002: Sketch digest email template (plain text) | priority: medium | estimate: — | status: in-progress
- [x] TASK-003: Run feed-parser spike to pick library | priority: high | estimate: 1d | status: done

## Workstreams (project-level)

### hosting-options
> Mirror of [workstreams/hosting-options/TASKS.md](../workstreams/hosting-options/TASKS.md)

- [ ] TASK-001: Compare fly.io free-ish tier vs $4/mo VPS | priority: high | estimate: — | status: todo
- [ ] TASK-002: Confirm outbound email options on free tiers | priority: high | estimate: — | status: todo
- [ ] TASK-003: Write comparison notes into research/hosting-comparison.md | priority: medium | estimate: — | status: in-progress
- [x] TASK-004: List candidate providers | completed: 2026-10-21
- [x] TASK-005: Rule out card-verified free tiers | completed: 2026-10-22
