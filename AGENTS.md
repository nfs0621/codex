# Repository Guidelines

## Project Structure & Module Organization
- Root Rust workspace: `codex-rs/` with crates prefixed `codex-` (e.g., `codex-core`, `codex-tui`, `codex-common`, `codex-protocol`).
- Source: `codex-rs/<crate>/src` • Integration tests: `codex-rs/<crate>/tests`.
- Snapshots (insta): under each crate’s `tests/snapshots/` (notably in `codex-rs/tui`).
- TUI styles: `codex-rs/tui/styles.md` documents conventions used by `ratatui`.

## Build, Test, and Development Commands
- Setup shell: `cd codex-rs`
- Format code: `just fmt` — runs rustfmt across the workspace.
- Lint/fix (scoped): `just fix -p <project>` — runs Clippy for a crate (faster than workspace-wide).
- Test a crate: `cargo test -p codex-tui` — run only that crate’s tests.
- Full suite (when core/common/protocol change): `cargo test --all-features`.
- Snapshots: `cargo insta pending-snapshots -p codex-tui` → review; accept with `cargo insta accept -p codex-tui`.

## Coding Style & Naming Conventions
- Rust style: 4-space indent, no tabs; keep functions small and cohesive.
- Formatting: always run `just fmt` before pushing.
- Linting: fix Clippy warnings via `just fix -p <project>`; keep warnings at zero.
- Crate naming: prefix with `codex-`; modules `snake_case`, types `UpperCamelCase`, functions/vars `snake_case`.
- Strings/formatting: prefer `format!("Hello {name}")` with inlined variables.
- TUI: use `ratatui::Stylize` helpers like `"text".red().bold()` over manual `Style`/`Span` construction.

## Testing Guidelines
- Frameworks: `cargo test` (unit/integration) and `insta` snapshots (TUI output).
- Update snapshots only for intentional UI changes; review `*.snap.new` before accept.
- Some tests skip in sandboxed environments; do not alter logic tied to `CODEX_SANDBOX` or `CODEX_SANDBOX_NETWORK_DISABLED`.

## Commit & Pull Request Guidelines
- Commits: imperative mood, concise subject, optional scope. Example: `tui: refine patch summary colors`.
- Reference issues: `Fixes #123` or `Refs #123` in body.
- PRs: clear description, rationale, linked issues, validation steps, and screenshots/GIFs for TUI changes. Note if snapshots were updated and why.

## Security & Configuration Tips
- Do not add or modify code related to `CODEX_SANDBOX` or `CODEX_SANDBOX_NETWORK_DISABLED` handling.
- Avoid adding network-dependent tests; if unavoidable, gate them behind explicit feature flags or env checks.
