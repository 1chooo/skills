---
name: justhold-i18n
description: >-
  JustHold next-intl locales, message files, and admin English-only exception.
  Use when adding routes under app/[locale], editing messages/*.json, or
  wiring R2 locale keys.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
paths:
  - "app/[locale]/**/*"
  - "messages/**/*.json"
  - "i18n/**/*"
---

# JustHold i18n

- Locales: `zh-hant` (default/primary) and `en`, defined in `i18n/routing.ts` with `localePrefix: 'always'` (e.g. `/zh-hant/holdings`, `/en/holdings`).
- Public app routes live under `app/[locale]/…`. UI copy lives in `messages/zh-hant.json` and `messages/en.json` — add keys there; don't hardcode user-facing strings in components when a message namespace already exists.
- Prefer Traditional Chinese first; ship `en` when a translation is ready.
- **Admin exception**: `app/admin/` is English-only (no `[locale]` segment). Don't wrap admin screens in next-intl routing.
- Content/R2 locale keys must match routing: `zh-hant` and `en` (see `lib/r2.ts` and the `justhold-mdx-content` skill).
