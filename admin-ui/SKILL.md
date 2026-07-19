---
name: admin-ui
description: Admin UI button and tab switcher conventions — plain buttons, square corners, no Radix Tabs, consistent active/inactive states
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# Admin Button & Tab Switcher Style

All tab switchers, period selectors, and view toggles in `app/(admin)/` must use plain `<button>` elements — **never** Radix `TabsTrigger`/`TabsList`. Radix `TabsContent` with its base styles causes active-state overrides to fail silently.

## Pattern

```tsx
// ✅ GOOD — plain buttons, consistent active/inactive
<div className="flex gap-1">
  {(['bar', 'pie', 'table'] as const).map((v) => (
    <button
      key={v}
      type="button"
      onClick={() => setView(v)}
      className={cn(
        'px-2 py-0.5 text-xs font-medium lowercase border transition-colors',
        view === v
          ? 'bg-rurikon-800 text-white border-rurikon-600'
          : 'border-rurikon-border text-rurikon-400 hover:border-rurikon-400 hover:text-rurikon-600',
      )}
    >
      {v}
    </button>
  ))}
</div>

// ❌ BAD — Radix TabsTrigger overrides are unreliable
<TabsList className="...">
  <TabsTrigger className="data-[state=active]:bg-rurikon-800 ...">bar</TabsTrigger>
</TabsList>
```

## Rules

- **No `rounded` or `rounded-sm`** — buttons are square-cornered (unlike services).
- **Active state**: `bg-rurikon-800 text-white border-rurikon-600`
- **Inactive state**: `border-rurikon-border text-rurikon-400 hover:border-rurikon-400 hover:text-rurikon-600`
- **Base classes**: `border font-medium lowercase transition-colors` + size padding (`px-2 py-0.5` for compact, `px-2 py-1` for normal).
- **Content switching**: Use `useState` + `{view === 'bar' && <div>…</div>}` instead of `TabsContent`.
- **Outer frame** (optional): Wrap the button group in `<div className="flex gap-1 border border-rurikon-border p-0.5">` when a grouped appearance is needed.

## Reference

Canonical reference: `app/(admin)/admin/_components/photos/photos-manager-tabs.tsx`
