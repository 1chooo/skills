---
name: web-service-stack
description: >-
  Shared foundation for 1chooo.com and JustHold web apps — Next.js 16 stack,
  code placement, imports, RSC defaults, proxy.ts, and three-shell layout.
  Use when scaffolding features, placing files, or aligning cross-project
  conventions.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# Web Service Stack

Shared conventions for **1chooo.com** and **JustHold**. Project overlays add design systems, i18n, and product-specific UI — follow those after this skill.

## Stack

| Layer | Choice |
|-------|--------|
| Framework | Next.js 16 App Router, React 19 |
| Language | TypeScript |
| Styling | Tailwind CSS v4, `cn()` (`clsx` + `tailwind-merge`) |
| Auth + DB | Supabase (`@supabase/ssr`, Postgres, RLS) |
| Package manager | `pnpm` |
| Content | MDX; assets often on Cloudflare R2 |

## Three shells

Every app has three layout zones. Pick the matching shell; do not mix marketing chrome into member/admin.

| Shell | Role |
|-------|------|
| **Public** | Marketing, blog, landing — site header/footer |
| **Authenticated** | Member / services workspace — sidebar or workspace shell |
| **Admin** | Role-gated dashboard — admin chrome; often English-only |

## Where to put code

| Need | Location |
|------|----------|
| Domain helpers, formatters, constants | `lib/` |
| Auth helpers (server-only) | `utils/auth.ts` or `lib/auth.ts` |
| Supabase clients | `utils/supabase/{server,client,service}.ts` |
| Shared UI primitives | `components/ui/` (CVA + `cn()`) |
| Feature-local components | `app/.../_components/` or feature folders |
| REST handlers | `app/api/<namespace>/` |
| Early session refresh + CORS | root `proxy.ts` |
| DB schema reference | `supabase/` (migrations / `schema.sql`) |

## Core conventions

- **Imports:** `@/` path alias — avoid `../` across directories.
- **Named exports** for components and utilities; **default exports** only for Next.js `page.tsx` / `layout.tsx` and MDX.
- **Server Components by default** — `"use client"` only for state, effects, browser APIs, or Context consumers.
- **Async params (Next.js 16):** always `await params` / `searchParams` — they are `Promise`s.
- **`proxy.ts` not `middleware.ts`** — Next.js 16 root proxy for session refresh and API CORS. Do not add a root `middleware.ts`.
- **UI tokens:** use semantic theme classes / CSS variables — never hardcode hex/oklch in components.
- **Icons:** `lucide-react` with `size-*` (not `h-*`/`w-*`); `aria-hidden="true"` when decorative.
- **MDX:** keep article bodies separate from metadata (frontmatter export or CMS/DB headers).

## Commands (typical)

| Action | Command |
|--------|---------|
| Install | `pnpm install` |
| Dev | `pnpm dev` |
| Build | `pnpm build` |
| Lint | `pnpm lint` |

## Project overlays

After this skill, apply the app-specific package:

- **1chooo.com** — `1chooo-project` and related UI/API skills
- **JustHold** — `justhold-project`, `justhold-i18n`, `justhold-design-tokens`, `justhold-ui-layout`
