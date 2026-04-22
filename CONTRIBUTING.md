# Contributing

Skills Graph accepts contributions that improve the repository as a structured skill system, not as a loose prompt collection.

## What To Contribute

Good contributions include:
- new domains with clear ownership boundaries
- upgrades of older skills to the execution-layer format
- higher-quality references, decision tables, and review playbooks
- examples of how agents consume the graph
- validation improvements for schema, routing, and dependency quality

## Contribution Rules

- Keep the primary model intact: discovery, routing, execution planning, dependency expansion, then skill knowledge.
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
