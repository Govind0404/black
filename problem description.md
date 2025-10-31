# Black plugin pipeline (project‑scoped, opt‑in)

Projects sometimes need safe, local rewrites during formatting: migrating APIs, enforcing naming, or inserting headers. Add an opt‑in plugin pipeline around Black’s core formatter that runs only when explicitly enabled and never changes Black’s style rules.

Requirements
- Opt‑in gate: plugins run only with an explicit flag.
- Deterministic order: execute plugins exactly as configured.
- Hooks: support pre_format and post_format transforms with signature (source: str, mode: black.Mode) -> str.
- Discovery and config: discover by entry point group black.plugins; allow configuration via [tool.black].plugins and equivalent CLI.
- Guarantees: transforms must be pure, deterministic, and effectively idempotent; validate where practical.
- Isolation and failure policy: quarantine plugin errors (report, skip) so core formatting still succeeds; do not crash or change exit status if the formatter itself succeeds.
- Dry‑run and telemetry: provide a dry‑run mode that prints per‑plugin change summaries; optionally print basic per‑plugin timing.
- No style changes: core formatting output must be identical when plugins are disabled; with plugins enabled, preserve Black’s invariants.
- Cross‑platform behavior: consistent across supported Python versions and OSes.

CLI and configuration
- Flag to enable plugins, plus options for dry‑run and telemetry.
- Ordered plugin list via [tool.black].plugins or CLI to define execution order.

Error handling and reporting
- Surface plugin errors and non‑idempotence warnings in stderr/summary.
- Summaries should indicate each plugin’s name and number of changes.

Scope
- Keep overhead modest. The feature is additive and fully disabled by default.
