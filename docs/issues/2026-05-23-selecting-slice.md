---
title: active-slice-h3-stale-after-decompose-mutation
stage: cross-stage
surfaced-in: a project using the skills
detected-by: 2026-05-23
issue-class: skill-procedure
status: resolved
---

# Active slice's H3 in select.md goes stale after Decompose mutates the slice's demo line

## Summary

`selecting-slice` says the Active slice's H3 header *is* its identifier in
`docs/select.md` — a verbatim copy of a Confirmed Slice's demo line in
`docs/slices.md`, which is how this file names which Confirmed Slice is
currently Active. But no skill maintains that identifier link when Decompose
later edits, splits, merges, or deletes the slice — so `docs/select.md`'s
Active identifier can silently come to name a slice that no longer exists.

## Problem

On 2026-05-23, during a Select call, the user noticed that an already-Active
slice's demo line needed wording polish (the host project name had to be made
explicit). The user invoked `decomposing-slices` and used Clarified to refine
the wording in `docs/slices.md`. After the Decompose call returned,
`docs/select.md`'s Active H3 still carried the *old* demo line — the identifier
link declared by Select ("the H3 names which Confirmed Slice is Active,
verbatim") was broken on disk: select.md was now pointing at a demo line that
no longer existed in `docs/slices.md`'s Confirmed Slices. No skill procedure
surfaced this. The user had to make a mechanical Edit outside any skill to
restore the identifier, then a separate commit.

This was not specific to Clarified: Split, Merged, and Deleted in Decompose can
also mutate a Confirmed Slice's demo line or remove the slice entirely. In any
of those cases, an Active slice in `docs/select.md` would be left dangling.

## Root cause

The identifier link ("the Active H3 names a current Confirmed Slice,
verbatim") was declared in `selecting-slice` but no skill was assigned
responsibility for keeping that link valid:

1. `selecting-slice` reads `docs/slices.md` only at slice-naming time (Step 3).
   After that, the H3 is frozen in `docs/select.md`. No procedure re-reads
   slices.md later to check for drift.
2. `decomposing-slices` writes to `docs/slices.md` but does not read
   `docs/select.md`, so it cannot detect that its own write has invalidated
   an Active H3 in the consumer document.

Structurally identical to the 2026-05-17 dead-letter-queue defect
(`2026-05-20-defining-foundation.md`) but reversed: there, the producer
(Decompose/Select flagging FUC) existed but no consumer read them. Here, the
write-once path (Select copies H3 at slice-naming) exists but no re-sync
consumer reads slices.md again later.

## Remediation

- `skills/decomposing-slices/SKILL.md` Step 8 — after writing `docs/slices.md`,
  read `docs/select.md` (read-only). If select.md's Active H3 (the Active
  slice's identifier) no longer names any current Confirmed Slice, print a
  one-line nudge directing the user to `selecting-slice` Re-sync mode.
- `skills/decomposing-slices/SKILL.md` "What this skill does NOT do" — clarified
  that the drift check reads select.md but never writes to it.
- `skills/selecting-slice/SKILL.md` Step 1 — added a drift-detection bullet
  that flags a *dangling Active* when its H3 — the Active slice's identifier —
  no longer names any current Confirmed Slice in slices.md.
- `skills/selecting-slice/SKILL.md` Step 2 — added a fourth menu option,
  "Re-sync Active H3 from `docs/slices.md`", and a notice prepended when
  dangling Active is detected.
- `skills/selecting-slice/SKILL.md` new Step 4b — user names which current
  Confirmed Slice the Active corresponds to; the H3 is overwritten with that
  slice's current demo line; closed demo line and Visible Outcomes remain
  untouched. If the slice is truly gone, the user is directed to Close mode.
- `skills/selecting-slice/SKILL.md` Update mode recap — re-sync branch added.

## Verification

Confirmed by structural review of `skills/decomposing-slices/SKILL.md` and
`skills/selecting-slice/SKILL.md` after the edits land, and by walking the
2026-05-23 case scenario against the revised procedures: a Decompose call that
edits an Active slice's demo line now ends with a one-line drift nudge, and a
subsequent `selecting-slice` Re-sync call closes the loop by overwriting the
Active H3 from slices.md.

## Follow-ups

None.

## Related

- `2026-05-20-defining-foundation.md` (dead-letter-queue precedent)
- `skills/decomposing-slices/SKILL.md`
- `skills/selecting-slice/SKILL.md`
