---
name: cursor-style-desktop-demo
description: >-
  Build Cursor get-started-style landing demos: a wallpaper desktop stage with
  overlapping product windows, click-to-raise z-order, and a manual-first Play
  script. Use when creating interactive marketing showcases, multi-window product
  previews, or cursor.com/get-started-like landing demos.
license: MIT
metadata:
  author: 1chooo
  version: "1.1.0"
---

# Cursor-style landing desktop demo

Build an interactive product showcase like [cursor.com/get-started](https://cursor.com/get-started): a **desktop stage** (wallpaper + overlapping windows) where visitors can poke around manually, or press **Play** to watch a scripted walkthrough.

## What you are building

1. A wide **stage** with atmospheric wallpaper
2. Two (or more) **floating windows** that overlap on desktop, stack on mobile
3. **Click-to-raise** focus between windows
4. A **Play** control that resets state and runs a timed script (type, click, commit…)
5. **Manual-first** — no autoplay on load; any user input pauses the script

Implement this composition + interaction model for whatever product surface the user specifies (editor + preview, chat + settings, input tool + result dashboard, etc.).

## When to use this vs a simple product frame

| Pattern | Use for |
|---------|---------|
| **Desktop stage** (this skill) | Multi-surface product story. Overlapping windows, scripted Play, user can interact. |
| **Single product chrome frame** | One live widget in a simple chrome border. No multi-window choreography. |

Do **not** nest a desktop stage inside a single product frame / card.

## File layout

```
components/desktop-demo/
  desktop-stage.tsx       # wallpaper, overlap, Play, z-order
  demo-window.tsx         # traffic-light chrome
  use-demo-playback.ts    # timer loop + highlights
  demo-script.ts          # pure steps + SCRIPT_* constants
XxxDesktopDemo.tsx        # product leaf orchestrator only
```

## Layer architecture

```
page section (copy + CTA above, wider stage below)
  └─ XxxDesktopDemo            # domain state, reset / skipToEnd
       ├─ useDemoPlayback      # runs pure script; calls handlers
       ├─ buildDemoScript      # pure steps + timings (unit-testable)
       └─ DesktopStage
            ├─ DemoWindow main      # result surface
            └─ DemoWindow satellite # input / tool surface
```

| Layer | Owns | Must not own |
|-------|------|--------------|
| **Script** | Step list, timings, digit/key helpers | React, DOM, product UI |
| **Playback hook** | `idle \| playing \| paused \| finished`, highlights, `activeWindow` | Domain data |
| **Orchestrator** | Seed data, reset / skip-to-end, UI feedback | Layout geometry / wallpaper |
| **Stage + chrome** | Overlap, z-order, Play, traffic lights | Business logic |

## Composition rules

1. **One stage, overlapping windows (md+)** — main wide top-start; satellite narrower bottom-end; equal-ish left/right wallpaper margins (~5–7%). Mobile: vertical stack, no overlap.
2. **Wallpaper is atmosphere** — theme tokens / CSS variables; no hardcoded brand hex.
3. **Window chrome** — traffic lights + centered mono title; active ring; content below title bar.
4. **Manual-first** — start `idle`; show Play whenever not `playing`; never autoplay on mount.
5. **Play = reset + script** — `play()` resets to seed, then runs the script. User pointer/key during play → `pause()`.
6. **Script drives focus; click raises** — playing: follow `activeWindow`; idle/paused: user raise; finished: land on **main**.
7. **Visible keyframes** — highlight the control being “pressed”; flash new result rows / pulse totals after climax.
8. **`prefers-reduced-motion: reduce`** — skip the timer loop; `onSkipToEnd()` once; mark `finished` on main.

## Concrete recipes

### Stage geometry + wallpaper + z-order

```tsx
// DesktopStage — layout only. Prefer motion/react for entrance + Play presence; CSS-only is OK.
type Front = 'main' | 'satellite'

// Shell: relative mx-auto w-full overflow-hidden rounded-2xl border border-border shadow-sm
//         min-h-112 md:aspect-16/10 md:min-h-0 md:max-h-176
// Wallpaper (pointer-events-none absolute inset-0), default when !wallpaperClassName:
//   bg-muted/60
//   radial at 15% 20%: color-mix(in oklch, var(--primary) 28%, transparent)
//   radial at 85% 75%: color-mix(in oklch, var(--primary) 16%, transparent)
//   radial at 55% 45%: color-mix(in oklch, var(--background) 35%, transparent)
// Bottom fade: linear-gradient to bottom → color-mix(in oklch, var(--background) 45%, transparent)
// Mobile: flex flex-col gap-4 p-3 md:hidden (satellite then main)
// Desktop cluster: absolute inset-y-[5%] inset-x-[5%] lg:inset-x-[7%]
//   main:      absolute top-0 left-0 w-[76%] lg:w-[78%]  → front ? z-20 : z-10
//   satellite: absolute right-0 bottom-0 w-[30%] lg:w-[28%] → front ? z-20 : z-10
// Raise: onPointerDownCapture → onFrontChange(id) + onUserRaise?.()
// Hold raise callbacks in refs so handlers stay fresh.
// Entrance (optional motion): main y:18→0 0.5s; satellite y:28→0 0.55s delay 0.14;
//   ease [0.22, 1, 0.36, 1]
// Play pill: absolute inset-x-0 bottom-3 md:bottom-4 z-30; show when showPlay
//   rounded-full border bg-background/90 px-4 py-2 text-sm shadow-lg backdrop-blur-sm
```

### Window chrome

```tsx
function DemoWindow({ title, active, children }: {
  title: string; active?: boolean; children: React.ReactNode
}) {
  // rounded-xl border bg-card/95 shadow-2xl backdrop-blur-md
  // active ? ring-1 ring-primary/25 : border-border/80
  // title bar: three size-2.5 rounded-full bg-border dots
  // title: absolute-centered font-mono text-[11px] text-muted-foreground
  // body: border-t border-border/60 bg-background
}
```

### Single product chrome frame (not this skill’s stage)

Use when the story is **one** widget only:

```tsx
// muted outer frame p-1.5 rounded-xl border bg-muted/60
// optional traffic dots + mono title
// inner rounded-lg border bg-background → children
// Never wrap DesktopStage inside this.
```

### Pure script

```ts
type Step =
  | { kind: 'setMode'; mode: string; durationMs: number }
  | { kind: 'pressKey'; key: string; durationMs: number }
  | { kind: 'commit'; payload: unknown; durationMs: number }
  | { kind: 'idle'; durationMs: number }

export const SCRIPT_PRIMARY_AMOUNT = 25 // keep skipToEnd in sync
const KEY_MS = 280, TYPE_MS = 700, COMMIT_MS = 1100, IDLE_MS = 500

export function buildDemoScript(): Step[] {
  return [
    { kind: 'idle', durationMs: 400 },
    { kind: 'setMode', mode: 'primary-action', durationMs: TYPE_MS },
    // Prefer multiple pressKey steps so the UI animates clearly (e.g. "25.00")
    { kind: 'pressKey', key: '2', durationMs: KEY_MS },
    { kind: 'pressKey', key: '5', durationMs: KEY_MS },
    { kind: 'pressKey', key: '.', durationMs: KEY_MS },
    { kind: 'pressKey', key: '0', durationMs: KEY_MS },
    { kind: 'pressKey', key: '0', durationMs: KEY_MS },
    { kind: 'commit', payload: { amount: SCRIPT_PRIMARY_AMOUNT }, durationMs: COMMIT_MS },
    { kind: 'idle', durationMs: IDLE_MS },
    // …second beat (another mode → type → commit)
    { kind: 'idle', durationMs: 600 },
  ]
}

// Export digit applicator (applyDemoKey) + helpers that list commit order for unit tests.
```

### Playback hook

```ts
type Status = 'idle' | 'playing' | 'paused' | 'finished'

function useDemoPlayback(handlers: {
  onSetMode: (mode: string) => void
  onPressKey: (key: string) => void
  onCommit: (payload: unknown) => void
  onReset: () => void
  onSkipToEnd: () => void
}) {
  // state: status, highlightedKey, highlightCommit, activeWindow: 'main'|'satellite', generation
  // showPlay = status !== 'playing'
  // play() → onReset(); clear highlights; activeWindow='satellite'; status='playing'; generation++
  // pause() → if playing → paused; clear highlights
  // effect deps: [status, generation] — cancel timeouts on cleanup / generation bump
  // if prefers-reduced-motion → onSkipToEnd(); activeWindow='main'; finished; return
  // initial delay ~200ms, then for each step:
  //   commit → activeWindow='main' + highlightCommit
  //   other non-idle → activeWindow='satellite' + highlightedKey when pressKey
  //   apply handler; wait durationMs
  // on last step → clear highlights; activeWindow='main'; finished
}
```

### Orchestrator + page section

```tsx
function XxxDesktopDemo() {
  // seed realistic demo data (not empty)
  // resetState() → seed; skipToEnd() → final SCRIPT_* commits (no animation)
  // while playing: frontWindow = playback.activeWindow
  // when finished: frontWindow = 'main'
  // manual handlers: if playing → pause(); then mutate
  // after manual commit: raise main
  return (
    <DesktopStage
      frontWindow={frontWindow}
      onFrontChange={setFrontWindow}
      onUserRaise={() => { if (status === 'playing') pause() }}
      showPlay={showPlay}
      onPlay={play}
      mainWindow={<DemoWindow title="…" active={frontWindow === 'main'}>{/* result */}</DemoWindow>}
      satelliteWindow={<DemoWindow title="…" active={frontWindow === 'satellite'}>{/* input */}</DemoWindow>}
    />
  )
}

// Page: copy band max-w-6xl; stage band wider max-w-[90rem] so wallpaper margins breathe
<section className="overflow-hidden border-b border-border">
  <div className="mx-auto max-w-6xl px-4 md:px-6">{/* heading + sentence + CTA */}</div>
  <div className="mx-auto max-w-[90rem] px-4 md:px-8">
    <XxxDesktopDemo />
  </div>
</section>
```

### Unit tests (pure script)

```ts
expect(demoScriptKinds()).toEqual([
  'idle', 'setMode', 'pressKey', /* … */, 'commit', 'idle', /* second beat… */
])
expect(demoScriptCommitOrder()).toEqual([
  { amount: SCRIPT_PRIMARY_AMOUNT },
  // …
])
expect(applyDemoKey('0', '2')).toBe('2')
```

## Implementation checklist

- [ ] Seed realistic demo data
- [ ] `resetState` restores seed; `skipToEnd` matches final scripted outcome
- [ ] Manual interactions pause playback
- [ ] Climax / finish brings the result window to front
- [ ] Play label + window titles + stage `aria-label` are accessible (localize if the app does)
- [ ] Pure script helpers are unit-tested
- [ ] Reduced-motion path skips the timer loop
- [ ] Theme tokens for wallpaper/chrome — no one-off brand hex dumps
- [ ] Portable names: `DesktopStage`, `DemoWindow`, `useDemoPlayback`, `buildDemoScript`, `main` / `satellite`

## Anti-patterns

- Autoplaying on mount
- Nesting the stage in a card / single product chrome frame
- Timers and step lists living inside the orchestrator UI component
- Inset side-panel “hero” media — the stage should read as a full visual plane in its section
- Decorative motion with no state change (noise without keyframes)
- Skipping `prefers-reduced-motion`
- Product-leaky window ids in shared primitives (`calculator`, feature slugs) — keep `main` / `satellite`
