# 010 — Implement `__check_auth` signature verification on smart-account

**Pillar:** `contracts/`
**Difficulty:** advanced
**Estimated effort:** 8–12h
**Reward tier:** large

## Goal

Implement the `__check_auth` entry point on `contracts/smart-account` so the contract can actually authorize calls per the Soroban custom account model.

## Context

The smart-account contract today stores signers, threshold, and nonce, but does not yet verify signatures or expose itself as a Soroban custom account. Without `__check_auth`, the contract cannot be used as a real wallet. This is the single highest-impact unblock for the whole project.

See [ARCHITECTURE.md §3.1](../../ARCHITECTURE.md#31-smart-account-smart-account) and the [Soroban custom account docs](https://developers.stellar.org/docs/build/guides/custom-account/) for the required signature.

## Scope

**Touch:**
- `contracts/smart-account/src/lib.rs`
- `contracts/smart-account/src/test.rs`

**Do not touch:**
- Any other contract crate
- Plugin invocation logic (covered in a later issue)

## Acceptance criteria

- [ ] `__check_auth` is implemented per Soroban's custom account contract trait
- [ ] Verifies each signature in the payload against the stored signer set using Ed25519 (Soroban classic) and/or secp256r1 (passkey) per signer kind
- [ ] Enforces the stored threshold: enough valid signatures must be present
- [ ] Increments the stored nonce on every successful authorization; mismatched nonce rejects (replay protection)
- [ ] All existing tests still pass
- [ ] Plugin dispatch is left as a `TODO` comment — out of scope for this issue

## Tests required

- Valid single-signer + correct nonce → passes
- Wrong signature → panics with a clear error
- Correct sig but stale nonce → panics
- Multisig 2-of-3 with only 1 valid signature → panics
- Multisig 2-of-3 with 2 valid signatures → passes and nonce increments by 1

## References

- [ARCHITECTURE.md §3.1](../../ARCHITECTURE.md#31-smart-account-smart-account)
- [Soroban custom account guide](https://developers.stellar.org/docs/build/guides/custom-account/)
- [`soroban-sdk` `auth` module](https://docs.rs/soroban-sdk/latest/soroban_sdk/auth/index.html)
