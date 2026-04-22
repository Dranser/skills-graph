# Pull Request

## Summary

<!-- What changed and why? Keep this concrete. -->

## Scope

Changed domains:

- [ ] assembly-analysis
- [ ] code-maintenance
- [ ] data-oriented-pipeline
- [ ] framework
- [ ] gpu
- [ ] modding
- [ ] neural
- [ ] performance
- [ ] unity-ui
- [ ] meta / repository policy

Change type:

- [ ] New domain
- [ ] New skill
- [ ] Existing skill upgrade
- [ ] Reference update
- [ ] Routing / dependency metadata
- [ ] Validation / tooling
- [ ] Documentation

## Skills Graph Checklist

- [ ] Skill JSON follows `meta/skill-schema.json`.
- [ ] New or reworked skills include routing and execution layers.
- [ ] `knowledge/<stack>/<domain>/<domain>_entry.json` is updated when entry points or routing signals changed.
- [ ] `knowledge/<stack>/<domain>/<domain>_manifest.json` is updated when skills were added, removed, or renamed.
- [ ] `meta/index.json` is updated for new domains or skill file lists.
- [ ] `meta/router.json` is updated for routing candidates or skill paths.
- [ ] `meta/depgraph.json` is updated for new or changed dependencies.
- [ ] Local references point to existing files.
- [ ] Cross-domain dependencies are intentional and do not override the primary domain owner.
- [ ] Manifest and entry files are treated as metadata only, not executable skills.

## Validation

Commands run:

```text

```

Validation notes:

<!-- Mention JSON validation, dependency validation, reference path checks, or why validation was not run. -->

## Risk

- [ ] Low risk: metadata/reference-only or narrowly scoped skill change.
- [ ] Medium risk: routing or dependency behavior changed.
- [ ] High risk: schema, router, execution policy, or broad domain ownership changed.

Known risks or review focus:

<!-- Call out ambiguous ownership, routing conflicts, missing references, or areas needing careful review. -->
