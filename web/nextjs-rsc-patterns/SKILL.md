---
name: nextjs-rsc-patterns
description: >-
  Shared Next.js 16 App Router patterns — Server/Client boundaries, async
  params, layout auth redirects, proxy.ts, and data-fetching matrix. Use when
  adding pages, layouts, or client interactivity in 1chooo.com or JustHold.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# Next.js RSC Patterns

Project-agnostic App Router rules shared by 1chooo.com and JustHold.

## Server Components by default

Pages and layouts are async Server Components unless they need interactivity. Fetch on the server; pass data as props to Client leaves.

```tsx
export default async function FeatureLayout({ children }: { children: React.ReactNode }) {
  const supabase = await createClient()
  const { data: { user } } = await supabase.auth.getUser()
  if (!user) redirect('/sign-in?from=/member')
  return <>{children}</>
}
```

Push `"use client"` as far down the tree as possible.

Add `"use client"` only for: `useState`, `useEffect`, event handlers, browser APIs, React Context consumers.

## Async params and searchParams

Route handlers, pages, and `generateMetadata` receive async params:

```tsx
export default async function Page(props: {
  params: Promise<{ slug: string }>
  searchParams: Promise<{ from?: string }>
}) {
  const params = await props.params
  const searchParams = await props.searchParams
}
```

Always `await params` before use — never `props.params.slug` directly.

## `proxy.ts` — session refresh and CORS

Next.js 16 uses root `proxy.ts` (not `middleware.ts`). Export **named** `async function proxy(req)` + `config.matcher`.

Typical responsibilities:

| Matcher | Behavior |
|---------|----------|
| `/api/*` | CORS helpers when cross-origin clients exist |
| Authenticated path prefixes | Session refresh; redirect unsigned users to sign-in with `?from=` |
| Admin paths | Session check only — **role gates live in layouts / handlers**, not in `proxy.ts` |

Do not wire unused `utils/supabase/middleware.ts` as a root middleware.

## Auth guard layers

| Layer | Pattern |
|-------|---------|
| Layout / page | `createClient().auth.getUser()` (or project profile helper) → `redirect(...?from=...)` |
| Admin layout | Session + admin/role check → login or access-denied |
| API route | Auth helper in the handler (`requireUser` / `getCurrentProfile` / equivalent) |

## Data fetching

| Context | Pattern |
|---------|---------|
| Server pages / layouts | Supabase via `createClient()` in async Server Components |
| Client reads | SWR / SWR Infinite (or equivalent) |
| Client mutations | `fetch('/api/...')` with credentials, or a shared `apiFetch` helper |
| External APIs | Server-side `fetch` with `next: { revalidate: N }` when caching is intentional |
| Static MDX | Dynamic `import()` + `generateStaticParams()` when applicable |

## Missing resources

Use `notFound()` from `next/navigation` for missing entities.

## Mutations note

Apps differ on Server Actions:

- **1chooo.com** — prefer REST route handlers only (see `api-routes`)
- **JustHold** — route handlers for many APIs; Server Actions exist in some flows

Prefer the pattern already used in the feature you are editing. New shared CRUD should follow the project's dominant style and `api-supabase-routes`.
