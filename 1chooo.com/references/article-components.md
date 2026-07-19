# Article component conventions

Reference implementations: `components/articles/css-layout/` (`DemoBoxModel`, `DemoFlexbox`, `DemoCSSGrid`, `DemoPosition`).

Make components artistic and minimalistic. Use the **rurikon** palette.

## Simple (non-interactive) components

```tsx
<div className="my-8 border border-rurikon-border bg-[#fefefe] font-serif">
</div>
```

## Two-column interactive layout

```
┌─────────────────────────────┬─────────────────┐
│  Preview / visualisation    │  Controls /      │
│  area (flex-1)              │  CSS output      │
│                             │  (lg:w-85)       │
├─────────────────────────────┴─────────────────┤
│  Footer bar (label + action buttons)           │
└────────────────────────────────────────────────┘
```

```tsx
<div className="my-8 w-full min-w-[50vw]">
  <div className="overflow-hidden border border-rurikon-border bg-[#fefefe] font-serif">
    <div className="flex flex-col lg:flex-row">

      {/* Left — preview + footer */}
      <div className="flex-1 min-w-0 lg:border-r border-rurikon-border flex flex-col">
        <div className="flex-1 flex items-center justify-center p-4 sm:p-6" style={{ minHeight: 200 }}>
          {/* visualisation */}
        </div>
        <div className="flex items-center gap-1 px-3 py-2 border-t border-rurikon-border flex-wrap">
          <span className="font-serif text-xs text-rurikon-400 mr-auto italic">label</span>
          {/* action buttons */}
        </div>
      </div>

      {/* Right — controls / info */}
      <div className="w-full lg:w-85 flex flex-col justify-center bg-[#fdfcfa]">
        <div className="p-3 flex flex-col gap-3">{/* controls */}</div>
      </div>

    </div>
  </div>
</div>
```

## Buttons

```tsx
// Toggle (active/inactive)
className={cn(
  "text-xs rounded px-2.5 py-1.5 border transition-colors",
  isActive
    ? "bg-rurikon-800 text-white border-rurikon-600"
    : "border-rurikon-border text-rurikon-400 hover:border-rurikon-400 hover:text-rurikon-600"
)}

// Monospace code-label variant
className={cn(
  "font-mono text-[11px] rounded px-2 py-1 border transition-colors",
  ...
)}
```

**Buttons use `rounded` (4px), not `rounded-xs` or `rounded-sm`.**

## Right-panel blocks

```tsx
// CSS output
<div className="font-mono text-xs bg-rurikon-50/60 border border-rurikon-border/40 p-3 text-rurikon-600 leading-6">
  <span className="text-[#5a9fd4]">property</span>: <span className="text-[#9b72d4]">{value}</span>;
</div>

// Description
<div className="border border-rurikon-border/40 bg-rurikon-50/30 px-3 py-3 flex flex-col gap-1">
  <span className="font-mono text-xs text-rurikon-700 font-medium">title</span>
  <span className="text-xs text-rurikon-500">short</span>
  <span className="text-xs text-rurikon-400 italic">detail</span>
</div>
```

## Item color palette (index order)

| Index | Colors |
|-------|--------|
| 0 blue | `bg-[#dce8f5] border-[#5a9fd4] text-[#1a5a8b]` |
| 1 green | `bg-[#dce8dc] border-[#7aab4e] text-[#3a6b1a]` |
| 2 orange | `bg-[#f5e8dc] border-[#d4a96a] text-[#8b6914]` |
| 3 purple | `bg-[#ecdcf5] border-[#9b72d4] text-[#5a1a8b]` |
| 4 pink | `bg-[#f5dce8] border-[#d45a9f] text-[#8b1a5a]` |
| 5 teal | `bg-[#dcf5f0] border-[#4ed4b8] text-[#1a8b72]` |

## File organization

```
components/articles/
├── css-layout/
│   ├── demo-box-model.tsx    # one demo per file, named export
│   └── index.ts              # barrel: export { DemoBoxModel } from './demo-box-model'
├── dsa/
│   └── index.ts
└── index.ts                  # re-exports all topics for mdx-components.tsx
```

- Topic folder name matches the article slug or subject (`css-layout`, `dsa`, `tcp`, …).
- Demo files use `demo-<name>.tsx`; export `Demo<Name>` (PascalCase).
- Use **named exports** (`export function DemoBoxModel()`), not default exports.
- Register new demos in `mdx-components.tsx` via `dynamic()`.

## Rules

- Add `"use client"` when using state.
- Use `cn()` from `@/lib/utils` for conditional classes.
- Always use `@/` alias for imports — never relative `../` paths.
- **Do NOT wrap two-column demos in `<BlockSideTitle>`.** It collapses the flex shell. Place demos bare in MDX.
