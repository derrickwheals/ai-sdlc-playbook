---
name: build
description: Implement an approved plan.md on its own branch, test-first where practical, keeping the plan truthful and finishing only when the plan's proof passes and the verifier agent confirms it. Use when the user wants to implement, build or start coding a change whose plan is approved.
argument-hint: "<change id or folder>"
---

# Stage 4 — Build

Implement exactly what the approved plan describes, and prove it works.

Change: $ARGUMENTS

## Preconditions

- `plan.md` has `status: approved` (or `in-progress` when resuming). If it doesn't, stop. **No approved plan means no code.**
- The working tree is clean, or the user confirms the existing changes belong to this change.

## Steps

1. **Read** `plan.md`, plus `spec.md` if it exists and `intent.md`. Treat these as the full brief.

2. **Branch.** Create or switch to the branch named in the plan (default `change/<id>`) from the up-to-date base branch. Set the plan's `status: in-progress` and commit that with the first step.

3. **Work through *Order of work* one step at a time.** For each step:
   - write or adjust the test first where practical, and see it fail for the right reason;
   - make the change, following `CLAUDE.md` and the surrounding code's conventions;
   - run the relevant tests;
   - tick the step in `plan.md` and commit code, tests and plan together, e.g. `feat(<id>): step 2 — parse trailing row`.

4. **Keep the plan truthful.** If the work departs from the plan (a different file, an extra step, a changed approach), update `plan.md`, add a line under *Deviations*, and commit it **in the same commit** as the code. If the departure changes *what* is delivered rather than *how*, stop and ask the user. That calls for a plan re-approval, not a deviation note.

5. **Run the proof.** Run every command under *Proof* in the plan. All must pass. Fix your own failures. Don't weaken or skip a test to get a pass. If a test itself is wrong, say so and get agreement before changing it.

6. **Verify independently.** Launch the `ai-sdlc:verifier` agent with the change id. It exercises the changed behaviour and reports only; it doesn't fix. Resolve anything it finds, then re-verify.

7. **Finish.** Set `status: done` in `plan.md`, commit, and point to `/ai-sdlc:review <id>`. Don't push or open a PR in this stage.
