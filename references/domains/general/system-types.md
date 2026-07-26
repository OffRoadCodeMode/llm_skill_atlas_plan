# General domain pack — system types

Intentionally **minimal — starts empty and grows from usage**. The general pack
makes no assumptions about the project. When a distinct area of work emerges,
name it and (optionally) record a reusable type here.

| Type | Notes |
|------|-------|
| _(none predefined)_ | Add types as they emerge for your project domain. |

## Relationship types (for the system map)

Used in `system-map.md` and each system `INDEX.md` → Relationships to describe
*how* two systems connect. Generic + physical-project types:

| Type | Meaning |
|------|---------|
| **depends-on** | One system needs another to function or be built first |
| **physical-adjacency** | They share or constrain the same physical space |
| **power** | One draws from or feeds another's power/energy budget |
| **structural** | One bears load for / is mounted on another |
| **data** | They share information/records |
| **informs** | Output of one feeds decisions in another |

(For a van conversion: electrical `power` lighting; cabinets `physical-adjacency`
plumbing; etc. Add project-specific types as they emerge, or make a dedicated pack.)

## Guidance

- A "system" is any coherent area with its own boundaries, decisions, and
  artifacts. For a van conversion that might be `electrical`, `plumbing`,
  `insulation`; for a business launch, `legal`, `marketing`, `product`.
- If a project domain recurs, consider promoting its types into a **dedicated
  domain pack** (e.g. `van-conversion`) so it's reusable — see `references/governance.md`.
- Test for the core vs a pack: *"would a completely different project also use
  this?"* If not, it belongs in a pack or the project, not here.
