---
name: intent
description: Capture a new bug fix, enhancement or feature as a committed intent.md in docs/changes/, the first stage of the ai-sdlc workflow. Use when the user wants to start work on a change, raise an intent, promote a backlog item into work, or says "let's fix/build/add X" in a project that has docs/changes/.
argument-hint: "<what you want changed> | <path to backlog item>"
---

# Stage 1 — Intent

Write a short intent that records **what** is being asked for, **why**, and **within what constraints**, before anyone designs or builds anything. The intent is in the originator's words, sharpened by conversation. It is not a design.

Input: $ARGUMENTS

## Steps

1. **Establish the source.** If the input is a path to a backlog item, read it and use it as the starting point. Otherwise use the user's description.

2. **Pick the track:**
   - `bug`: existing behaviour is wrong;
   - `enhancement`: improves existing behaviour without new concepts or interfaces;
   - `feature`: new capability, interface or data, or "done" is arguable.

   Propose a track with a one-line reason. If it's genuinely unclear, ask.

3. **Brainstorm briefly.** Ask only what is needed to fill the template. For a `bug`, get observed vs. expected behaviour and a reproduction. Look at the code only enough to name the affected systems accurately. Don't design a solution. The intent says what outcome is wanted, not how to get it.

4. **Write the intent.** Create `docs/changes/<YYYY-MM-DD>-<slug>/intent.md` from `template.md` in this skill's directory. Use today's date and a slug of 3–6 lowercase hyphenated words. Set `status: draft`. Record unresolved questions under *Open questions*. Don't guess answers.

5. **Gate: ask the user to accept or reject.** Show the intent and stop. Don't proceed until the user answers.
   - **Accept:** set `status: accepted` and `accepted: <today>`, then commit this file, together with the backlog item update in step 6 if there is one: `intent(<id>): accept`.
   - **Reject:** set `status: rejected` and add one line of reasoning under *Decision*, then commit: `intent(<id>): reject`.
   - **Revise:** edit and ask again.

6. **Link the backlog item** (if there was one): set its status to `in-progress` and add a link to the change folder, in the same commit as the acceptance.

7. **Point to the next stage.** `feature` → `/ai-sdlc:spec <id>`. `bug` or `enhancement` → `/ai-sdlc:plan <id>`. Suggest starting it in a fresh session.
