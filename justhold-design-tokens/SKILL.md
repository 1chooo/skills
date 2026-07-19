---
name: justhold-design-tokens
description: >-
  JustHold design tokens — warm editorial palette, Fraunces/Geist fonts, and
  radius scale. Use when styling components, editing globals.css, or choosing
  colors for market data vs UI chrome.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
paths:
  - "app/globals.css"
  - "components/**/*.tsx"
  - "app/**/*.tsx"
  - "lib/fonts.ts"
---

# JustHold Design Tokens

Theme is shadcn `base-nova` + Tailwind v4 (`@theme inline` in `app/globals.css`). Fonts are registered in `lib/fonts.ts`. Always use semantic CSS variables/utility classes — never hardcode hex/oklch values in components.

## Palette (warm editorial)

- `background` / `foreground`: warm cream canvas (light) ↔ cool slate-blue (dark)
- `primary`: terracotta/clay accent — CTAs, active states, links, brand mark
- `card`, `secondary`, `muted`, `accent`, `border`: layered neutrals for surfaces
- `gain` / `loss`: market-only colors — use `text-[color:var(--color-gain)]` style for financial data (price changes, tickers), never generic success/error UI
- Dark mode is a deliberate palette swap via `.dark` and OS `prefers-color-scheme`, not inverted lightness

## Typography

- `font-heading` (Fraunces) — headings (`h1`–`h3`), card titles, brand wordmark
- `font-sans` (Geist) — body, nav, UI chrome (default)
- `font-mono` (Geist Mono) — numbers/prices/tickers, uppercase eyebrow labels (`tracking-widest`)

## Radius

Single `--radius` base (`0.5rem`) drives `sm`/`md`/`lg`/`xl`/`2xl`/`3xl`/`4xl` via `calc()`. Use `rounded-md` / `rounded-lg` / `rounded-xl` / `rounded-2xl` — don't invent custom radius values.
