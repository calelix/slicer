# 01 — Problem

The full problem statement lives at [`docs/methodology/01-problem.md`](../../../docs/methodology/01-problem.md) in this repository.

In short: when an AI is given a goal and asked to brainstorm, plan, or generate code, it tends to reach for the ultimate goal in *one big inference*, filling unanswered blanks with its own reasoning. The same failure mode reproduces at three depths (surface, depth 1, depth 2). The `selecting-slice` skill exists to keep the *closed demo line + 1–3 Visible Outcomes* answer clean from in-slice path-level inference, and to prevent two specific Select-stage failures: DoD inflation (long checklists that invite implementation detail) and slice rubber-stamping (the user accepting an AI-proposed slice without engaging with it).

Read the canonical document for the full diagnosis.
