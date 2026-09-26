# ai-sdlc-playbook

A Claude Code plugin marketplace for a lightweight, artifact-driven development process: bug fixes, enhancements and features go through committed `intent.md` → `spec.md` → `plan.md` → code + tests → `review.md` + PR, with a human approval gate at each handoff.

It adapts Anthropic's [AI-Native SDLC Playbook](https://claude.com/blog/the-ai-native-sdlc-playbook) for small projects. The aim is one standard way of working across personal projects, and a proving ground before proposing it at larger scale.

- **[docs/methodology.md](docs/methodology.md)**: the process, including tracks, gates, artifacts, backlog integration, and what is deliberately left out.
- **[docs/provenance.md](docs/provenance.md)**: which parts come from Anthropic's material and which are ours.

## What's in the plugin

`plugins/ai-sdlc/`:

| Component | Purpose |
|---|---|
| `/ai-sdlc:adopt` | Set up a project: `docs/changes/`, `REVIEW.md`, a `CLAUDE.md` section |
| `/ai-sdlc:intent` | Stage 1: capture a change as `intent.md`. Gate: accept/reject |
| `/ai-sdlc:spec` | Stage 2 (feature track): interview and write `spec.md`. Gate: approve |
| `/ai-sdlc:plan` | Stage 3: write `plan.md` with files, order, risks and proof. Gate: approve |
| `/ai-sdlc:build` | Stage 4: implement the approved plan on a branch, keeping the plan truthful |
| `/ai-sdlc:review` | Stage 5: fresh-context review against `REVIEW.md`, then `review.md` and the PR |
| `/ai-sdlc:status` | Show every change, its stage, and what it is waiting on |
| `verifier` agent | Read-only: runs the proof and exercises the behaviour, then reports |

## Install

Add the marketplace and install the plugin from inside Claude Code:

```
/plugin marketplace add ~/GitHub/ai-sdlc-playbook
/plugin install ai-sdlc@ai-sdlc-playbook
```

Once the repo is on GitHub, use `/plugin marketplace add <owner>/ai-sdlc-playbook`. Then, in each project:

```
/ai-sdlc:adopt
```

To have the plugin enabled for anyone who opens a project, `/ai-sdlc:adopt` can add the marketplace and plugin to that project's `.claude/settings.json`.

## Updating

Edit the skills here, bump `version` in both `plugins/ai-sdlc/.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`, commit, then run `/plugin marketplace update ai-sdlc-playbook` in Claude Code. Every project using the plugin picks up the change, with no copies to keep in sync.

## Layout

```
.claude-plugin/marketplace.json     marketplace catalogue
docs/                               methodology and provenance
plugins/ai-sdlc/
  .claude-plugin/plugin.json        plugin manifest
  agents/verifier.md
  skills/<stage>/SKILL.md           one skill per stage
  skills/<stage>/template.md        the artifact that stage writes
  skills/adopt/                     REVIEW.md and CLAUDE.md templates
```
