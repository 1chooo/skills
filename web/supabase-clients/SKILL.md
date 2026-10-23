---
name: supabase-clients
description: >-
  Shared Supabase client matrix for Next.js apps — server, browser, and service
  clients; never expose service role to the client. Use when adding auth, DB
  access, or SSR session handling in 1chooo.com or JustHold.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# Supabase Clients

Shared client layout used by both apps under `utils/supabase/`.

## Client matrix

| File | Export | Use when |
|------|--------|----------|
| `utils/supabase/server.ts` | `async createClient()` | Server Components, route handlers — cookie session via `next/headers` |
| `utils/supabase/client.ts` | `createClient()` | Browser Client Components only |
| `utils/supabase/service.ts` | `createServiceClient()` | Trusted server paths that must bypass RLS — needs `SUPABASE_SERVICE_ROLE_KEY` |
| `proxy.ts` | inline `createServerClient` | Early session refresh + redirects only |

Optional extras (project-specific): `public.ts` for anon public reads, legacy `middleware.ts` helpers — do not revive as root `middleware.ts`.

## Hard rules

- **Never** import the service-role client or server auth helpers into Client Components.
- **Never** expose `SUPABASE_SERVICE_ROLE_KEY` to the browser or public env.
- Prefer RLS + cookie `createClient()` for user-owned data; use `createServiceClient()` only when bypassing RLS is intentional (admin tools, trusted webhooks, analytics ingest).
- When using the service client on user data, still filter by `user_id` / ownership in application code.
- Cookie writes from `server.ts` `setAll` may no-op in pure Server Components — session refresh belongs in Route Handlers / `proxy.ts`.

## Auth + cookies (typical)

- Sessions via `@supabase/ssr` cookie adapters
- Optional cookie domain / SameSite overrides via env (e.g. `AUTH_COOKIE_DOMAIN`) for cross-subdomain auth
- Leave cookie domain unset on localhost

## CORS (when needed)

If APIs are called cross-origin, apply CORS in `proxy.ts` for `/api/*` from an allowlist env (e.g. `CORS_ALLOWED_ORIGINS`). Credentials + `Vary: Origin` when cookies are involved.

## Schema

Treat `supabase/` migrations or `schema.sql` as the source of truth for tables and RLS. Read relevant sections before adding tables or API routes. Prefer RLS on all exposed tables; never authorize solely via `user_metadata`.
