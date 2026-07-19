# MDX content conventions

## Article sections

| Section | MDX path | URL |
|---------|----------|-----|
| Thoughts | `app/(home)/(article)/thoughts/_articles/` | `/thoughts/[slug]` |
| Notes | `app/(home)/(article)/notes/_articles/` | `/notes/[slug]` |
| Episodes | `app/(home)/(article)/episodes/_articles/` | `/episodes/[slug]` |
| Drafts | `app/(home)/(article)/drafts/_articles/` | `/drafts/[slug]` |

`_articles/` is a private folder (not a URL segment). Slug = filename without `.mdx` (kebab-case).

## Frontmatter

Export a JS object — not YAML frontmatter:

```mdx
export const metadata = {
  title: 'AI',
  date: '2026.05.23',
  description: '...',
}
```

| Field | Required | Notes |
|-------|----------|-------|
| `title` | yes | Display title |
| `date` | yes | `YYYY.MM.DD` format |
| `description` | yes | SEO / OG description |
| `chinese: true` | no | Adds `font-zh` + `lang="zh-Hans"` on slug page |
| `draft: true` | no | Excluded from article listings |

## Scaffolding new content

```bash
pnpm create-thoughts title="My Post" date=2026.02.28 description="Some desc"
pnpm create-episode  title="Trip to Tokyo" date=2026.02.28 description="Tokyo, Japan" [slug=tokyo]
```

Notes and drafts are created manually in their `_articles/` folders.

## Slug pages

Each section has `[slug]/page.tsx` that:

1. Awaits `params: Promise<{ slug: string }>` (Next.js 16 async params)
2. Dynamic-imports `../_articles/${slug}.mdx`
3. Exports `generateStaticParams()` reading the `_articles/` directory
4. Exports `generateMetadata()` from imported `metadata`

Directory constants live in `lib/path.ts` (`THOUGHT_DIRECTORY`, `NOTE_DIRECTORY`, etc.).

## MDX component map

All custom components are registered in `mdx-components.tsx` at the **repo root** (not under `app/`):

- HTML overrides (headings, links, tables, code with Shiki + twoslash)
- Article demos via `next/dynamic` for code-splitting
- Shared components: `BlockSideTitle`, `FootNote`, `FootNoteRef`, `ArticleTable`, etc.

## Adding a new interactive demo

1. Create `components/articles/<topic>/demo-*.tsx` (see [article-components.md](article-components.md))
2. Export from `components/articles/<topic>/index.ts`
3. Re-export in `components/articles/index.ts`
4. Add `dynamic()` import in `mdx-components.tsx`
5. Register in the `components` map at the bottom of `mdx-components.tsx`
6. Use in MDX: `<DemoMyFeature />`

Heavy demos must use `dynamic()` — do not import article components directly in `mdx-components.tsx`.

## MDX prose patterns

- **Footnotes:** `<FootNoteRef number={1} />` in text, `<FootNote number={1}>...</FootNote>` at bottom
- **Side annotations:** `<BlockSideTitle title="...">` for small inline visuals only — **not** for two-column demos
- **Local images:** Place in `assets/images/`, reference as `![alt](filename.png)`
- **Math:** `<InlineMath />` / `<BlockMath />` (KaTeX)

## LLM routes

- `app/(llms)/llms.txt/route.ts` — site index
- `app/(llms)/llms-full.txt/route.ts` — full content dump
- `app/(llms)/llm/[[...slug]]/route.ts` — per-page markdown

Metadata is parsed via regex from MDX source (same pattern as article listings).
