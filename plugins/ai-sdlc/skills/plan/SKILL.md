---
name: plan
description: Write plan.md for an accepted change (which files change, order of work, risks, proof) and get it approved before any code is written, the planning half of the ai-sdlc build stage. Use after an intent is accepted (bug/enhancement) or a spec is approved (feature).
argument-hint: "<change id or folder>"
---

# Stage 3 — Plan

Produce a plan an engineer can approve in a few minutes, and that a fresh session can implement without asking what was meant. **Write no production code in this stage.**

Change: $ARGUMENTS

## Preconditions

- `intent.md` has `status: accepted`.
- For `track: feature`: `spec.md` exists with `status: approved`. If it does not, stop and point to `/ai-sdlc:spec`.
- If the argument is empty, run the logic of `/ai-sdlc:status` and ask which change to plan.

## Steps

1. **Read the artifacts**: `intent.md`, plus `spec.md` if there is one. These are the only inputs about what is wanted.

2. **Explore the code** the change touches, and how it's tested. For a `bug`, reproduce it first if you can, and record how in the plan. A plan for a bug you couldn't reproduce must say so. Use an Explore subagent for broad searches.

3. **Check the track.** If planning shows that a `bug` or `enhancement` needs behaviour defined, not just implemented, stop and recommend escalating to `feature` with a spec. Don't write the spec inside the plan.

4. **Write** `docs/changes/<id>/plan.md` from `template.md` in this skill's directory, with `status: draft`:
   - **Files that change**: every file to be created, modified or deleted, with its role.
   - **Order of work**: numbered, checkable steps small enough to commit individually. Where practical, a failing test comes before the fix or feature.
   - **Risks**: what could break, and how the plan guards against it.
   - **Proof**: the tests to add or change, and the exact commands that must pass. For features, map each spec requirement (R1…) to the test or check that proves it.

5. **Gate: ask the user to approve.** Show the plan and stop.
   - **Approve:** set `status: approved` and `approved: <today>`, then commit only this file: `plan(<id>): approve`.
   - **Revise:** edit and ask again.

6. **Point to the next stage:** `/ai-sdlc:build <id>`, ideally in a fresh session.
