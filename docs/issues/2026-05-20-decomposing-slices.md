---
title: decompose-misses-gating-constraint-outside-foundation
stage: decompose (with foundation implication)
surfaced-in: a project using the skills
detected-by: 2026-05-17
issue-class: methodology, skill-procedure
status: resolved
---

# decomposing-slices cannot cross-check a gating constraint that lives outside Foundation

## Summary

A slice contradicted a binding project constraint, but that constraint lived outside the
Slicer artifacts, and `decomposing-slices` reads only `docs/foundation.md` — so the
contradiction passed unchecked.

## Problem

A slice ("ask N predefined questions") contradicted a binding project constraint: that
dialogue depth must be calibrated per request, not run as a fixed script. The constraint
was recorded in a separate project document, outside the Slicer artifacts.
`decomposing-slices` Step 1 reads only `docs/foundation.md`, so the contradiction was
invisible to it, and the slice was written. It was eventually caught at Select time — but
because the user noticed it, not because any procedure surfaced it.

## Root cause

Not "the constraint was not in a Slicer document." That framing is a symptom. The real
cause:

1. **A gating constraint is destination material.** A constraint strong enough to
   invalidate a slice is, by definition, part of what "done" means — it is a property of
   the End Goal. It belonged *in* `docs/foundation.md`. `defining-foundation` never elicits
   constraints alongside the End Goal, so it escaped into a side document.
2. **`decomposing-slices` Step 5 checks only slice size.** It evaluates
   right-size / too-big / too-small, never consistency with Foundation. Even with the
   constraint in Foundation, Step 5 would not have looked.

The defect is that a slicing-gating constraint lived outside the one document Decompose
reads — independent of what that outside document is named.

## Remediation

- `skills/defining-foundation/SKILL.md` Section 1 — add a constraint-elicitation question
  paired with the End Goal question, scoped to gating constraints only (destination scope,
  not path).
- `skills/decomposing-slices/SKILL.md` Step 5 — add a Foundation-contradiction check
  alongside the existing size check: does this slice line contradict the End Goal, a Core
  Value, or a stated constraint?
- Explicitly rejected: a dedicated constraints document or a fourth skill. The 3-skill
  minimalism is preserved; gating constraints are recognized as Foundation content, not a
  new artifact.

## Verification

Resolved by the Phase 2 edits of the 2026-05-20 improvement pass. Verified by structural
review:

- `skills/defining-foundation/SKILL.md` Section 1 now asks a third question eliciting
  gating constraints alongside the End Goal, scoped by its parenthetical to constraints
  strong enough to define "done" — destination scope, not path.
- `skills/decomposing-slices/SKILL.md` Step 5 now carries a "Contradicts Foundation" check
  alongside the size checks. Walked against the recorded case, a slice line that
  contradicts the End Goal, a Core Value, or a stated constraint is now named to the user
  and routed to drop, rework, or a Foundation update.

With gating constraints landing in Foundation — the one document Decompose reads — and
Step 5 checking against it, a slice contradicting a binding constraint is caught without
any new document or skill.

## Follow-ups

Constraint / risk mapping as a standalone activity is out of Slicer scope — that is
Brainstorming/Plan territory. Only *gating* constraints (those that define what "done"
means) belong in Foundation. A project keeping its own separate document for non-gating
concerns is a legitimate project artifact and not Slicer's to absorb.

## Related

- `2026-05-20-selecting-slice.md`
- `skills/defining-foundation/SKILL.md`
- `skills/decomposing-slices/SKILL.md`
