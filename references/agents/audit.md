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
3. **Index / dashboard completeness** — every system (including sub-systems)
   appears in `DASHBOARD.md`; every artifact appears in its folder `INDEX.md`;
   dashboard system rows are derivable from `OVERVIEW.md` files. Sub-systems
   name their parent in the dashboard `Parent` column. Walk `project/systems/`
   recursively, descending into any `systems/` subfolder.
4. **Frontmatter validation** — required fields present on claim-bearing artifacts;
   DRs/requirements have IDs; `confidence` set on research/DRs.
5. **Phase-gate satisfaction** — for each system (including sub-systems), check
   the current phase's exit criteria against real evidence; report gates met /
   unmet / blocked. Sub-system phases are checked independently of their parent.
6. **Open questions** — unresolved questions tagged as gating a phase (blocking).
   Also reconcile the shared `project/shared/open-questions.md` per-system index
   against the actual per-system `open-questions.md` files: flag per-system OQs
   that are blocking but missing from the shared index, and index entries pointing
   to questions that have been resolved or no longer exist (stale index lines).
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
    into a new system or sub-system).
13. **Risks** — high-severity risks with no owner or mitigation; risks stuck `open`
    past a phase gate; `DASHBOARD.md` Top Risks out of sync with the risk registers.
14. **Costs** — estimates with `confidence: low` or no assumptions; DASHBOARD
    budget/drivers out of sync with the cost registers; estimates not refreshed
    after a related decision changed.
15. **System relationships** — edges in an `OVERVIEW.md` pointing to a non-existent
    system (dangling); edges in one system not reconciled in the other
    (asymmetric); `project/shared/system-map.md` out of sync with per-system
    Relationships; in a multi-system project, a system with no relationships at all
    (a possible missed link). **Hierarchy consistency**: sub-system `OVERVIEW.md`
    `parent:` field must point to a real parent system; parent `OVERVIEW.md` must
    list all sub-systems in its Sub-systems section; system map `subgraph` clusters
    must match the parent/child relationships in `OVERVIEW.md` files.
16. **Decision conflicts** — scan all Decision Records across every system's
    `design/decisions/` directory for contradictory claims on the same topic.
    Decisions conflict when two DRs assert mutually incompatible positions with
    no `supersedes` relationship between them (e.g. "use 12V system" and "use
    24V system" are both active, or two DRs choose different base vehicles).
    **This check runs last because it may write a conflict file.**

    ### How to detect conflicts

    For each DR, extract the core claim (the Decision section). Group DRs by
    subject matter using title keywords, tags, and system proximity. Flag groups
    where two or more `status: fresh` (not `superseded`) DRs make different
    choices on the same topic. Do NOT flag:
    - DRs that explicitly supersede another (the superseding DR replaces the old)
    - DRs that are about different aspects of the same system (e.g. battery
      chemistry vs battery voltage are different decisions)
    - DRs in different systems that affect independent concerns (e.g. electrical
      system voltage and workshop wall colour)

    ### If a conflict is found

    1. **Create a conflict file** at `sessions/conflicts/CONFLICT-XXX.md` with:

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
       [One sentence/paragraph explaining the incompatibility.]

       ## Resolution
       *Pending user input.*
       ```

       Number sequentially: `CONFLICT-001`, `CONFLICT-002`.

    2. **Alert via all open gateways.** Use `cronjob(action="run", job_id="<audit-id>")` 
       if running inside a scheduled audit, or deliver the finding directly. The
       alert must include:
       - The conflict title and file path
       - Which DRs are in conflict (with paths)
       - A one-line summary of why they conflict
       - A request for the user to review and advise on resolution

       **Delivery targets:** send to *all* gateways the user has configured
       (Telegram, Discord, etc.). If running inside a cron job, the job's
       `deliver` setting already handles this — use `"all"` to fan out to every
       connected gateway.

    3. **Log the audit finding** as a high-severity item in the main audit report.

    ### Conflict lifecycle

    - **Created:** audit detects the conflict, writes the file, alerts the user.
    - **Resolved:** the user advises which DR to supersede or how to reconcile.
      The agent updates the relevant DR's `status: superseded` or edits the
      conflict to `status: resolved` with a resolution note.
    - **Stale:** an open conflict whose related DRs have all been superseded
      (auto-closes: `status: resolved` + note "DRs no longer active").

    On every audit run, re-check open conflicts: if the conflicting DRs are no
    longer both `status: fresh`, auto-resolve the conflict file.

    ### Gateways notice

    When conflict alerts are delivered to a gateway (Telegram, Discord, etc.),
    they appear in the user's normal feed for that platform. The user can reply
    with instructions. If the gateway supports continuable sessions
    (`attach_to_session`), the response can be picked up directly; otherwise the
    agent notes the instruction on the next session start (via the session-continuity
    conflict check).

## Reconcile

After checks, the audit may **regenerate**: folder `INDEX.md` files (walking
`project/systems/` recursively, including sub-system folders), the
`DASHBOARD.md` system rows (from `OVERVIEW.md`, with each sub-system's `Parent`
column set from its `parent:` field), and the Top Risks / Costs roll-ups (from
the risk/cost registers).

## Review before write

- **Low-risk mechanical fixes** (regenerating an INDEX, refreshing a dashboard row,
  correcting an `updated:` date) may be applied directly.
- **Unattended runs (cron) OR reconciliations touching 10+ artifacts** must be
  **staged as proposals and delivered for review**, not silently applied. This
  keeps the single-writer rule intact.

## Conservatism guardrails

The audit's job is to **find and fix mechanical problems**, not to refactor or
improve the project. When in doubt, report, do not rewrite.

- **Never refactor content.** Do not reorganize files, merge/split systems,
  rewrite prose, or "improve" structure. These are user decisions.
- **Never delete artifacts.** Flag orphans, stale content, or contradictions;
  do not remove them. The user decides what to retire.
- **Auto-fix only mechanical issues:** broken links (repoint to correct path),
  missing INDEX/dashboard rows (regenerate), stale `updated:` dates (correct
  from git). Everything else is a report item.
- **Subjective checks are report-only.** Mission alignment (check 7), freshness
  (check 9), quality signals (check 10), and size/structure (check 12) surface
  findings and suggest actions, but the agent does not act on them
  autonomously.
- **One fix per problem.** If a link is broken, fix the link. Do not also
  restructure the file it lives in.

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
          alert if not); then run the checklist from references/agents/audit.md.
          Auto-fix only mechanical issues (broken links, missing INDEX/dashboard
          rows, stale updated: dates). Everything else is report-only: do NOT
          refactor, restructure, rewrite, or delete content. Deliver a short
          summary of findings and any fixes applied.",
  deliver="telegram",          # adjust to user's configured gateway — see below
)
```

- **`workdir` is required** — cron runs detached; set it to the planning repo so
  file tools and `AGENTS.md` resolve correctly.
- **Pin provider/model** for unattended runs (an unpinned job fails closed on
  default change), or use `hermes setup --portal`. The gateway must be running.
- **Delivery target** — set `deliver` to a gateway the user has configured
  (e.g. `"telegram"`, `"discord"`, `"all"`). On CLI-only sessions there is no
  live delivery channel — use `deliver="local"` to save results viewable via
  `cronjob action='list'`, or omit `deliver` to save silently. Ask the user
  which gateway they want during onboarding.
- **Conversational audits** — if the user may want to follow up on audit findings
  conversationally, set `attach_to_session=true` so the delivery is continuable
  (the user can reply and the agent has the audit brief in context).
- **Honesty:** this is interval polling, not real-time watching; it's still
  "audit on demand", just schedule-invoked. The incremental git-diff and
  conflict pre-flight assume shell/git access — verify against the Hermes tool
  schema; the full-pass fallback covers the case where it's unavailable.
