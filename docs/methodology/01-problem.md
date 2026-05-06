# The Problem

## Foundation

Foundation is the first document written at the start of a project. It is the text that downstream tools — brainstorming, planning, code generation — *read* to align their behavior. It functions as their input.

Foundation contains three things.

1. **The ultimate problem or ultimate goal.** What is this project trying to solve? At what point, if reached, can it be called done?
2. **The overall outline of the project.** Vision, definition of success, priorities. What it is heading toward, what counts as well-done, which values come first among many.
3. **The rough tech stack.** The large tool choices made at the start — language, framework, core dependencies. Detailed implementation choices are not included.

This is where Foundation's answers end. It answers *what* to build, *why* to build it, and *with what tools*, but it does not answer *in what order*, *in what increments at a time*, or *what counts as evidence of working*. This asymmetry is the doorway through which the next section's problem enters.

## The Problem: One Big Inference

When an AI is given a Foundation and asked to brainstorm, plan, or generate code, it tends to reach for the ultimate goal from the Foundation in *one big inference*. Tools of this class all share this tendency — less a defect of any one tool than a natural property of reasoning that wants to go further.

The place this tendency operates is exactly Foundation's asymmetry. Foundation answers the *destination* but not the *path*. When the AI fills the unanswered blanks of the path with its own reasoning, the answered overwhelm the unanswered, and the AI heads toward code while assuming it knows what it does not.

The result is consistent — progress stalls without anything end-to-end ever working, partially generated artifacts accumulate, or an elaborate non-working structure is left behind.

## Three Layers of the Same Problem

The tendency to reach the goal in one big inference does not stay in one place. It reproduces with the same shape on three layers.

1. **Surface layer.** It tries to move from Foundation to implemented code in a single step. It stalls midway, with no end-to-end working artifact ever produced.
2. **Depth 1.** The AI reasons while filling unknowns about its environment — whether scaffolding exists, what dependencies are present, what the codebase actually looks like — by defaulting them toward *present*. Reasoning that proceeds over blanks easily ignores those blanks.
3. **Depth 2.** Even when work is sliced finely, the tendency does not disappear — each sliced unit itself swells back into another single big inference.
