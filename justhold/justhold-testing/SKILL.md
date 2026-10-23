---
name: justhold-testing
description: >-
  Write and run JustHold unit, integration, and e2e tests. Use when adding
  Vitest/Playwright tests, covering lib/ helpers, RLS integration, or updating
  coverage include lists.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
paths:
  - "tests/**/*"
  - "lib/**/*.ts"
  - "vitest.config.ts"
  - "playwright.config.ts"
---

# JustHold Testing

## When to Use

- Adding or updating unit tests for pure `lib/` helpers
- Writing Supabase RLS / RPC integration tests
- Extending Playwright e2e flows
- Updating Vitest `coverage.include` after solidly covering a pure module

## Instructions

### Layers

| Layer | Location | Runner | Use for |
|-------|----------|--------|---------|
| Unit | `tests/unit/` | Vitest (node) | Pure helpers in `lib/` — no DB/network |
| Integration | `tests/integration/` | Vitest + local Supabase | RLS, RPCs, DB behaviour |
| E2E | `tests/e2e/` | Playwright | Critical user flows |

### Scripts

- `pnpm test` / `pnpm test:watch` — unit
- `pnpm test:coverage` — unit + coverage thresholds (CI)
- `pnpm test:int` — integration (local only)
- `pnpm test:e2e` — Playwright (local only)

### Unit tests

- Prefer pure, stable domain logic in `lib/` (money FIFO, hold-days, formatters, sort keys).
- Mirror fixture style from `tests/unit/money.test.ts` and `tests/unit/holdings-math.test.ts`.
- `@/` aliases work; `server-only` / `next/headers` are stubbed via `tests/stubs/` (see `vitest.config.ts`).
- Do not invent new test roots (`__tests__`, colocated `*.test.ts` next to source).

### Coverage

- `coverage.include` in `vitest.config.ts` lists only fully-pure, well-tested modules so thresholds stay meaningful.
- After solidly covering a new pure module, add its path to that include list.

### Integration / e2e

- Integration uses `.env.test.local` + skip guards when no test DB.
- Prefer existing helpers under `tests/helpers/` over ad-hoc Supabase clients.
- E2E reuses the app via Playwright config; keep specs serial and focused on signup / holdings gating patterns already in `tests/e2e/`.
