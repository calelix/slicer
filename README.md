# Agent Workflow

A structured, AI-assisted workflow for taking any project from idea to completion through small, verifiable slices.

## High-Level Flow

```
  ┌───────────────────────┐
  │  Start a new project  │
  └───────────┬───────────┘
              ▼
  ┌───────────────────────┐
  │  Foundation Document  │ ◄──────────────┐
  └───────────┬───────────┘                │
              ▼                            │
  ┌───────────────────────┐                │
  │       Decompose       │ ◄────────┐     │
  └───────────┬───────────┘          │     │
              ▼                      │     │
  ┌───────────────────────┐          │     │
  │    Select a Slice     │          │     │
  └───────────┬───────────┘          │     │
              ▼                      │     │
      ┌───────────────┐              │     │
      │ Brainstorming │              │     │
      └───────┬───────┘              │     │
              ▼                      │     │
       ┌──────────────┐              │     │
       │     Plan     │              │     │
       └──────┬───────┘              │     │
              ▼                      │     │
       ┌──────────────┐              │     │
       │ Development  │              │     │
       └──────┬───────┘              │     │
              ▼                      │     │
       ┌──────────────┐              │     │
       │    Review    │              │     │
       └──────┬───────┘              │     │
              ▼                      │     │
       ┌──────────────┐              │     │
       │   Compound   │              │     │
       └──────┬───────┘              │     │
              ▼                      │     │
       ┌──────────────┐  Next Slice  │     │
       │              │──────────────┘     │
       │   Repeat     │  Update Foundation │
       │              │────────────────────┘
       └──────────────┘
```

## Step 1. Foundation Document

```
                       ┌───────────────────────┐
                       │  Foundation Document  │
                       └───────────┬───────────┘
            ┌──────────────────────┼─────────────────────┐
            ▼                      ▼                     ▼
  ┌───────────────────┐ ┌─────────────────────┐ ┌──────────────────┐
  │ 1. Core Problem   │ │ 2. Vision / Users / │ │ 3. Tech Stack    │
  │    & End Goal     │ │    Success / Values │ │    (Initial)     │
  └───────────────────┘ └─────────────────────┘ └──────────────────┘
```

The Foundation Document captures only what is intentionally fixed at the very beginning of a project — the reason for starting and the starting point. Each section answers a question that must be settled before work begins:

1. **Core Problem & End Goal** — Why does this project exist, and what does "done" look like?
2. **Vision / Target Users / Success / Core Values** — Who is it for, and what does success look like to them?
3. **Tech Stack (Initial)** — What do we start building with?

Details — especially the tech stack — are expected to evolve as work progresses.

## Step 2. Decompose into Slices

Work is intentionally kept small. Each slice has a concrete deliverable that "works" — meaning the goal defined for that slice is fully completed and verifiable. Decompose runs **incrementally**: each call derives or updates the slice(s) visible right now (often just one), and is invoked again at the start of the next slice cycle. The slice list is never produced in one shot.

## Step 3. Slice Execution Loop

```
  ┌────────────────┐   ┌───────────────┐   ┌──────┐   ┌─────────────┐   ┌────────┐   ┌──────────┐   ┌────────┐
  │ Select a Slice │──►│ Brainstorming │──►│ Plan │──►│ Development │──►│ Review │──►│ Compound │──►│ Repeat │
  └────────────────┘   └───────────────┘   └──────┘   └─────────────┘   └────────┘   └──────────┘   └────────┘
```

Each selected slice flows through these stages in sequence, carrying its *closed demo line* and *1–3 Visible Outcomes* (set in the Select stage) as the goal the loop is converging on. After Compound, the loop returns to a new slice — or, when needed, back to the Foundation Document for revision.

## Integrated View

```
  ┌─────────────────────────────────────────────────────────────────────────────────────────┐
  │                                    Foundation Phase                                     │
  └────────────────────────────────────────┬────────────────────────────────────────────────┘
                                           ▼
  ┌─────────────────────────────────────────────────────────────────────────────────────────┐
  │                                     Decompose Phase                                     │
  └────────────────────────────────────────┬────────────────────────────────────────────────┘
                                           ▼
  ┌─────────────────────────────────────────────────────────────────────────────────────────┐
  │                                  Slice Execution Loop                                   │
  │                                                                                         │
  │  Select a Slice ─► Brainstorming ─► Plan ─► Development ─► Review ─► Compound ─► Repeat │
  │          ▲                                                                       │      │
  │          └─────────────────────────── Next Slice ────────────────────────────────┘      │
  └────────────────────────────────────────┬────────────────────────────────────────────────┘
                                           │
                                           │  Update Foundation when needed
                                           ▼
                              (Loops back to Foundation Phase)
```

## Stage Principles

| Stage | Principle |
|-------|-----------|
| **Foundation** | Fix only the long-lived essentials: the problem, the vision, and an initial tech stack. Avoid premature detail. |
| **Decompose** | Break work into small slices. Each slice must produce something that works. |
| **Select a Slice** | Pick the next slice and close its Definition of Done as a *closed demo line* plus 1–3 *Visible Outcomes*. |
| **Brainstorming** | Diverge. Explore possible approaches before committing to one. |
| **Plan** | Converge. Turn the chosen approach into a concrete, executable plan. |
| **Development** | Implement against the plan. Verify the slice works end-to-end. |
| **Review** | Check correctness, quality, security, and performance against the slice goal. |
| **Compound** | Capture what was learned — patterns, reusable assets, automations — so the next slice starts faster and at a higher baseline. |
| **Repeat** | Move to the next slice. Update the Foundation Document if the project's direction has shifted. |
