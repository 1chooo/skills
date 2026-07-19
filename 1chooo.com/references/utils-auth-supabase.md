# Utils — auth, Supabase, CORS

`utils/` is server-only except `utils/supabase/client.ts`. **Never import `utils/auth.ts` in client components.**

## Supabase client matrix

| File | Export | Use when |
|------|--------|----------|
| `utils/supabase/server.ts` | `async createClient()` | Server Components, route handlers (cookie session via `next/headers`) |
| `utils/supabase/client.ts` | `createClient()` | Browser client components |
| `utils/supabase/service.ts` | `createServiceClient()` | Server DB writes; bypasses RLS; requires `SUPABASE_SERVICE_ROLE_KEY` |
| `proxy.ts` | inline `createServerClient` | Early session refresh + redirects only |

`utils/supabase/server.ts` `setAll` silently catches errors when called from Server Components — cookie writes only work in Route Handlers / `proxy.ts`.

## Auth helpers (`utils/auth.ts`)

| Helper | Use for |
|--------|---------|
| `getUser()` | Optional auth — `Authorization: Bearer` first, then cookie session |
| `requireUser()` | Authenticated routes — returns `{ user, error }` where `error` is a ready-made `NextResponse` |
| `requireAdmin()` | `requireUser()` + `isAdmin()` |
| `isAdmin(userId)` | Queries `user_roles` table via service-role client |

```ts
const { user, error: authError } = await requireUser()
if (authError) return authError
```

## Auth cookies (`utils/supabase/auth-cookie-options.ts`)

`mergeAuthCookieOptions()` is applied whenever Supabase sets cookies (`proxy.ts`, `server.ts`, `app/api/admin/login/route.ts`).

- When `AUTH_COOKIE_DOMAIN` is set (e.g. `.1chooo.com`): cookies get `domain`, `SameSite=None`, `Secure`
- Leave unset on localhost (host-only cookies)
- Overrides: `AUTH_COOKIE_SAME_SITE`, `AUTH_COOKIE_SECURE=false` (local HTTPS testing)

## CORS (`utils/api-cors.ts`)

Applied globally in `proxy.ts` for `/api/*`.

- If `CORS_ALLOWED_ORIGINS` is empty or request has no `Origin`, returns `NextResponse.next()` (no CORS headers)
- `OPTIONS` preflight: matching origin → `204`; non-matching → `403`
- Sets `Access-Control-Allow-Credentials: true` and `Vary: Origin`

## Dual sign-in routes

- **Services:** `POST /api/auth/sign-in` — uses `utils/supabase/server.ts`
- **Admin:** `POST /api/admin/login` — builds `NextResponse` first, attaches cookies via inline `createServerClient` + `mergeAuthCookieOptions`

## Legacy

- No root `middleware.ts` — Next.js 16 uses root `proxy.ts` (named export `async function proxy`)
- `utils/supabase/middleware.ts` is **unused** — do not wire it up

## DB schema

Canonical schema + RLS: `supabase/schema.sql` — read relevant sections before adding tables or API routes.
