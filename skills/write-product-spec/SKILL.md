---
name: write-product-spec
description: Write a PRODUCT.md spec for a significant user-facing feature or externally consumed surface in any product, app, service, API, CLI, library, or data model. Use when the user asks for a product spec, desired behavior doc, PRD, or behavior-first spec, wants to define feature behavior before implementation, or when the feature is substantial or behaviorally ambiguous enough that a written spec would improve implementation or review.
---

# write-product-spec

Write a `PRODUCT.md` spec for a significant feature in the target product, app, service, API, CLI, library, or data model.

## Overview

The product spec should make the desired behavior unambiguous enough that an agent can implement it correctly and avoid regressions. Describe the feature purely from the user's perspective — what the user sees, does, and experiences, and the invariants that must hold for them. Do not include implementation details (internal types, state layout, module boundaries, data flow, algorithms).

"User" means whoever consumes the surface being designed:

- For UI / UX features: the human using the product or application.
- For a data model: the code that reads and writes that model.
- For an API, protocol, or library: the callers of that API — other services, client code, plugins, or agents.
- For a CLI tool or developer-facing surface: the developer invoking it.

The spec should describe behavior from that consumer's perspective: the shape of the surface, the operations they can perform, what they see back, invariants they can rely on, and edge cases they must handle — without prescribing how the surface is implemented underneath.

Implementation details, validation, and test planning live in a companion `TECH.md`, produced by the `write-tech-spec` skill. Writing the product spec is usually the first step of a two-step process: once `PRODUCT.md` is agreed on, invoke `write-tech-spec` to produce `TECH.md` for the same feature (or let the user know that's the expected next step). The product spec should be written so the tech spec can be written directly from it.

Write specs to `specs/<id>/PRODUCT.md`, where `<id>` is one of:

- a Linear ticket number (e.g. `specs/APP-1234/PRODUCT.md`)
- a GitHub issue id, prefixed with `gh-` (e.g. `specs/gh-4567/PRODUCT.md`)
- a short kebab-case feature name (e.g. `specs/vertical-tabs-hover-sidecar/PRODUCT.md`)

`specs/` should contain only id-named directories as direct children — no engineer-named subdirectories.

Ticket / issue references are optional. If the user has a Linear ticket or GitHub issue, use its id. If they don't, ask them for a feature name to use as the directory. Only create a new Linear ticket or GitHub issue when the user explicitly asks for one; in that case use the available Linear or GitHub tools respectively, and ask the user directly if team, labels, or repo are unclear.

## Before writing

Gather only the context you need: directory id (Linear ticket, GitHub issue, or feature name), feature summary, target users, key behaviors, edge cases, and validation expectations for later `TECH.md` use only. Ask the user for missing product intent rather than guessing.

### Figma mocks

If the feature has any UI or interaction design, ask the user whether a Figma mock exists before drafting the Behavior section, and include the link in the spec when one is provided. A mock is often the most reliable source of truth for visual states, spacing, and edge-case layouts — not asking can cause the Behavior section to guess at intent the designer already settled.

- If the user provides a link, include it under a short `## Figma` section (or inline near the top of Behavior) as `Figma: <link>`.
- If the user confirms no mock exists, note `Figma: none provided` so the absence is explicit rather than ambiguous.
- If the feature is purely backend (data model, API, CLI with no visual surface), skip the question and omit the section.

Do not silently drop design context; an explicit "none" is preferable to no mention at all on features where design would normally be expected.

## Structure

Required sections:

1. **Summary** — 1–3 sentences describing the feature and desired outcome.
2. **Behavior** — The meat of the spec. An exhaustive English description of how the feature works, written as numbered, testable invariants. See "The Behavior section" below — this is where the spec earns its length, and everything else should stay thin to avoid duplicating it.

Optional sections — include only when they add signal beyond the core. Omit the heading entirely if empty; do not write "None" as a placeholder.

- **Problem** — Include only when the motivation isn't obvious from Summary.
- **Goals / Non-goals** — Include when scope is ambiguous or has been contested.
- **Figma** — Include with a link when one exists, or an explicit `Figma: none provided` note when design matters but no mock exists. Omit entirely for non-visual features. See "Figma mocks" above.
- **Open questions** — Prefer inline `**Open question:** …` next to the relevant behavior. Include a dedicated section only if there are multiple unresolved questions worth collecting.

Do not include Validation, Success criteria, or Testing sections. Validation and test planning live in the companion `TECH.md` (produced by `write-tech-spec`). Write Behavior as numbered invariants that are testable on their own — the tech spec can reference them directly.

## The Behavior section

Behavior is the spec. Everything else is framing.

The goal of Behavior is a complete English description of how the feature works, detailed enough that a tech spec can be written directly from it without the author having to guess or re-derive product intent. If a reader finishes Behavior with questions about what the feature does in some situation, the section is not done.

Describe, at minimum:

- Default behavior and the happy-path user flow.
- Every user-visible state and the transitions between them.
- All inputs the user can provide and how the feature responds.
- Empty states, error states, loading / pending states, and cancellation.
- Edge cases a reasonable implementer would not think to ask about — permission denied, offline, timeouts, races between state changes, multiple concurrent instances, stale or missing data, focus loss mid-interaction, interactions with adjacent features.
- Keyboard, accessibility, and focus expectations where relevant.
- Invariants that must hold at all times and behaviors that must not regress.

Length Behavior to match the feature. Trivial features may need a handful of invariants; complex features may need many, with sub-sections per flow or state. The rest of the spec should stay thin so Behavior can be as exhaustive as the feature requires without producing a bloated document overall. Err toward enumerating one more edge case rather than one fewer.

## Length heuristic

Behavior should be as long as the feature requires — do not truncate edge cases to hit a line target. The heuristic below applies to everything around Behavior (Summary, optional sections): keep that framing thin so the spec's total length reflects the feature's actual complexity, not structural overhead.

- Trivial fix or narrow UI tweak: no spec.
- Small feature (single module, few edge cases): framing plus Behavior typically ~30–60 lines total.
- Medium feature (cross-module, multiple states): typically ~80–150 lines total.
- Large or behaviorally rich feature: longer is fine, and most of the length should live in Behavior.

If you find yourself writing the same idea in Summary, Problem, Goals, and Behavior, collapse the framing — not the Behavior content.

## Writing guidance

- Prefer concrete, observable behavior over aspirational wording.
- Write Behavior as a list of invariants rather than prose when possible.
- Capture invariants that must not regress and edge cases that are easy to miss.
- Avoid implementation details unless unavoidable for the UX.
- Each section should earn its place — if a section would repeat another or contain only boilerplate, omit it.

## Keep the spec current

Approved specs may ship in the same PR as the implementation. As implementation evolves, update `PRODUCT.md` in the same PR when user-facing behavior or UX details change. The checked-in spec should describe the feature that actually ships.

For large features, the implementer may optionally keep a `DECISIONS.md` file summarizing concrete decisions made during design and implementation. Offer it when it would help future agents; otherwise skip it.

## Related Skills

- `implement-specs`
- `write-tech-spec`
- `spec-driven-implementation`

## Example Behavior section

For a full sample Behavior section, read `references/behavior-example.md` when you need a concrete model for the expected shape.
