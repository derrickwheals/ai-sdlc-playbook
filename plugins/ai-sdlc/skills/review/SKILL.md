---
name: review
description: Review a built change against the project's REVIEW.md with a fresh-context reviewer, record categorised findings in review.md, fix Important findings, feed recurring mistakes back into CLAUDE.md, then draft the PR. Use when a change's plan is done and the user wants to review it, prepare it for merge, or open a PR.
argument-hint: "<change id or folder>"
---

# Stage 5 — Review and PR

Every change gets the same review passes, whoever or whatever wrote it. The agent that built the change never approves it. A human merges.

Change: $ARGUMENTS

## Preconditions

- `plan.md` has `status: done`, and the change branch is checked out.
- `REVIEW.md` exists at the project root. If it doesn't, suggest `/ai-sdlc:adopt`, or fall back to the default passes: bugs, security, conventions.

## Steps

1. **Collect the review input:** the diff of the branch against its base, `REVIEW.md`, `CLAUDE.md`, and the change's `intent.md`, `spec.md` (if any) and `plan.md`.

2. **Review with fresh context.** Launch a subagent that didn't take part in the build. Use the `code-review` or `pr-review-toolkit` agents if they are installed; otherwise use a general-purpose subagent. Give it the input above and have it run each pass in `REVIEW.md` separately. Every finding needs: category, severity (Important or Nit), file and line, and a concrete failure scenario. Drop findings that can't state a failure scenario, and respect the nit cap.

3. **Write** `docs/changes/<id>/review.md` from `template.md` in this skill's directory.

4. **Resolve Important findings.** Fix each one (with a test if it's a behaviour bug), commit, and mark it `fixed` in `review.md`. If you believe a finding is wrong, mark it `disputed` with the reason and leave it for the human. Never mark your own dispute as accepted. Nits are fixed at your discretion, or left `open`. If you changed any code, re-run every command under *Proof* in `plan.md`; all must pass. Record the commit they passed on as `reproven: <sha>` in `review.md`'s frontmatter. `head` stays the commit that was reviewed.

5. **Feed back.** For each finding that reflects a mistake likely to recur in this project, propose one line for the *Things Claude gets wrong here* section of `CLAUDE.md`. Show the proposals and add only the ones the user agrees to.

6. **Commit** `review.md` (and any `CLAUDE.md` update): `review(<id>): findings`.

7. **Open the PR, after confirming with the user.** Pushing and opening a PR are visible outside this machine, so ask first. Then push the branch and run `gh pr create`. Set `pr: <url>` in `review.md` and commit it: `review(<id>): open PR`. Use the intent's title, and a body that links the change folder's artifacts, summarises what changed, lists review findings still open or disputed, and gives the proof commands.

8. **After merge** (whenever the user reports it): close the linked backlog item, if there is one, and set the intent's frontmatter `merged: <date>`.
