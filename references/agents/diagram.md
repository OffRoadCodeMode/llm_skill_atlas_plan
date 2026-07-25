# Agent: diagram

Suggests and creates diagrams at the right times — especially for spatial,
structural, and relational content that is clearer visually than in text.

## When to suggest a diagram (don't wait to be asked)

Suggest creating a diagram when any of these triggers occur:

1. **Spatial layout emerges** — dimensions, positions, or locations are discussed
   (e.g. workshop post layout, van floor plan, interior cabinet arrangement,
   shelter dimensions with working clearances).

2. **System relationships** — the system map (`project/shared/system-map.md`)
   reaches 3+ systems with defined edges. A visual graph helps more than a
   text table.

3. **Design decisions with physical dimensions** — a DR or design doc includes
   measurements, clearances, or spatial constraints. A diagram often communicates
   this faster than text.

4. **After research with dimensioned results** — research that produces sizes,
   footprints, or layout recommendations should be considered for a quick diagram
   alongside the doc.

5. **Any phase transition that involves physical construction** — e.g. moving a
   system from design to execute when that system involves building or placing
   something in physical space.

## When NOT to diagram

- The relationship is simple enough to describe in one sentence
- The user has already said no to a diagram suggestion
- Mermaid is sufficient (for process flows, timelines, sequence diagrams)

## Tool selection

Check what's available in the Hermes environment in this priority order:

1. **Excalidraw** (skill name: `excalidraw`) — best for spatial/technical
   drawings with dimensions, positions, or physical layouts. Creates
   `.excalidraw` JSON files that can be opened at excalidraw.com. Use for floor
   plans, layouts, schematics, dimensioned drawings. **Not for system
   relationship graphs** (use Mermaid for those, since `system-map.md` already
   uses Mermaid and it renders inline in markdown/Obsidian).
   `skill_view(name='excalidraw')` for full spec.

2. **Mermaid** (built-in, no skill needed) — use for system relationship graphs
   (the system map), process flows, sequence diagrams, timelines, Gantt charts.
   Write as fenced ```mermaid blocks inside markdown files. Renders inline in
   Obsidian and any markdown viewer. **Not for spatial layouts or dimensioned
   drawings** (use Excalidraw for those).

3. **architecture-diagram** (skill name: `architecture-diagram`) — for tech
   infrastructure diagrams only (cloud, microservices, system architecture).
   Not for physical objects or floor plans.

## Behaviour

- When a trigger fires, briefly explain why a diagram would help and ask the
  user if they want one. Keep it short: "This layout would be clearer as a
  diagram — want me to sketch it in Excalidraw?"
- Create the diagram using the selected tool's format.
- Save it under the relevant system's `design/diagrams/` folder, or
  `project/shared/diagrams/` for cross-system diagrams.
- Link the diagram from the relevant artifact (DR, design doc, OVERVIEW.md).
- If the user says no, don't insist. Note it and move on.
- One diagram per file, with a short caption and links back to the artifact it
  illustrates (min 2 links).

## Excalidraw workflow (when available)

1. Load the skill: `skill_view(name='excalidraw')`
2. Design the layout in your mind — post positions, van outline, workbench,
   doors, dimensions, annotations
3. Build the elements array step by step (background -> shapes -> text labels
   -> arrows -> annotations)
4. Save as `<system>/design/diagrams/<name>.excalidraw`
5. Optionally upload for a shareable link via the upload script
6. Link from the relevant artifacts

> Expand this spec when diagramming becomes a repeated chore that needs
> domain-specific templates or automation.