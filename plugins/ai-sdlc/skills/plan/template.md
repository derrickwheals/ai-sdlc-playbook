---
type: plan
id: YYYY-MM-DD-slug
intent: ./intent.md
spec: ./spec.md        # feature track only; delete otherwise
status: draft          # draft | approved | in-progress | done
approved:
branch: change/YYYY-MM-DD-slug
---

# Plan: <Title>

## Approach

<One paragraph: how the change will be made, and why this way rather than the obvious alternative, if there is one.>

## Reproduction

<Bug track: how the bug was reproduced, or a statement that it could not be. Delete for other tracks.>

## Files that change

- `path/to/file` — <what changes and why>

## Order of work

- [ ] 1. <Step. Small enough to commit on its own.>
- [ ] 2. <…>

## Risks

- <What could break> — <how the plan guards against it>

## Proof

Tests to add or change:

- `path/to/test` — <what it proves>

Must pass before the change is complete:

```
<exact test / lint / build commands>
```

Requirement coverage (feature track):

| Requirement | Proven by |
|---|---|
| R1 | <test or check> |

## Deviations

<Filled during build. One line per departure from this plan, added in the same commit as the code that departs.>
