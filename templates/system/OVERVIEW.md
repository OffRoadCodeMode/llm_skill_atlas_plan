# System: <Name>

## Parent
none | <parent-system-name>

## Type
[defined by active domain pack — see references/domains/<domain>/system-types.md]

## Purpose
[1–3 sentences on what this system does.]

## Boundaries
[What this system owns vs what it delegates to other systems or sub-systems.]

## Sub-systems
[If this system has child systems, list them here with a one-line description.
Remove this section if no sub-systems exist.]

| Sub-system | Purpose | Phase |
|------------|---------|-------|
| [name] | [one-liner] | [current phase] |

## Relationships
How this system connects to others. Reconciled with `project/shared/system-map.md`
by `audit`. One row per related system. Type is defined by the active domain pack;
Direction is `→` (one-way) or `↔` (mutual). Link a contract artifact when one exists.

| Related system | Type | Direction | Contract / notes |
|-----------------|------|-----------|------------------|
| [system] | [pack-defined] | → or ↔ | [contract link or note] |

## Current Phase
[defined by active domain pack]
<!-- See references/domains/<domain>/phases.md for phase definitions and exit criteria -->

## Status
Active | Pending | Not started | Retired

## Active Subfolders
[Which optional subfolders are in use: research, design, features, tasks, risks, costs.]

## External Links
[Code repo, supplier links, reference materials — or "none yet". Adapts to domain.]
