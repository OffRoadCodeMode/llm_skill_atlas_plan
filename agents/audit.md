# Agent: audit

**Verb: Audit.** Integrity + phase-gate + mission-alignment check. Runs when
**invoked** — manually or by a Hermes cron job. It does not passively self-heal;
a scheduled run is just another invocation. Report findings grouped by severity,
each with the specific file path and a suggested action.

## Pre-flight (halt condition — runs FIRST, before any check or write)

Confirm the repo is clean and merged:

- No git merge-conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) in any tracked
  file.
- No unmerged paths (`git status`).

**If any are found: HALT. Write nothing. Alert the user to resolve the conflict
manually.** This prevents ingesting conflict markers as literal content (which
corrupts parsing and cascades) or writing on top of a half-merged tree — the key
risk when "git is the backstop" meets an unattended writer.

> If `git status` isn't reachable in the current interface, scan file contents for
> the marker strings instead.

## Checklist (ordered, severity-grouped)

1. **Broken links** — relative markdown links pointing to files that don't exist.
   *(highest severity)*
2. **Orphans** — artifacts with zero inbound links (violates the min-2-links rule).
3. **Index / dashboard completeness** — every system appears in `DASHBOARD.md`;
   every artifact appears in its folder `INDEX.md`; dashboard system rows are
   derivable from `OVERVIEW.md` files.
4. **Frontmatter validation** — required fields present on claim-bearing artifacts;
   DRs/requirements have IDs; `confidence` set on research/DRs.
5. **Phase-gate satisfaction** — for each system, check the current phase's exit
   criteria against real evidence; report gates met / unmet / blocked.
6. **Blocking open questions** — unresolved questions tagged as gating a phase.
7. **Mission alignment** — artifacts with missing `mission_link` or direction that
   drifted from `mission.md` (hand off to `scope-check`).
8. **Contradictions** — artifacts marked `contested`/`contradicts`, or same-topic
   artifacts asserting conflicting claims.
9. **Freshness** — artifacts `status: stale`/`superseded`, and content older than a
   dependency change or phase transition. **Judge freshness from git history
   (last-commit date), not the frontmatter `updated:` string** — agents forget to
   bump it. Where they disagree, auto-correct `updated:` from git (low-risk fix).
10. **Quality signals** — `confidence: low` artifacts, and single-source claims
    with no confidence set.
11. **Source drift** — for ingested sources with `sha256`, recompute and flag
    mismatches.
12. **Size / structure** — artifacts too large to scan (candidates for splitting
    into a new system).
13. **Risks** — high-severity risks with no owner or mitigation; risks stuck `open`
    past a phase gate; `DASHBOARD.md` Top Risks out of sync with the risk registers.
14. **Costs** — estimates with `confidence: low` or no assumptions; DASHBOARD
    budget/drivers out of sync with the cost registers; estimates not refreshed
    after a related decision changed.
15. **System relationships** — edges in an `OVERVIEW.md` pointing to a non-existent
    system (dangling); edges in one system not reconciled in the other
    (asymmetric); `project/shared/system-map.md` out of sync with per-system
    Relationships; in a multi-system project, a system with no relationships at all
    (a possible missed link).

## Reconcile

After checks, the audit may **regenerate**: folder `INDEX.md` files, the
`DASHBOARD.md` system rows (from `OVERVIEW.md`), and the Top Risks / Costs roll-ups
(from the risk/cost registers).

## Review before write

- **Low-risk mechanical fixes** (regenerating an INDEX, refreshing a dashboard row,
  correcting an `updated:` date) may be applied directly.
- **Unattended runs (cron) OR reconciliations touching 10+ artifacts** must be
  **staged as proposals and delivered for review**, not silently applied. This
  keeps the single-writer rule intact.

## Incremental reconciliation (efficiency)

Only re-derive rows/indexes for artifacts that **changed since the last audit**.
Get the change set from:

```
git diff --name-only <last-audit-sha>..HEAD
```

One command, no per-file reads — and more correct than file mtimes, which reset on
clone/cloud-sync. Store the last-audited commit SHA in a small state file
(`sessions/.atlas-audit-state`). If git/shell isn't available, fall back to a full
read-and-compare pass (correct, just more tokens).

## When to run

- On the Hermes cron schedule (see below).
- Before ending a work session.
- At every phase transition.
- On demand ("audit the wiki").

## Scheduled audit (Hermes cron)

During onboarding, offer to create a recurring, skill-backed job via Hermes's
`cronjob` tool:

```
cronjob(
  action="create",
  name="atlas-audit",
  schedule="every 1h",
  workdir="<absolute-path-to-planning-repo>",   # REQUIRED — cron runs detached
  skills=["atlas"],
  prompt="Run the Atlas audit: FIRST verify the repo is conflict-free (halt and
          alert if not); then reconcile indexes and DASHBOARD.md from OVERVIEW.md
          files, flag stale/broken links and unresolved blocking questions.
          Deliver a short summary.",
  deliver="telegram",
)
```

- **`workdir` is required** — cron runs detached; set it to the planning repo so
  file tools and `AGENTS.md` resolve correctly.
- **Pin provider/model** for unattended runs (an unpinned job fails closed on
  default change), or use `hermes setup --portal`. The gateway must be running.
- **Honesty:** this is interval polling, not real-time watching; it's still
  "audit on demand", just schedule-invoked. The incremental git-diff and
  conflict pre-flight assume shell/git access — verify against the Hermes tool
  schema; the full-pass fallback covers the case where it's unavailable.
