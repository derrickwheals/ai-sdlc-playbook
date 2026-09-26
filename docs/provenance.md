# Provenance

Anthropic has not published an official implementation or template kit for the AI-Native SDLC Playbook. The playbook article includes example artifacts inline, and links out to the Claude Code documentation. This repository is therefore our own implementation, drawing on the sources below. None of their text is copied verbatim. Each item was re-written for this process.

## Anthropic sources

| Source | What we took | Where it landed |
|---|---|---|
| [The AI-Native SDLC Playbook](https://claude.com/blog/the-ai-native-sdlc-playbook) (Anthropic Applied AI) and the [Claude Academy course](https://academy.claude.com/courses/ai-native-sdlc-playbook) | The artifact chain and the commit-per-stage rule; the intent, spec and plan section headings; `plan.md` updated in the same commit as departing code; the `REVIEW.md` structure (named passes, Important vs. Nit, nit cap); the read-only verifier subagent; review findings fed back into `CLAUDE.md`; the three control layers; the "agent may not approve its own work" principle | `docs/methodology.md`, all stage skills and templates, `agents/verifier.md`, `skills/adopt/REVIEW.template.md` |
| [Claude Code best practices](https://code.claude.com/docs/en/best-practices) | The interview-then-write-spec pattern using `AskUserQuestion`; the definition of a good spec (self-contained, names files and interfaces, states what is out of scope, ends with end-to-end verification); a fresh session between spec and implementation; Explore → Plan → Implement → Commit | `skills/spec`, `skills/plan`, the methodology's *Fresh context at each stage* |
| [`feature-dev` plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/feature-dev) (Apache-2.0) | The pattern of exploring with subagents, then reading the files they identify; asking clarifying questions before designing; confidence-filtered review findings | `skills/spec`, `skills/plan`, `skills/review`. Structure only, no text copied |
| [`code-review`](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/code-review) / [`pr-review-toolkit`](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/pr-review-toolkit) plugins | Used as the reviewer when installed. Not bundled | `skills/review` step 2 |
| [Plugin marketplace docs](https://code.claude.com/docs/en/plugin-marketplaces) and the official marketplace's layout | Repository structure, manifests, `extraKnownMarketplaces` / `enabledPlugins` project settings | `.claude-plugin/`, `plugins/ai-sdlc/.claude-plugin/`, `skills/adopt` |

## Community implementations reviewed

These are third-party, not Anthropic's. They were looked at as reference points, and nothing was taken from them:

- [jsnkle/ai-native-sdlc](https://github.com/jsnkle/ai-native-sdlc) (MIT): the closest in shape, a plugin with intent/spec/plan/babysit-pr/adopt skills, subagents, hooks and a brownfield runbook. Worth revisiting when adding hooks.
- [JHashimoto0518/ai-native-sdlc-playbook-sample](https://github.com/JHashimoto0518/ai-native-sdlc-playbook-sample): a minimal worked example of one requirement through the full chain.
- [bashebr/ai-native-sdlc](https://github.com/bashebr/ai-native-sdlc), [maoyoushi/ai-native-sdlc](https://github.com/maoyoushi/ai-native-sdlc), [ScimaxGlobal/ai-native-sdlc](https://github.com/ScimaxGlobal/ai-native-sdlc): the full six stages with governance material, closer to what an enterprise rollout would need.

## Our own additions

Not in the playbook, and introduced here for small-project use:

- **Tracks** (`bug` / `enhancement` / `feature`), with the spec only on the feature track, and the escalation rule.
- The per-change folder `docs/changes/<date>-<slug>/`, and artifact frontmatter as the tracker (`/ai-sdlc:status`).
- Integration with the existing `backlog/` folder as the inbox for intents.
- The explicit list of playbook parts not yet adopted, in `docs/methodology.md`.
