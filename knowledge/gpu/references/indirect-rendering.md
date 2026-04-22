# Indirect Rendering

Indirect rendering moves draw instance counts from CPU loops into GPU-produced argument buffers.

## Flow

1. Compute stages fill or update per-instance data.
2. Visibility or LOD stages append visible indices.
3. `CopyCount` copies append count into a raw count buffer.
4. A small kernel writes draw arguments.
5. The renderer binds instance buffers and submits `DrawMeshInstancedIndirect`.

## Rules

- Keep draw args layout explicit.
- Rebuild draw args when visible count, mesh index data, or instance source changes.
- Use visible-index indirection when culling or LOD compacts instances.
- Set conservative draw bounds; too small clips valid instances, too large harms culling.
- Batch by compatible mesh, material, shader keywords, and buffer layout.

## Pitfall

Do not read visible counts back to the CPU each frame just to issue a draw. That defeats the point of indirect rendering.
