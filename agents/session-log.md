# Agent: session-log

**Build first.** Maintains session continuity: the incremental daily log and the
rolling `CURRENT_STATE.md`. Without this, continuity across sessions breaks.

## Two files

1. **`sessions/week-XX/YYYY-MM-DD.md`** — appended *as significant exchanges
   happen*, not per message and not verbatim. Brief summaries of what was discussed
   and decided; captures the organic evolution — concerns raised, directions
   shifted, decisions made and why.
2. **`sessions/CURRENT_STATE.md`** — rolling "where are we now", updated when state
   meaningfully changes (decisions, files created, phase transitions).

## When to append to the daily log

Append when something **meaningful** happens — NOT on every message:

- A decision is made.
- A new concern or question is raised.
- Files are created or significantly changed.
- The user's thinking shifts direction.
- A phase transition occurs.

The goal is a narrative of how the project evolved, not a transcript.

## Daily log format

```markdown
# Session Log — YYYY-MM-DD

## HH:MM — [short title]
[2–4 lines: what was discussed / decided / changed, and why.]
```

## Updating CURRENT_STATE.md

Keep it *complete but compact* — the single narrative a cold start reads. Update it
when state changes; do not let it become a running transcript. It should always
answer: what are we doing, where is each system, what's the current focus, what's
blocking, what's next.

## Weekly roll-up (compaction)

When a week's logs accumulate, or `CURRENT_STATE.md` grows past a comfortable scan
length:

1. Compact the closing week's salient context into `CURRENT_STATE.md`.
2. Leave the week's daily logs in place as immutable history (archive, don't
   delete); they are not loaded on cold start.
3. Start the new `sessions/week-YY/` folder clean.

Trigger is size / turn-of-week; can piggyback the cron audit. See
`session-continuity.md`.

## Cold start (Resume)

Read `DASHBOARD.md` + `CURRENT_STATE.md` + `mission.md` only; drill into a recent
daily log solely if a needed detail is missing. Never front-load multiple days of
logs.
