# Tasks — <system>

> Software pack variant. Markdown-native so any future sync tool (e.g. Linear via
> the `sync-tasks` agent) can parse it. `estimate` is optional and never blocks a
> task from being actionable — use `—` when unknown.

- [ ] TASK-001: Description | priority: high | estimate: 2d | status: todo
- [ ] TASK-002: Description | priority: medium | estimate: — | status: todo
- [x] TASK-003: Description | priority: medium | estimate: 1d | status: done

## Conventions

- **status:** todo | in-progress | blocked | done
- **priority:** high | medium | low
- Link tasks to the feature/requirement they serve where useful.
- Task status changes do NOT go in the changelog; they live here.
