# The Two Principles

The `defining-foundation`, `decomposing-slices`, and `selecting-slice` skills share two principles. They exist for the failure mode described in [01-problem.md](01-problem.md) — the tendency to reach the goal in *one big inference*.

## Principle 1 — Each stage answers only its own destination, leaving the next stage's path blank

A stage answers what belongs at its own level and intentionally leaves the next level open. The destination at this level should not contain the path to the next.

| Stage      | Answers (destination)                        | Leaves blank (path)                              |
| ---------- | -------------------------------------------- | ------------------------------------------------ |
| Foundation | Why, what, initial tooling                   | How to slice the work                            |
| Decompose  | Slices currently visible, with priorities | How to implement each slice |
| Select     | The chosen slice's closed demo line + 1–3 Visible Outcomes | The implementation procedure for that slice      |

The same shape repeats at each level. This is the answer to the depth-2 reproduction of the failure mode (see [01-problem.md](01-problem.md)) — even fine slicing is not enough; each level must apply the same discipline.

## Principle 2 — The AI does not fill blanks by inference

Within its own destination scope, a skill records only what the user has explicitly stated. It does not:

- Guess or paraphrase into specificity
- Invent details to make the document feel complete
- Smooth over gaps with plausible defaults

When a value is missing, the skill either asks the user or marks the slot explicitly as `TBD: <reason>`. A blank that is honestly blank is always preferable to a blank that has been silently filled.

This is the enforcement mechanism for Principle 1. Without it, an LLM will draw the destination/path boundary itself — using inference — and fall back into the same trap at a finer scale. The two principles must travel together.

## How the principles apply per stage

Each stage is a thin restatement of the same shape:

- **Foundation:** ask only about destination (why, what, with what). Defer any path question to Decompose. Mark any unanswered destination value as `TBD`.
- **Decompose:** ask only about slicing — what slice(s) are currently visible and in what order. Decompose runs incrementally: each call derives or updates the slice(s) visible right now, not a full N-list. Defer any in-slice implementation question to Select or downstream. Mark any unanswered slicing value as `TBD`.
- **Select:** ask only about the chosen slice's closed demo line and its 1–3 Visible Outcomes. Defer any in-slice implementation procedure to downstream. Mark any unanswered DoD value as `TBD`.

The pattern is the same; only what counts as destination at this level changes.
