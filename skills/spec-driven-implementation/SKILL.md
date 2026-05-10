---
name: spec-driven-implementation
description: Drive a spec-first workflow for substantial features by writing PRODUCT.md before implementation, writing TECH.md when warranted, and keeping both specs updated as implementation evolves. Use when starting a significant feature, planning agent-driven implementation, or when the user wants product and tech specs checked into source control.
---

# spec-driven-implementation

Drive a spec-first workflow for substantial features in the target product, app, service, API, CLI, library, or data model.

## Overview

Use this skill for significant features where a written spec will improve implementation quality, reduce ambiguity, or make review easier. Be pragmatic: not every change needs specs.

Specs should usually live in:

- `specs/<id>/PRODUCT.md`
- `specs/<id>/TECH.md`

`<id>` should match the product and tech spec skills:

- a Linear ticket number, such as `specs/APP-1234/PRODUCT.md`
- a GitHub issue id prefixed with `gh-`, such as `specs/gh-4567/PRODUCT.md`
- a GitLab issue id prefixed with `gl-`, such as `specs/gl-7890/PRODUCT.md`
- a short kebab-case feature name, such as `specs/vertical-tabs-hover-sidecar/PRODUCT.md`

`specs/` should contain only id-named directories as direct children. Do not create engineer-named subdirectories there.

Ticket / issue references are optional. If the user has a Linear ticket, GitHub issue, or GitLab issue, use its id. If they do not, ask for a short feature name to use as the directory id. Only create a new ticket or issue when the user explicitly asks for one; use the relevant Linear, GitHub, or GitLab tools, and ask the user directly if required project, team, repo, labels, or metadata are unclear.

These specs should largely be written by agents, not by hand, and should be checked into source control so they can be reviewed and kept current with the code.

## When specs are required

Strongly prefer specs when the change is substantial, such as:

- product or architectural ambiguity
- expected implementation size around 1k+ LOC
- deep or cross-cutting stack changes
- risky behavior changes where regressions would be expensive
- work where agent quality will improve materially from clearer inputs

Specs are often unnecessary for:

- small, local bug fixes
- straightforward refactors
- narrow UI tweaks with little ambiguity

For pure UI changes, the product spec is often useful while the tech spec may be unnecessary.

## Workflow

### 1. Decide whether the feature needs specs

Evaluate the size, ambiguity, and risk of the feature. If specs will not meaningfully improve execution or review, skip them and focus on verification instead.

### 2. Write the product spec first

Before implementation, create `PRODUCT.md` describing the desired user-facing behavior.

Use the `write-product-spec` skill to produce it. The product spec should define:

- what problem is being solved
- the desired user experience
- invariants and edge cases
- every user-visible state, transition, and behavior that must not regress

If the feature has UI or interaction design, ask for a Figma mock if one exists. If there is no mock, continue but call that out explicitly in the product spec.

Reference the source ticket, issue, or feature id in the spec when one exists.

### 3. Write the tech spec when warranted

Use the `write-tech-spec` skill for substantial or ambiguous implementation work.

Prefer a tech spec when:

- the implementation spans multiple subsystems
- architecture or extensibility matters
- there are meaningful tradeoffs to document
- reviewers will benefit more from reviewing the plan than the raw code

It is acceptable to write the tech spec after an e2e prototype if that leads to a more accurate implementation plan. Do not force a premature tech spec when the implementation details are still too uncertain.

### 4. Implement approved specs

After the specs are approved, use the `implement-specs` skill to build from the approved `PRODUCT.md` and `TECH.md` when present.

The implementation can often be pushed in the same PR as the product and tech specs. As the engineer iterates, keep `PRODUCT.md`, `TECH.md`, code changes, and tests in that same PR so the review reflects the feature that will actually ship.

For large features, the implementer may optionally offer:

- `PROJECT_LOG.md` to track explored paths, checkpoints, and current implementation state
- `DECISIONS.md` to capture concrete product and technical decisions made during design and implementation

These are optional aids, not required outputs.

### 5. Keep specs current during implementation

If implementation changes from the spec, update the spec rather than leaving it stale.

Update `PRODUCT.md` when:

- user-facing behavior changes
- UX details or edge cases change
- behavior invariants or externally visible acceptance expectations change

Update `TECH.md` when:

- the implementation approach changes
- architectural boundaries move
- risks, dependencies, or rollout details change
- the testing or validation plan changes

The checked-in specs should describe the feature that actually ships, not just the initial intent. Keep those spec updates in the same PR as the related code changes whenever practical.

### 6. Verify behavior against the spec

Before considering the work complete, make sure verification maps back to the specs. Prefer tests and artifacts that validate the product behavior directly:

- unit tests using the repository's existing test framework
- integration or end-to-end tests for critical user flows
- walkthroughs, screenshots, videos, or other artifacts when useful for UI-heavy work

## Best Practices

- Be pragmatic above all else.
- Write specs to improve input quality for agents, not as ceremony.
- Keep product specs behavior-oriented and implementation-light.
- Keep tech specs implementation-oriented and grounded in current codebase patterns.
- Use review time to validate specs and behavior, not to over-index on code style nits.

## Related Skills

- `implement-specs`
- `write-product-spec`
- `write-tech-spec`
