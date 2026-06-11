# 021 — Create route skeleton for wallet pages

**Pillar:** `web/`
**Difficulty:** good-first-issue
**Estimated effort:** 3h
**Reward tier:** small

## Goal

Add the route directories that every other web issue will fill in: `/onboard`, `/dashboard`, `/send`, `/receive`, `/history`, `/settings`. Each renders a placeholder page using the shadcn `Card` component.

## Context

Most subsequent web work targets one of these routes (send form lives in `/send`, passkey onboarding in `/onboard`, etc.). Landing the routes upfront prevents merge conflicts between parallel contributors.

Depends on issue **020** (shadcn `Card` must exist).

## Scope

**Touch:**
- `web/app/app/onboard/page.tsx` (new)
- `web/app/app/dashboard/page.tsx` (new)
- `web/app/app/send/page.tsx` (new)
- `web/app/app/receive/page.tsx` (new)
- `web/app/app/history/page.tsx` (new)
- `web/app/app/settings/page.tsx` (new)

**Do not touch:**
- Existing `app/page.tsx`, `app/layout.tsx`, or `app/globals.css`

## Acceptance criteria

- [ ] Six new route files exist
- [ ] Each renders a `Card` with the route name as title and a `"Coming soon"` body
- [ ] Each is typed (no `any`)
- [ ] Each is the default export of its file
- [ ] `pnpm -r build` succeeds — routes show up in the build summary as static `○` pages
- [ ] `pnpm lint` clean

## Tests required

None required for this issue.

## References

- Depends on: [020 — shadcn base components](020-shadcn-base-components.md)
- [Next.js App Router conventions](https://nextjs.org/docs/app/building-your-application/routing/defining-routes)
