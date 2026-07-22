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
  version: "1.0.0"
---

# Cursor-style landing desktop demo

Build an interactive product showcase like [cursor.com/get-started](https://cursor.com/get-started): a **desktop stage** (wallpaper + overlapping windows) where visitors can poke around manually, or press **Play** to watch a scripted walkthrough.

## What you are building

A marketing section that sells the product by **showing it in context**, not as an inset screenshot:

1. A wide **stage** with atmospheric wallpaper
2. Two (or more) **floating windows** that overlap on desktop, stack on mobile
3. **Click-to-raise** focus between windows
4. A **Play** control that resets state and runs a timed script (type, click, record…)
5. **Manual-first** — no autoplay on load; any user input pauses the script

The agent’s job is to implement this composition + interaction model for whatever product surface the user specifies (calculator + dashboard, editor + preview, chat + settings, etc.).

## When to use this vs a simple product frame

| Pattern | Use for |
|---------|---------|
| **Desktop stage** (this skill) | Multi-surface product story. Overlapping windows, scripted Play, user can interact. |
| **Single product frame** | One live widget in a simple chrome border. No multi-window choreography. |

Do **not** nest a desktop stage inside a single product frame / card.

## Layer architecture

Keep four layers separate:

```
page section (copy + CTA above, wider stage below)
  └─ DemoOrchestrator          # owns demo domain state
       ├─ useDemoPlayback      # runs pure script; calls handlers
       ├─ demoScript           # pure steps + timings (unit-testable)
       └─ DesktopStage         # wallpaper, layout, Play, z-order
            ├─ DemoWindow A    # primary surface (result / main app)
            └─ DemoWindow B    # satellite surface (input / tool)
```

| Layer | Owns | Must not own |
|-------|------|--------------|
| **Script** | Step list, timings, digit/key helpers | React, DOM, product UI |
| **Playback hook** | `idle \| playing \| paused \| finished`, highlights, which window the script wants focused | Domain data (entries, amounts, form fields) |
| **Orchestrator** | Seed data, reset / skip-to-end, wiring handlers → UI feedback | Layout geometry / wallpaper |
| **Stage + window chrome** | Overlap layout, z-order, Play visibility, traffic-light chrome | Business logic |

## Composition rules

1. **One stage, overlapping windows (md+)**  
   Primary window: wide, top-start. Satellite: narrower, bottom-end. Leave equal-ish left/right wallpaper margins (~5–7%). Mobile: vertical stack, no overlap.

2. **Wallpaper is atmosphere, not content**  
   Layered soft gradients (or a photo later). Prefer theme tokens / CSS variables over hardcoded brand hex.

3. **Window chrome**  
   Traffic lights + centered title. Active window gets a subtle focus ring. Content lives below the title bar.

4. **Manual-first**  
   Start `idle`. Show Play whenever not `playing`. Never autoplay on mount.

5. **Play = reset + script**  
   `play()` resets to seed, then runs the script. Any user pointer/key during play → `pause()`.

6. **Script drives focus; click raises**  
   - Playing: follow playback’s `activeWindow` (satellite while “typing”; primary on climax / finish).  
   - Idle/paused: user click raises a window. Raising while playing also pauses.  
   - Finished: land on the primary window so the result is visible.

7. **Visible keyframes**  
   Highlight the control the script is “pressing” (key, button). Briefly flash new result rows / pulse totals after climax actions.

8. **`prefers-reduced-motion: reduce`**  
   Skip the timer loop; jump once to the final scripted state and mark `finished`.

## Pseudo-code

### Pure script

```ts
type Step =
  | { kind: 'setMode'; mode: string; durationMs: number }
  | { kind: 'pressKey'; key: string; durationMs: number }
  | { kind: 'commit'; payload: unknown; durationMs: number }
  | { kind: 'idle'; durationMs: number }

function buildDemoScript(): Step[] {
  return [
    { kind: 'idle', durationMs: 400 },
    { kind: 'setMode', mode: 'primary-action', durationMs: 700 },
    // Prefer multiple pressKey steps so the UI animates clearly
    { kind: 'pressKey', key: '2', durationMs: 280 },
    { kind: 'pressKey', key: '5', durationMs: 280 },
    { kind: 'commit', payload: { /* final value */ }, durationMs: 1100 },
    { kind: 'idle', durationMs: 500 },
    // …second beat (another mode → type → commit)
    { kind: 'idle', durationMs: 600 },
  ]
}
```

Export named constants for scripted amounts/values so `skipToEnd` and the script stay in sync. Unit-test the pure script (order of commits, keys that build the amount).

### Playback hook

```ts
function useDemoPlayback(handlers: {
  onSetMode: (mode: string) => void
  onPressKey: (key: string) => void
  onCommit: (payload: unknown) => void
  onReset: () => void
  onSkipToEnd: () => void
}) {
  // status: idle | playing | paused | finished
  // showPlay = status !== 'playing'
  // play() → onReset(); status = 'playing'; bump generation
  // pause() → if playing → paused; clear highlights
  // while playing:
  //   if prefers-reduced-motion → onSkipToEnd(); finished; return
  //   for each step: apply handler, set activeWindow + highlights, wait durationMs
  //   on last step → activeWindow = 'main'; finished
}
```

Focus heuristic while running:

- `commit` → focus **main** (result surface)
- other interactive steps → focus **satellite** (input surface)
- after script ends → **main**

### Stage (layout only)

```tsx
function DesktopStage({
  mainWindow,
  satelliteWindow,
  frontWindow,      // controlled: 'main' | 'satellite'
  onFrontChange,
  onUserRaise,      // fire only on user click (orchestrator uses to pause)
  showPlay,
  onPlay,
  wallpaperClassName,
}) {
  // rounded frame, aspect ~16/10 on md+
  // wallpaper layers (pointer-events: none)
  // md+: absolute overlap cluster with ~5–7% inset margins
  // mobile: flex column stack
  // Play pill centered at bottom when showPlay
  // pointerDown on a window → onFrontChange + onUserRaise
}
```

### Window chrome

```tsx
function DemoWindow({ title, active, children }) {
  // traffic lights · centered mono title
  // active ? subtle primary ring : quieter border
  // children in body below title bar
}
```

### Orchestrator

```tsx
function ProductDesktopDemo() {
  // seed realistic demo data (not empty)
  // domain state: items, input digits/fields, frontWindow, highlight pulses
  // resetState() → seed
  // skipToEnd() → apply final scripted commits without animation
  // wire useDemoPlayback handlers
  // while playing: frontWindow follows playback.activeWindow
  // when finished: frontWindow = 'main'
  // manual handlers: if playing → pause(); then mutate state
  // after manual commit: raise main so the result is visible
  return (
    <DesktopStage
      frontWindow={frontWindow}
      onFrontChange={setFrontWindow}
      onUserRaise={() => { if (playing) pause() }}
      showPlay={showPlay}
      onPlay={play}
      mainWindow={<DemoWindow title="…">{/* result UI */}</DemoWindow>}
      satelliteWindow={<DemoWindow title="…">{/* input UI */}</DemoWindow>}
    />
  )
}
```

### Page section

```tsx
<section className="overflow-hidden /* section border */">
  <div className="/* normal marketing max-width */">
    {/* heading + one supporting sentence + CTA */}
  </div>
  <div className="/* wider max-width so wallpaper margins breathe */">
    <ProductDesktopDemo />
  </div>
</section>
```

## Implementation checklist

- [ ] Seed realistic demo data
- [ ] `resetState` restores seed; `skipToEnd` matches final scripted outcome
- [ ] Manual interactions pause playback
- [ ] Climax / finish brings the result window to front
- [ ] Play label + window titles + stage `aria-label` are localized / accessible
- [ ] Pure script helpers are unit-tested
- [ ] Reduced-motion path skips the timer loop
- [ ] Theme tokens for wallpaper/chrome — no one-off brand hex dumps

## Anti-patterns

- Autoplaying on mount
- Nesting the stage in a card / single product frame
- Timers and step lists living inside the orchestrator UI component
- Inset side-panel “hero” media — the stage should read as a full visual plane in its section
- Decorative motion with no state change (noise without keyframes)
- Skipping `prefers-reduced-motion`

## Naming guidance

Use product-agnostic names in new code (`DesktopStage`, `DemoWindow`, `useDemoPlayback`, `buildDemoScript`) so the pattern stays portable. Name the orchestrator after the product only at the leaf (`XxxDesktopDemo`).
