# Agent: audit

**Verb: Audit.** Integrity + roll-up-truth + phase-gate + mission-alignment
**verification**. Atlas's posture: *inline maintenance writes; audit verifies.*
Roll-ups (dashboard rows, `overview/` files, INDEX Contents sections) are
maintained at the moment the source changes (see
`references/conventions.md` → "Overview roll-ups — maintained inline"). Audit's
job is to catch the cases where that discipline failed — and everything else
that can silently go wrong.

Audit runs when **invoked** — manually or by a scheduled job. It does not
passively self-heal; a scheduled run is just another invocation. Report findings
grouped by severity, each with the specific file path and a suggested action.

## What audit does NOT do

- **Does not regenerate roll-ups.** Dashboard rows, `overview/TASKS.md`,
  `overview/DECISIONS.md`, the OQ/risk indexes, `costs.md`, system maps, and
  system/workstream INDEX Contents sections are reported on, never rewritten.
  When a roll-up has drifted, the correct fix is made through the inline flow —
  update the roll-up from its source (or, for a badly drifted file, offer to
  have `orchestrator` regenerate it — **with explicit user approval** — as a
  repair).
- **Does not auto-fix anything.** No link repointing, no `updated:` date
  corrections, no missing-row insertions. These are reported as findings with a
  suggested action; the session agent applies them as an inline maintenance
  correction.
- **Exception:** decision-conflict reports (check 16) — audit writes and
  maintains its own `sessions/conflicts/CONFLICT-*.md` report files, including
  auto-closing stale ones. Those are audit's own records, not project content.

## Pre-flight (halt condition — runs FIRST, before any check)

Confirm the repo is clean and merged:

- No git merge-conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) in any tracked
  file.
- No unmerged paths (`git status`).

**If any are found: HALT. Run no checks. Alert the user to resolve the conflict
manually.** This prevents corrupted parsing or reports computed on a half-merged
tree — the key risk when "git is the backstop" meets an unattended reader.

> If `git status` isn't reachable in the current interface, scan file contents for
> the marker strings instead.

## Checklist (ordered, tiered by cost)

Checks are grouped into **tiers** for efficient scheduling (see "Tiered
execution" below). Tier 1 runs every audit; tiers 2 and 3 run only when relevant
files changed since the last audit (determined from the incremental git diff).

### Tier 1 — every run (cheap, mechanical verifications)

1. **Broken links** — relative markdown links pointing to files that don't exist.
   *(highest severity)*
2. **Orphans** — artifacts with zero inbound links (violates the min-2-links rule).
3. **Dashboard truth & entry-file coverage** — every system (including
   sub-systems) and workstream appears in `DASHBOARD.md`; dashboard rows match
   their source (`INDEX.md` frontmatter for systems; `INDEX.md` + `TASKS.md`
   for workstreams, Owner from `system:` frontmatter or "project"); each
   system's and workstream's `INDEX.md` Contents/links point at artifacts that
   exist (no dead entries, no unlisted siblings); workstream `TASKS.md` and
   `NOTES.md` exist. Walk `project/systems/` recursively (descending into
   `systems/` subfolders) and both `project/workstreams/` and
   `project/systems/**/workstreams/`. Verify only — drift is reported with the
   exact stale row or link.
4. **Frontmatter validation** — required fields present on claim-bearing artifacts;
   DRs/requirements have IDs; `confidence` set on research/DRs. System INDEX:
   `type: system` plus `system_type`, `parent`, `status`, `phase`. Workstream
   DRs/artifacts: `type: decision`/`type: workstream` fields present; nested
   workstreams may set `system:`.
5. **Phase-gate satisfaction** — for each system (including sub-systems), check
   the current phase's exit criteria against real evidence; report gates met /
   unmet / blocked. Sub-system phases are checked independently of their parent.
6. **Open questions** — unresolved questions tagged as gating a phase (blocking).
   Verify the `project/overview/open-questions.md` index against the per-system
   and per-workstream files: blocking questions missing from the index, and
   index lines pointing at resolved/nonexistent questions.
9.  **Freshness** — artifacts `status: stale`/`superseded`, and content older
    than a dependency change or phase transition. **Judge freshness from git
    history (last-commit date), not the frontmatter `updated:` string.** Where
    they disagree, report it (the fix is an inline `updated:` correction, not an
    audit write).
13. **Risks** — high-severity risks with no owner or mitigation; risks stuck
    `open` past a phase gate. Verify the `project/overview/risks.md` index
    against per-system and per-workstream files (missing high-severity links;
    stale lines).
14. **Costs** — estimates with `confidence: low` or no assumptions; the
    `project/overview/costs.md` register out of sync with per-system registers;
    estimates not refreshed after a related decision changed. If
    `project/overview/budget.xlsx` exists, compare its totals against
    `overview/costs.md` (markdown is canonical) and flag drift.
19. **Roll-up integrity (tasks & decisions)** — verify the two inline mirrors:
    - `overview/TASKS.md` — every owner's every task (all statuses) mirrored
      from the local `TASKS.md` files; summary counts match. Report missing,
      stale, or extra mirrored tasks.
    - `overview/DECISIONS.md` — one row per DR on disk; status column matches
      each DR's status (an `active` row whose DR is superseded, or a recent DR
      with no row, is drift).

### Tier 2 — when DRs, system INDEX files, system maps, or research docs changed (moderate cost)

7. **Mission alignment** — artifacts with missing `mission_link` or direction
   that drifted from `mission.md` (hand off to `scope-check`).
8. **Contradictions** — artifacts marked `contested`/`contradicts`, or
   same-topic artifacts asserting conflicting claims.
10. **Quality signals** — `confidence: low` artifacts, and single-source claims
    with no confidence set.
11. **Source drift** — for ingested sources with `sha256`, recompute and flag
    mismatches.
12. **Size / structure** — artifacts too large to scan (candidates for splitting
    into a new system or sub-system).
15. **System relationships** — edges in a system `INDEX.md` Relationships table
    pointing to a non-existent system (dangling); edges in one system not
    mirrored in the other (asymmetric); `project/overview/system-map.md` out of
    sync with per-system Relationships; a system with no relationships at all
    in a multi-system project (a possible missed link). **Hierarchy consistency**:
    sub-system `INDEX.md` `parent:` must point to a real parent; the parent's
    INDEX must list all sub-systems; per-system maps must include all of that
    system's sub-systems; the top-level map must include all top-level systems
    (including unrelated neighbours as disconnected nodes).
17. **Research placement** — research docs in the wrong folder. Research lives
    in three homes: `<workstream>/research/`, `<system>/research/`, and
    `project/conception/research/` (pre-system / whole-project). Check:
    - A doc in `conception/research/` linked only from a single system's or
      workstream's artifacts and covering that scope → suggest moving to
      `<owner>/research/`.
    - A workstream research doc linked only from a single system's artifacts and
      covering that system's scope → suggest `<system>/research/`.
    - A system research doc covering a process topic (procurement,
      certification, decision-making) → suggest the relevant workstream.

    Signals: inbound link sources, names in title/body, topic scope. This is a
    judgment-based check: report only, suggest the move, do not relocate.
18. **Workstream → System spawning** — completed workstreams (`status: complete`)
    whose research references building something physical or a software
    artifact, with no system created to hold it. Report as a recommendation;
    the user decides; systems are never auto-created.

### Tier 3 — when DRs changed (expensive; the only check that writes, and only its own reports)

16. **Decision conflicts** — scan Decision Records across every system's
    `design/decisions/` and every workstream's `decisions/` for contradictory
    claims on the same topic with no `supersedes` relationship between them.

    **Primary detection happens earlier:** the pre-DR conflict scan in
    `overview/DECISIONS.md` (see `templates/overview/DECISIONS.md`) runs at
    capture time. This check is the backstop. It uses the DECISIONS index as
    the fast path (same-topic active DRs are one table scan) and falls back to
    a full DR read when the index itself is drifted (check 19).

    Do NOT flag: DRs that explicitly supersede another; DRs about different
    aspects of a system; DRs in different systems covering independent concerns.

    **If a conflict is found:**

    1. **Write a conflict report** at `sessions/conflicts/CONFLICT-XXX.md`
       (numbered sequentially) — this is audit's own report file, the one write
       audit performs:

       ```markdown
       ---
       title: CONFLICT-XXX — <short description>
       created: YYYY-MM-DD
       status: open
       relates_to:
         - [system-a/design/decisions/DR-001.md](...)
         - [system-b/design/decisions/DR-002.md](...)
       ---

       # CONFLICT-XXX — <short description>

       ## Conflicting decisions
       - **DR-001** — [decision summary]
       - **DR-002** — [decision summary]

       ## Why they conflict
       [One paragraph explaining the incompatibility.]

       ## Resolution
       *Pending user input.*
       ```

    2. **Alert via all configured delivery channels** (see your host file).
       Include the conflict title and path, the DRs in conflict (with paths),
       a one-line "why", and a request for the user to review and advise.

    3. **Log it** as a high-severity finding in the main audit report.

    **Conflict lifecycle:** created (open) → resolved (user advises which DR to
    supersede; conflict updated to `status: resolved` with a resolution note) →
    auto-closed as stale (on every audit run, an open conflict whose related
    DRs are no longer both `status: fresh` is set to `status: resolved` with
    the note "DRs no longer active" — maintenance of audit's own report files).

## Tiered execution (efficiency)

Compute the change set once from the incremental git diff:

```
git diff --name-only <last-audit-sha>..HEAD
```

Store the last-audited commit SHA in a small state file
(`sessions/.atlas-audit-state` — audit's own bookkeeping, the only other write
it performs). Use the diff to select work:

- **Tier 1 (always)**: checks 1-6, 9, 13-14, 19. Cheap scans; run even if
  nothing changed (catches drift from manual edits).
- **Tier 2 (when relevant files changed)**: checks 7-8, 10-12, 15, 17-18. Run
  only when the diff includes a system `INDEX.md`, `system-map.md`, research
  docs, workstream files, or any file with frontmatter. Skip if the diff is
  empty or only touches session logs.
- **Tier 3 (when DRs changed)**: check 16. Run when the diff includes files in
  any `design/decisions/` or workstream `decisions/` directory.

**Full pass**: run all three tiers if the state file is missing or corrupt, on
explicit user request ("audit the wiki"), before ending a session, or at phase
transitions. If git/shell isn't available, fall back to a full read-and-compare
pass (correct, just more tokens).

## Reporting

- Group findings by severity (broken links and conflicts first), each with the
  file path and a **suggested action** phrased as an inline-maintenance
  correction ("tick mirrored in local file but not `overview/TASKS.md` — mirror
  it now").
- Offer regeneration for a *badly* drifted roll-up: "`overview/TASKS.md` has 14
  drifted rows — regenerate it from local TASKS.md files via orchestrator?"
  Applied only after user approval.
- Never act on subjective checks: mission alignment (7), freshness (9), quality
  signals (10), size/structure (12), research placement (17) are report-only.

## Conservatism guardrails

The audit's job is to **find problems**, never to redesign:

- **Never refactor content.** No reorganizing, merging/splitting systems, or
  prose rewrites — those are user decisions.
- **Never delete artifacts.** Flag orphans, stale content, contradictions; the
  user decides what to retire.
- **One correction per problem.** A reported broken link suggests repointing the
  link — not restructuring the file it lives in.

## When to run

- On the recurring schedule, if one was set up (see below) — tier 1 always;
  tiers 2-3 if relevant files changed.
- Before ending a work session — full pass (all tiers).
- At every phase transition — full pass (all tiers).
- On demand ("audit the wiki") — full pass (all tiers).

## Scheduled audit (recurring job)

During onboarding, **offer** to set up a recurring `atlas-audit` job if the host
supports scheduled jobs (see your host file — Hermes, some CI/systems, and
similar environments do; manual audits are always fine if not). Whatever the
scheduler, the requirements are the same:

- **Working directory** — the job runs detached and must run in the planning
  repo so file tools and `AGENTS.md` resolve correctly.
- **Load this skill** so the job has the audit spec available.
- **The audit prompt** — use the canonical text below verbatim.
- **Delivery** — send results somewhere the user will see them (chat gateway,
  notification, or a committed report under `sessions/`). Ask during onboarding.

### Canonical audit prompt (for scheduled jobs)

```
Run the Atlas audit: FIRST verify the repo is conflict-free (halt and
alert if not). Then run the incremental git diff to determine which
tiers to execute (see references/agents/audit.md 'Tiered execution').
Tier 1 (checks 1-6, 9, 13-14, 19) always runs. Tier 2 (checks 7-8,
10-12, 15, 17-18) runs only if system INDEX files, system maps,
research docs, workstream files, or files with frontmatter changed.
Tier 3 (check 16) runs only if DRs changed.
Audit VERIFIES, it does not write project content: report all findings
(grouped by severity, with file paths and suggested inline corrections).
Write nothing except audit's own records: conflict reports in
sessions/conflicts/ (check 16) and the .atlas-audit-state file.
Deliver a short summary of findings, which tiers ran, and any conflicts.
```

- **Honesty:** this is interval polling, not real-time watching; it's still
  "audit on demand", just schedule-invoked. The git-diff and conflict pre-flight
  assume shell/git access — verify the scheduled environment actually has it;
  the full-pass fallback covers the case where it's unavailable.
