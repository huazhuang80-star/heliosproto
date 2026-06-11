# 013 — Bootstrap spending-limits plugin crate

**Pillar:** `contracts/`
**Difficulty:** intermediate
**Estimated effort:** 6–8h
**Reward tier:** medium

## Goal

Create the `contracts/plugins/spending-limits` crate that enforces per-token spending caps over rolling time windows (e.g., $100 USDC per day).

## Context

Spending limits are one of the most user-visible plugins — users set "max $X per day in token Y" and the account itself enforces it on every transfer. The complexity here is the **window rollover logic** (when the period ends, the remaining cap refills). Depends on the trait from issue 011.

## Scope

**Touch:**
- `contracts/Cargo.toml` — add `plugins/spending-limits` to workspace members
- `contracts/plugins/spending-limits/Cargo.toml` (new)
- `contracts/plugins/spending-limits/src/lib.rs` (new)
- `contracts/plugins/spending-limits/src/test.rs` (new)

**Do not touch:**
- Other plugin crates
- `smart-account`

## Acceptance criteria

- [ ] Crate compiles in the workspace
- [ ] Exposes contract functions `set_limit(token, cap, window_seconds)`, `remove_limit(token)`, `get_remaining(token) -> i128`, and `check(call_context) -> PolicyResult`
- [ ] `check` extracts the token and amount from the call context (assume SEP-41 `transfer` call shape), then:
  - If no limit for that token: returns `Defer` (let other plugins decide)
  - If under the remaining cap in the current window: returns `Allow` and decrements remaining
  - If at or over: returns `Deny`
- [ ] Window rollover: when `current_ts >= window_start + window_seconds`, reset `window_start = current_ts` and `remaining = cap` before checking
- [ ] Only the parent account can mutate limits

## Tests required

- No limit set → `Defer`
- Under limit → `Allow`, remaining decreases
- At limit (`amount == remaining`) → `Allow`, remaining = 0
- Over limit → `Deny`, remaining unchanged
- After window expires, remaining refills to cap
- Multiple tokens have independent windows and limits

## References

- [ARCHITECTURE.md §3.2](../../ARCHITECTURE.md#32-plugins-plugins)
- [SEP-41 Token Interface](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0041.md) — `transfer(from, to, amount)` call shape
- Depends on: [011 — session-keys plugin trait](011-bootstrap-session-keys-plugin.md)
