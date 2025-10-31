## Problem Brief

Introduce an opt‑in, project‑scoped plugin pipeline around Black’s core formatter to enable safe, local source rewrites (for example, API migrations, naming normalization, or standard headers). Plugins must never alter Black’s style rules and run only when explicitly enabled. Execution order is deterministic and exactly matches configuration. Transforms are expected to be pure, deterministic, and effectively idempotent; validate idempotence where practical. Quarantine plugin failures: report them and skip the offending step so the formatter can still succeed. Provide a dry‑run mode that prints concise per‑plugin change summaries, and an optional telemetry mode that exposes basic timing. When plugins are disabled, output is identical to baseline Black. The feature is additive, keeps overhead modest, and behaves consistently across supported Python versions and platforms.

## Agent Instructions

- Add a plugin pipeline that executes only when explicitly enabled.
- Execute plugins in a stable, configured order.
- Support two hooks: pre_format and post_format with signature (source: str, mode: black.Mode) -> str.
- Discover plugins via the entry point group black.plugins; allow configuration via [tool.black].plugins and equivalent CLI flags.
- Enforce purity, determinism, and effective idempotence; warn on non‑idempotence.
- Quarantine plugin errors (report + skip) without failing the overall run when core formatting succeeds.
- Provide dry‑run change summaries per plugin and optional timing telemetry.
- Preserve Black’s formatting invariants and change‑detection/diff semantics.

## Test Assumptions (optional)

- Public surfaces introduced: entry point group black.plugins; configuration key [tool.black].plugins (ordered list); CLI gates to enable plugins, dry‑run, and telemetry.
- Hooks: pre_format and post_format accept (source: str, mode: black.Mode) and return a new source string.
