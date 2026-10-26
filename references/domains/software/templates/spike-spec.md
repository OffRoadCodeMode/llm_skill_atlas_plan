---
title: Spike — <name>
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: spike-spec
status: proposed | running | concluded | abandoned
timebox: <hard limit, e.g. 2 days>
code_location: in-repo | external
code_ref: <path or URL — required when code_location is external>
mission_link: <how this spike serves mission.md>
---

# Spike spec — <name>

Written **before** running the experiment. The contract: one question, measurable
success criteria, a hard timebox.

## Question
[The single technical question this spike answers. One question per spike — if
you need two, write two spikes.]

## Hypothesis
[What we expect to be true, and why we think so.]

## Success criteria
[Measurable outcomes that let the spike conclude.
e.g. "sustains 5k msg/s for 10 min on a laptop", "cold start < 800 ms".]

## Approach
[What will be built and run. Keep the scope ruthlessly small — a spike is not
the feature, it's the cheapest experiment that answers the question.]

## Code location
[in-repo → code lives in this folder under `code/`. external → see `code_ref`
in frontmatter. Confirm with the user before running.]

## Timebox
[Hard limit. When it expires: conclude with findings so far, or escalate to the
user. Spikes never quietly extend.]

## Unblocks
[Which design decision, requirement, or open question this spike gates.
Link the entry in the system's open-questions.md if it has one.]

## Links
- Results: [results.md](results.md) (written when concluded)
- Informs: [design decision / requirement / OQ-XXX]
- Belongs to: [system]
