# 014 — Bootstrap factory crate with deterministic deployment

**Pillar:** `contracts/`
**Difficulty:** intermediate
**Estimated effort:** 6h
**Reward tier:** medium

## Goal

Create the `contracts/factory` crate that deploys smart-account contracts at a deterministic address derived from a passkey credential ID.

## Context

Deterministic deployment lets the wallet display the user's account address *before* the first transaction is signed (CREATE2-style on EVM). Critical UX for onboarding — you can show "your account address is X, fund it to get started" without the user having signed anything yet.

See [ARCHITECTURE.md §3.4](../../ARCHITECTURE.md#34-factory-factory).

## Scope

**Touch:**
- `contracts/Cargo.toml` — add `factory` to workspace members
- `contracts/factory/Cargo.toml` (new)
- `contracts/factory/src/lib.rs` (new)
- `contracts/factory/src/test.rs` (new)

**Do not touch:**
- `smart-account` — factory references the smart-account wasm hash but doesn't modify the crate

## Acceptance criteria

- [ ] Crate compiles in the workspace
- [ ] Exposes `deploy_account(credential_id: BytesN<32>, init_args: InitArgs) -> Address`
- [ ] Uses `env.deployer().with_address(...).deploy(...)` (or equivalent) keyed off `credential_id` so the same input always yields the same address
- [ ] The deployed contract is initialized with the supplied `init_args` atomically with deployment
- [ ] Stores the deployment WASM hash on first call (or in instance storage at factory init time)
- [ ] Provides a read-only `predict_address(credential_id) -> Address` that returns the address *without* deploying

## Tests required

- `predict_address(cred)` returns same value as `deploy_account(cred, ...)`
- Calling `deploy_account(cred, ...)` twice with the same cred fails or returns the existing address
- Different credential IDs produce different addresses
- Deployed account is initialized with the right signers and threshold

## References

- [ARCHITECTURE.md §3.4](../../ARCHITECTURE.md#34-factory-factory)
- [Soroban deployer docs](https://docs.rs/soroban-sdk/latest/soroban_sdk/deploy/index.html)
