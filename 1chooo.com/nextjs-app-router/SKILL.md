---
name: nextjs-app-router
description: Next.js 16 App Router patterns — Server/Client boundaries, async params, layouts, auth guards, and proxy.ts
---

# Next.js App Router Patterns

## Server Components by default

Pages and layouts are async Server Components unless they need interactivity. Fetch data in Server Components, pass as props to Client wrappers.

```tsx
// ✅ Server page fetches, client wrapper handles state
export default async function JobsLayout({ children }: { children: React.ReactNode }) {
  const supabase = await createClient()
  const { data: { user } } = await supabase.auth.getUser()
  if (!user) redirect('/services/sign-in?from=/services/jobs')
  return (
    <JobsWorkspaceProvider initialApplications={apps}>
      <JobsShell>{children}</JobsShell>
    </JobsWorkspaceProvider>
  )
}
```

Keep client components as leaf nodes — push `"use client"` as far down the tree as possible.

Add `"use client"` only for: `useState`, `useEffect`, `useCallback`, event handlers, browser APIs, React Context consumers.

## Async params and searchParams (Next.js 16)

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

Always `await params` before use — never access `props.params.slug` directly.

## `proxy.ts` — early auth redirects and CORS

Next.js 16 uses root `proxy.ts` (not `middleware.ts`) for early redirects and API CORS. Exports **named** `async function proxy(req)` + `config.matcher`. Do not add `middleware.ts`. `utils/supabase/middleware.ts` is unused legacy.

`proxy.ts` checks **signed-in only** for `/admin/*` — it does **not** call `isAdmin()`. Role gate is in `app/(admin)/admin/layout.tsx`.

| Matcher | Behavior |
|---------|----------|
| `/api/*` | `applyApiCors()` from `utils/api-cors.ts` |
| `/admin/*` (except `/login`) | Session refresh; redirect unsigned to `/login?from=...` |
| `/services/orders`, `/notes`, `/stock`, `/big-money` | Session refresh; redirect unsigned to `/services/sign-in?from=...` |

## Auth guard layers

| Area | Pattern |
|------|---------|
| Services | Layout or page checks `createClient().auth.getUser()` → `redirect('/services/sign-in?from=...')` |
| Admin | `app/(admin)/admin/layout.tsx` checks `getUser()` + `isAdmin()` → `/login` or `/access-denied` |
| API | `requireUser()` / `requireAdmin()` in route handlers |

## Data fetching

| Context | Pattern |
|---------|---------|
| Static MDX | Dynamic `import()` + `generateStaticParams()` |
| Article listings | `fs.readdir` + dynamic import of `metadata` |
| Server pages | Supabase via `createClient()` in async Server Components |
| Client reads | SWR / SWR Infinite |
| Client mutations | `fetch('/api/...')` or `apiFetch()` from `@/lib/api-fetch.ts` |
| External APIs | Server-side `fetch` with `next: { revalidate: N }` |

No Server Actions — mutations go through `app/api/` route handlers.

## React Context for workspace state

Services use a Provider initialized from server-fetched data:

```tsx
<JobsWorkspaceProvider initialApplications={apps} initialCategories={categories}>
  {children}
</JobsWorkspaceProvider>
```

## Feature folder layout

```
app/(services)/services/<feature>/
  layout.tsx          # auth guard + data fetch + provider
  page.tsx            # entry page
  [id]/page.tsx       # detail routes
  _components/        # feature-local components (private folder)
```

## Full-bleed escape hatch (services)

Default services column is `max-w-5xl` centered. Pages that need full width opt out:

```tsx
<div className="relative left-1/2 -translate-x-1/2 w-screen max-w-[100vw] -my-8 sm:-my-12">
  {/* full-bleed content */}
</div>
```

## Missing resources

Use `notFound()` from `next/navigation` — no dedicated `error.tsx` / `loading.tsx` files.

## Install

Copy this file to `.cursor/skills/nextjs-app-router/SKILL.md` in your project, or clone from [github.com/1chooo/skills](https://github.com/1chooo/skills).
