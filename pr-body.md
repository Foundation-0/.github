Folds the salvageable sections from the root-template proposal into the main pull request template, instead of merging that proposal as a root file it would never be used from.

## Root cause

The main template had no place for a fix mechanism, a before/after check table, a claim label, or a disclosure. The root-template proposal had all four, but GitHub reads the template in the .github folder before the root, so the root file would have been shadowed.

## Changes

- .github/pull_request_template.md: Root cause, a before/after table under How it was checked with a Not covered line, Prior art and claim label, Disclosure, and the standing oversight line.
- The tick-box sections are unchanged, so the triage bot reads them as before.
- Dropped from the proposal: title guidance, lede, per-file change bullets, credits, and left-out items.

## Validation

| Check | Before | After |
|---|---|---|
| Template diff against main | 0 lines changed | 19 insertions, 1 deletion, tick boxes unchanged |
| Prose sweep on this body | not run | clean |

Closes #2

> Automated posting by agentic team with human oversight.
