---
name: status
description: List every change in docs/changes/ with its track, current stage, status and next action, read from the artifacts' frontmatter. Use when the user asks what is in progress, what is waiting for approval, where a change is up to, or what to work on next in an ai-sdlc project.
argument-hint: "[change id]"
allowed-tools: [Read, Glob, Grep, Bash]
---

# Status

Report where each change stands, based only on the committed artifacts. This skill is read-only and changes nothing.

Filter: $ARGUMENTS

## Steps

1. Find every folder under `docs/changes/`. Read the frontmatter of `intent.md`, `spec.md`, `plan.md` and `review.md` in each (whichever exist).

2. Work out each change's stage and next action:

   | State | Stage | Next action |
   |---|---|---|
   | intent `draft` | Intent | Awaiting accept/reject by the user |
   | intent `rejected` | Closed | None |
   | intent `accepted`, feature, no approved spec | Spec | `/ai-sdlc:spec` or approve the draft spec |
   | intent `accepted` (bug/enhancement) or spec `approved`, no approved plan | Plan | `/ai-sdlc:plan` or approve the draft plan |
   | plan `approved` or `in-progress` | Build | `/ai-sdlc:build` |
   | plan `done`, no `review.md` | Review | `/ai-sdlc:review` |
   | `review.md` exists, no `pr` | Review | Resolve findings and open the PR (`/ai-sdlc:review`) |
   | `review.md` has `pr`, intent has no `merged` | Awaiting merge | Merge the PR (human) |
   | intent has `merged` | Done | None |

3. Flag inconsistencies, such as a plan approved before its intent was accepted, a feature without a spec, uncommitted artifact changes (`git status`), or a `plan.md` marked `in-progress` whose branch doesn't exist.

4. Output one table, grouped as **Waiting on you** (gates and merges), **Ready for Claude**, and **Done / closed** (the last only when asked, or when it has five or fewer items). If a change id was given, show only that change, in more detail.
