---
name: 1chooo-project
description: Foundation conventions for 1chooo.com — stack, route groups, where to put code, imports, and design system
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# 1chooo.com Project Foundation

Personal site + authenticated mini-apps monolith on **Next.js 16 App Router**, **React 19**, **TypeScript**, **Tailwind CSS v4**, **MDX**, and **Supabase**.

## Stack

- **Package manager:** `pnpm`
- **Auth + DB:** Supabase (cookies via `@supabase/ssr`)
- **Styling:** Tailwind CSS v4, `rurikon-*` palette, light-only, `cn()` from `@/lib/utils`
- **Content:** MDX articles via `@next/mdx` + Shiki code highlighting
- **Fonts:** Inter (sans), Lora (serif), Iosevka (mono)

## Route groups

| Path | Purpose |
|------|---------|
| `app/(home)/` | Public portfolio, blog, articles |
| `app/(services)/` | Authenticated mini-apps (stock, jobs, notes, split, …) |
| `app/(admin)/` | Admin dashboard (role-gated) |
| `app/(llms)/` | `llms.txt`, per-page markdown for LLMs |
| `app/api/` | REST route handlers (no Server Actions) |

## Where to put new code

| Need | Location |
|------|----------|
| Shared types | `types/services/<name>/` |
| Domain constants, formatters | `lib/<name>.ts` |
| Auth helpers | `utils/auth.ts` (server-only) |
| Supabase clients | `utils/supabase/server.ts`, `client.ts`, `service.ts` |
| Shared UI primitives | `components/ui/` (shadcn-style, CVA + Radix) |
| Site layout (navbar, header) | `components/layout/` |
| Article interactive demos | `components/articles/<topic>/` |
| Feature-local components | `app/<route-group>/.../_components/` |
| API endpoints | `app/api/<namespace>/` |
| Article MDX | `app/(home)/(article)/<section>/_articles/` |
| Content path constants | `lib/path.ts` |
| Site / admin nav constants | `lib/services-nav.ts`, `lib/admin-nav.ts` — types in `types/nav.ts` |
| CORS + early auth redirects | `proxy.ts` (root) |
| MDX component map | `mdx-components.tsx` (repo root) |
| DB schema reference | `supabase/schema.sql` |
| Client fetch helper | `lib/api-fetch.ts` |

## Commands

| Action | Command |
|--------|---------|
| Install deps | `pnpm install` |
| Dev server | `pnpm dev` |
| Build | `pnpm build` |
| Lint | `pnpm lint` |
| New thought | `pnpm create-thoughts title="..." date=2026.02.28 description="..."` |
| New episode | `pnpm create-episode title="..." date=2026.02.28 description="..." [slug=...]` |

## Core conventions

- **Imports:** Always use `@/` path alias — never relative `../` paths across directories.
- **Type imports:** `import type { X } from '@/types/...'`
- **Named exports** for components and utilities; **default exports** only for Next.js `page.tsx` / `layout.tsx` and MDX content.
- **Server Components by default** — add `"use client"` only when needed (state, effects, browser APIs).
- **Async params (Next.js 16):** always `await params` — `params: Promise<{ slug: string }>`.
- **No Server Actions** — mutations go through `app/api/` route handlers.
- **No test framework** — verify with `pnpm dev` / `pnpm build`.

## Design system

- **Palette:** `rurikon-*` tokens (custom grays) in `app/globals.css`
- **Light-only:** `colorScheme: 'only light'` — no dark mode
- **Class merging:** `cn()` from `@/lib/utils` (`clsx` + `tailwind-merge`)

## Dev-only navigation

- `NavItem.isDev: true` in `lib/services-nav.ts` — hidden in production (`getVisibleServiceNavItems`)
- Home navbar shows Drafts + Admin links only when `NODE_ENV === 'development'`

## `NavItem` name collision

| Type | Path | Shape | Use |
|------|------|-------|-----|
| Site nav | `types/nav.ts` | `{ label, href, isDev? }` | `lib/services-nav.ts`, `lib/admin-nav.ts` |
| Workspace tabs | `types/services/workspace/shell.ts` | `{ id, label, icon }` | `ServiceWorkspaceShell` internal nav |

Use `ServiceNavItem` alias from `types/services/nav.ts` in services code to avoid the collision.
