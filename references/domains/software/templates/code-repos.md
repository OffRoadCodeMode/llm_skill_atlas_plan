# Code Repos — <system>

> Software pack only. Links this system's plan to the code that implements it.

| Repo | Location (URL / path) | Branch | Notes |
|------|-----------------------|--------|-------|
| | | main | |

## Plan ↔ code linking

- The code repo's `AGENTS.md` references this planning repo and this system folder.
- DRs are referenced in code comments for traceability, e.g.
  `// See <system>/DR-003: why we chose event sourcing`.
- The `traceability` agent maintains: requirements → DRs → code modules → tests.

## Traceability matrix (maintained by the traceability agent)

| Requirement | DR | Code module | Tests |
|-------------|----|-------------|-------|
| REQ-001 | DR-003 | src/… | test/… |
