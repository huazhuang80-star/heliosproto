# 012 — Bootstrap allowlist plugin crate

**Pillar:** `contracts/`
**Difficulty:** good-first-issue
**Estimated effort:** 4h
**Reward tier:** small

## Goal

Create the `contracts/plugins/allowlist` crate that restricts outbound contract calls to a whitelist of contract addresses.

## Context

The allowlist plugin is the simplest of the policy plugins — it stores a set of allowed destination contract addresses and denies anything not in the set. Great first issue for a contributor learning Soroban storage patterns.

This issue depends on issue **011** landing first (defines the `Plugin` trait / `PolicyResult` enum).

## Scope

**Touch:**
- `contracts/Cargo.toml` — add `plugins/allowlist` to workspace members
- `contracts/plugins/allowlist/Cargo.toml` (new)
- `contracts/plugins/allowlist/src/lib.rs` (new)
- `contracts/plugins/allowlist/src/test.rs` (new)

**Do not touch:**
- Other plugin crates
- `smart-account`

## Acceptance criteria

- [ ] New crate compiles as part of the workspace
- [ ] Exposes contract functions `add(addr)`, `remove(addr)`, `list() -> Vec<Address>`, and `check(call_context) -> PolicyResult` implementing the trait from issue 011
- [ ] `check` returns `Allow` if the call's destination is in the stored set, `Deny` otherwise
- [ ] Only the account contract itself can mutate the allowlist (require auth from the parent account)
- [ ] `cargo fmt --check`, `cargo clippy -- -D warnings`, `cargo test` clean

## Tests required

- Empty allowlist denies all destinations
- `add(addr)` then `check(call_to_addr)` returns `Allow`
- `add(addr1); add(addr2); remove(addr1)` leaves only `addr2`
- Unauthorized mutation panics

## References

- [ARCHITECTURE.md §3.2](../../ARCHITECTURE.md#32-plugins-plugins)
- Depends on: [011 — session-keys plugin trait](011-bootstrap-session-keys-plugin.md)
