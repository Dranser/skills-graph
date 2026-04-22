# Skill Document Format

This repository now distinguishes between two layers inside each practical skill JSON.

## Routing Layer

The routing layer decides:
- when a skill should own a request
- when a skill should be attached
- which signals, dependencies, and conflicts matter

Typical fields:
- `signals`
- `use_for`
- `do_not_use_for`
- `dependencies`
- `ownership`

## Execution Layer

The execution layer carries practical working knowledge.

Typical fields:
- `problem_space`
- `core_principles`
- `constraints`
- `decision_rules`
- `architecture_guidance`
- `recipes`
- `pitfalls`
- `review_checklist`
- `references`

## Reference Priority

References may now include:
- `priority`
- `interpretation`

Use `priority` to indicate how strongly a source should influence reasoning:
- `preferred`: use first for active guidance
- `supporting`: use to confirm, supplement, or narrow details
- `baseline-only`: use to understand capability or platform behavior, but not as the default architectural recommendation

Use `interpretation` to say how the source should be read. This matters when a source describes available platform features that the domain intentionally does not recommend as the default path.

## Policy

- New domains should use the full schema from [skill-schema.json](../meta/skill-schema.json).
- New or heavily reworked skills should follow [skill-template.json](../meta/skill-template.json).
- Entry and manifest files remain metadata only.
- Local references are preferred for practical guidance; official docs remain valid supporting sources.
- When needed, references should mark whether they are `preferred`, `supporting`, or `baseline-only`.
