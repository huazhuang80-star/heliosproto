# 025 — Build send-transaction form with simulation preview

**Pillar:** `web/`
**Difficulty:** intermediate
**Estimated effort:** 8h
**Reward tier:** medium

## Goal

On the `/send` route, build the multi-step send-transaction flow: form → simulation preview → confirmation modal. Simulation results are mocked for this issue — real RPC integration lands later.

## Context

Send is the most-used wallet action. Getting the UX scaffolding right early lets contributors swap in the real simulation backend without changing the form structure.

Depends on issues **020** (shadcn), **021** (routes), **023** (network).

## Scope

**Touch:**
- `web/app/app/send/page.tsx`
- `web/app/components/send/` (new — `SendForm`, `SimulationPreview`, `ConfirmDialog`)
- `web/app/lib/validation/send.ts` (new — Zod schema)
- `web/app/package.json` (add `react-hook-form`, `@hookform/resolvers`, `zod`)

**Do not touch:**
- Backend services
- `sdk-react` — real RPC call is wired up in a later issue

## Acceptance criteria

- [ ] Form fields: recipient (Stellar address, G or C), asset (dropdown — XLM or hardcoded test token), amount (positive number, max 7 decimal places), memo (optional, max 28 chars)
- [ ] Zod schema validates all fields; invalid input shows inline errors
- [ ] On valid submit → `SimulationPreview` shows a placeholder structured object with mocked fee estimate, destination, asset, amount
- [ ] User clicks "Confirm" → `ConfirmDialog` opens asking for final confirmation
- [ ] User clicks confirm in the dialog → a `console.log("would submit", payload)` placeholder (real submission in a later issue)
- [ ] All three components are independently testable (props-driven)
- [ ] No `any` types

## Tests required

- Form validation rejects: empty recipient, malformed address, negative amount, amount > 7 decimal places, memo over 28 chars
- Form validation accepts: well-formed `G…` and `C…` addresses
- Confirmation dialog only opens after valid submit
- Vitest unit tests for the Zod schema directly

## References

- [Stellar address format](https://developers.stellar.org/docs/learn/fundamentals/data-format/stellar-data-types/strkey)
- [react-hook-form + zod resolver](https://react-hook-form.com/get-started#SchemaValidation)
- Depends on: [020](020-shadcn-base-components.md), [021](021-route-skeleton.md), [023](023-network-switcher.md)
