# Types and constants

Shared types belong in `types/`, not in `lib/`, API routes, or component files.
Domain constants (labels, colors, option lists, enum arrays) belong in `lib/`, not in `types/`.

## Import paths

```tsx
// ✅ GOOD — types from @/types, constants from @/lib
import type { JobApplication, ApplicationStatus } from '@/types/services/jobs'
import { STATUS_LABELS, APPLICATION_STATUSES } from '@/lib/jobs'

// ❌ BAD — types from lib
import type { ApplicationStatus } from '@/lib/jobs-import'

// ❌ BAD — constants in types/
// types/services/jobs/constants.ts with STATUS_LABELS

// ❌ BAD — colocated types inside a feature folder
import type { JobApplication } from '@/app/(services)/services/jobs/_components/jobs-types'
```

## `types/` vs `lib/`

- **`types/`** — type definitions only (`type`, `interface`, union types).
- **`lib/`** — runtime values: constants, parsers, normalizers, formatters. Import types from `@/types`.

```ts
// types/services/jobs/application.ts
export type ApplicationStatus = 'not_started' | 'applied' | ...

// lib/jobs.ts
import type { ApplicationStatus } from '@/types/services/jobs/application'
export const APPLICATION_STATUSES = [...] as const satisfies readonly ApplicationStatus[]
export const STATUS_LABELS: Record<ApplicationStatus, string> = { ... }
```

## Service types folder layout

```
types/services/
  jobs/
    application.ts   # core entity / enum types
    analytics.ts     # API response / chart data shapes
    ui.ts            # view-layer types (sort keys, column keys, tabs)
    index.ts         # barrel re-export
  stock/
    position.ts
    ui.ts
    index.ts
  split/
    event.ts
    index.ts
  notes/
    category.ts
    index.ts
  workspace/
    shell.ts         # shared NavItem, PaletteAction (stock/split shells)
    index.ts
  nav.ts             # re-exports NavItem as ServiceNavItem from types/nav.ts
```

## Non-service types (site-wide)

| Location | Use for |
|----------|---------|
| `types/nav.ts` | `NavItem` — site, services, and admin nav |
| `types/ui/chart.ts` | `ChartConfig` — Recharts wrappers |
| `types/photos.ts` | Admin photo manager |
| `types/content.ts` | `Post` — article listing summaries |

## `NavItem` name collision

| Type | Path | Shape | Use |
|------|------|-------|-----|
| Site nav | `types/nav.ts` | `{ label, href, isDev? }` | `lib/services-nav.ts`, `lib/admin-nav.ts` |
| Workspace tabs | `types/services/workspace/shell.ts` | `{ id, label, icon }` | `ServiceWorkspaceShell` internal nav |

`types/services/nav.ts` re-exports site nav as **`ServiceNavItem`** — use that alias in services code.

## lib/ layout

```
lib/
  jobs.ts              # APPLICATION_STATUSES, STATUS_LABELS, STATUS_COLORS
  split.ts             # EVENT_STATUSES, STATUS_LABELS, STATUS_STYLES
  stock.ts             # CHART_COLORS, formatters, parsers
  services-nav.ts      # SERVICE_NAV_ITEMS, getActiveServiceHref
  admin-nav.ts         # ADMIN_NAV_ITEMS
  api-fetch.ts         # client fetch (credentials + throw on error)
  shiki.ts             # highlighter singleton (shared with MDX)
  path.ts              # article directory path constants
```

## API route types

Define response/request shapes in `types/services/<service>/`, not inline in route files:

```ts
// app/api/jobs/analytics/route.ts
import type { AnalyticsResponse } from '@/types/services/jobs/analytics'
```

## Rules

- Add a new service folder under `types/services/` when types are shared across multiple files.
- Add or extend a matching `lib/<service>.ts` for constants tied to those types.
- Keep one-off props interfaces next to the component that uses them only.
