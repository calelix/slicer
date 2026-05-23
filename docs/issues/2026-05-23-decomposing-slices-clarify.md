---
title: decompose-clarify-menu-undersells-its-scope
stage: decompose
surfaced-in: a project using the skills
detected-by: 2026-05-23
issue-class: skill-procedure, output-format
status: resolved
---

# Decompose's Clarify menu item advertises only one of the two sub-cases the Clarified kind actually covers

## Summary

The Step 2 menu item for the `Clarified` kind reads "Clarify a tentative slice
into a confirmed slice", advertising only the promote sub-case. The same kind
is also legitimately used to refine the wording of an already-Confirmed slice
(precedent 2026-05-17, recurrence 2026-05-23), but a user wanting that
operation does not recognize their case in the menu.

## Problem

On 2026-05-23, a user invoked `decomposing-slices` to refine the wording of an
already-Confirmed slice (the host project name had to be made explicit). The
Step 2 menu showed:

> Clarify a tentative slice into a confirmed slice

That wording did not match the user's case — the target slice was already
Confirmed, not Tentative. The same pattern had occurred on 2026-05-17. The
underlying change kind `Clarified` was the right one; the menu wording was
mis-scoped.

The Change Log output format compounded the issue: it recorded both sub-cases
under a single `Clarified: "<short quote>"` line, so the historical record
could not later distinguish "this was a promotion from Tentative" from "this
was a rewording of an already-Confirmed slice".

## Root cause

A single change kind (`Clarified`) covers two sub-operations, but the Step 2
menu wording only advertises one. The Change Log output format collapses both
into a single line, losing the distinction in the historical record.

## Remediation

- `skills/decomposing-slices/SKILL.md` Step 2 — menu item rewritten to:
  `Clarify a slice — promote a tentative slice to confirmed, or refine the wording of a confirmed slice`.
- `skills/decomposing-slices/SKILL.md` Step 3 — added a paragraph describing
  the refine-wording sub-case flow: the user first names which Confirmed Slice
  to refine (by number or by quoting its current demo line), then states the
  new demo line using the same questions as for new slices.
- `skills/decomposing-slices/SKILL.md` Output format, Change Log section —
  extended the `Clarified` format to distinguish two sub-cases:
  - Promote: `Clarified (tentative→confirmed): "<old tentative line>"`
  - Rewording: `Clarified (rewording): "<old line>" → "<new line>"`

## Verification

Confirmed by structural review of `skills/decomposing-slices/SKILL.md` after
the edit lands, and by walking the 2026-05-23 case scenario against the
revised Step 2 menu: a user wanting to refine an already-Confirmed slice's
wording now recognizes their case in the menu wording and selects Clarify
without confusion.

## Follow-ups

None.

## Related

- `2026-05-23-decomposing-slices.md` (Defect ②, state preview that complements
  this menu wording)
- `skills/decomposing-slices/SKILL.md`
