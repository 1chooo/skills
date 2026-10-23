---
name: justhold-mdx-content
description: >-
  Author and wire JustHold MDX blog posts via admin CMS, Supabase metadata,
  and R2 bodies. Use when editing posts, MDX components, R2 keys, or the
  contents admin UI.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
paths:
  - "app/admin/contents/**/*"
  - "lib/mdx/**/*"
  - "lib/posts.ts"
  - "lib/r2.ts"
---

# JustHold MDX Content (CMS)

## When to Use

- Editing the admin contents UI or post actions
- Changing MDX component registry or article rendering
- Updating R2 post keys, locales, or `lib/posts` metadata loading

## Instructions

- Posts are authored in `/admin/contents` (Monaco + live preview). Metadata lives in Supabase (`posts` + `post_locales`); MDX bodies live in Cloudflare R2.
- R2 object layout (see `lib/r2.ts`): `posts/<slug>/<locale>/content.mdx` where `locale` is `zh-hant` or `en`.
- Shared fields on `posts`: slug, category, author, status, featured, published_at.
- Locale-specific fields on `post_locales`: title, excerpt, reading_time. Body is only in R2.
- Public pages load published posts via `lib/posts.ts` + `lib/mdx/render` (same component registry as the admin preview).
- Do not hardcode title/date/category in the MDX body — the public page renders `<ArticleHeader post={...} />` from Supabase metadata.
- All markdown elements are styled via `lib/mdx/components.tsx` — don't add custom className overrides per-post.
- Use `<Sidenote>text</Sidenote>` for tangential commentary/citations (Tufte-style margin note on wide screens, inline toggle on mobile).
- Prefer Traditional Chinese (`zh-hant`) as the primary locale; add `en` when a translation is ready.
