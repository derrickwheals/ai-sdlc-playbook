---
type: review
id: YYYY-MM-DD-slug
plan: ./plan.md
reviewed: YYYY-MM-DD
base: main
head: <commit sha reviewed>
reproven:              # sha the plan's proof passed on after fixes; blank if no fixes
pr:                    # PR url, set when the PR is opened
---

# Review: <Title>

## Summary

<Important: N (fixed N, disputed N, open N) · Nits: N>

## Findings

| # | Category | Severity | Location | Finding | Failure scenario | Status |
|---|---|---|---|---|---|---|
| 1 | Bugs | Important | `path/file:42` | <one sentence> | <concrete input or state → wrong result> | fixed \| disputed \| open |

## Disputed

<For each disputed finding: why it is believed wrong. Left for the human to decide.>

## Fed back to CLAUDE.md

- <Line added, or "none">
