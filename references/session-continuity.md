# Session continuity — logging & resuming

Two files, different cadences:

1. **`sessions/week-XX/YYYY-MM-DD.md`** — incremental daily log, appended
   *as significant exchanges happen* (not verbatim, not per-message).
2. **`sessions/CURRENT_STATE.md`** — rolling "where are we now" overview, updated
   when state meaningfully changes.

The `session-log` agent owns both (see `references/agents/session-log.md`).

## Resume (cold start)

**Read only these three by default** — they are the resume backbone and must be
kept current:

1. `project/DASHBOARD.md` — fastest "where is everything" snapshot.
2. `sessions/CURRENT_STATE.md` — the narrative of where we left off and why.
3. `project/mission.md` — re-anchor on the goal.

Then **check active workstreams** alongside `project/systems/` — both
project-level (`project/workstreams/`) and nested
(`project/systems/<system>/workstreams/`). Active workstreams may have
in-progress tasks or unresolved open questions that need attention. The
dashboard's Workstreams table provides the quick overview (its Owner column
shows project vs system ownership); `CURRENT_STATE.md` should reference active
workstreams and their current focus.

Then **check for outstanding decision conflicts** — if `sessions/conflicts/`
exists, scan it for any `CONFLICT-*.md` files. If any exist with `status: open`,
summarise them and ask the user to resolve before continuing. Conflicts block
safe advancement.

Then skim **`project/overview/TASKS.md`** — the global task roll-up is the
actionable "what's next": in-progress and blocked items across every system
and workstream, in one table.

Then read the root `AGENTS.md` for house rules (and any system `AGENTS.md` for
systems you'll work in — most systems won't have one).

## Context budget — avoid front-loading history

Raw daily logs are **reference material, not default context**. Do **not** load
several days of logs on start — it degrades signal-to-noise and invites drift.
Only drill into a recent daily log if `CURRENT_STATE.md` is missing a detail you
actually need.

## Memory pointer

In addition to the project wiki, save a **compact pointer** so the agent knows
the project exists even before loading this skill. Where to store it depends on
the host — see your host file (`references/hosts/<host>.md` → "Memory pointer").
Persistent memory if the host has it; otherwise the planning repo's root
`AGENTS.md`:

```
Atlas project '<name>' at <abs-path>. Phase: <phase>. Key focus: <one line>. Active systems: <list>. Active workstreams: <list>.
```

Update this pointer when phase or focus meaningfully changes (not every session).
Keep it to 1-2 lines — it's a signpost to the project, not a duplicate of
`CURRENT_STATE.md`. Without this, a fresh session that doesn't mention Atlas has
no idea the project exists.

## Recalling past sessions

If `CURRENT_STATE.md` is missing a detail about *why* a decision was made or
*what* was discussed, use the host's session/history search before reading
daily logs, if it has one (see your host file → "Session search"). It's often
faster and more targeted than scanning log files. Daily logs remain the durable
written record; session search is the fast path to the conversation context.

## Weekly roll-up (compaction)

To keep `CURRENT_STATE.md` (and loaded context) from growing unbounded, perform a
**roll-up** when a week's logs accumulate or `CURRENT_STATE.md` grows past a
comfortable scan length:

1. **Compact** the closing week's salient context — decisions, direction shifts,
   unresolved threads — into `CURRENT_STATE.md` (complete but compact).
2. **Archive, don't delete** — leave the week's daily logs in `sessions/week-XX/`
   as immutable history; they are not loaded on cold start.
3. **Start clean** — begin the new `sessions/week-YY/` folder.

Trigger is size / turn-of-week (not a fixed clock time); a natural cadence is
weekly. The roll-up can piggyback on the cron audit.

## End of session (light touch)

When wrapping up:

1. Append a final daily-log entry with a "next steps" summary.
2. Update `CURRENT_STATE.md` if state changed.
3. **Verify the roll-ups held.** Under the inline rule, `overview/TASKS.md`,
   `overview/DECISIONS.md`, the OQ/risk indexes, and dashboard rows should
   already mirror this session's local changes. Spot-check briefly and correct
   any drift now — this is a check, not a deferred maintenance pass (a miss
   here is exactly what the inline rule exists to prevent).
4. Update system/workstream INDEX.md Contents sections only if content was
   added/changed without being
   indexed inline (same spirit as step 3 — a verification pass).
5. **Capture any decisions not yet captured.** Decision Records are written
   mid-session at the moment of choice — but review the conversation once more
   for anything missed. Each DR gets its file *and* its
   `overview/DECISIONS.md` row (see `references/agents/decision-record.md`).
6. **Check for decision conflicts.** The pre-DR conflict scan runs at capture
   time; if this session's DRs contradict an active DR without superseding it,
   flag now — the next audit will formalise any conflict.
7. **Save the memory pointer** if phase or focus changed (see "Memory pointer"
   above).

## Mid-session decision capture

Don't wait for end-of-session to capture decisions. When a significant choice is
made during a conversation, create the Decision Record immediately — this is
faster, the context is fresh, and it prevents "I meant to capture that" drift.
The router table in `SKILL.md` lists "Decide" as a first-class verb for this
reason: as soon as you hear "let's go with X" or "we decided Y", pause and
capture it.
