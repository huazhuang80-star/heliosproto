# 023 — Add network switcher component

**Pillar:** `web/`
**Difficulty:** good-first-issue
**Estimated effort:** 4h
**Reward tier:** small

## Goal

Add a dropdown that lets the user switch between Stellar testnet, futurenet, and mainnet. Persist the choice in localStorage. Expose a `useNetwork()` hook so other components read the current network.

## Context

Every Soroban RPC call and Horizon URL is network-scoped. We need this primitive before the send form (025), the React SDK (026), or anything else that talks to the chain. Default to testnet for now — mainnet is opt-in.

Depends on issue **020** (shadcn).

## Scope

**Touch:**
- `web/app/lib/network.ts` (new — type + constants + Zustand store)
- `web/app/components/network-switcher.tsx` (new)
- `web/app/app/layout.tsx` (mount switcher in header)
- `web/app/package.json` (add `zustand`)

**Do not touch:**
- Existing route pages — they read the network via the hook, but updating them is a follow-up issue

## Acceptance criteria

- [ ] `Network` type covers `"testnet" | "futurenet" | "mainnet"`
- [ ] `NETWORKS` constant exposes per-network `rpcUrl`, `horizonUrl`, `passphrase` for each
- [ ] `useNetwork()` returns `{ network, setNetwork, config }` and persists `network` to localStorage
- [ ] Switcher dropdown uses shadcn `Select` (add it via shadcn CLI in this PR)
- [ ] Default network is `testnet`
- [ ] Switching networks does not require a page reload

## Tests required

- Hook reads default `testnet` on first load
- After `setNetwork('mainnet')`, hook returns `mainnet`
- After page reload (simulated by reinstantiating the store), the persisted value is restored

## References

- [Stellar network passphrases](https://developers.stellar.org/docs/learn/fundamentals/networks)
- [Zustand persistence middleware](https://docs.pmnd.rs/zustand/integrations/persisting-store-data)
- Depends on: [020 — shadcn base components](020-shadcn-base-components.md)
