---
name: adopt
description: Set up the ai-sdlc process in the current project by creating docs/changes/, a REVIEW.md review policy, and a short CLAUDE.md section describing the workflow. Use when the user wants to adopt, install, set up or bootstrap the intent → spec → plan workflow in a repository.
argument-hint: "[--dry-run]"
---

# Adopt the ai-sdlc process

Set up the current repository so the other `ai-sdlc` skills can run in it. This skill is idempotent: it never overwrites existing content, and running it twice changes nothing the second time.

Arguments: $ARGUMENTS

## Steps

1. **Check the repository.** Confirm the working directory is a git repository. If it is not, stop and say so. Adoption without git defeats the process, because commits are the gate records.

2. **Survey what exists.** Look for, and report:
   - `CLAUDE.md` (or `AGENTS.md`) at the root, and whether it already has an `## Development workflow (ai-sdlc)` section;
   - `REVIEW.md` at the root;
   - `docs/changes/`;
   - a `backlog/` folder. If one exists, note that it will act as the inbox (see the methodology's *Relationship to the backlog*);
   - any existing implementation-plan or tracker documents. List them. Don't move or delete them; migration is the user's call.
   - the project's test, lint and build commands, if discoverable from `package.json`, `pyproject.toml`, `Makefile` or similar.

3. **Show the plan and confirm.** Tell the user which files you will create or append to. If `--dry-run` was passed, stop here.

4. **Create what is missing:**
   - `docs/changes/.gitkeep`
   - `REVIEW.md` from `REVIEW.template.md` in this skill's directory. Fill in the project's test command if known.
   - Append the contents of `claude-md-section.md` in this skill's directory to `CLAUDE.md`. Create `CLAUDE.md` if there is none. If the project uses `AGENTS.md` instead, append there. Fill in the test and lint commands if known; otherwise leave the placeholders and say so.

5. **Commit** the setup as one commit: `chore: adopt ai-sdlc workflow`. Do not push.

6. **Report** what was created, and suggest a first change to run through the process. An open backlog item is ideal.

## Enabling the plugin for everyone who clones the project

If the user wants the plugin enabled automatically for this project, add this to `.claude/settings.json`. Merge it with any existing content; never overwrite it. The marketplace repository is private, so anyone opening the project needs read access to it on GitHub:

```json
{
  "extraKnownMarketplaces": {
    "ai-sdlc-playbook": {
      "source": { "source": "github", "repo": "derrickwheals/ai-sdlc-playbook" }
    }
  },
  "enabledPlugins": {
    "ai-sdlc@ai-sdlc-playbook": true
  }
}
```
