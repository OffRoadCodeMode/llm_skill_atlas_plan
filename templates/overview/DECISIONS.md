# Decisions — <Project Name>

> Index of every Decision Record in the project, across all systems and
> workstreams (project-level and nested).
>
> **Maintained inline.** Whenever a DR is created, superseded, or reactivated,
> add or update its row here **in the same action** — the index never lags.
> The DR files are the source of truth; `audit` verifies this index against them.

## Active decisions

| DR | Owner | Decision | Created | Link |
|----|-------|----------|---------|------|
| DR-001 | <system/workstream> | [one-line decision] | YYYY-MM-DD | [DR file](../project/systems/<system>/design/decisions/DR-001.md) |

## Superseded

| DR | Owner | Decision | Created | Superseded by | Link |
|----|-------|----------|---------|---------------|------|
| DR-000 | <owner> | [one-line decision] | YYYY-MM-DD | <owner>/DR-XXX | [DR file] |

## Conventions

- **Active = `status: fresh`.** A DR stays in the Active table while its
  frontmatter is `status: fresh`; when superseded, move its row to the
  Superseded table (its frontmatter becomes `status: superseded`) — both files
  in the same action.
- **Conflict scan before writing a DR:** before creating a new Decision Record,
  scan the Active table for a decision on the same topic. If one exists and the
  new DR disagrees with it, either supersede the old DR in the new DR (set its
  `supersedes:` frontmatter) or stop and flag the conflict to the user (see
  `references/agents/audit.md` → check 16).
- DR IDs are per-owner; qualify across owners: `<owner>/DR-XXX`.
- Keep the Decision cell to one line — the DR file holds the context.
