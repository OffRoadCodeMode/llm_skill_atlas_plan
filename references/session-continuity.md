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

Then **check for outstanding decision conflicts** — scan `sessions/conflicts/` for
any `CONFLICT-*.md` files. If any exist with `status: open`, summarise them and
ask the user to resolve before continuing. Conflicts block safe advancement.

Then read the root `INDEX.md` for structure and the relevant `AGENTS.md`.

## Context budget — avoid front-loading history

Raw daily logs are **reference material, not default context**. Do **not** load
several days of logs on start — it degrades signal-to-noise and invites drift.
Only drill into a recent daily log if `CURRENT_STATE.md` is missing a detail you
actually need.

## Hermes memory pointer

In addition to the project wiki, save a **compact pointer** to Hermes persistent
memory so the agent knows the project exists even before loading this skill:

```
memory(
  action="add",
  target="memory",
  content="Atlas project '<name>' at <abs-path>. Phase: <phase>. Key focus: <one line>. Active systems: <list>. Active workstreams: <list>."
)
```

Update this pointer when phase or focus meaningfully changes (not every session).
Keep it to 1-2 lines — it's a signpost to the project, not a duplicate of
`CURRENT_STATE.md`. Without this, a fresh session that doesn't mention Atlas has
no idea the project exists.

## Recalling past sessions

If `CURRENT_STATE.md` is missing a detail about *why* a decision was made or
*what* was discussed, use Hermes's `session_search` tool before reading daily
logs — it's often faster and more targeted than scanning log files. Search by
keyword (e.g. `session_search(query="electrics certification")`) to find the
session where a topic was discussed. Daily logs remain the durable written
record; session_search is the fast path to the conversation context.

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
3. Update `DASHBOARD.md` if any system phase/status changed or workstream
   status changed.
4. Update INDEX.md files if content was added/changed (including workstream
   INDEX files).
5. **Capture any decisions made this session.** Review the conversation for
   choices that shaped the project direction — if any were made, create a
   Decision Record in the relevant system's `design/decisions/` folder or the
   workstream's `decisions/` folder (see `references/agents/decision-record.md`).
6. **Check for decision conflicts.** If this session created or modified DRs
   that contradict an existing DR in another system or workstream, flag them —
   the next audit run will formalise any conflict.
7. **Save the Hermes memory pointer** if phase or focus changed (see "Hermes
   memory pointer" above).

## Mid-session decision capture

Don't wait for end-of-session to capture decisions. When a significant choice is
made during a conversation, create the Decision Record immediately — this is
faster, the context is fresh, and it prevents "I meant to capture that" drift.
The router table in `SKILL.md` lists "Decide" as a first-class verb for this
reason: as soon as you hear "let's go with X" or "we decided Y", pause and
capture it.
