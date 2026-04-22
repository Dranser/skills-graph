# Contributing

Skills Graph accepts contributions that improve the repository as a structured skill system, not as a loose prompt collection.

Current repository bias:
- the existing mature guidance is primarily oriented toward `.NET`, `C#`, and Unity-related engineering
- contributions outside that stack are allowed, but they should be introduced deliberately with clear ownership and routing boundaries
- do not treat projects centered on third-party frameworks or heavy library ecosystems as the default target shape for this repository
- do not mix materially different stacks inside one domain surface when stack-specific guidance would diverge in practice

## What To Contribute

Good contributions include:
- new domains with clear ownership boundaries
- upgrades of older skills to the execution-layer format
- higher-quality references, decision tables, and review playbooks
- examples of how agents consume the graph
- validation improvements for schema, routing, and dependency quality

## Contribution Rules

- Keep the primary model intact: discovery, routing, execution planning, dependency expansion, then skill knowledge.
- If multiple technology stacks are supported, model stack explicitly in repository structure and routing rather than blending stack-specific implementation guidance into one skill.
- Do not treat manifest or entry files as executable skills.
- Prefer explicit ownership, explicit lifecycle, and explicit boundaries.
- Do not introduce third-party packages or frameworks as the default implementation path in skill guidance.
- Keep references prioritized: `preferred`, `supporting`, or `baseline-only` where appropriate.
- By contributing, you agree that your contributions are submitted under the repository license.
- For new or heavily reworked skills, follow:
  - [meta/skill-schema.json](./meta/skill-schema.json)
  - [meta/skill-template.json](./meta/skill-template.json)
  - [knowledge/README-SKILL-FORMAT.md](./knowledge/README-SKILL-FORMAT.md)

## Style Expectations

- Use concise, inspectable JSON.
- Keep routing logic separate from execution guidance.
- Add local references when practical guidance is needed.
- Prefer ASCII unless a file already requires another character set.
- Avoid vague marketing language inside skill files.

## Pull Request Guidance

When opening a PR, make it easy to review:

- explain which domain owns the new work
- explain whether the change affects routing, execution, dependencies, or references
- call out any new cross-domain links
- mention validation you ran

## Scope Discipline

Do not bundle unrelated changes into one PR.

Good PR shapes:
- one new domain
- one upgraded mature domain
- one schema or policy improvement
- one reference-quality pass

## Validation

Before submitting:

- ensure changed JSON files are valid
- ensure new dependencies resolve to known skills
- ensure new domains are represented consistently in root routing metadata
- ensure references point to real local paths or valid external sources
