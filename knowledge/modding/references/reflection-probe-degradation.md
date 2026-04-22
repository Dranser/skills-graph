# Reflection Probe Degradation

Reflection probes are adapters for unstable game internals. They should make fragile access visible, narrow, and recoverable.

## Adapter Rules

- Keep one adapter per concern or game surface.
- Prefer read-only access unless mutation is explicitly proven safe.
- Try known member names in priority order.
- Convert raw game objects into snapshots before feature code consumes them.
- Log missing members once per adapter and expose unavailable state through diagnostics.
- Do not throw from optional probes during normal runtime flow.

## Confidence

Use a simple confidence model:

- `stable`: public or directly referenced API with expected type and lifecycle;
- `patched`: available through an active hook;
- `reflected`: private or version-sensitive access succeeded;
- `unavailable`: target is missing, invalid, or not currently in the right game state.

## Review

A reflection probe is acceptable when the alternative is worse coupling. It is not acceptable when feature code can call the reflected member directly or when missing state causes unrelated systems to fail.
