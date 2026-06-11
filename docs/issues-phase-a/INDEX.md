# Phase A — Foundational issues

The first 18 issues we publish for Helios Protocol's Grantfox + Drips Wave campaign. Each depends only on the current scaffold and unblocks downstream feature work.

## Distribution

| Pillar | Count | Mix |
|---|---|---|
| `contracts/` | 5 | 1 good-first, 3 intermediate, 1 advanced |
| `web/` | 7 | 4 good-first, 2 intermediate, 1 advanced |
| `backend/` | 4 | 1 good-first, 3 intermediate |
| `docs/` | 2 | 2 good-first |
| **Total** | **18** | 8 good-first, 8 intermediate, 2 advanced |

## Difficulty → reward tier mapping

Grantfox and Drips Wave use different reward schemes. Use this table when publishing:

| Difficulty | Grantfox tier (USDC) | Drips Wave complexity | Drips points |
|---|---|---|---|
| good-first-issue | small ($50–$150) | Trivial | 100 |
| intermediate | medium ($200–$400) | Medium | 150 |
| advanced | large ($500–$1000) | High | 200 |

Maintainer sets the exact dollar amount in the Grantfox portal when listing.

## Issues — contracts/

| # | Title | Difficulty | Effort |
|---|---|---|---|
| 010 | [Implement `__check_auth` signature verification on smart-account](010-implement-check-auth.md) | advanced | 8–12h |
| 011 | [Bootstrap session-keys plugin crate and policy trait](011-bootstrap-session-keys-plugin.md) | intermediate | 4–6h |
| 012 | [Bootstrap allowlist plugin crate](012-bootstrap-allowlist-plugin.md) | good-first-issue | 4h |
| 013 | [Bootstrap spending-limits plugin crate](013-bootstrap-spending-limits-plugin.md) | intermediate | 6–8h |
| 014 | [Bootstrap factory crate with deterministic deployment](014-bootstrap-factory-crate.md) | intermediate | 6h |

## Issues — web/

| # | Title | Difficulty | Effort |
|---|---|---|---|
| 020 | [Install shadcn/ui and add base components](020-shadcn-base-components.md) | good-first-issue | 3–4h |
| 021 | [Create route skeleton for wallet pages](021-route-skeleton.md) | good-first-issue | 3h |
| 022 | [Add dark mode toggle with next-themes](022-dark-mode-toggle.md) | good-first-issue | 3h |
| 023 | [Add network switcher (testnet / futurenet / mainnet)](023-network-switcher.md) | good-first-issue | 4h |
| 024 | [Implement passkey enrollment ceremony in onboarding](024-passkey-enrollment.md) | advanced | 10–12h |
| 025 | [Build send-transaction form with simulation preview](025-send-transaction-form.md) | intermediate | 8h |
| 026 | [Bootstrap @heliosproto/react with useStellarRpc()](026-bootstrap-react-sdk.md) | intermediate | 4–6h |

## Issues — backend/

| # | Title | Difficulty | Effort |
|---|---|---|---|
| 030 | [Add Pydantic Settings with env loading and DI](030-pydantic-settings.md) | good-first-issue | 3–4h |
| 031 | [Bootstrap indexer service with HTTP polling skeleton](031-bootstrap-indexer.md) | intermediate | 6h |
| 032 | [Bootstrap prices service with Reflector adapter](032-bootstrap-prices.md) | intermediate | 6h |
| 033 | [Implement SEP-10 web auth challenge + verify endpoints](033-sep-10-auth.md) | intermediate | 8h |

## Issues — docs/

| # | Title | Difficulty | Effort |
|---|---|---|---|
| 040 | [ADR-001: Plugin architecture vs monolithic account](040-adr-plugin-architecture.md) | good-first-issue | 3h |
| 041 | [Developer setup guide consolidating per-pillar onboarding](041-developer-setup-guide.md) | good-first-issue | 4h |

## Publishing workflow

When the Grantfox or Drips Wave registration is approved:

1. Open this directory's files one at a time
2. For each, create a GitHub issue with title and body matching the markdown
3. Apply labels: `pillar:<contracts|web|backend|docs>`, difficulty label, and `Stellar Wave` if Drips applies
4. In Grantfox portal: set the reward tier per the mapping table above
5. Move file to `docs/issues-phase-a/published/` once the GitHub issue is live (so this directory stays as draft inbox)
