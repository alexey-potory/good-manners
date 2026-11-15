# Rust Rules

Apply this module to Rust code and tooling. The [shared development rules](../SKILL.md) remain in effect.

## Skills

- Apply `rust-skills:rust-router` before Rust work, then load the specialized skills it requires for the task.
- For unsafe code or FFI, read and apply `rust-skills:unsafe-checker`.

## Structure and implementation

- The Rust module structure does not need to mirror a frontend or another service.

## Formatting and static analysis

- Ensure `rustfmt` and Clippy are available for the project's configured toolchain. With rustup, install missing components using `rustup component add rustfmt clippy`; declare them in an existing `rust-toolchain.toml` so local development and CI use the same setup.
- Use `cargo fmt` as the formatting authority. Keep project-specific options in `rustfmt.toml` or `.rustfmt.toml`, using settings supported by the configured toolchain.
- Configure Rust and Clippy lint levels in the existing Cargo manifest or source attributes. For shared workspace lint configuration, ensure members inherit it. Use `clippy.toml` or `.clippy.toml` for lint-specific parameters when needed.

| Purpose                                             | Baseline command                            |
| --------------------------------------------------- | ------------------------------------------- |
| Check formatting without changing files             | `cargo fmt --all -- --check`                |
| Apply formatting                                    | `cargo fmt --all`                           |
| Run static analysis with warnings treated as errors | `cargo clippy --all-targets -- -D warnings` |

- Adapt the analysis command to the project's workspace, features, and supported targets. Cover relevant workspace members and feature combinations explicitly; do not assume `--all-targets` covers every platform or enable incompatible features together. An existing toolchain-supported warning-as-error configuration may replace the command-line flag.

References: [rustfmt](https://github.com/rust-lang/rustfmt), [Clippy usage](https://doc.rust-lang.org/clippy/usage.html).
