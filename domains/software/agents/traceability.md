# Agent: traceability (software pack — emerges when implementation begins)

Maintains the chain **requirements → DRs → code modules → tests** so the plan and
the code stay in sync.

## Behaviour (minimum)

- Build/refresh the traceability matrix in each system's `code-repos.md`.
- Flag **orphans in both directions**: requirements with no realising DR/code, and
  code referencing DRs/requirements that no longer exist.
- Verify DR references in code comments (`// See <system>/DR-XXX`) resolve to real
  DRs.
- On audit, report requirements with no tests and DRs with no code.

> Software pack only. Expand as implementation practice solidifies.
