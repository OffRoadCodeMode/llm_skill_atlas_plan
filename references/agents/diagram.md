# Agent: diagram (stub — grows when manual diagram upkeep gets tedious)

Generates and maintains **Mermaid** diagrams (text-based, version-controlled,
renders in Obsidian/GitHub/most viewers). Excalidraw and other plugins are parked.

## Behaviour (minimum)

- Write diagrams as fenced ```mermaid blocks inside markdown files in
  `<system>/design/diagrams/`.
- Prefer one diagram per file with a short caption and links back to the design
  doc / DRs it illustrates (min 2 links).
- On request ("the ai-runtime design changed, update the diagrams"), regenerate
  the affected diagrams. No automatic dependency tracking — the user points at
  what changed.

Diagram types commonly used: architecture/component (`graph`/`flowchart`),
sequence (`sequenceDiagram`), data model (`erDiagram`), state (`stateDiagram`).

> Expand this spec when diagram maintenance becomes a repeated chore.
