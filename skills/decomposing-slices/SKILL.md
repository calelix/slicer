---
name: decomposing-slices
description: Use when the user wants to derive or update the next slice(s) for
  a project that already has docs/foundation.md. Second stage of the slicing
  methodology, after Define and before Select. Operates incrementally — produces
  one or more slices at a time, not a full N-list. Outputs docs/slices.md.
---

# Decomposing Slices

Help the user derive or update the slice(s) currently visible from a project's Foundation. A *slice* is a small, demonstrable unit of work that produces something the user can see working, expressed as a single rough demo line ("when this is done, the user can do X"). Decompose answers *what slices and in what order*; it does not answer *how each slice is implemented* — that is the job of Select and the downstream stages.

**Output:** `docs/slices.md` in the user's project.

## When to use

- The user has a `docs/foundation.md` and wants to derive the first slice(s) for the project.
- The user just finished a slice cycle and wants to update the slice list — adding, splitting, merging, reordering, deleting, or clarifying — based on what the cycle produced.
- The user has tentative slices in `docs/slices.md` and wants to clarify one of them into a confirmed slice.

This skill assumes a `docs/foundation.md` exists. If it does not, refuse and direct the user to the `defining-foundation` skill — do not generate a shorthand Foundation yourself.

## Background

This skill is the second stage of the slicing methodology. The methodology rests on a specific failure mode: when an AI is given a goal and asked to brainstorm, plan, or generate code, it tends to reach for the ultimate goal in *one big inference*, filling unanswered blanks with its own reasoning.

Decomposition is itself susceptible to this failure mode. If you try to produce a full N-slice list at once, you must reason about *how the project will evolve* across slices — including slices whose preconditions don't exist yet. That is a small one-big-inference. The skill therefore operates **incrementally**: each call derives or updates only the slice(s) visible right now, with priorities relative to what is currently known. Future slices remain undecided until they come into view, typically after a slice cycle completes.

See `references/01-problem.md` for the full problem statement and `references/02-principle.md` for the two governing principles.

## Two principles you must follow

### Principle 1 — Answer destination, leave path blank

You answer only the destination at this level: what slice(s) are currently visible and in what order. You do *not* answer:

- How any slice should be implemented (that is downstream of Select)
- What language, framework, or technique should be used inside a slice (that is downstream)
- How long any slice will take (that is downstream)

If the user volunteers a path-level answer ("this slice will use React", "I'll do TDD for this one", "let's build the auth backend first then the frontend"), acknowledge it and explicitly defer:

- If it is an *implementation choice*, do not absorb it into the slice. Note that it sounds like an in-slice implementation decision and tell the user it will be addressed in Select or later.
- If it is a *new tech decision* that wasn't in `docs/foundation.md` (e.g., "we'll use PostgreSQL"), do not absorb it into the slice either, but DO list it under `## Foundation Update Candidates` in `docs/slices.md`. Tell the user the actual Foundation update happens in `defining-foundation` update mode — Decompose only flags the candidate.

### Principle 2 — Do not fill blanks by inference

Within destination scope (the slice list), record only what the user has explicitly stated. Do not:

- Guess what the next slice should be
- Paraphrase a vague intent into a specific demo line
- Invent slices to make the list feel complete
- Smooth over gaps with plausible defaults

When the user is uncertain, either:

- Mark the slot as `TBD: <reason>` and place it under `## Tentative Slices`, or
- Use the B4 fallback (below) — but only if the user explicitly signals being stuck.

A blank that is honestly blank is always better than a blank silently filled.

## Procedure

### Step 1 — Context check

Quickly inspect:

- Is there a `docs/foundation.md`? If **no**, refuse: "This skill needs a Foundation document at `docs/foundation.md`. Run the `defining-foundation` skill first." Do not generate a shorthand Foundation. Stop.
- Is there a `docs/slices.md`? If **yes**, switch to **update mode** (see below). If **no**, this is a first call.
- If `docs/slices.md` or `docs/select.md` has unresolved items under `## Foundation Update Candidates` (a candidate is unresolved if its bullet has no `resolved in foundation.md` marker), print a one-line count: "N unresolved Foundation Update Candidate(s) across slices.md/select.md — consider running `defining-foundation` in update mode." Count only; do not infer which candidates matter or act on them.
- Read `docs/foundation.md`'s three sections, focusing on the End Goal (Section 1) and any constraints from Vision/Users (Section 2). You will use Foundation as the *starting point* for slice derivation, not copy its content into slices (slices are not Foundation's path).

### Step 2 — First call vs. update mode branch

#### First call (no `docs/slices.md`)

Go to Step 3.

#### Update mode (`docs/slices.md` exists)

Read the existing file. Then ask the user which kind(s) of change this call is for. Present this multi-select question (translate into the user's language as needed):

> Which of these does this call cover? Pick all that apply.
>
> - [ ] Add a new slice
> - [ ] Split an existing slice into two or more
> - [ ] Merge multiple slices into one
> - [ ] Reorder slice priorities
> - [ ] Delete a slice
> - [ ] Clarify a tentative slice into a confirmed slice
> - [ ] Other / I'm not sure yet — let me describe it freely

If the user picks "Other", let them describe in their own words and reflect back which of the six kinds (or combination) it most resembles. Confirm with the user before proceeding.

For each selected kind, walk through Step 3's derivation procedure scoped to that kind. Sections of `docs/slices.md` that the user did not address in this call remain untouched.

### Step 3 — Derive or update slice(s) — B2 default pattern

For each slice being derived or updated in this call, ask the user — one question at a time — to *state* the slice in their own words. Use questions like:

- "To reach the End Goal in `docs/foundation.md`, what is the very first thing that needs to *work* — something a user could see actually doing something?"
- "When this slice is done, what one thing can be demonstrated?"
- "What can a user actually do after this slice ships that they couldn't do before?"

The agent does **not** propose candidate slices. The user states; the agent records.

When the user has stated a slice, draft it as a single rough demo line in the user's language for confirmation: "Did I capture this right? `<one line>`". On approval, this becomes a *confirmed slice*, written to `docs/slices.md` in English (see Language policy).

If the user gives a vague intent that they cannot yet collapse to a single line, record it as a *tentative slice* under `## Tentative Slices` with a `TBD: <reason>` annotation describing why it is not yet pinned down. Tentative slices are first-class citizens in `docs/slices.md` — they hold space for what is not yet ready to be confirmed.

### Step 4 — B4 fallback (user-explicit only)

The agent may propose 1–3 candidate slices **only when the user explicitly signals being stuck.** Examples of explicit signals (in any language):

- "I don't know"
- "Give me some options"
- "Help me — I'm stuck"
- "What might be next?"

The agent must NOT infer stuckness from tone, hesitation, silence, or short answers. If the user is just thinking, wait.

When the fallback triggers, the agent presents 1–3 candidate slices, each as a single demo line, briefly noting why each is plausible from the Foundation. Then the agent stops and waits.

**The user must restate the chosen candidate in their own words** before it is recorded. Accepting a candidate verbatim ("yes, that one") is *not* sufficient — ask the user to say it back in their own voice. This is the enforcement of Principle 2: it prevents the user from rubber-stamping an AI-filled blank without engaging with it.

If the user can't restate, the slice does not get recorded. The user can use the original AI candidate as inspiration in a future call.

### Step 5 — Slice-line sanity check — size and Foundation consistency

For each newly written slice line, evaluate:

- **Right size** — One line that names a *user-visible action* the user can demonstrate when the slice is done. Approve.
- **Too big** — Hard to fit on one line; multiple actions bundled. Suggest splitting and ask the user to split it themselves (do not split it for them).
- **Too small** — The line names an *internal step* ("create the database schema", "draw the login form") rather than a user-visible demo. Suggest absorbing it into a larger slice or merging with a sibling. Ask the user; do not merge for them.
- **Contradicts Foundation** — The slice line conflicts with something stated in `docs/foundation.md` — most often the End Goal, a Core Value, or a stated constraint. Do not silently accept it, and do not rewrite it yourself. Name the conflict to the user — quote the Foundation text and the slice line — and ask the user to choose: drop or rework the slice, or run `defining-foundation` in update mode if the Foundation itself is now wrong. This is a consistency check against the one document Decompose already reads; it is not blank-filling by inference.

The user always makes the final call on size and on resolving any Foundation conflict.

### Step 6 — Path-question deflection (in-slice implementation)

If the user starts answering at the path level inside a slice ("this slice will use React", "let's TDD this one", "I'll deploy via Vercel for this slice"), do not absorb it. To classify, check whether the technology or technique already appears in `docs/foundation.md`'s `## Tech Stack (Initial)` section: if it does, this is an implementation choice within an established stack (case 1 below); if not, it is a new tech decision that belongs in Foundation, not in a slice (case 2 below).

Either:

- It is an *implementation choice for this slice* — say: "That sounds like an in-slice implementation decision. I'll leave it for Select or later — I'm only capturing what the slice demonstrates, not how it's built." Return to the slice-line question.
- It is a *new tech decision not in `docs/foundation.md`* — say: "That sounds like a tech-stack decision that should live in Foundation, not in a slice. I'll list it as a Foundation update candidate; you can run `defining-foundation` in update mode to actually fold it in." Add it to `## Foundation Update Candidates` and return to the slice-line question.

### Step 7 — Approval

For each slice derived or updated in this call (whether new, split, merged, reordered, deleted, or clarified), get explicit user approval before writing it to `docs/slices.md`. If a slot is left unfilled, mark it `TBD: <reason>` rather than guessing.

### Step 8 — Termination

The skill terminates only when **all** of the following hold:

- Each slice derived or updated in this call has been explicitly approved by the user.
- Any unfilled values are explicitly marked as `TBD: <reason>`.

When the conditions are met:

1. Write or update `docs/slices.md` (see format below).
2. **Drift check against `docs/select.md`.** If `docs/select.md` exists and has an Active slice whose H3 demo line does not appear verbatim in the current `## Confirmed Slices` of `docs/slices.md`, print a one-line nudge: "The Active slice's H3 in `docs/select.md` is now stale — run `selecting-slice` and pick 'Re-sync Active H3' mode to update it." Read-only check; do **not** write to `docs/select.md`. If `docs/select.md` does not exist, has no Active slice, or its Active H3 still matches a current Confirmed Slice, emit nothing.
3. Confirm with the user whether to commit the file to git, then commit if confirmed (respect the user project's commit policy).
4. Inform the user: "Decompose call complete. The next stage is Select for one of the confirmed slices. The skill ends here — what you do next is your choice."

Do not recommend any specific downstream tool.

## Output format (`docs/slices.md`)

- Single markdown file at the user project's `docs/slices.md`.
- Frontmatter:

```yaml
---
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
---
```

Use `date +%Y-%m-%d` to obtain the date — never guess. Set `created` only on first write; `updated` on every save.

- Body, in this fixed section order:

```markdown
# Slices

## Confirmed Slices

1. <demo line> — priority: <high | medium | low>
2. <demo line> — priority: <high | medium | low>
...

## Tentative Slices

- <vague demo line> — `TBD: <reason>`
...

## Change Log

### <YYYY-MM-DD>

- <kind>: "<short quote of affected slice>" <(optional one-clause reason)>
- <kind>: "..."
...

### <earlier YYYY-MM-DD>

- ...

## Foundation Update Candidates

- <candidate tech decision> — flagged on <YYYY-MM-DD>
```

Section rules:

- **Confirmed Slices** — Numbered list. Numbers are positional in the current sort order; they are not stable identifiers. Each entry is a single rough demo line plus a priority tag. Do not include implementation detail; if such detail leaks in, the user has answered at the path level — strip it before writing.
- **Tentative Slices** — Bullet list. Each entry is a vague demo line followed by `TBD: <reason>` describing why it is not yet pinned down. Tentative slices hold space; they are not failures.
- **Change Log** — H3 date headers, most recent first. Each call appends one new H3 block. Within a block, one bullet per change, of form: `- <kind>: "<short quote>"` where `<kind>` is one of `Added`, `Split`, `Merged`, `Reordered`, `Deleted`, `Clarified`. Quotes use the demo line as it stood at the time of the change. Optionally append a one-clause reason.
- **Foundation Update Candidates** — Bullet list. Each entry is a candidate tech / scope decision discovered during a Decompose call that should probably be folded into `docs/foundation.md`. List date flagged. The actual Foundation update is the responsibility of `defining-foundation` in update mode — Decompose only flags. A candidate may also carry a `— resolved in foundation.md on <date>` marker appended by `defining-foundation` once that candidate has been folded in; preserve any such marker verbatim when rewriting this section. When no candidates exist, keep the section heading and write `(none)` as the only line; do not omit the heading.

The slice line itself: a *single rough demo line* of what works when the slice is done, from the user's point of view. Open-ended phrasing (closure happens in Select). No implementation detail.

Length guidance: deliberate brevity. Detail belongs in Select and downstream stages, not here.

## Update mode (recap)

When `docs/slices.md` exists at skill invocation:

1. Read the current document.
2. Ask the user which kind(s) of change apply (multi-select question shown above in Step 2).
3. For each selected kind, walk through Step 3 scoped to that kind.
4. Append a fresh H3 entry to `## Change Log` describing what changed.
5. On termination, set `updated` in the frontmatter to today's date. Leave `created` unchanged.
6. Confirm git commit with the user.

Update mode is the *primary* mode of operation in the methodology — first calls are rare; subsequent calls per slice cycle are the norm.

## Language policy

- This `SKILL.md` is in English so Claude's skill-matching mechanism works reliably; treat the questions inside as guides, not literal scripts.
- **Conversation (questions you ask, drafts you show for approval):** the user's language. Translate the English questions, and present approval drafts in the user's language so the user can verify intent in their own words.
- **Written artifact (`docs/slices.md`):** English, regardless of the conversation language. After the user approves a slice line in their language, translate it to English when writing it to the file. This applies to slice lines, tentative-slice descriptions, change-log entries, and Foundation Update Candidates.
- Preserve proper nouns and user-supplied technical terms (product names, library names, domain jargon) as the user wrote them, even inside English translations.

## What this skill does NOT do

- It does not propose how to implement any slice. (Brainstorming and Plan stages, downstream of Select.)
- It does not commit to slices that aren't currently visible. Future slices wait for the next call.
- It does not generate a shorthand Foundation if `docs/foundation.md` is missing. It refuses and points to `defining-foundation`.
- It does not absorb in-slice implementation decisions or new tech decisions. Implementation decisions are deferred to Select; new tech decisions are flagged as Foundation update candidates.
- It does not recommend any specific downstream tool. The user chooses.
- It does not infer that the user is stuck. The B4 fallback fires only on explicit user signal.
- It does not record an AI-proposed candidate verbatim. The user must restate it in their own words.
- It does not write to `docs/select.md`. The Step 8 drift check reads it to detect a stale Active-slice H3 and emits only a one-line nudge — the user resolves drift by re-invoking `selecting-slice` in Re-sync mode.
