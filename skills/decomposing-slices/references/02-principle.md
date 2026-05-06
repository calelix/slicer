# 02 — Principle

The full principle statement lives at [`docs/methodology/02-principle.md`](../../../docs/methodology/02-principle.md) in this repository.

In short: two principles govern the early-stage skills.

1. **Each stage answers only its own destination, leaving the next stage's path blank.** For Decompose, the destination is the slice(s) currently visible and their priorities. The path — how each slice is implemented — is left to Select and downstream. Decompose runs incrementally, once per slice cycle.
2. **The AI does not fill blanks by inference.** Within destination scope, only what the user explicitly stated is recorded. Missing values become `TBD: <reason>`, never guessed. For `decomposing-slices`, this also means the AI does not propose slices unless the user explicitly signals being stuck (the B4 fallback) — and even then the user must restate any chosen candidate in their own words.

Read the canonical document for the full table of stages and per-stage application.
