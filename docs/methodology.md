# Methodology

This is a scaled-down adoption of Anthropic's [AI-Native SDLC Playbook](https://claude.com/blog/the-ai-native-sdlc-playbook), sized for small projects with one or two people. It keeps the playbook's core mechanism and drops the parts that only pay off at organisational scale. The drops are listed at the end, so the gap to the full playbook is visible when this goes to a larger team.

## The one rule

Each stage ends by committing a Markdown artifact, and the next stage starts by reading it. The next stage does not rely on the conversation, on memory, or on the previous session's context. Only the committed file carries forward.

This rule does two things. The agent starts every stage with a small, self-contained context, and git becomes the audit trail. The intent, spec, plan, diff and review findings together record what was asked, what was agreed, what was built and what was checked, with author and timestamp on each.

## The chain

```
backlog item ──► intent.md ──► spec.md ──► plan.md ──► code + tests ──► review.md + PR
  (optional)        gate         gate        gate                        human merge
```

| Stage | Artifact | Skill | Gate (who, what) |
|---|---|---|---|
| Intent | `intent.md` | `/ai-sdlc:intent` | Owner accepts or rejects the intent |
| Spec | `spec.md` | `/ai-sdlc:spec` | Owner approves the spec. Feature track only |
| Plan | `plan.md` | `/ai-sdlc:plan` | Engineer approves the plan before any code is written |
| Build | code + tests, plan kept truthful | `/ai-sdlc:build` | Proof in `plan.md` passes. The verifier agent confirms it |
| Review | `review.md`, then a PR | `/ai-sdlc:review` | A human merges. The agent never approves its own work |

`/ai-sdlc:status` lists every change and the stage it has reached. `/ai-sdlc:adopt` sets up a project.

## Tracks: size the process to the change

The full playbook has seven human gates. Most personal-project changes don't justify that many, so each change picks a track when its intent is written:

| Track | Use for | Chain |
|---|---|---|
| `bug` | Behaviour differs from what was intended | intent → plan → build → review |
| `enhancement` | Improves something that already exists, without new concepts or interfaces | intent → plan → build → review |
| `feature` | New capability, new interface, new data, or anything whose "done" is arguable | intent → spec → plan → build → review |

The track can be escalated at any gate. If planning a `bug` or `enhancement` shows that behaviour needs defining, not just fixing, switch to `feature` and write the spec. Tracks never de-escalate once a spec exists.

Trivial changes, such as typos, dependency bumps and one-line config changes, stay outside the chain entirely. A good test: skip the chain when writing the intent would take longer than making the change.

## Where artifacts live

Each change gets one folder, named by the date the intent was raised and a short slug:

```
docs/changes/
  2026-09-26-csv-import-drops-last-row/
    intent.md
    plan.md
    review.md
  2026-09-28-monthly-forecast-view/
    intent.md
    spec.md
    plan.md
    review.md
```

The folder is the unit of work. Its artifacts' frontmatter holds the change's state, so no separate tracker is needed. `/ai-sdlc:status` reads the frontmatter and reports it.

## Gates

A gate means the human reads the artifact and says yes. Then the skill:

1. sets `status: accepted` or `status: approved` in the artifact's frontmatter, with the date;
2. commits the artifact on its own (an intent's gate commit also carries its linked backlog item's update), with a message such as `intent(2026-09-26-csv-import-drops-last-row): accept`.

The commit is the gate record. Rejections are committed too, with `status: rejected` and one line of reasoning, so the reason survives and the idea isn't raised again unknowingly.

Between gates the agent works autonomously. At a gate it stops and waits. No skill may approve its own artifact or move past a gate because the next step looks obvious.

## Fresh context at each stage

Start a new session (or `/clear`) between stages where practical, especially between spec and plan and between plan and build. The artifact is the handoff. A stage that only works with the previous conversation still loaded is a sign the artifact is incomplete, so fix the artifact.

## Keeping the plan truthful

When implementation departs from `plan.md`, update the plan in the same commit as the code that departs from it, and add a line to its *Deviations* section. A plan that no longer describes the code is worse than no plan.

## Relationship to the backlog

The existing `backlog/` folder (from the `backlog-creator` skill) stays as the **inbox**: cheap, uncommitted-to ideas and known problems. A backlog item enters this process when `/ai-sdlc:intent` is run on it. The intent links back to the item, and the item is marked `in-progress` and linked forward to the change folder. When the change merges, the backlog item is closed.

This replaces the old *implementation plan* and *tracker* documents. `plan.md` is the implementation plan, and the artifact frontmatter is the tracker.

## Project controls

The playbook puts controls in three layers, in ascending strength. This plugin uses them as follows:

| Layer | Enforcement | Used here for |
|---|---|---|
| `CLAUDE.md` | Advisory, always loaded | The project's build/test/lint commands, conventions, and a short section pointing at this process |
| Skills | Advisory, loaded when relevant | The stage skills in this plugin, plus any project-specific policy skills |
| Hooks | Deterministic, blocks the action | Nothing yet. See *Not yet adopted* |

`REVIEW.md` at the project root is the review policy. `/ai-sdlc:review` applies it to every change.

Review findings feed back. When a review finds a mistake Claude is likely to repeat, `/ai-sdlc:review` proposes a line for the project's `CLAUDE.md`, so the next change avoids it and it isn't re-reviewed forever.

## Not yet adopted

These playbook parts are deliberately left out of v0.1. Each belongs in a later version or at organisational scale:

- **Hooks that enforce gates**, for example blocking source edits when no approved `plan.md` exists for the current branch. This is the first addition worth making once the skills have settled.
- **An eval suite for the agent configuration**: 20–50 real tasks with expected outcomes, run on changes to `CLAUDE.md`, skills or hooks. This matters at organisational scale, and is overkill for one person until the skills are shared across many projects.
- **Deploy-stage permission tiers** and a production gate hook.
- **The Maintain stage**: deterministic anomaly bands (`bands.yaml`) feeding new `intent.md` files.
- **Managed settings** for regulated environments.
- **Metrics.** The playbook's leading indicators (intent → spec time, plan approval → merge time) can be derived from the artifact commit timestamps later, so no instrumentation is needed now. Capturing the timestamps consistently, which the gate commits already do, is what keeps that option open.
