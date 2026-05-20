---
title: foundation-update-candidates-have-no-consumer
stage: cross-stage
surfaced-in: a project using the skills
detected-by: 2026-05-17
issue-class: skill-procedure
status: resolved
---

# Foundation Update Candidates are produced but never consumed or cleared

## Summary

`decomposing-slices` and `selecting-slice` both write `## Foundation Update Candidates`
sections, but `defining-foundation` update mode never reads them and nothing clears a
resolved candidate — a producer with no consumer, a dead-letter queue.

## Problem

Decompose flags candidates into `docs/slices.md`; Select flags them into
`docs/select.md`. `defining-foundation` update mode Step 2 asks only "which section(s) do
you want to update?" — it never reads either `## Foundation Update Candidates` section.
Flagged candidates are write-only.

There is also no resolution protocol: even after a candidate is folded into Foundation, it
stays in the candidate list, so resolved and unresolved items are indistinguishable and
the list rots. In practice, flagged candidates accumulate and go stale, with no procedure
ever consuming them. Select cannot modify `docs/slices.md`, so slices.md candidates are
especially stuck.

## Root cause

The candidate-flagging mechanism was specified producer-side (Decompose and Select write
the section) but never consumer-side. `defining-foundation` update mode has no intake step
that reads the candidate sections, and no skill has a clearing protocol. A flag with a
producer and no consumer is a dead-letter queue, not a loop.

## Remediation

- `skills/defining-foundation/SKILL.md` update mode — add an intake step (between current
  steps 1 and 2) that reads `docs/slices.md` and `docs/select.md`, collects every
  `## Foundation Update Candidates` item, and presents unresolved ones before the
  section-selection question.
- `skills/defining-foundation/SKILL.md` update mode — a clearing protocol: a narrow,
  append-only exception letting update mode mark a folded-in candidate
  `resolved in foundation.md on <date>` in its source file. This is the only case in which
  `defining-foundation` writes a file other than `docs/foundation.md`.
- `skills/decomposing-slices/SKILL.md` and `skills/selecting-slice/SKILL.md` Step 1 — a
  pending-count nudge: print the count of unresolved candidates and suggest running
  `defining-foundation` update mode. Count only, no inference.

## Verification

Resolved by the Phase 3 edits of the 2026-05-20 improvement pass (clearing protocol
Option 3A). Verified by structural review of `skills/defining-foundation/SKILL.md` update
mode:

- A new step 2 reads `docs/slices.md` and `docs/select.md` and surfaces unresolved
  `## Foundation Update Candidates` before the section-selection question — the consumer
  the loop was missing.
- A new step 7 appends a `resolved in foundation.md on <date>` marker to each folded-in
  candidate's bullet in its source file (append-only, user-confirmed); the "What this skill
  does NOT do" list records this as the single, narrow write-scope exception.
- `decomposing-slices` and `selecting-slice` output formats now state the marker must be
  preserved verbatim on rewrite, so a later producer call cannot silently undo a clearing.
- `decomposing-slices` and `selecting-slice` Step 1 print an unresolved-candidate count on
  each call (count only, no inference).

Update-mode step numbering re-checked end to end (steps 1–8, with step 2 referencing
step 7). The producer→consumer loop is now wired and self-clearing.

## Follow-ups

The clearing protocol (step 7) expands `defining-foundation`'s write scope beyond
`docs/foundation.md`. This is a deliberate, append-only, user-confirmed exception —
recorded here because it changes a structural boundary the rest of the design relied on.

## Related

- `skills/defining-foundation/SKILL.md`
- `skills/decomposing-slices/SKILL.md`
- `skills/selecting-slice/SKILL.md`
