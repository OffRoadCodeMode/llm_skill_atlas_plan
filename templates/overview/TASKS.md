# Tasks Roll-up — <Project Name>

> **Full mirror** of every task across the project — every system and every
> workstream (project-level and nested), all statuses.
>
> **Maintained inline.** Whenever a task is added, ticked, edited, or blocked in
> any local `TASKS.md`, mirror the same change here **in the same action** — a
> roll-up is never allowed to lag its source. Local `TASKS.md` files are the
> source of truth; `audit` verifies this file against them and flags drift.

## Summary

| Owner | Type | Todo | In progress | Blocked | Done |
|-------|------|------|-------------|---------|------|
| <system-or-workstream> | system / workstream (/ nested) | 0 | 0 | 0 | 0 |

## Systems

### <system-name>
> Mirror of [<system>/TASKS.md](../project/systems/<system>/TASKS.md)

- [ ] TASK-001: Description | priority: high | estimate: — | status: todo
- [x] TASK-002: Description | priority: low | estimate: — | status: done

## Workstreams (project-level)

### <workstream-name>
> Mirror of [workstreams/<name>/TASKS.md](../project/workstreams/<name>/TASKS.md)

- [ ] [task] ...

## Workstreams (nested in systems)

### <system>/<workstream-name>
> Mirror of [systems/<system>/workstreams/<name>/TASKS.md](../project/systems/<system>/workstreams/<name>/TASKS.md)

- [ ] [task] ...

## Conventions

- Task IDs are qualified by owner when referenced from outside: `<owner>/TASK-XXX`
  (e.g. `electrical/TASK-002`, `financing/TASK-001`), same as DRs.
- Owners appear here only once they have a local `TASKS.md`. Remove an owner's
  section when its `TASKS.md` is retired; keep done tasks while the file lives.
- Status vocabulary follows the local templates: `todo | in-progress | blocked | done`.
