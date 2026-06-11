# 024 — Implement passkey enrollment ceremony in onboarding

**Pillar:** `web/`
**Difficulty:** advanced
**Estimated effort:** 10–12h
**Reward tier:** large

## Goal

On the `/onboard` route, run a WebAuthn registration ceremony that creates a new passkey, stores its credential metadata in IndexedDB, and derives the deterministic smart-account address that the factory contract would deploy.

## Context

This is the centerpiece of Helios's "no seed phrase" pitch. The passkey *is* the wallet identity. The factory contract (issue 014) derives the account address from the passkey credential ID, so the address can be shown to the user before the first transaction.

Depends on issues **020** (shadcn) and **023** (network config).

## Scope

**Touch:**
- `web/app/app/onboard/page.tsx`
- `web/app/lib/passkey.ts` (new — WebAuthn helpers)
- `web/app/lib/credential-store.ts` (new — IndexedDB wrapper)
- `web/app/package.json` (add `idb` for IndexedDB ergonomics)

**Do not touch:**
- Other route pages
- The factory contract — this issue only uses its *predicted* address logic; the contract itself is delivered in issue 014

## Acceptance criteria

- [ ] On `/onboard`, user clicks "Create wallet" → browser prompts for passkey
- [ ] Credential is created with `pubKeyCredParams: [{ alg: -7 }]` (ES256 / secp256r1, what Soroban supports)
- [ ] Credential ID + public key stored in IndexedDB under a stable key
- [ ] After ceremony, page displays:
  - Truncated public key
  - Predicted account address (compute locally using the same address-derivation logic the factory contract uses)
- [ ] Reload preserves the saved credential — re-visiting `/onboard` with an existing credential shows the existing account instead of starting a new ceremony
- [ ] Handle ceremony cancellation gracefully (no crash, friendly message)
- [ ] No `any` types

## Tests required

- Playwright e2e test using a virtual authenticator covering:
  - Fresh visit → ceremony → credential stored → predicted address shown
  - Reload → existing credential reused
  - User cancels the ceremony → friendly error displayed

## References

- [WebAuthn registration ceremony](https://www.w3.org/TR/webauthn-3/#sctn-registering-a-new-credential)
- [Playwright virtual authenticator](https://playwright.dev/docs/api/class-cdpsession) (`WebAuthn.addVirtualAuthenticator`)
- Depends on: [020 — shadcn](020-shadcn-base-components.md), [023 — network switcher](023-network-switcher.md)
- Future dependency: factory contract address derivation must match issue [014](014-bootstrap-factory-crate.md)
