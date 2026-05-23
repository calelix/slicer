---
title: decompose-menu-lacks-slice-state-preview
stage: decompose
surfaced-in: a project using the skills
detected-by: 2026-05-23
issue-class: skill-procedure
status: resolved
---

# Decompose's Step 2 mode menu hides the current slice state — letting users pick the wrong kind-of-change

## Summary

Step 2 of `decomposing-slices` (the multi-select Kind-of-change menu) presents
verb options without showing the current state of slices, leaving the user to
choose by memory alone — which makes it easy to pick an action that doesn't
fit what slices.md actually contains.

## Problem

On 2026-05-23, a user invoked `decomposing-slices` intending to Add a new
slice and picked Add from the Step 2 menu. Mid-conversation, they realized one
of the existing Confirmed Slices (②) needed wording polish first, aborted the
Add, and switched to Clarify. The menu surface had given no indication of:

- Which Confirmed Slices currently existed and what they said (so the user
  could be reminded "oh, ② needs polish").
- Which slices had been worked on before, and with what outcome (`completed`,
  `paused`, etc., recorded in select.md History).
- Whether a slice was currently Active in select.md.

The unresolved-FUC count was already printed, but separately from per-slice
state and without per-slice resolution.

## Root cause

Step 2 of `decomposing-slices` is a pure verb-selector. Step 1's context check
read foundation.md and slices.md but treated select.md only as one of two
sources for the FUC count bullet — so the cross-document state needed for an
informed kind-of-change decision was not collected before the menu.

A single per-slice completion-status field on slices.md does not exist
(priority is present, but completion status lives in select.md's History). The
state the user wanted to see was therefore inherently *cross-document*; Step 1
needed to join slices.md and select.md before Step 2 could meaningfully
surface it.

## Remediation

- `skills/decomposing-slices/SKILL.md` Step 1 — added a state-preview table,
  emitted in update mode only (skipped on first call). The table joins
  slices.md (Confirmed Slices, Tentative Slices) with select.md (Active marker
  via "★" on the row number, per-slice last-cycle status drawn from History)
  so the user sees the full current state before the Step 2 verb menu.
- The preview is quote-and-count only — verbatim demo lines, no
  recommendations or interpretation, preserving Principle 2 (no blank-filling
  by inference).

## Verification

Confirmed by structural review of `skills/decomposing-slices/SKILL.md` after
the edit lands, and by walking the 2026-05-23 case scenario against the
revised Step 1: with the preview table showing every Confirmed Slice's demo
line just before the menu, the user would have recognized "② needs polish"
and chosen Clarify instead of Add.

## Follow-ups

None.

## Related

- `2026-05-23-selecting-slice.md` (Defect ①: H3 sync — Decompose now also
  reads select.md at Step 8 to detect drift; this defect adds the symmetric
  read at Step 1)
- `skills/decomposing-slices/SKILL.md`
