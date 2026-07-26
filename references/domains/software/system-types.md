# Software domain pack — system types

A **living taxonomy**. Starts minimal and grows from usage. A system's type informs
which template subfolders it needs and how its "External Links" column reads (code
repos, etc.). A system can carry more than one type.

| Type | Typical of | Common subfolders |
|------|-----------|-------------------|
| **backend** | services, APIs, business logic | research, design (+decisions, diagrams), features, tasks |
| **frontend** | web/mobile UIs | research, design (+wireframes/diagrams), features, tasks |
| **ai-runtime** | model serving, inference, orchestration | research, design, features, tasks, risks, costs |
| **data-ingestion** | pipelines, ETL, streaming | research, design (+diagrams), tasks |
| **domain-knowledge** | knowledge bases, ontologies, RAG corpora | research, design, tasks |
| **integration** | connecting external / legacy systems | research, design (+decisions), tasks, risks |
| **infrastructure** | IaC, networking, environments | design (+diagrams), tasks, costs, risks |
| **platform** | shared internal libraries/services | design, features, tasks |

## Relationship types (for the system map)

Used in `system-map.md` and each system `INDEX.md` → Relationships to describe
*how* two systems connect (see `references/system-lifecycle.md`):

| Type | Meaning |
|------|---------|
| **api** | One system calls another's API (REST/gRPC/etc.) |
| **event-stream** | Communicate asynchronously via a message/event bus |
| **shared-data** | Share a database, schema, or data store |
| **library** | One depends on another as a shared internal library/package |
| **depends-on** | Generic build/runtime dependency not covered above |

## Adding a type

Add a row here (commit `domain: add system type '<name>' to software pack`) when a
recurring kind of system appears that the existing types don't fit. Keep it generic
enough to reuse across projects; project-specific quirks stay in the project.
