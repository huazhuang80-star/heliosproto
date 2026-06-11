# 040 — ADR-001: Plugin architecture vs monolithic account

**Pillar:** `docs/`
**Difficulty:** good-first-issue
**Estimated effort:** 3h
**Reward tier:** small

## Goal

Write `docs/decisions/001-plugin-architecture.md` — the first Architecture Decision Record explaining why Helios uses composable plugin contracts instead of a monolithic smart account contract.

## Context

ADRs make architectural intent durable. ADR-001 captures the most load-bearing decision in the whole project — every plugin issue (011, 012, 013) and the smart-account contract (010) depend on this choice. New contributors should be able to read this doc and understand "why is this not just one big contract?"

## Scope

**Touch:**
- `docs/decisions/001-plugin-architecture.md` (new)
- `docs/decisions/README.md` (new — brief intro to the ADR practice, links to template)

**Do not touch:**
- Any other directory

## Acceptance criteria

- [ ] ADR follows the standard structure: **Status** (Accepted), **Context**, **Decision**, **Consequences**, **Alternatives considered**
- [ ] **Context** section explains the problem: smart-account contracts need to evolve (new policies: session keys, recovery, allowlists), but redeploying the whole account loses state and forces user migration
- [ ] **Decision** section states clearly: account stores a set of installed plugin addresses; `__check_auth` aggregates plugin verdicts
- [ ] **Consequences** lists both positive (modularity, no fork-to-add-policy, audit surface stays small per plugin) and negative (more contract calls per auth, harder to reason about composed policies)
- [ ] **Alternatives** discusses at least: (a) monolithic account with feature flags, (b) ERC-7579-style modular accounts ported to Soroban, (c) per-user account factory that bakes policies in at deploy time
- [ ] References [ERC-7579](https://eips.ethereum.org/EIPS/eip-7579) and any Stellar-specific prior art
- [ ] No `TODO`s in the doc

## Tests required

None — documentation.

## References

- [adr.github.io](https://adr.github.io/) — the canonical ADR practice
- [ARCHITECTURE.md §3.2](../../ARCHITECTURE.md#32-plugins-plugins) — already states the design at a high level; this ADR explains *why*
