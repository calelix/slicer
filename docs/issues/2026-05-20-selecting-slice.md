---
title: select-visible-outcome-absorbs-undecided-path
stage: select
surfaced-in: a project using the skills
detected-by: 2026-05-19
issue-class: skill-procedure
status: resolved
---

# selecting-slice's Visible Outcome absorbed an undecided implementation path

## Summary

A Visible Outcome written in `selecting-slice` Step 6 baked in an undecided output path,
so the slice's Definition of Done broke when a downstream stage legitimately chose a
different path.

## Problem

A Visible Outcome was recorded as "invoking the skill from a specific directory produces a
spec file at that path." The clause "at that path" fixes an output *location* — a
path-level decision that had not been made at Select time. When a downstream stage
legitimately placed the file elsewhere, the Visible Outcome was no longer satisfiable and
the DoD was effectively wrong.

"A spec file is produced" is a destination — an externally observable result. "It lands at
a particular path" is a path. The Outcome conflated the two.

## Root cause

Three procedural gaps, plus a Principle 2 violation:

1. **Step 6 has no per-Outcome destination/path filter.** Its only guards are count (1–3
   OK, 4+ rejected) and ambiguity (0 = too vague). Path detail can enter *inside* a single
   Outcome without lengthening the list, so the count cap never catches it.
2. **The DoD-inflation model is framed purely as a count problem.** The Background and
   `references/01-problem.md` describe "DoD inflation" only as "5+ items invite path-level
   detail." A locator baked into one Outcome is a separate channel the count cap does not
   defend.
3. **Step 8 deflection nets only two cases** — "in-slice implementation choice" and "new
   tech decision." An observation that legitimately carries an implementation locator is
   neither: the produced file genuinely *is* externally observable, so the locator entered
   through the front door, as the answer to Step 6's own question.
4. **Principle 2 violation.** The output location was an undecided blank at Select time. It
   was filled with a plausible default and pinned into the DoD.

`decomposing-slices` already has a symmetric strip rule for slice lines ("if such detail
leaks in... strip it before writing"). `selecting-slice` had no equivalent for Visible
Outcomes.

## Remediation

- `skills/selecting-slice/SKILL.md` Step 6 — add a per-Outcome destination/path scan
  before each Outcome is finalized.
- `skills/selecting-slice/SKILL.md` Background — reframe "DoD inflation" to name the
  in-Outcome channel as a defense line separate from the count cap.
- `skills/selecting-slice/SKILL.md` Step 8 — add a third deflection case: an observation
  accompanied by an implementation locator → strip the locator, keep the observation.
- `skills/selecting-slice/references/01-problem.md` and `references/02-principle.md` —
  reframe the DoD-inflation parenthetical; add a path-baked / stripped anti-example pair.
- Route a location that later becomes a fixed contract through the existing "Refine
  current Active's DoD" mode — pinned *after* the decision, never as a guessed default
  before it.

## Verification

Resolved by the Phase 1 edits of the 2026-05-20 improvement pass. Verified by structural
review of `skills/selecting-slice/SKILL.md` and its `references/`:

- Step 6 now runs a per-Outcome destination-locator scan before each Outcome is finalized.
  Walked against the recorded Outcome, "invoking the skill from a specific directory
  produces a spec file at that path" is caught and stripped to "produces a spec file."
- Step 8 carries a third deflection case for an observation accompanied by an
  implementation locator — the same input volunteered mid-question.
- The Background and `references/01-problem.md` reframe DoD inflation as two channels
  (by count, by depth); `references/02-principle.md` carries a path-baked / stripped
  anti-example pair.

This is a procedure change verified by review and scenario walk-through; confirmation in
real use is the remaining check.

## Follow-ups

None.

## Related

- `2026-05-20-decomposing-slices.md`
- `skills/selecting-slice/SKILL.md`
