# 1chooo/skills

[![skills.sh](https://skills.sh/b/1chooo/skills)](https://skills.sh/1chooo/skills)

Skills by [1chooo](https://github.com/1chooo) for building [1chooo.com](https://1chooo.com) and [JustHold](https://github.com/1chooo/justhold) with AI agents — Next.js, Supabase, MDX, and the rest of the stack.

- skills.sh: [1chooo/skills](https://www.skills.sh/1chooo/skills)
- Browse on the site: [1chooo.com/skills](https://1chooo.com/skills)
- Background: [AI Agent](https://1chooo.com/thoughts/ai-agent)
- Page layout: [`skills.sh.json`](./skills.sh.json) groups skills on the skills.sh repo page

## Install

```bash
npx skills add 1chooo/skills
```

Or specific skills:

```bash
npx skills add 1chooo/skills --skill api-routes --skill mdx-content
```

1chooo.com (Cursor + Copilot):

```bash
npx skills add 1chooo/skills \
  --skill 1chooo-project \
  --skill nextjs-app-router \
  --skill api-routes \
  --skill types-and-constants \
  --skill utils-auth-supabase \
  --skill mdx-content \
  --skill article-components \
  --skill components-ui \
  --skill services-ui \
  --skill services-confirm-toasts \
  --skill admin-ui \
  -a cursor \
  -a github-copilot \
  -y
```

JustHold (Cursor + Copilot):

```bash
npx skills add 1chooo/skills \
  --skill justhold-mdx-content \
  --skill justhold-testing \
  -a cursor \
  -a github-copilot \
  -y
```

## Skills

### 1chooo.com

| Skill | When to use |
|-------|-------------|
| [`1chooo-project`](./1chooo-project) | Stack, route groups, code placement, imports, design system |
| [`nextjs-app-router`](./nextjs-app-router) | Server/Client split, async params, `proxy.ts`, workspace shells |
| [`api-routes`](./api-routes) | REST handlers, Zod, `requireUser()`, response shapes |
| [`types-and-constants`](./types-and-constants) | `types/` vs `lib/`, NavItem collision |
| [`utils-auth-supabase`](./utils-auth-supabase) | Supabase clients, cookies, CORS, auth helpers |
| [`mdx-content`](./mdx-content) | Articles, frontmatter, demos, LLM routes |
| [`article-components`](./article-components) | Two-column demos, rurikon article UI |
| [`components-ui`](./components-ui) | Radix, Dialog, ConfirmDialog, Sonner, Charts |
| [`services-ui`](./services-ui) | Lowercase copy, mono buttons, workspace shells |
| [`services-confirm-toasts`](./services-confirm-toasts) | Destructive confirms, toast rules |
| [`admin-ui`](./admin-ui) | Square admin tab switchers |

### JustHold

| Skill | When to use |
|-------|-------------|
| [`justhold-mdx-content`](./justhold-mdx-content) | JustHold MDX posts — admin CMS, Supabase metadata, R2 bodies |
| [`justhold-testing`](./justhold-testing) | JustHold unit, integration, and e2e tests (Vitest / Playwright) |

## Stack these skills target

- Next.js 16 App Router & React 19
- TypeScript, Tailwind CSS v4, MDX, Supabase
- Cursor / Claude Code / Codex and other agents via the [skills](https://www.skills.sh/) CLI
