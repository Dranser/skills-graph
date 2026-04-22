# Dump Pipeline

A dump pipeline exists to make reverse engineering repeatable.

## Output Shape

Prefer stable outputs that can be compared across game versions:

- one file or record per type when the dump is large;
- preserved namespace, type, base type, interfaces, methods, fields, properties, events, attributes, and signatures;
- stable sorting by namespace, declaring type, metadata token, or source order;
- explicit notes for unresolved references and failed decompilation;
- optional indexes for patch candidates, lifecycle candidates, and reflection-only members.

## Rules

- Do not mix runtime mod behavior into the dump generator.
- Do not rename or reinterpret symbols during raw extraction.
- Keep normalized output deterministic so diffs show actual game changes.
- Keep enough metadata to reconnect formatted output to the original member.
- Treat decompiler output as evidence, not as a source-compatible replacement for the game.

## Runtime Link

The dump is useful when it feeds a later integration layer. The handoff should identify:

- exact target member signatures for Harmony or direct calls;
- private members that need reflection adapters;
- observed state objects that should become snapshots;
- fragile assumptions that need diagnostics in the mod runtime.
