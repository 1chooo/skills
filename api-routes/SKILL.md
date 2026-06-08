---
name: api-routes
description: REST API route handler conventions for 1chooo.com — auth, Zod validation, Supabase, and response shapes
---

# API Route Conventions

All mutations go through Route Handlers — there are no Server Actions (`"use server"`).

Reference implementation: `app/api/jobs/applications/[id]/route.ts`

## Handler structure

```ts
/** PATCH /api/jobs/applications/[id] */
export async function PATCH(
  req: NextRequest,
  { params }: { params: Promise<{ id: string }> },
) {
  const { user, error: authError } = await requireUser()
  if (authError) return authError

  const { id } = await params
  // parse body → validate → query → respond
}
```

## Auth

Import from `@/utils/auth.ts` (server-only — never in client components):

| Helper | Use for |
|--------|---------|
| `requireUser()` | Any authenticated user — returns `{ user, error }` where `error` is a ready-made `NextResponse` |
| `requireAdmin()` | Admin-only routes |
| `getUser()` | Optional auth (returns `null` if unsigned) |

```ts
const { user, error: authError } = await requireUser()
if (authError) return authError
```

Bearer token auth is supported alongside cookie sessions.

## Validation

Use **Zod** with `safeParse()` — return 422 on failure:

```ts
const parsed = PatchSchema.safeParse(body)
if (!parsed.success) {
  return NextResponse.json({ error: parsed.error.flatten() }, { status: 422 })
}
```

Define request/response shapes in `types/services/<service>/`, not inline in route files.

## Supabase

- Use `createServiceClient()` from `@/utils/supabase/service` for DB operations.
- **Always filter by `user_id`** on user-owned resources: `.eq('user_id', user.id)`.
- Check `error` field → return 502 with `error.message`.
- Missing row → 404 `{ error: 'not found' }`.

## Response shapes

| Status | Body |
|--------|------|
| 200 | `{ <entityName>: data }` — e.g. `{ application }`, `{ categories }`, `{ ok: true }` |
| 201 | Same entity-named pattern or `{ ok: true }` |
| 204 | No body (deletes) |
| 400 | `{ error: 'invalid json' }` |
| 401 | `{ error: 'unauthorized' }` |
| 403 | `{ error: 'forbidden' }` |
| 404 | `{ error: 'not found' }` |
| 422 | `{ error: parsed.error.flatten() }` |
| 502 | `{ error: error.message }` |

Error strings are lowercase.

## Body parsing

```ts
let body: unknown
try { body = await req.json() } catch {
  return NextResponse.json({ error: 'invalid json' }, { status: 400 })
}
```

## Revalidation

- `export const revalidate = 0` for admin/analytics routes that must be fresh.
- `export const revalidate = false` for static content routes (e.g. `llms.txt`).

## CORS

Use `utils/api-cors.ts` when endpoints are consumed cross-origin; origins come from `CORS_ALLOWED_ORIGINS` env.

## API namespaces

```
app/api/
  admin/       # requireAdmin() — photos, analytics, store, split, cursor-usage
  auth/        # sign-in, sign-out, sign-up
  jobs/        # job tracker CRUD + analytics + import
  stock/       # quotes, transactions, watchlist, chart
  user/        # notes, influencers, split events (owner), orders
  store/       # public storefront by token + order edit
  split/       # public split event by token (participants, items, payments)
  photos/      # public photo endpoints + upload
  money/       # big-money calculator entries
  npm/         # npm downloads chart data
  track/       # page-view analytics
  og/          # dynamic OG image generation
```

Public token routes (`/api/split/[token]`, `/api/store/[token]`) do not use `requireUser()` — validate the token instead.

## Install

Copy this file to `.cursor/skills/api-routes/SKILL.md` in your project, or clone from [github.com/1chooo/skills](https://github.com/1chooo/skills).
