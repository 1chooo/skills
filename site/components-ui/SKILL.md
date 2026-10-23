---
name: components-ui
description: Shared UI primitives in components/ui/ — Radix package, Dialog API, ConfirmDialog, Sonner placement, and Charts
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# Components UI Conventions

Shared primitives in `components/ui/` — shadcn-style (CVA + Radix). Reference: `components/ui/button.tsx`, `components/ui/dialog.tsx`.

## Radix import style

Use the unified **`radix-ui`** package:

```ts
import { Dialog as DialogPrimitive } from "radix-ui"
```

Not `@radix-ui/react-*`. Components use `data-slot="..."` attributes (shadcn v4 style).

## When to use `<Button>` vs custom buttons

| Area | Pattern |
|------|---------|
| `components/ui/button.tsx` | shadcn CVA (`rounded-md`, `variant`/`size`) — used sparingly |
| Services | Custom `font-mono text-xs rounded` buttons — see `services-ui` rule |
| Admin | Square-cornered plain `<button>` — see `admin-ui` skill |
| `ConfirmDialog` | Plain `<button>`, not `Button` |

## Dialog API

`Dialog` exports custom subcomponents: `DialogHeader`, `DialogBody`, `DialogTitle`, `DialogDescription`.

- `DialogTitle` is **lowercase by default** (`text-sm font-semibold text-rurikon-700 lowercase`)
- `DialogContent` uses `rounded-sm`, `border-rurikon-border`, `max-w-lg`
- Services confirms override to `max-w-sm`

`ConfirmDialog` (`components/ui/confirm-dialog.tsx`) wraps `Dialog` for destructive actions.

## Sonner

`components/ui/sonner.tsx` wraps Sonner with Lucide icons. `<Toaster>` is mounted in **area layouts**, not root:

- `app/(services)/layout.tsx` — `position="bottom-center"`
- `app/(admin)/layout.tsx` — `position="bottom-right"`

**Home layout has no Toaster** — don't `toast()` on public pages without adding one first.

## Charts

`components/ui/chart.tsx` — Recharts wrapper (`ChartContainer`, `ChartTooltip`, …).

- Requires `ChartConfig` from `types/ui/chart.ts`
- `useChart()` throws if used outside `<ChartContainer />`
