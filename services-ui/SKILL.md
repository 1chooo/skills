---
name: services-ui
description: Services UI conventions for 1chooo.com — lowercase copy, monospace buttons, forms, workspace shells, and client fetch patterns
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# Services UI Conventions

Authenticated mini-apps under `app/(services)/services/`. Complements `services-confirm-toasts` skill (destructive actions + toasts).

## Copy and typography

- **Lowercase** labels, buttons, headings, and dialog copy
- **Monospace buttons:** `font-mono text-xs`
- **Form labels:** `text-xs text-rurikon-400 lowercase`
- **Inputs:** bordered, `rounded-sm`, `text-sm text-rurikon-700`, `focus:border-rurikon-400`

```tsx
const inputCls =
  'w-full border border-rurikon-border rounded-sm px-3 py-1.5 text-sm text-rurikon-700 placeholder:text-rurikon-200 focus:outline-none focus:border-rurikon-400 bg-white'

const labelCls = 'block text-xs text-rurikon-400 lowercase mb-1'
```

Reference: `app/(services)/services/stock/_components/add-transaction-dialog.tsx`

## Button states

Services use **rounded** buttons (unlike admin's square corners):

```tsx
className={cn(
  'font-mono text-xs rounded px-2.5 py-1.5 border transition-colors',
  isActive
    ? 'bg-rurikon-800 text-white border-rurikon-600'
    : 'border-rurikon-border text-rurikon-400 hover:border-rurikon-400 hover:text-rurikon-600',
)}
```

## Dialogs

| Use case | Component |
|----------|-----------|
| Destructive confirm (delete) | `ConfirmDialog` from `@/components/ui/confirm-dialog` |
| Forms (add/edit) | `Dialog` from `@/components/ui/dialog` directly |

Dialog content: `max-w-sm` for confirms, appropriate width for forms.

## Client fetch

```tsx
// Simple helper — throws on non-OK, includes credentials
import { apiFetch } from '@/lib/api-fetch'

// Or raw fetch for mutation control
const res = await fetch('/api/...', { method: 'POST', credentials: 'include', body: JSON.stringify(data) })
const json = await res.json()
if (!res.ok) { toast.error(json.error ?? 'failed'); return }
toast.success('saved')
```

Always `credentials: 'include'` for cookie auth.

## Workspace shells

Multi-view services (stock, split, jobs) use a shared shell pattern:

- `ServiceWorkspaceShell` or feature-specific shell (`JobsShell`) — `app/(services)/services/_components/workspace/`
- Wrap with `WorkspaceUIProvider` for sidebar collapse + ⌘K command palette (`useCommandPaletteShortcut`)
- Workspace nav types in `types/services/workspace/shell.ts` — **not** site `NavItem` from `types/nav.ts`

## Sign-in redirect

```tsx
if (!user) redirect('/services/sign-in?from=/services/jobs')
```

After sign-in, redirect back to the `from` path.

## Toaster placement

Sonner `<Toaster>` is in `app/(services)/layout.tsx` (`position="bottom-center"`) — not on public home pages.

## Autosave

Notes use autosave with inline status — do **not** toast on autosave. Toast only on explicit create/delete/rename actions.
