---
created: 2026-05-17
updated: 2026-05-17
---

## Core Problem & End Goal

**Core Problem**

When an AI is given a Foundation and asked to brainstorm, plan, or generate code, it tends to reach the ultimate goal in *one big inference*. Foundation by its nature answers only the *destination* (why / what / with what) and leaves the *path* (in what order, in what increments, what counts as evidence of working) blank. The AI fills those blanks with its own reasoning, and the answered overwhelms the unanswered.

The result is consistent — progress stalls without anything end-to-end ever working, partially generated artifacts accumulate, or an elaborate non-working structure is left behind.

This tendency reproduces with the same shape on three layers: at the **surface** (it tries to go from Foundation to implemented code in a single step and stalls midway), at **depth 1** (it defaults unknowns about its environment to "present" and reasons over the blanks), and at **depth 2** (even when work is sliced finely, each sliced unit itself swells back into another big inference).

**Who has this problem**

Individuals or organizations using AI-driven workflows.

**End Goal**

Success for this project is that the user can produce work units small enough to hand off to downstream processes (brainstorming, planning, implementation, etc.) without those units swelling back into another big inference.

This bar is not something to meet once and be done with. The project has no explicit terminal state — it remains a living tool that evolves alongside the user's workflow.

## Vision / Target Users / Success / Core Values

**Vision**

Become the tool that turns the basic unit of work in AI-driven workflows from a *big inference* into a *small unit*.

**Target Users**

Same as "Who has this problem" above: individuals or organizations using AI-driven workflows.

**Success (to those users)**

Before handing work to the AI, the user can clarify it as a small, verifiable sliced unit.

**Core Values** (priorities when a tradeoff arises)

- **Honest blanks ≻ smooth inference.** The convenience of filling a blank with plausible reasoning yields to the honesty of leaving it explicitly empty as `TBD: <reason>`.
- **Sharp stage boundaries ≻ feature richness.** The richness of one stage answering the *path* belonging to the next yields to the sharpness of each stage answering only its own *destination* and stopping.
- **Progress in small slices ≻ completion of a large artifact.** The attempt to complete a large artifact in one big inference yields to verifiable progress in small slices.

## Tech Stack (Initial)

**Language**

No specific language. Documents are written primarily in Markdown.

**Frameworks / Major Libraries**

Claude Code plugin.

**Other Major Tooling Decisions**

None.
