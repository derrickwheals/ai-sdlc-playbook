---
name: spec
description: Turn an accepted intent.md into a self-contained spec.md by exploring the codebase and interviewing the user, the design stage of the ai-sdlc workflow. Use for feature-track changes after the intent is accepted, or when a bug/enhancement is escalated because its behaviour needs defining.
argument-hint: "<change id or folder>"
---

# Stage 2 — Spec (feature track)

Produce a spec that someone with no access to this conversation could implement correctly and verify. A good spec is self-contained. It names the files and interfaces involved, states what is out of scope, and ends with an end-to-end verification that proves the feature works.

Change: $ARGUMENTS

## Preconditions

- `docs/changes/<id>/intent.md` exists with `status: accepted`. If it is still draft or rejected, stop and say so.
- If the argument is empty, run the logic of `/ai-sdlc:status` and ask which accepted change to spec.

## Steps

1. **Read the intent** in full. Read nothing from earlier conversations. The intent is the only input about what is wanted.

2. **Explore the codebase** for what the intent touches: existing patterns, similar features, the interfaces and data involved, and how things are tested. Use an Explore subagent for broad sweeps, so exploration output stays out of the main context. Read the key files it identifies yourself.

3. **Interview the user.** Use `AskUserQuestion` for decisions the intent leaves open: edge cases, error behaviour, UX, data shape, trade-offs. Ask about things that would change the design, not things you can settle from the code or a sensible default. Keep going until the open questions in the intent are resolved or explicitly deferred.

4. **Flag, don't resolve, conflicts.** If the intent contradicts itself, contradicts existing behaviour, or conflicts with a policy in `CLAUDE.md` or a project skill, record it under *Flags* and ask. Never silently pick a side.

5. **Write** `docs/changes/<id>/spec.md` from `template.md` in this skill's directory, with `status: draft`. Number every requirement (R1, R2…) so the plan and review can refer to them.

6. **Gate: ask the user to approve.** Show the spec and stop.
   - **Approve:** set `status: approved` and `approved: <today>`, then commit only this file: `spec(<id>): approve`.
   - **Revise:** edit and ask again.
   - If the discussion changes what is wanted, not just how, update `intent.md` too and say so.

7. **Point to the next stage:** `/ai-sdlc:plan <id>`, ideally in a fresh session.
