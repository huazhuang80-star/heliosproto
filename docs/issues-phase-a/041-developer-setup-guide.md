# 041 — Developer setup guide consolidating per-pillar onboarding

**Pillar:** `docs/`
**Difficulty:** good-first-issue
**Estimated effort:** 4h
**Reward tier:** small

## Goal

Write `docs/guides/developer-setup.md` — a single doc that takes a new contributor from zero to "all three pillars' tests pass locally," combining the existing per-pillar READMEs with platform-specific gotchas.

## Context

Per-pillar READMEs cover their own pillar. A new contributor wants the consolidated journey. Today they have to read three READMEs and stitch them. This guide is what gets linked from CONTRIBUTING.md as "first day on Helios."

## Scope

**Touch:**
- `docs/guides/developer-setup.md` (new)
- `CONTRIBUTING.md` (add a "First day" pointer to the new guide)

**Do not touch:**
- Per-pillar READMEs

## Acceptance criteria

- [ ] Setup steps are numbered, work on Linux + macOS, with platform notes where they diverge
- [ ] Covers: clone, Rust toolchain (`rustup` + `wasm32-unknown-unknown`), Stellar CLI, Node 20+ + pnpm 10 (via `corepack`), Python 3.10+ (with `uv` recommended), Docker (for backend compose)
- [ ] Each pillar has a "verify it works" step:
  - contracts: `cd contracts && cargo test`
  - web: `cd web && pnpm install && pnpm -r build`
  - backend: `cd backend/api && uv venv .venv && uv pip install -e ".[dev]" && .venv/bin/pytest`
- [ ] Includes a **Common gotchas** section: PEP 668 on Ubuntu (`python3-venv` missing), pnpm `onlyBuiltDependencies`, first cargo build is slow because of soroban-sdk
- [ ] Includes a one-liner "run all three pillars' tests" command for verifying setup
- [ ] CONTRIBUTING.md links to this guide from its "Dev environment" section

## Tests required

None — documentation. Verification is: a contributor reading the guide and reaching "all tests pass" without asking for help.

## References

- Existing pillar READMEs: [`contracts/`](../../contracts/README.md), [`web/`](../../web/README.md), [`backend/`](../../backend/README.md)
- [CONTRIBUTING.md](../../CONTRIBUTING.md)
