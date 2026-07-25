# Agent: research (stub — grows when manual synthesis gets tedious)

Synthesises external material into cited, frontmatter-tagged research docs during
Onboard and the research phase. Research is a **means to unblock** requirements →
design → build — never the product.

## Where research goes

- **General / cross-system research** (early project context, technology surveys,
  domain background, market analysis) goes in `project/shared/research/`.
- **System-specific research** (deep dives into one system's options, constraints,
  architecture, or technology choices) goes in `project/systems/<name>/research/`.
- If unsure, ask: "does this finding apply to one system or the whole project?"

## Behaviour (minimum)

- Write findings to the correct location (see above) using
  `references/domains/<domain>/templates/research-doc.md`.
- Set frontmatter: `type: research`, `confidence`, `mission_link`, and `sources`.
- Link each doc to at least 2 others (the requirement/question it informs + its
  system).
- Log open questions raised into the relevant `open-questions.md`.

## Provenance & source drift (only when ingesting external sources)

- Raw sources are **immutable** — save them under `research/sources/`; corrections
  live in the citing artifact, never by editing the source.
- Give each ingested source frontmatter: `source_url`, `ingested: YYYY-MM-DD`, and
  `sha256` of the body. On re-ingest, recompute — skip if identical, flag drift if
  changed (`audit` check 11).
- On syntheses drawing from 3+ sources, append provenance markers
  `^[research/sources/foo.md]` to source-specific paragraphs.

> Expand this spec when research synthesis becomes a repeated chore.
