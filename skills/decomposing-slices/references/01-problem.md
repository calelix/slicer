# 01 — Problem

The full problem statement lives at [`docs/methodology/01-problem.md`](../../../docs/methodology/01-problem.md) in this repository.

In short: when an AI is given a goal and asked to brainstorm, plan, or generate code, it tends to reach for the ultimate goal in *one big inference*, filling unanswered blanks with its own reasoning. The same failure mode reproduces at three depths (surface, depth 1, depth 2). The `decomposing-slices` skill exists to keep the *what slices and in what order* answer clean from in-slice path-level inference, and to keep decomposition itself from becoming a small one-big-inference by operating incrementally — one slice cycle at a time.

Read the canonical document for the full diagnosis.
