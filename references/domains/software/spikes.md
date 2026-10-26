# Software domain pack — spikes

A **spike** is a timeboxed, hands-on experiment that answers a technical question
research alone can't settle: *can this queue sustain 5k msg/s? will the model fit
in 8 GB VRAM? does this library handle our edge case?* Where research synthesises
existing knowledge, a spike produces **code and measurements**.

Spikes are a first-class part of the software pack's **design** phase — the exit
gate requires spike prototypes for risky components (see `phases.md`).

## Where spikes live

Spikes belong to a **system**, in their own top-level folder — separate from
`research/`:

```
<system>/spikes/
└── queue-throughput/          ← one folder per spike, kebab-case
    ├── spec.md                ← the definition — written BEFORE running
    ├── results.md             ← the findings — written when concluded
    └── code/                  ← optional: spike code, if in-repo (see below)
```

Why separate from `research/`: research docs are desk work (surveys, comparisons,
source synthesis). A spike has a controlled lifecycle — **spec → run → results** —
and the spec/results split makes that lifecycle explicit. Mixing them would let
measured results blur into prose research, and would hide which experiments are
still open.

Create `<system>/spikes/` when the first spike is proposed — don't pre-create it.

## The spec / results split

Two files, two moments:

- **`spec.md`** (template: `templates/spike-spec.md`) — written **before** running
  the experiment. The question, hypothesis, success criteria, approach, timebox,
  and the code-location decision. The spec is the contract: it stops scope creep
  and lets the user approve the experiment before effort is spent.
- **`results.md`** (template: `templates/spike-results.md`) — written when the
  spike concludes. Outcome versus each success criterion, measurements, surprises,
  and next steps. Once concluded, results are **immutable history** — if findings
  are later invalidated, don't edit; capture it in a DR.

The split is deliberate: **anyone (or any later session) can read `results.md`
without re-running the experiment, and can check that the experiment actually
tested what it claimed by comparing against `spec.md`.**

## Code location: in-repo or external

Spike code's home is a **decision made at spec time — ask the user, don't
default silently**. Record it in `spec.md` frontmatter:

- `code_location: in-repo` → code lives in `spikes/<name>/code/`. Good for small
  throwaway scripts and benchmarks; keeps the experiment reproducible next to its
  results.
- `code_location: external` → code lives elsewhere (the project's code repo, a
  scratch repo) — set `code_ref:` to the path or URL. Good when the spike needs
  the real codebase or toolchain.

Naming note: spike code is **throwaway** unless the user promotes it. Production
code derived from a spike belongs in the project's code repo, not in `spikes/`.

## Lifecycle

Track `status:` in the spec frontmatter: `proposed` → `running` → `concluded`
(or `abandoned`).

1. **Propose** — identify the risky assumption (design discussion, open question,
   or audit gate). Write `spec.md` from the template; confirm approach and
   code location with the user.
2. **Run** — build the experiment, respecting the timebox. If the timebox expires,
   conclude with findings so far or escalate to the user — spikes never quietly
   extend.
3. **Conclude** — write `results.md`. If the outcome amounts to a decision
   ("yes, we can use library X"), **capture a DR immediately**
   (`references/agents/decision-record.md`) and link it from results.

Spikes can **unblock**: when a spike is raised against a gating question, note it
in the system's `open-questions.md` (with `blocks:` if it gates a phase) and
close the question when the spike concludes.
