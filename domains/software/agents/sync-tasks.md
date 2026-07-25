# Agent: sync-tasks (software pack — STUB, build when a PM tool is set up)

Bi-directional sync between `TASKS.md` files and an external project tool
(e.g. Linear). **Not implemented** — the detailed design will be written when the
tool is actually chosen, to avoid speculative coupling.

## Intended behaviour (future)

- Map `TASKS.md` rows ↔ tool issues (id, title, status, priority, estimate).
- Two-way reconciliation with a clear conflict rule (last-writer or tool-wins,
  TBD).
- Keep `TASKS.md` the source of truth for planning; the tool for execution.

## Why a stub

`TASKS.md` is deliberately kept simple and markdown-native so whatever sync
mechanism we build later can parse it. Building the sync now would be premature
(*prove before scaling*).
