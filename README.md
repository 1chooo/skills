# skills

Cursor Agent Skills I wrote while building [1chooo.com](https://1chooo.com) with AI — conventions for Next.js, Supabase, MDX, and the rest of the stack.

- Browse and copy on the site: [1chooo.com/skills](https://1chooo.com/skills)
- Raw markdown for LLMs: `/llm/skills/<slug>` on [1chooo.com](https://1chooo.com)
- Background: [AI Agent](https://1chooo.com/thoughts/ai-agent)

## Skills

| Skill | When to use |
|-------|-------------|
| `1chooo-project` | Starting any task — stack, route groups, where to put code |
| `nextjs-app-router` | `app/**`, `proxy.ts` — Server/Client, async params, auth guards |
| `api-routes` | `app/api/**` — REST handlers, Zod, Supabase, response shapes |
| `types-and-constants` | `types/**`, `lib/**` — types vs runtime constants |
| `utils-auth-supabase` | `utils/**`, `proxy.ts` — auth helpers, Supabase clients, CORS |
| `mdx-content` | Article MDX — frontmatter, scaffolding, demo registration |
| `article-components` | `components/articles/**` — interactive demo layout |
| `components-ui` | `components/ui/**` — Radix, Dialog, ConfirmDialog, Charts |
| `services-ui` | `app/(services)/**` — lowercase copy, workspace shells |
| `services-confirm-toasts` | Services destructive actions — ConfirmDialog, Sonner |
| `admin-ui` | `app/(admin)/**` — plain button tab switchers |

## Install

Copy a skill folder into your project:

```text
.cursor/skills/<skill-name>/SKILL.md
```

Example:

```bash
mkdir -p .cursor/skills/api-routes
curl -o .cursor/skills/api-routes/SKILL.md \
  https://raw.githubusercontent.com/1chooo/skills/main/api-routes/SKILL.md
```

Or clone this repo and copy the folders you need.

## Stack these skills target

- Next.js 16 App Router & React 19
- TypeScript, Tailwind CSS v4, MDX, Supabase
- Cursor Agent Skills + project rules (`.cursor/rules/`)

This repo shares **how** the site was built with an AI agent — not the application source.
