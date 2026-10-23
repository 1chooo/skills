---
name: api-supabase-routes
description: >-
  Shared REST API route conventions for Supabase-backed Next.js apps — auth
  gate, Zod validation, scoped queries, and response shapes. Use when adding or
  editing app/api handlers in 1chooo.com or JustHold.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# API + Supabase Route Conventions

Shared handler recipe for both apps. Project overlays may add namespaces, CORS details, or stricter “no Server Actions” rules.

## Handler flow

1. **Auth** — reject unsigned users with 401
2. **Parse body** — JSON → 400 on failure
3. **Validate** — Zod `safeParse()` → 422 on failure
4. **Query** — Supabase scoped by owner (`user_id` / profile id)
5. **Respond** — entity-named JSON or standard error body

```ts
export async function POST(req: NextRequest) {
  // 1. auth (project helper — see below)
  // 2. body = await req.json() with try/catch → 400
  // 3. Schema.safeParse(body) → 422
  // 4. supabase.from(...).eq('user_id', ownerId)
  // 5. return NextResponse.json({ entries: data })
}
```

## Auth helpers (both styles OK)

| Style | Example | Used by |
|-------|---------|---------|
| `{ user, error }` tuple | `requireUser()` / `requireAdmin()` | 1chooo.com |
| Profile / null | `getCurrentProfile()` → 401 if missing | JustHold |

```ts
// Tuple style
const { user, error: authError } = await requireUser()
if (authError) return authError

// Profile style
const profile = await getCurrentProfile()
if (!profile) {
  return NextResponse.json({ error: 'Unauthorized.' }, { status: 401 })
}
```

Never import server auth helpers into Client Components.

## Validation

```ts
const parsed = CreateSchema.safeParse(body)
if (!parsed.success) {
  return NextResponse.json({ error: parsed.error.flatten() }, { status: 422 })
}
```

Prefer shared types under `types/` (or project equivalent) over large inline schemas in route files.

## Supabase in handlers

| Need | Client |
|------|--------|
| User-scoped reads/writes under RLS | `createClient()` from `utils/supabase/server` |
| Bypass RLS (admin, trusted server writes) | `createServiceClient()` from `utils/supabase/service` |

Always scope user-owned rows: `.eq('user_id', ownerId)`.

- Supabase `error` → **502** with `error.message`
- Missing row → **404** `{ error: 'not found' }` (or project casing)

## Response shapes

| Status | Body |
|--------|------|
| 200 / 201 | `{ <entityName>: data }` — e.g. `{ entries }`, `{ application }`, `{ ok: true }` |
| 204 | No body (deletes) |
| 400 | `{ error: 'invalid json' }` |
| 401 | `{ error: 'unauthorized' }` (casing may vary slightly by project) |
| 403 | `{ error: 'forbidden' }` |
| 404 | `{ error: 'not found' }` |
| 422 | `{ error: parsed.error.flatten() }` |
| 502 | `{ error: error.message }` |

Prefer lowercase error strings.

## Body parsing

```ts
let body: unknown
try {
  body = await req.json()
} catch {
  return NextResponse.json({ error: 'invalid json' }, { status: 400 })
}
```

## Async route params

```ts
export async function PATCH(
  req: NextRequest,
  { params }: { params: Promise<{ id: string }> },
) {
  const { id } = await params
}
```
