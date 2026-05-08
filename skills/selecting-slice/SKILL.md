---
name: selecting-slice
description: Use when the user wants to select the next slice from docs/slices.md
  and close its Definition of Done. Third stage of the agent-workflow methodology,
  after Decompose and before Brainstorming. Outputs docs/select.md with one Active
  slice (closed demo line + 1-3 Visible Outcomes) and a History of past selections.
---

# Selecting Slice

Help the user pick one slice from `docs/slices.md`'s Confirmed Slices and close its Definition of Done. The DoD takes a fixed form: a *closed demo line* (one sentence stating what the user can demonstrate when the slice is finished) plus *1–3 Visible Outcomes* (externally observable results). Select answers the slice's destination one step further than Decompose; it does not answer *how* the slice is implemented — that is the job of Brainstorming and the downstream stages.

**Output:** `docs/select.md` in the user's project.

## When to use

- The user has a `docs/foundation.md` and a `docs/slices.md` with at least one Confirmed Slice, and wants to select the next slice to work on.
- The user wants to *refine* the Active slice's DoD (closed demo line or Visible Outcomes) without changing which slice is Active.
- The user wants to *close* the Active slice (mark it `completed`, `superseded`, `deferred`, or `paused`) and move it to History — possibly followed by a new selection.
- The user wants to view the current Active slice (Continue / Review only) before resuming work.

This skill assumes both `docs/foundation.md` and `docs/slices.md` exist, and that `docs/slices.md` has at least one Confirmed Slice. If either file is missing or no Confirmed Slice exists, refuse and direct the user to the appropriate prior skill — do not generate shorthand inputs yourself.

## Background

This skill is the third stage of the agent-workflow methodology. The methodology rests on a specific failure mode: when an AI is given a goal and asked to brainstorm, plan, or generate code, it tends to reach for the ultimate goal in *one big inference*, filling unanswered blanks with its own reasoning.

Select is the last destination-stage of the methodology. After Select, the methodology only refines *how* (Brainstorming converges to an approach; Plan converges to a procedure; Development executes; Review verifies; Compound captures learning). The Goal itself is finalized here, in the form of the closed demo line plus 1–3 Visible Outcomes.

Select is itself susceptible to the failure mode in two specific ways:

1. **DoD inflation.** If the agent allows long checklists (5+ items), each new item invites path-level detail ("login button is in the top-right", "/auth/login endpoint is called"). The skill caps Visible Outcomes at 1–3 to keep the DoD a *destination*, not a path.
2. **Slice rubber-stamping.** If the agent proposes a candidate slice and the user says "yes, that one", the user has not actually engaged with the choice. The skill restricts agent-proposed candidates to a B4 fallback that fires only on explicit user signal, and even then requires the user to restate the chosen candidate in their own words.

See `references/01-problem.md` for the full problem statement and `references/02-principle.md` for the two governing principles.

## Two principles you must follow

### Principle 1 — Answer destination, leave path blank

You answer only the destination at this level: which slice is Active, its closed demo line, and its 1–3 Visible Outcomes. You do *not* answer:

- How the slice should be implemented (that is downstream of Brainstorming)
- What language, framework, or technique should be used inside the slice (that is downstream)
- How the slice will be split into developer tasks (that is Plan's job)
- How long the slice will take (that is downstream)

If the user volunteers a path-level answer ("this slice will use React Router", "the login button goes top-right", "I'll TDD this one"), acknowledge it and explicitly defer:

- If it is an *implementation choice for this slice*, do not absorb it into the closed demo line or Visible Outcomes. Note that it sounds like an in-slice implementation decision and tell the user it will be addressed in Brainstorming or later.
- If it is a *new tech decision* that wasn't in `docs/foundation.md` (e.g., "we'll use PostgreSQL"), do not absorb it into the slice either, but DO list it under `## Foundation Update Candidates` in `docs/select.md`. Tell the user the actual Foundation update happens in `defining-foundation` update mode — Select only flags the candidate.

### Principle 2 — Do not fill blanks by inference

Within destination scope (the Active slice and its DoD), record only what the user has explicitly stated. Do not:

- Guess which slice the user should pick next
- Paraphrase a vague intent into a specific closed demo line
- Invent Visible Outcomes to make the DoD feel complete
- Smooth over gaps with plausible defaults

When the user is uncertain:

- For *slice naming*: use the B4 fallback (below) — but only if the user explicitly signals being stuck.
- For *closed demo line* and *Visible Outcomes*: do not propose candidates. If the user cannot pin down either, this is a signal that the slice is too vague — suggest re-running `decomposing-slices` to clarify or split the slice. Mark the unfilled slot as `TBD: <reason>`.

A blank that is honestly blank is always better than a blank silently filled.

## Procedure

### Step 1 — Context check

Quickly inspect:

- Is there a `docs/foundation.md`? If **no**, refuse: "This skill needs a Foundation document at `docs/foundation.md`. Run the `defining-foundation` skill first." Do not generate a shorthand Foundation. Stop.
- Is there a `docs/slices.md`? If **no**, refuse: "This skill needs a Decompose document at `docs/slices.md`. Run the `decomposing-slices` skill first." Stop.
- Does `docs/slices.md` have at least one slice under `## Confirmed Slices`? If **no**, refuse: "There are no Confirmed Slices yet. Run the `decomposing-slices` skill to confirm at least one slice first." Stop.
- Is there a `docs/select.md`? If **yes**, read it and check for an Active slice.

### Step 2 — Mode branch

If `docs/select.md` does not exist, or it exists but has no Active slice, skip this step and go straight to Step 3.

If `docs/select.md` has an Active slice, ask the user a single-select question (translate to the user's language as needed):

> The Active slice in `docs/select.md` is:
>
> `<copy the Active slice's demo line here>`
>
> What does this call cover? Pick one.
>
> ( ) Continue / Review only — show the Active slice and stop (you'll go back to working on it)
> ( ) Refine current Active's DoD — change closed demo line or Visible Outcomes (but keep the same slice Active)
> ( ) Close current Active — assign a status and move it to History

Branch on the user's choice:

- **Continue / Review only** — Print the Active slice as it stands and terminate. Do not modify the file.
- **Refine current Active's DoD** — Stay on the same Active slice. Go to Step 5 (skip slice-naming). Allow the user to overwrite either the closed demo line or any Visible Outcome (or both). On termination (Step 9), update the file's `updated:` frontmatter.
- **Close current Active** — Go to Step 4 (close-and-maybe-select).

### Step 3 — Slice naming — B2 default pattern

Ask the user to *name* one slice from `docs/slices.md`'s `## Confirmed Slices`. Use questions like (translate as needed):

- "Which Confirmed Slice in `docs/slices.md` is next? You can quote the demo line or use its current number."
- "Which slice are you taking on now?"

The agent does **not** propose candidate slices. The user names; the agent records.

When the user names a slice (by number or by quoting the demo line), look it up in `docs/slices.md` and use that slice's demo line *verbatim* as the Active slice's heading in `docs/select.md`. The user does not need to restate the demo line — it was already authored by the user during Decompose, so re-statement would be redundant.

If the user's reference is ambiguous (e.g., the number doesn't match any current Confirmed Slice, or the quoted phrase matches multiple), ask the user to clarify before recording.

After the slice is named, continue to Step 5.

### Step 4 — Close-and-maybe-select branch (from "Close current Active")

Ask a single-select status question (translate to the user's language; record the English keyword in the file):

> What status does the closing Active slice receive?
>
> ( ) completed — the DoD was met; the slice is fully done
> ( ) superseded — a higher-priority slice replaces it (you changed your mind)
> ( ) deferred — push it off; not started yet
> ( ) paused — started but stopped midway; intend to resume later

Move the closing slice to `## History` with the assigned status, prepending it to History (most-recent first). Then branch on the status:

- **completed** — Ask: "Pick a new slice now, or stop here?" If "pick now", go to Step 3. If "stop", terminate (Step 9).
- **superseded** — Go straight to Step 3 (the user already implied a new selection by choosing this status).
- **deferred** or **paused** — Ask: "Pick a new slice now, or stop here?" If "pick now", go to Step 3. If "stop", terminate (Step 9). Defaulting to "stop" is fine here — the user may be ending a work session.

### Step 5 — Closed demo line — user-stated only

Ask the user (translate as needed):

- "When this slice is finished, what's a single sentence that captures what's done — written from the *finished* point of view?"
- "If you were demoing this slice tomorrow, what's the one-sentence headline?"

The agent does **not** propose candidates. The agent does not paraphrase the demo line from `docs/slices.md` into a closed form — it asks the user.

When the user states a sentence, write it as-is (in the user's language). Confirm: "Did I capture this right? `<one sentence>`". On approval, this becomes the closed demo line.

If the user cannot pin down a closed demo line:

- Tell the user: "If we can't close the demo line in one sentence, the slice may be too vague. Consider re-running `decomposing-slices` to split or clarify it."
- Record the slot as `TBD: <reason>` if the user wants to proceed anyway, and continue to Step 6.

There is **no B4 fallback** at this step. The closed demo line is the core destination — the agent does not propose candidates here.

### Step 6 — Visible Outcomes — user-stated, 1–3 only

Ask the user (translate as needed):

- "What can someone observe externally to know this slice is done? List 1 to 3 outcomes."
- "What does a user (or an observer) actually see when this slice is finished?"

The agent does **not** propose candidates.

Record each Outcome as the user states it (in the user's language). Confirm each one before adding the next. After the user is finished:

- **0 Outcomes** — The slice may be too vague. Tell the user: "We don't have any Visible Outcome yet. The slice may be too vague — consider re-running `decomposing-slices` to clarify or split it." Record the slot as `TBD: <reason>` if the user insists on proceeding.
- **1–3 Outcomes** — OK, proceed.
- **4 or more Outcomes attempted** — The slice is too big. Tell the user: "This slice has 4+ Visible Outcomes. That usually means it should be split. Consider re-running `decomposing-slices` to split it." Do not record more than 3 Outcomes; ask the user to either drop down to ≤3 or halt this Select call to go split the slice. Do **not** silently truncate.

There is **no B4 fallback** at this step.

### Step 7 — B4 fallback (slice-naming step only, user-explicit only)

The agent may propose 1–3 candidate slices **only when the user explicitly signals being stuck during the slice-naming step (Step 3).** Examples of explicit signals (in any language):

- "I don't know"
- "Give me some options"
- "Help me — I'm stuck"
- "What should I pick?"

The agent must NOT infer stuckness from tone, hesitation, silence, or short answers. If the user is just thinking, wait.

When the fallback triggers, the agent presents 1–3 candidate slices from `docs/slices.md`'s `## Confirmed Slices`, ordered by priority (high first; ties broken by current list order). For each candidate, briefly note its priority. Then the agent stops and waits.

**The user must restate the chosen candidate in their own words** before it is recorded — even though the demo line itself is being copied from `docs/slices.md`. The restatement is a *commitment signal*, not a re-authoring of the demo line. Accepting a candidate verbatim ("yes, that one") is *not* sufficient — ask the user to say it back in their own voice. This is the enforcement of Principle 2: it prevents the user from rubber-stamping an AI-filled blank without engaging with it.

If the user can't restate, the slice does not get recorded. The user can use the original AI candidate as inspiration in a future call.

The B4 fallback is **only** for slice-naming. It does **not** apply to closed demo line or Visible Outcomes.

### Step 8 — Path-question deflection

If at any point the user starts answering at the path level inside the slice ("this slice will use React Router", "the login button goes top-right", "I'll deploy via Vercel for this slice"), do not absorb it into the slice's DoD. Decide which case applies:

- It is an *implementation choice for this slice* — say: "That sounds like an in-slice implementation decision. I'll leave it for Brainstorming or later — I'm only capturing what the slice's done state looks like, not how it's built." Return to the question you were on.
- It is a *new tech decision not in `docs/foundation.md`* — say: "That sounds like a tech-stack decision that should live in Foundation, not in a slice's DoD. I'll list it as a Foundation update candidate; you can run `defining-foundation` in update mode to actually fold it in." Add it to `## Foundation Update Candidates` in `docs/select.md` (and return to the question you were on).

Note: `docs/slices.md` may also have a `## Foundation Update Candidates` section from prior Decompose calls. Select does not modify `docs/slices.md` — it only writes to its own file. The user reconciles both files when running `defining-foundation` in update mode.

### Step 9 — Approval and termination

The skill terminates only when **all** of the following hold (for the modes that produce a new or refined Active):

- The Active slice's demo line, closed demo line, and Visible Outcomes (or `TBD: <reason>` markers) are explicitly approved by the user.
- For close-then-select calls, the closing slice's status has been explicitly chosen.
- Any unfilled values are explicitly marked as `TBD: <reason>`.

For Continue / Review only mode, no approval is needed — the file is not modified.

When the conditions are met:

1. Write or update `docs/select.md` (see format below).
2. Confirm with the user whether to commit the file to git, then commit if confirmed (respect the user project's commit policy).
3. Inform the user: "Select call complete. Brainstorming and the downstream stages (Plan, Development, Review, Compound) are outside this plugin's scope — continue with whichever external plugin or workflow fits. This plugin does not recommend a specific tool; the choice is yours."

Do not recommend any specific downstream tool.

## Output format (`docs/select.md`)

- Single markdown file at the user project's `docs/select.md`.
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
# Select

## Active

### <demo line copied verbatim from docs/slices.md>

**Closed demo line**: <one sentence in user's language>

**Visible Outcomes**:
- <outcome 1>
- <outcome 2>
- <outcome 3>

**Selected on**: <YYYY-MM-DD>

## History

### <YYYY-MM-DD> — <demo line of past Active>

**Closed demo line**: <one sentence>

**Visible Outcomes**:
- <outcome 1>
- ...

**Selected on**: <YYYY-MM-DD>

**Status**: <completed | superseded | deferred | paused>

### <earlier YYYY-MM-DD> — <demo line>

...

## Foundation Update Candidates

- <candidate tech decision> — flagged on <YYYY-MM-DD>
```

Section rules:

- **Active** — At most one slice. The H3 header carries the slice's demo line *exactly as it appears in `docs/slices.md`'s `## Confirmed Slices`* — do not paraphrase. Below the H3, three bold-label fields: `**Closed demo line**:`, `**Visible Outcomes**:` (1–3 bullets), `**Selected on**:` (date). When no slice is Active, omit the `### <demo line>` block but keep the `## Active` H2 header with a single line `(none)`.
- **History** — Reverse-chronological (most recent first). Each entry is one H3 with the closing date and the demo line, then four bold-label fields: `**Closed demo line**:`, `**Visible Outcomes**:`, `**Selected on**:` (the date the slice originally became Active), `**Status**:` (one of `completed`, `superseded`, `deferred`, `paused`). When History is empty, write `(none)` under the `## History` H2.
- **Foundation Update Candidates** — Bullet list. Each entry is a candidate tech / scope decision discovered during a Select call that should probably be folded into `docs/foundation.md`. List date flagged. The actual Foundation update is the responsibility of `defining-foundation` in update mode — Select only flags. When no candidates exist, the section heading is kept and `(none)` is written as the only line; do not omit the heading.

The status keywords are recorded in **English** (`completed`, `superseded`, `deferred`, `paused`) regardless of the user's chat language. The skill translates the keywords when *speaking* to the user but writes the English form to the file.

Length guidance: deliberate brevity. Detail belongs in Brainstorming and downstream stages, not here.

## Update mode (recap)

When `docs/select.md` exists at skill invocation:

1. Read the current document and identify the Active slice (if any).
2. Branch on the mode question (Step 2):
  - Continue / Review only → output the Active slice and terminate (no file write).
  - Refine current Active's DoD → walk through Steps 5 and 6 only; keep the same Active slice header.
  - Close current Active → walk through Step 4; status is assigned, slice moves to History; possibly continue to Step 3 for a new selection.
3. On termination (whenever the file is written), set `updated` in the frontmatter to today's date. Leave `created` unchanged.
4. Confirm git commit with the user.

Update mode is the *primary* mode of operation in this methodology — first calls happen once per project, but every subsequent slice cycle calls Select again.

## Language policy

- This `SKILL.md` is in English so Claude's skill-matching mechanism works reliably.
- The questions inside the procedure are written in English here, but the agent translates them to the user's language when speaking with the user.
- The agent records the closed demo line and Visible Outcomes in the user's language, faithfully reflecting the user's wording.
- Status keywords (`completed`, `superseded`, `deferred`, `paused`) are recorded in English in the file. The agent translates when speaking to the user but writes the English keyword.

## What this skill does NOT do

- It does not propose how to implement the Active slice. (Brainstorming and Plan stages, downstream.)
- It does not modify `docs/slices.md`. Slice priorities, splits, merges, and reorderings are Decompose's job.
- It does not generate shorthand inputs. Missing `docs/foundation.md`, missing `docs/slices.md`, or empty Confirmed Slices → refuse and direct to the appropriate prior skill.
- It does not absorb in-slice implementation decisions or new tech decisions. Implementation decisions are deferred to Brainstorming; new tech decisions are flagged as Foundation update candidates.
- It does not recommend any specific downstream tool. The user chooses.
- It does not infer that the user is stuck. The B4 fallback fires only on explicit user signal, and only at the slice-naming step (Step 3).
- It does not record an AI-proposed candidate verbatim. The user must restate it in their own words.
- It does not propose closed demo lines or Visible Outcomes. Those are user-stated only — there is no B4 fallback for those steps.
- It does not auto-resume `paused` slices. To resume a paused slice, the user re-runs Select and re-selects it; the prior paused entry remains in History.
- It does not allow more than one Active slice (WIP > 1 is rejected to preserve the methodology's "small unit, one at a time" stance).
