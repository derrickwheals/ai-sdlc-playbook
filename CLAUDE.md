# ai-sdlc-playbook

A Claude Code plugin marketplace containing one plugin, `ai-sdlc`. The process it implements is defined in `docs/methodology.md`. Keep that document, the skills and the templates consistent with each other. A change to one usually needs a change to the others.

## Conventions

- One skill per stage, in `plugins/ai-sdlc/skills/<name>/SKILL.md`. The artifact a skill writes sits beside it as `template.md`, and the skill refers to it as "in this skill's directory".
- Skills are instructions for Claude working in *another* project. Don't reference files in this repo (such as `docs/methodology.md`) from a skill, because they won't exist there. Put what the skill needs in the skill itself.
- Every gate stops and waits for the user. No skill may set its own artifact to accepted/approved.
- Markdown prose isn't hard-wrapped: one paragraph per line.
- When something is taken from an external source, record it in `docs/provenance.md`.

## Releasing a change

1. Bump `version` in `plugins/ai-sdlc/.claude-plugin/plugin.json` and in the plugin's entry in `.claude-plugin/marketplace.json`, keeping them identical.
2. Run `claude plugin validate .` from the repo root.
3. Commit with a message saying what changed in the process, not just which files.
