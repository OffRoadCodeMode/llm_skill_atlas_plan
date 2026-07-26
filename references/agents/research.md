# Agent: research (stub — grows when manual synthesis gets tedious)

Synthesises external material into cited, frontmatter-tagged research docs during
Onboard and the research phase. Research is a **means to unblock** requirements →
design → build — never the product.

## Where research goes

Research is a folder that can live in three places:

- **Workstream research** (`<workstream>/research/`): research done as part of a
  process you're completing (e.g. vehicle selection research inside a
  `van-research` workstream). See `references/workstreams.md`.
- **System-specific research** (`project/systems/<name>/research/`): deep dives
  into one system's options, constraints, architecture, or technology choices.
- **General / cross-cutting research** (`project/shared/research/`): early
  project context, technology surveys, domain background, market analysis that
  doesn't belong to any single system or workstream.

If unsure, ask: "does this finding apply to one system, a specific process
(workstream), or the whole project?"

## Behaviour (minimum)

- Write findings to the correct location (see above) using
  `references/domains/<domain>/templates/research-doc.md`.
- Set frontmatter: `type: research`, `confidence`, `mission_link`, and `sources`.
- Link each doc to at least 2 others (the requirement/question it informs + its
  system).
- Log open questions raised into the relevant `open-questions.md`.

## Research methodology

### When to delegate vs do inline

- **Inline** (use `web_search` + `web_extract` directly): quick lookups, single
  questions, when the answer is likely 1-2 searches away.
- **Delegate** (use `delegate_task`): multi-source research, comparative analyses,
  anything requiring 5+ searches, or when the intermediate results would flood
  the main context. Pass prior research file paths and project context in the
  `context` field so the subagent doesn't duplicate work.
- **Parallel batch** (use `delegate_task` with `tasks`): when 2-3 independent
  research streams can run simultaneously.

### Search strategy

1. Start broad with `web_search` to map the landscape (3-5 results).
2. Use `web_extract` on the most promising URLs for full content (not just
   snippets). Note: some backends are search-only and cannot extract — fall back
   to multiple targeted `web_search` queries with different phrasing.
3. Cross-reference claims across at least 2 independent sources before rating
   confidence `high`. Single-source or vendor-authored claims cap at `medium`.
4. For pricing/availability data, search listing sites directly (e.g.
   `site:donedeal.ie`, `site:carzone.ie`) alongside forums and guides.
5. **YouTube as a research source.** For practical/build domains (van conversions,
   construction, electronics, DIY), YouTube is often the richest source — build
   tutorials, product reviews, owner walkthroughs, failure reports. Use the
   `youtube-content` skill (`skill_view(name='youtube-content')`) to fetch
   transcripts and summarise them into cited research-doc material. Cite as
   `[YouTube: <channel name>, <video title>, <URL>]` in the Sources section.
   Video sources are particularly valuable for:
   - Step-by-step installation guides (what the process actually looks like)
   - Known issues and failure modes (owner experience > spec sheets)
   - Product comparisons with visual demonstrations
   - Region-specific practical advice (e.g. Irish camper conversion channels)

### Handling conflicting sources

- Note both positions with dates in the research doc.
- Set `contested: true` in frontmatter if the conflict is unresolved.
- State which source is more authoritative and why (independent review vs vendor
   blog, recent vs dated, peer-reviewed vs anecdotal).
- Let `audit` (check 8) flag it for later resolution.

### Structuring a research doc for scannability

- Lead with a one-paragraph summary + recommendation (if any).
- Use comparison tables for multi-option research.
- Put known issues / failure modes in a dedicated section — these are often the
  most valuable part for the user.
- End with a `## Sources` section (required per sourcing rules above).
- Keep it to 200-400 lines. If longer, split by sub-topic.

## Cost research

When researching a system that has material cost (hardware, materials, services),
gather pricing data as a first-class output:

- Realistic price ranges (low / mid / high) from multiple sources.
- Note what's included vs excluded (installation, tax, shipping).
- **Distinguish one-off vs recurring costs.** Some costs are upfront (hardware
  purchase, installation, permits); others are recurring (subscriptions, hosting,
  maintenance, fuel, insurance). Tag each item with its cadence so the budget
  spreadsheet can separate capital outlay from ongoing commitments.
- Flag prices that are indicative vs confirmed quotes.
- Record in the research doc and also in the system's `costs.md` (or
  `project/shared/costs.md` if cross-system).
- Set `confidence: low` on estimates from single sources or non-local pricing
  (e.g. US prices for an Irish project).

## Cost tracking — markdown + spreadsheet

Atlas uses two layers for cost tracking. They serve different purposes and stay
in sync via the audit:

1. **`costs.md` (per system + project-wide)** — the durable record. Markdown
   tables of line items with confidence levels and assumptions. This is what
   audit rolls up into the dashboard. It's version-controlled, diffable, and
   always readable in any editor.

2. **`project/shared/budget.xlsx`** — the living spreadsheet. When the `xlsx`
   skill is available, maintain a spreadsheet with formulas for running totals,
   contingency calculations, per-system breakdowns, and "what-if" scenarios.
   The spreadsheet is the working tool; the markdown is the canonical record.

### Sync strategy

- **Markdown is canonical.** If they disagree, the markdown `costs.md` wins —
  the spreadsheet is a derived view.
- **Update flow:** when research produces new pricing, update `costs.md` first,
  then update the spreadsheet (or let audit propose the sync).
- **Audit check:** the audit can compare `costs.md` totals against the
  spreadsheet and flag drift.
- **Per-system sheets:** the spreadsheet can have one sheet per system plus a
  summary sheet that pulls from each. This avoids the "multiple files syncing"
  problem — it's one file with tabs.
- **Don't duplicate line items.** Each cost lives in one system's `costs.md`.
  The project-wide `costs.md` rolls up (references the system registers, doesn't
  copy them). The spreadsheet does the same — one row per item, tagged by system.

## Sourcing & citation

- **Source list.** Every research doc must end with a `## Sources` section
  listing all URLs consulted, with a one-line description of what each source
  contributed. This is for the user's later reference and further reading.
- **Cite key findings, not every sentence.** Inline citations (e.g.
  `[source-name, URL]` or `^[research/sources/foo.md]`) go on the **key
  findings** and **opinions taken from external sources**, not on generic
  background knowledge. The reader should be able to trace any non-obvious
  claim back to its source.
- **State certainty/trust.** When the research adopts an opinion or
  recommendation from an online source, state how much trust to place in it.
  Use the frontmatter `confidence` field (high / medium / low) and add an
  inline note where the opinion appears, e.g.:
  > "Service X is recommended for low-latency inference (medium confidence:
  > single vendor blog, not independently benchmarked)."
  Factors that lower confidence: single source, vendor-authored, no
  independent verification, dated material, anecdotal evidence.

## Provenance & source drift (only when ingesting external sources)

- Raw sources are **immutable** — save them under `research/sources/`; corrections
  live in the citing artifact, never by editing the source.
- Give each ingested source frontmatter: `source_url`, `ingested: YYYY-MM-DD`, and
  `sha256` of the body. On re-ingest, recompute — skip if identical, flag drift if
  changed (`audit` check 11).
- On syntheses drawing from 3+ sources, append provenance markers
  `^[research/sources/foo.md]` to source-specific paragraphs.

> Expand this spec when research synthesis becomes a repeated chore.
