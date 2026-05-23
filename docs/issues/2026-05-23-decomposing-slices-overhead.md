---
title: slicer-skill-calls-are-loud-on-clean-state-and-repeat-menu-detail
stage: cross-stage
surfaced-in: a project using the skills
detected-by: 2026-05-23
issue-class: skill-procedure, output-format
status: resolved
---

# Slicer skills emit Step 1 output and full Step 2 menu detail on every call, making a multi-call cycle feel verbose

## Summary

The three Slicer skills emit Step 1 context-check output and full Step 2
mode-menu detail on every invocation — even within a single slice cycle where
the same files are unchanged and the user has just seen the same menu. The
accumulated chatter makes a 3-4 call cycle feel verbose without adding new
information.

## Problem

On 2026-05-23, the user ran four Slicer calls in one cycle. Each call
re-emitted:

1. Step 1's full context-check output, including "0 unresolved Foundation
   Update Candidates" — a no-news line on a clean state.
2. Step 2's mode-menu with per-option descriptions, repeated verbatim from
   the prior call within the same session.
3. Termination and deflection messages that named the next abstract stage
   ("the next stage is Select") rather than the specific next call and its
   mode.

Some of the apparent overhead was caused by Defect ① (manual H3 sync needed
because of cross-document drift); that portion is now absorbed by the
Defect ① remediation. The residual overhead remains an output-only verbosity
problem.

## Root cause

- Step 1 emits its check bullets unconditionally rather than only when there
  is an actionable item.
- Step 2 menus tend to be expanded with per-option descriptions inline; once
  the user is mid-cycle the descriptions are noise.
- Termination and deflection messages stop at "the next stage is X" or
  "consider re-running Y" without naming the specific next call + mode, even
  when one is clearly indicated (e.g., Select Step 6 deflects to "re-run
  `decomposing-slices`" without saying which menu option to pick).
- A session-state cache that could elide Step 1 entirely is outside Slicer's
  scope (3-skill minimalism, no 4th artifact, no inference-driven mode
  auto-detection). Output-only reductions are what is in scope.

## Remediation

- All three SKILL.md files, Step 1 — added a *silent-on-clean* directive:
  Step 1's checks still run, but Step 1 emits output only when there is an
  actionable item (missing prerequisite, unresolved-FUC count `> 0`,
  dangling Active, state preview required by update mode). The
  unresolved-FUC count bullet is now explicitly conditional on `N > 0`.
- `skills/decomposing-slices/SKILL.md` Step 2 and
  `skills/selecting-slice/SKILL.md` Steps 2/4 — added a *label-only*
  directive: present the question with option labels as written; do not
  elaborate per-option descriptions in the response unless the user asks
  about a specific option.
- `skills/decomposing-slices/SKILL.md` Step 5 and Step 8, and
  `skills/selecting-slice/SKILL.md` Step 6 — refined deflection and
  termination messages to name the specific next call and menu option
  (e.g., "Run `/decomposing-slices` and pick **Clarify a slice**" instead
  of "consider re-running `decomposing-slices`").

## Verification

Confirmed by structural review of the three SKILL.md files after the edits
land, and by walking the 2026-05-23 four-call cycle against the revised
procedures: on a clean state, Step 1 emits nothing; the Step 2 menus appear
as labels only; deflection and termination messages give a concrete
next-call instruction.

## Follow-ups

Session-state caching remains explicitly out of scope (3-skill minimalism,
write-scope rule). If future use surfaces friction beyond what these
output-only changes address, that is a separate methodology decision.

## Related

- `2026-05-23-selecting-slice.md` (Defect ①: H3 sync — Defect ④ rests
  partly on ①'s remediation absorbing the manual-sync step)
- `2026-05-23-decomposing-slices.md` (Defect ②)
- `2026-05-23-decomposing-slices-clarify.md` (Defect ③)
- `2026-05-20-defining-foundation.md` (dead-letter-queue precedent — same
  output-only loop-closing pattern)
- `skills/defining-foundation/SKILL.md`
- `skills/decomposing-slices/SKILL.md`
- `skills/selecting-slice/SKILL.md`
