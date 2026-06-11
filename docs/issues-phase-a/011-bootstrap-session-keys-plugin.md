# 011 — Bootstrap session-keys plugin crate and policy trait

**Pillar:** `contracts/`
**Difficulty:** intermediate
**Estimated effort:** 4–6h
**Reward tier:** medium

## Goal

Create the `contracts/plugins/session-keys` crate, define the shared `Plugin` policy trait that all plugins will implement, and stub the session-key check function so it compiles and runs trivially.

## Context

Plugins are how Helios accounts get composable policies (session keys, recovery, spending limits, etc.) without forking the core account contract. This issue lands the foundational plugin pattern. All later plugin issues depend on the trait shape decided here.

See [ARCHITECTURE.md §3.2](../../ARCHITECTURE.md#32-plugins-plugins).

## Scope

**Touch:**
- `contracts/Cargo.toml` — add `plugins/session-keys` to workspace members
- `contracts/plugins/session-keys/Cargo.toml` (new)
- `contracts/plugins/session-keys/src/lib.rs` (new)
- `contracts/plugins/session-keys/src/test.rs` (new)

**Do not touch:**
- Other plugin crates
- `smart-account` — wiring of plugins into `__check_auth` is a later issue

## Acceptance criteria

- [ ] New crate compiles as part of the workspace
- [ ] Defines a `Plugin` trait (or contract method) that all plugins will implement, with a `check(env, account, call_context) -> PolicyResult` signature
- [ ] Defines a `PolicyResult` enum with variants `Allow`, `Deny`, `Defer`
- [ ] Session-keys crate implements the trait with a stub that returns `Allow` unconditionally
- [ ] Storage scaffolding exists for `SessionKey { signer, scope, expiry, spending_remaining }` but is not yet enforced
- [ ] `cargo fmt --check`, `cargo clippy -- -D warnings`, `cargo test` all clean

## Tests required

- Stub `check` returns `Allow`
- Storing and retrieving a `SessionKey` via storage round-trips correctly

## References

- [ARCHITECTURE.md §3.2](../../ARCHITECTURE.md#32-plugins-plugins)
- [ERC-7579 modular accounts](https://eips.ethereum.org/EIPS/eip-7579) — reference for trait shape (we are adapting concepts, not implementing)
