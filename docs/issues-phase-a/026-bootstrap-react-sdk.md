# 026 — Bootstrap `@heliosproto/react` with `useStellarRpc()`

**Pillar:** `web/`
**Difficulty:** intermediate
**Estimated effort:** 4–6h
**Reward tier:** medium

## Goal

Make `@heliosproto/react` a real publishable package: add `@stellar/stellar-sdk` as a peer dep, ship a `<StellarProvider>` + `useStellarRpc()` hook that wraps `SorobanRpc.Server`, and add Vitest tests.

## Context

The React SDK is the integration surface for third-party dApps. Right now it's an empty `export const VERSION` skeleton. Landing the first real hook lets the wallet app dogfood it (replacing any direct `@stellar/stellar-sdk` imports in `web/app`).

Depends on issue **023** (network config — the provider reads network from the same `useNetwork()` hook so SDK consumers can pass `network` or accept the app default).

## Scope

**Touch:**
- `web/sdk-react/package.json` — add `@stellar/stellar-sdk` as peer dep, `react` as peer dep
- `web/sdk-react/src/provider.tsx` (new)
- `web/sdk-react/src/hooks/use-stellar-rpc.ts` (new)
- `web/sdk-react/src/index.ts` — re-export public surface
- `web/sdk-react/tests/use-stellar-rpc.test.ts` (new)
- `web/sdk-react/vitest.config.ts` (new — happy-dom or jsdom env)

**Do not touch:**
- `web/app` — wiring up dogfooding is a follow-up issue

## Acceptance criteria

- [ ] `<StellarProvider network={...}>` mounts a context with a memoized `SorobanRpc.Server` instance for the network
- [ ] `useStellarRpc()` returns `{ server, network, config }`
- [ ] Provider re-instantiates server when `network` prop changes
- [ ] Public exports: `StellarProvider`, `useStellarRpc`, `Network` type, `NETWORKS` constants
- [ ] `pnpm --filter @heliosproto/react build` produces typed `dist/`
- [ ] `pnpm --filter @heliosproto/react test` passes with ≥2 tests

## Tests required

- Hook throws if used outside `StellarProvider`
- Hook returns a server instance for the configured network
- Re-rendering provider with a new network swaps the server

## References

- [`@stellar/stellar-sdk` `SorobanRpc.Server`](https://stellar.github.io/js-stellar-sdk/SorobanRpc.Server.html)
- Depends on: [023 — network switcher](023-network-switcher.md)
