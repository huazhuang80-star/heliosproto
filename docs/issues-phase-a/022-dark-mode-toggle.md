# 022 — Add dark mode toggle with next-themes

**Pillar:** `web/`
**Difficulty:** good-first-issue
**Estimated effort:** 3h
**Reward tier:** small

## Goal

Add a dark mode toggle to the wallet using `next-themes`, with no flash-of-incorrect-theme on first paint.

## Context

Every wallet ships dark mode. Doing it now means every subsequent UI issue automatically gets light/dark variants for free.

Depends on issue **020** (shadcn primitives).

## Scope

**Touch:**
- `web/app/app/layout.tsx` (wrap children with `ThemeProvider`)
- `web/app/components/theme-provider.tsx` (new)
- `web/app/components/theme-toggle.tsx` (new)
- `web/app/app/globals.css` (add `dark:` variant tokens)
- `web/app/package.json` (add `next-themes`)

**Do not touch:**
- Route pages — the toggle should live in a shared header or in a fixed corner

## Acceptance criteria

- [ ] Toggle component renders with three states: light / dark / system
- [ ] Selection persists across page reloads (localStorage handled by `next-themes`)
- [ ] No flash of incorrect theme on first paint (use the standard `suppressHydrationWarning` + `attribute="class"` pattern)
- [ ] CSS uses `:root` and `.dark` selectors with semantic tokens (`--background`, `--foreground`, `--card`, `--card-foreground`)
- [ ] Toggle is visible somewhere on every page (suggest fixed top-right)

## Tests required

None required for this issue. Visual verification via `pnpm dev`.

## References

- [next-themes README](https://github.com/pacocoursey/next-themes#readme)
- [shadcn dark mode guide](https://ui.shadcn.com/docs/dark-mode/next)
- Depends on: [020 — shadcn base components](020-shadcn-base-components.md)
