---
name: justhold-ui-layout
description: >-
  JustHold UI primitives and page shells — Base UI buttons, cards, public /
  member / admin layouts, containers, and article chrome. Use when building
  components or pages in the JustHold app.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
paths:
  - "components/**/*.tsx"
  - "app/**/*.tsx"
---

# JustHold UI + Layout

Also follow `justhold-design-tokens` for colors/fonts and Shared `web-service-stack` for RSC defaults.

## UI components

- Base UI primitives (`@base-ui/react`) + `class-variance-authority` for variants, following shadcn's `base-nova` style. Canonical pattern: `components/ui/button.tsx` (`cva` variants, `cn()` merge, `data-slot`).
- One-off styled links (nav CTAs, hero buttons) are hand-rolled `<Link>`s with utility classes — match existing patterns (`rounded-md bg-primary px-5 py-2.5 text-sm font-medium text-primary-foreground transition-opacity hover:opacity-90`) instead of forcing `Button`.
- Category/status pills: `inline-flex items-center rounded-full bg-secondary px-3 py-1 text-xs font-medium text-secondary-foreground`.
- Icons from `lucide-react`, sized with `size-*` (not `h-*`/`w-*`), `aria-hidden="true"` when decorative.
- Meta rows (author · date · reading time) use `&middot;` separators and `<time dateTime>`; format via `formatDate` / `formatPrice` / `formatPercent` / `formatCurrency` from `lib/format.ts` — never format dates/numbers inline. Post metadata comes from `lib/posts.ts`.
- Client components (`'use client'`) sparingly — only for interactivity (mobile nav, sidenote, live ticker/chart via SWR). Prefer server components by default.
- Brand mark pattern: `components/logo.tsx` — documented comment block + semantic Tailwind fills/strokes (`fill-background`, `stroke-primary`, `fill-primary`) so it tracks light/dark themes.

## Shells (pick the right one)

**Public marketing** (home, blog, pricing, tools, etc.) — see `app/[locale]/page.tsx`:

```tsx
<div className="flex min-h-screen flex-col">
  <TickerTape />
  <SiteHeader />
  <main className="flex-1">{/* sections */}</main>
  <SiteFooter />
</div>
```

**Member app** — sidebar shell in `app/[locale]/member/layout.tsx` (`SidebarProvider` + `components/member/site-header`). No `TickerTape` / marketing footer.

**Admin** — English-only root in `app/admin/layout.tsx` (admin sidebar + `components/admin/site-header`). No marketing shell.

**Auth / errors** — login is minimal (logo only). `not-found` / `unauthorized` use header + footer but no `TickerTape`.

## Container widths

- Marketing/listing pages: `mx-auto max-w-6xl px-4 md:px-6`
- Article/reading content: `mx-auto max-w-2xl px-4 md:px-6`

## Sections

Stack full-bleed `<section>` blocks separated by `border-b border-border`, each with its own `max-w-*` inner wrapper and vertical rhythm `py-12`–`py-16` (`md:py-14`–`md:py-24` for hero). Section headings use `font-heading text-2xl font-semibold tracking-tight`.

## Landing product demos

- **Multi-window / Play script** → skill `cursor-style-desktop-demo` (portable pattern; our home `#big-money` stage is one instance). Wider stage wrapper `max-w-360`.
- **Single framed widget** → `ProductFrame`. Do not nest a desktop stage inside `ProductFrame`.

## Cards & interactive tiles

Clickable cards are a single `<Link>` wrapping the whole tile: `rounded-xl border border-border bg-card p-6 transition-colors hover:border-primary/50`, with `group` for child hover motion (`group-hover:-translate-y-0.5 group-hover:translate-x-0.5`).

## Responsive grids

`grid gap-6 sm:grid-cols-2 lg:grid-cols-3` for post/category grids. Use `md:grid-cols-2` for two-up featured layouts.

## Article layout

MDX articles use `app/[locale]/blog/[slug]/layout.tsx`, wrapped in `<article className="... [counter-reset:sidenote]">` for Tufte sidenotes. Render `<ArticleHeader post={post} />` from `components/article-header.tsx` — metadata comes from `lib/posts.ts`, not the MDX body.
