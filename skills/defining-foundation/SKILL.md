---
name: defining-foundation
description: Use when starting a new project to create its Foundation Document
  — the destination (why, what, initial tooling) but not the path. First stage
  of the slicing methodology, before Decompose and Select. Outputs
  docs/foundation.md.
---

# Defining Foundation

Help the user define a new project's Foundation Document — the long-lived essentials that justify why the project exists, who it's for, and what it's built with. Foundation answers *destination*, not *path*. Decomposition into slices and slice selection are separate later stages and are not your concern here.

**Output:** `docs/foundation.md` in the user's project.

## When to use

- The user is starting a new project and wants to define its foundation.
- The user is retrofitting a Foundation Document onto an existing project.
- An existing `docs/foundation.md` needs targeted updates (the skill enters update mode).

## Background

The methodology this skill is part of rests on a specific failure mode: when an AI is given a Foundation and asked to brainstorm, plan, or generate code, it tends to reach for the ultimate goal in *one big inference*, filling unanswered blanks with its own reasoning and going further than warranted.

This skill exists to keep the destination (Foundation) clean from path-level inference. See `references/01-problem.md` for the full problem statement.

## Two principles you must follow

### Principle 1 — Answer destination, leave path blank

You answer only the destination at this level: why the project exists, who it's for, and what initial tooling it uses. You do *not* answer:

- How the project should be sliced into work units (that's the Decompose stage)
- What order to work in (that's Decompose and Select)
- How any feature should be implemented (that's downstream)

If the user volunteers a path-level answer ("I think we should start with auth," "we'll do this in three sprints"), acknowledge it and explicitly defer it: "That sounds like a decomposition decision — I'll leave it for the next stage." Do not absorb it into Foundation.

### Principle 2 — Do not fill blanks by inference

Within destination scope, record only what the user has explicitly stated. Do not:

- Guess or paraphrase into specificity
- Invent details to make the document feel complete
- Smooth over gaps with plausible defaults

When a value is missing, either ask the user or mark the slot as `TBD: <reason>`. A blank that is honestly blank is always better than a blank silently filled. See `references/02-principle.md` for the full principle statement.

## Procedure

**Note on language:** All questions in this file are written in English so that Claude's skill-matching mechanism works reliably; treat them as guides, not literal scripts.

Language conventions during execution:

- **Conversation (questions you ask, drafts you show for approval):** the user's language. Translate the English questions, and present approval drafts in the user's language so the user can verify intent in their own words.
- **Written artifact (`docs/foundation.md`):** English, regardless of the conversation language. After the user approves a section in their language, translate it to English when writing it to the file. Preserve proper nouns and user-supplied technical terms (product names, library names, domain jargon) as the user wrote them.

### Step 1 — Context check

Quickly inspect:

- Working directory layout (is this a new project? existing one?)
- Any existing `README.md` or `docs/` to understand current state
- Git state to see if work is in progress
- Whether `docs/foundation.md` already exists

If `docs/foundation.md` exists, switch to **update mode** (see below).

**Silent-on-clean.** Run these checks but emit Step 1 output only when there is an actionable item to surface (e.g., update mode triggered with unresolved Foundation Update Candidates from `docs/slices.md` or `docs/select.md`). If all checks pass quietly, proceed straight to Step 2 without printing a status line.

### Step 2 — Walk the three sections in order

The Foundation Document has three sections, in this fixed order:

1. **Core Problem & End Goal** — Why does this project exist? What does "done" look like, and what constraints must "done" honor?
2. **Vision / Target Users / Success / Core Values** — What is the vision? Who is it for? What does success look like to them? What values come first when there's a tradeoff?
3. **Tech Stack (Initial)** — What language(s)? What framework(s) or major libraries? Other large tooling decisions?

For each section:

- Ask one question at a time. Multiple-choice is preferred when there is a small set of distinguishable options; open-ended is fine when the answer is inherently free-form.
- After collecting the answer, write the section draft and present it to the user for explicit approval before moving to the next question or section.
- Any value the user did not state goes in as `TBD: <reason>`. Do not guess.

#### Questions for Section 1: Core Problem & End Goal

Ask, in order, until the answers are clear:

1. "What problem are you trying to solve? Who has this problem, and what's currently wrong with how it's being addressed?"
2. "What does 'done' look like for this project? When can it be called complete?"
3. "Are there constraints or non-negotiables the solution must honor — things that, if violated, mean the goal is effectively not met? (Capture only constraints this strong; ordinary preferences and implementation choices are not Foundation material.)"

#### Questions for Section 2: Vision / Target Users / Success / Core Values

Ask, in order, until all four are clear:

1. "What is the vision for this project? In a sentence or two, where is it heading?"
2. "Who is this project for? Be specific about the primary user."
3. "What does success look like *to those users*? What experience or outcome do they get?"
4. "What are the values that come first when there's a tradeoff? (e.g., simplicity over features, performance over flexibility, openness over control)"

#### Questions for Section 3: Tech Stack (Initial)

Ask, in order, until all are clear:

1. "What language(s) will the project use?"
2. "What framework(s) or major libraries will it depend on?"
3. "Are there any other large tooling decisions already made? (database, deployment target, runtime environment, etc.)"

### Step 3 — Path-question deflection

If at any point the user answers with a path-level decision ("we'll start with the login flow," "I want to do TDD," "let's split this into three milestones"), do this:

1. Acknowledge the input.
2. Note that it sounds like a decomposition or implementation decision.
3. Explain that Foundation answers destination only, and you will defer this to the appropriate later stage.
4. Return to the Foundation question you were on.

Do not write the path-level input into `docs/foundation.md`.

### Step 4 — Termination

The skill terminates only when **all** of the following hold:

- All three sections have been written and explicitly approved by the user.
- Any blanks remaining in the document are marked as `TBD: <reason>` — i.e., the blank is intentional and the user is aware of it.

When the conditions are met:

1. Write `docs/foundation.md` (see format below).
2. Confirm with the user whether to commit the file to git, then commit if confirmed.
3. Inform the user: "Foundation is complete. The next stage is Decompose. The skill ends here — what you do next is your choice."

Do not recommend any specific downstream tool.

## Output format (`docs/foundation.md`)

- Single markdown file.
- Three sections as H2 headers, with the names matching `README.md`:
  - `## Core Problem & End Goal`
  - `## Vision / Target Users / Success / Core Values`
  - `## Tech Stack (Initial)`
- Each section: short prose or a bullet list. No enforced format. Roughly one screen-page in length per section.
- Length guidance: deliberate brevity. Detail belongs in Decompose and downstream stages, not here.
- Frontmatter:

```yaml
---
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
---
```

On first write, set both `created` and `updated` to today's date. **Obtain the date by running `date +%Y-%m-%d` in the shell — do not guess and do not rely on what you think today is.** The `created` field is set once and never changed. The `updated` field is set to today's date (obtained the same way) on every subsequent save.

## Update mode

When `docs/foundation.md` already exists at skill invocation:

1. Read the current document.
2. **Foundation Update Candidate intake.** Read `docs/slices.md` and `docs/select.md` if they exist, and collect every item under their `## Foundation Update Candidates` sections. A candidate counts as *unresolved* if its bullet has no `resolved in foundation.md` marker (see step 7). If any unresolved candidates exist, present them to the user before the section-selection question: "Decompose/Select flagged N unresolved Foundation Update Candidate(s): <list each, with its source file and flag date>. Which of these should be folded into this update?" Walk each candidate the user picks through the relevant section's question list. This step only *reads* `docs/slices.md` and `docs/select.md` — it does not modify them here.
3. Show the three section names and ask the user which section(s) to update. Use multiple choice and allow multiple selections. (Candidates the user chose to fold in already imply their target section; still confirm the full section list with the user.)
4. For each selected section, walk it from the start of its question list, reusing the procedure above.
5. Sections the user did not select remain untouched.
6. On termination, set `updated` in the frontmatter to today's date — again obtained by running `date +%Y-%m-%d` in the shell, never guessed. Leave `created` unchanged.
7. **Clear resolved candidates.** For each Foundation Update Candidate folded into Foundation in this call, append ` — resolved in foundation.md on <YYYY-MM-DD>` to that candidate's bullet in its source file (`docs/slices.md` or `docs/select.md`). This is the *only* case in which this skill writes to a file other than `docs/foundation.md`: it appends a resolution marker to an already-existing candidate bullet and never alters anything else in those files. The bullet is never deleted — history is preserved. Confirm these marker edits with the user before saving.
8. Confirm git commit with the user (including the candidate-marker edits, if any).

If the user wants to add a `TBD` to a section that does not currently have one, treat that as updating that section.

---

## What this skill does NOT do

- It does not propose how to slice the work. (Decompose stage.)
- It does not pick what to build first. (Select stage.)
- It does not recommend any specific downstream tool. The user chooses.
- It does not guess at unanswered values. They become `TBD`.
- It does not edit `docs/slices.md` or `docs/select.md`, except — in update mode — to append a `resolved in foundation.md on <date>` marker to a Foundation Update Candidate it has just folded in. Slice content and Select content are never otherwise touched.
