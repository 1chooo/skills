# 1chooo/skills

[![skills.sh](https://skills.sh/b/1chooo/skills)](https://www.skills.sh/1chooo)

Skills by [1chooo](https://github.com/1chooo), divided by source. Each source is its own repository and has its own categories, same shape as [vercel-labs](https://www.skills.sh/vercel-labs).

- Org page: [skills.sh/1chooo](https://www.skills.sh/1chooo)
- Site: [1chooo.com/skills](https://1chooo.com/skills)
- Background: [AI Agent](https://1chooo.com/thoughts/ai-agent)

This repository is the index. Install a source from the table.

## Sources

| Source | Categories | Install |
|--------|------------|---------|
| [dev-habits](https://github.com/1chooo/dev-habits) | Commits | `npx skills add 1chooo/dev-habits` |
| [agent-workflow](https://github.com/1chooo/agent-workflow) | Context | `npx skills add 1chooo/agent-workflow` |
| [web](https://github.com/1chooo/web) | Stack, Data, UI | `npx skills add 1chooo/web` |
| [site](https://github.com/1chooo/site) | Foundation, Routing, Data, Content, UI | `npx skills add 1chooo/site` |
| [justhold-skills](https://github.com/1chooo/justhold-skills) | Foundation, Design, Content, Testing | `npx skills add 1chooo/justhold-skills` |

`1chooo/justhold` is the private app, so the public JustHold skills live in [justhold-skills](https://github.com/1chooo/justhold-skills).

## Install

### 1chooo.com

Shared web conventions, the site skills, and the commit habit.

```bash
npx skills add 1chooo/web -a cursor -a github-copilot -y
npx skills add 1chooo/site -a cursor -a github-copilot -y
npx skills add 1chooo/dev-habits -a cursor -a github-copilot -y
```

### JustHold

```bash
npx skills add 1chooo/web -a cursor -a github-copilot -y
npx skills add 1chooo/justhold-skills -a cursor -a github-copilot -y
npx skills add 1chooo/dev-habits -a cursor -a github-copilot -y
```

### Agent workflow

```bash
npx skills add 1chooo/agent-workflow -a cursor -a github-copilot -y
```

## Where each skill lives

### dev-habits

| Skill | When to use |
|-------|-------------|
| [`1chooo-commits`](https://github.com/1chooo/dev-habits/tree/main/1chooo-commits) | Step-narrative commits; plan the chain; split diffs; commit message style |

### agent-workflow

| Skill | When to use |
|-------|-------------|
| [`1chooo-balanced-context`](https://github.com/1chooo/agent-workflow/tree/main/1chooo-balanced-context) | Exploring, debugging, reviewing, or implementing with modest token use |

### web

| Skill | When to use |
|-------|-------------|
| [`web-service-stack`](https://github.com/1chooo/web/tree/main/web-service-stack) | Stack, code placement, imports, RSC defaults, three shells |
| [`nextjs-rsc-patterns`](https://github.com/1chooo/web/tree/main/nextjs-rsc-patterns) | Server/Client split, async params, `proxy.ts`, data fetching |
| [`api-supabase-routes`](https://github.com/1chooo/web/tree/main/api-supabase-routes) | REST handlers, Zod, scoped Supabase queries, response shapes |
| [`supabase-clients`](https://github.com/1chooo/web/tree/main/supabase-clients) | server / client / service client matrix |
| [`cursor-style-desktop-demo`](https://github.com/1chooo/web/tree/main/cursor-style-desktop-demo) | Wallpaper desktop stage, overlapping windows, manual-first Play script |

### site

| Skill | When to use |
|-------|-------------|
| [`1chooo-project`](https://github.com/1chooo/site/tree/main/1chooo-project) | Stack, route groups, code placement, imports, design system |
| [`nextjs-app-router`](https://github.com/1chooo/site/tree/main/nextjs-app-router) | Server/Client split, async params, `proxy.ts`, workspace shells |
| [`api-routes`](https://github.com/1chooo/site/tree/main/api-routes) | REST handlers, Zod, `requireUser()`, response shapes |
| [`types-and-constants`](https://github.com/1chooo/site/tree/main/types-and-constants) | `types/` vs `lib/`, NavItem collision |
| [`utils-auth-supabase`](https://github.com/1chooo/site/tree/main/utils-auth-supabase) | Supabase clients, cookies, CORS, auth helpers |
| [`mdx-content`](https://github.com/1chooo/site/tree/main/mdx-content) | Articles, frontmatter, demos, LLM routes |
| [`article-components`](https://github.com/1chooo/site/tree/main/article-components) | Two-column demos, rurikon article UI |
| [`components-ui`](https://github.com/1chooo/site/tree/main/components-ui) | Radix, Dialog, ConfirmDialog, Sonner, Charts |
| [`services-ui`](https://github.com/1chooo/site/tree/main/services-ui) | Lowercase copy, mono buttons, workspace shells |
| [`services-confirm-toasts`](https://github.com/1chooo/site/tree/main/services-confirm-toasts) | Destructive confirms, toast rules |
| [`admin-ui`](https://github.com/1chooo/site/tree/main/admin-ui) | Square admin tab switchers |

### justhold-skills

| Skill | When to use |
|-------|-------------|
| [`justhold-project`](https://github.com/1chooo/justhold-skills/tree/main/justhold-project) | Stack, shells, defaults, skill index |
| [`justhold-i18n`](https://github.com/1chooo/justhold-skills/tree/main/justhold-i18n) | next-intl locales, messages, admin exception |
| [`justhold-design-tokens`](https://github.com/1chooo/justhold-skills/tree/main/justhold-design-tokens) | Palette, fonts, radius |
| [`justhold-ui-layout`](https://github.com/1chooo/justhold-skills/tree/main/justhold-ui-layout) | Base UI components + public/member/admin shells |
| [`justhold-mdx-content`](https://github.com/1chooo/justhold-skills/tree/main/justhold-mdx-content) | JustHold MDX posts — admin CMS, Supabase metadata, R2 bodies |
| [`justhold-testing`](https://github.com/1chooo/justhold-skills/tree/main/justhold-testing) | Unit, integration, and e2e tests (Vitest / Playwright) |

## Stack these skills target

- Next.js 16 App Router & React 19
- TypeScript, Tailwind CSS v4, MDX, Supabase
- Cursor / Claude Code / Codex and other agents via the [skills](https://www.skills.sh/) CLI
