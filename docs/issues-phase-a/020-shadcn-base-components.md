# 020 — Install shadcn/ui and add base components

**Pillar:** `web/`
**Difficulty:** good-first-issue
**Estimated effort:** 3–4h
**Reward tier:** small

## Goal

Install shadcn/ui in `web/app`, configure it for Tailwind 4 + the App Router, and add four base components: `Button`, `Card`, `Input`, `Dialog`.

## Context

Almost every subsequent web issue needs these primitives. Landing them here unblocks the route skeleton (021), the network switcher (023), the send form (025), and onboarding (024).

## Scope

**Touch:**
- `web/app/components.json` (new — shadcn config)
- `web/app/components/ui/` (new — generated shadcn components)
- `web/app/lib/utils.ts` (new — `cn()` helper)
- `web/app/package.json` (add `class-variance-authority`, `clsx`, `tailwind-merge`, `@radix-ui/react-slot`, `@radix-ui/react-dialog`)
- `web/app/app/page.tsx` (use the new components on the landing page to verify they render)

**Do not touch:**
- `web/sdk-react` or `web/extension`
- The Tailwind config (works as-is for shadcn + Tailwind 4)

## Acceptance criteria

- [ ] `pnpm dlx shadcn@latest init` completed with the project's existing Tailwind 4 + App Router setup
- [ ] `Button`, `Card`, `Input`, `Dialog` added via `pnpm dlx shadcn@latest add ...`
- [ ] `app/page.tsx` uses at least `Button` and `Card` so the install is exercised
- [ ] `pnpm -r build` succeeds
- [ ] `pnpm lint` clean

## Tests required

No unit tests required for this issue. CI's build + lint suffices.

## References

- [shadcn/ui — Next.js install](https://ui.shadcn.com/docs/installation/next)
- [shadcn/ui — Tailwind v4 notes](https://ui.shadcn.com/docs/tailwind-v4)
