# Issues

A lightweight log of methodology and skill defects found by using Slicer.

## Why this exists

Slicer is a methodology expressed as three skills. When a skill procedure, an output
format, or a methodology principle has a gap, that gap is itself a *blank* — and by
Slicer's own Principle 2, a blank is recorded honestly, not silently patched. An issue
file is that honest record: it states the problem, names the root cause, fixes the
remediation, and notes how the fix was verified.

This is not a general bug tracker. It tracks defects in the *methodology and its three
skills*, not in any project that happens to use them.

## Convention

- One file per issue, in this directory.
- Filename: `YYYY-MM-DD-<skill-name>.md` — the date the issue was recorded, plus the name
  of the skill the issue primarily concerns (`defining-foundation`, `decomposing-slices`,
  or `selecting-slice`; for a cross-stage issue, use the skill where the fix primarily
  lands).
- Each file follows the template below. The issue's unique identifier is its `title`
  slug (see Fields), not its filename — two issues may concern the same skill on the same
  day.
- `status` moves `open` → `partial` → `resolved`. A record is written when the issue is
  identified (`open`) and updated in place as remediation lands.

## Fields

- **title** — a short kebab-case slug naming the issue. This is the issue's unique
  identifier; the filename is not.
- **stage** — where the defect lives: `foundation`, `decompose`, `select`, or
  `cross-stage`.
- **surfaced-in** — the general context where the defect showed up. Keep this
  Slicer-independent — name a stage or write "a project using the skills", not a
  specific project.
- **detected-by** — the date the defect was recorded, plus only context that is
  genuinely necessary.
- **issue-class** — one or more of: `methodology` (a gap in the principles or the problem
  model), `skill-procedure` (a gap in a SKILL.md procedure step), `output-format` (a gap
  in an artifact's section rules).
- **status** — `open`, `partial`, or `resolved`.
- **Summary** — one sentence.
- **Problem** — what went wrong, concretely.
- **Root cause** — the underlying cause, stated precisely, not the symptom.
- **Remediation** — the edits made or planned, named at file level.
- **Verification** — how the fix was confirmed.
- **Follow-ups** — anything deliberately left open.
- **Related** — links to other issue files, skills, or commits.

## Template

```
---
title: <short-kebab-case-slug>
stage: <foundation | decompose | select | cross-stage>
surfaced-in: <general context, e.g. a project using the skills>
detected-by: <date>
issue-class: <methodology | skill-procedure | output-format>
status: <open | partial | resolved>
---

# <readable issue title>

## Summary
<one sentence>

## Problem
<what went wrong>

## Root cause
<the underlying cause>

## Remediation
<the edits, file-level>

## Verification
<how the fix was confirmed>

## Follow-ups
<anything left open>

## Related
<links>
```
