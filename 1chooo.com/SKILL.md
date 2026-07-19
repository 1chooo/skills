---
name: 1chooo.com
description: >-
  Conventions for building 1chooo.com — Next.js 16 App Router, React 19, Supabase,
  MDX, Tailwind rurikon design system, API routes, services UI, and admin UI.
  Use when working on 1chooo.com, its route groups, API handlers, MDX articles,
  authenticated services, or admin dashboard.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# 1chooo.com

Procedural knowledge for the [1chooo.com](https://1chooo.com) monolith: personal site + authenticated mini-apps on Next.js 16, React 19, TypeScript, Tailwind CSS v4, MDX, and Supabase.

## When to Apply

- Starting any task in the 1chooo.com codebase
- Adding pages, layouts, or features under `app/(home)/`, `app/(services)/`, or `app/(admin)/`
- Writing or reviewing `app/api/**` route handlers
- Auth, Supabase clients, CORS, or `proxy.ts`
- MDX articles, interactive demos, or LLM routes
- Services / admin UI (buttons, dialogs, toasts, workspace shells)

## Stack

| Layer | Choice |
|-------|--------|
| Framework | Next.js 16 App Router, React 19 |
| Package manager | `pnpm` |
| Auth + DB | Supabase (`@supabase/ssr`) |
| Styling | Tailwind CSS v4, `rurikon-*`, light-only, `cn()` |
| Content | MDX + Shiki |
| Fonts | Inter, Lora, Iosevka |

## Hard rules

- Always `@/` imports — never relative `../` across directories
- Server Components by default; `"use client"` only when needed
- Always `await params` / `await searchParams` (Next.js 16)
- **No Server Actions** — mutations go through `app/api/`
- Types in `types/`, runtime constants in `lib/` — never mix
- Never import `utils/auth.ts` in client components
- No `window.confirm()` / `alert()` in services — use `ConfirmDialog` + Sonner
- Admin tabs: plain `<button>`, square corners — never Radix `TabsTrigger`

## Quick reference

| Topic | Read when | File |
|-------|-----------|------|
| Project foundation | Stack, route groups, where to put code | [references/project.md](references/project.md) |
| App Router | Server/Client, async params, `proxy.ts`, auth guards | [references/nextjs-app-router.md](references/nextjs-app-router.md) |
| API routes | REST handlers, Zod, response shapes, namespaces | [references/api-routes.md](references/api-routes.md) |
| Types & constants | `types/` vs `lib/`, NavItem collision | [references/types-and-constants.md](references/types-and-constants.md) |
| Auth & Supabase | Client matrix, cookies, CORS | [references/utils-auth-supabase.md](references/utils-auth-supabase.md) |
| MDX content | Frontmatter, scaffolding, demos, LLM routes | [references/mdx-content.md](references/mdx-content.md) |
| Article components | Two-column demos, rurikon UI | [references/article-components.md](references/article-components.md) |
| Components UI | Radix, Dialog, ConfirmDialog, Sonner, Charts | [references/components-ui.md](references/components-ui.md) |
| Services UI | Lowercase copy, mono buttons, workspace shells | [references/services-ui.md](references/services-ui.md) |
| Confirms & toasts | Destructive actions, toast rules | [references/services-confirm-toasts.md](references/services-confirm-toasts.md) |
| Admin UI | Square tab switchers | [references/admin-ui.md](references/admin-ui.md) |

## How to use

1. Skim **Hard rules** above for every change.
2. Open the matching `references/*.md` file for the area you are editing.
3. Prefer existing reference implementations cited in those files over inventing new patterns.
