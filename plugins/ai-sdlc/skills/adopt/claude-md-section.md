
## Development workflow (ai-sdlc)

Non-trivial changes go through committed artifacts in `docs/changes/<YYYY-MM-DD>-<slug>/`: `intent.md` → `spec.md` (features only) → `plan.md` → code + tests → `review.md` + PR. Each artifact is committed at its gate, and the next stage starts from the committed file, not from conversation context. Use the `ai-sdlc` skills: `/ai-sdlc:intent`, `/ai-sdlc:spec`, `/ai-sdlc:plan`, `/ai-sdlc:build`, `/ai-sdlc:review`, `/ai-sdlc:status`.

- Never write production code for a change without an approved `plan.md`.
- If the implementation departs from `plan.md`, update the plan in the same commit.
- Never mark your own artifact accepted or approved. Wait for the user.
- Typos, dependency bumps and one-line config changes skip the process.

Commands:

- Test: `<test command>`
- Lint: `<lint command>`

### Things Claude gets wrong here

<!-- Add one line per recurring mistake found in review. Keep this list short; remove entries once they stop recurring. -->
