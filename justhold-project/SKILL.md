---
name: justhold-project
description: >-
  JustHold foundation — stack, three app shells, defaults, and where conventions
  live. Use when working anywhere in the JustHold repo or scaffolding new
  features.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# JustHold Project Foundation

Also follow Shared skills: `web-service-stack`, `nextjs-rsc-patterns`, `api-supabase-routes`, `supabase-clients`.

## Stack

Next.js App Router, React 19, Supabase (Auth + Postgres + RLS), Tailwind CSS v4, shadcn `base-nova` / Base UI, next-intl (`zh-hant` primary, `en`), pnpm.

## App shells

- **Public marketing**: `TickerTape` + `SiteHeader` + `SiteFooter` under `app/[locale]/…`
- **Member**: sidebar shell in `app/[locale]/member/layout.tsx`
- **Admin**: English-only `app/admin/` (no locale segment)

## Defaults

- Prefer Server Components; `'use client'` only for real interactivity.
- Use semantic theme tokens from `app/globals.css` — never hardcode hex/oklch in components.
- Format dates/money via `lib/format.ts`.
- Unit-test pure helpers in `lib/` under `tests/unit/` — do not invent new test roots.
- For Supabase or shadcn tasks, follow the matching skill under `.agents/skills/`.

## Related JustHold skills

| Skill | When |
|-------|------|
| `justhold-i18n` | Locales, messages, admin exception |
| `justhold-design-tokens` | Palette, fonts, radius |
| `justhold-ui-layout` | UI primitives + page shells |
| `justhold-mdx-content` | Admin CMS MDX posts (Supabase + R2) |
| `justhold-testing` | Vitest unit/integration + Playwright e2e |

## Commands

| Action | Command |
|--------|---------|
| Install | `pnpm install` |
| Dev | `pnpm dev` |
| Build | `pnpm build` |
| Unit tests | `pnpm test` |
| Integration | `pnpm test:int` |
| E2E | `pnpm test:e2e` |
