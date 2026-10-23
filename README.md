# 1chooo/skills

[![skills.sh](https://skills.sh/b/1chooo/skills)](https://skills.sh/1chooo/skills)

Skills by [1chooo](https://github.com/1chooo) for building [1chooo.com](https://1chooo.com) and [JustHold](https://github.com/1chooo/justhold) with AI agents.

One repository, divided by source. Each source has its own categories. The page layout is [`skills.sh.json`](./skills.sh.json).

- skills.sh: [1chooo/skills](https://www.skills.sh/1chooo/skills)
- Org page: [skills.sh/1chooo](https://www.skills.sh/1chooo)
- Site: [1chooo.com/skills](https://1chooo.com/skills)
- Background: [AI Agent](https://1chooo.com/thoughts/ai-agent)

## Install

```bash
npx skills add 1chooo/skills
```

Or specific skills:

```bash
npx skills add 1chooo/skills --skill api-routes --skill mdx-content
```

### Web + site + dev habits (Cursor + Copilot)

```bash
npx skills add 1chooo/skills \
  --skill web-service-stack \
  --skill nextjs-rsc-patterns \
  --skill api-supabase-routes \
  --skill supabase-clients \
  --skill cursor-style-desktop-demo \
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
  --skill 1chooo-commits \
  -a cursor \
  -a github-copilot \
  -y
```

### Web + JustHold + dev habits (Cursor + Copilot)

```bash
npx skills add 1chooo/skills \
  --skill web-service-stack \
  --skill nextjs-rsc-patterns \
  --skill api-supabase-routes \
  --skill supabase-clients \
  --skill cursor-style-desktop-demo \
  --skill justhold-project \
  --skill justhold-i18n \
  --skill justhold-design-tokens \
  --skill justhold-ui-layout \
  --skill justhold-mdx-content \
  --skill justhold-testing \
  --skill 1chooo-commits \
  -a cursor \
  -a github-copilot \
  -y
```

### Agent workflow (Cursor + Copilot)

```bash
npx skills add 1chooo/skills \
  --skill 1chooo-balanced-context \
  -a cursor \
  -a github-copilot \
  -y
```

## Sources

### Dev habits

How I write code and history.

| Skill | When to use |
|-------|-------------|
| [`1chooo-commits`](./dev-habits/1chooo-commits) | Step-narrative commits; plan the chain; split diffs; commit message style |

### Agent workflow

How far a coding agent should look, when to stop, and what to keep.

| Skill | When to use |
|-------|-------------|
| [`1chooo-balanced-context`](./agent-workflow/1chooo-balanced-context) | Exploring, debugging, reviewing, or implementing with modest token use |

### Web

Cross-project conventions for both apps.

| Category | Skill | When to use |
|----------|-------|-------------|
| Stack | [`web-service-stack`](./web/web-service-stack) | Stack, code placement, imports, RSC defaults, three shells |
| Stack | [`nextjs-rsc-patterns`](./web/nextjs-rsc-patterns) | Server/Client split, async params, `proxy.ts`, data fetching |
| Data | [`api-supabase-routes`](./web/api-supabase-routes) | REST handlers, Zod, scoped Supabase queries, response shapes |
| Data | [`supabase-clients`](./web/supabase-clients) | server / client / service client matrix |
| UI | [`cursor-style-desktop-demo`](./web/cursor-style-desktop-demo) | Wallpaper desktop stage, overlapping windows, manual-first Play script |

### Site

Conventions for 1chooo.com.

| Category | Skill | When to use |
|----------|-------|-------------|
| Foundation | [`1chooo-project`](./site/1chooo-project) | Stack, route groups, code placement, imports, design system |
| Routing | [`nextjs-app-router`](./site/nextjs-app-router) | Server/Client split, async params, `proxy.ts`, workspace shells |
| Routing | [`api-routes`](./site/api-routes) | REST handlers, Zod, `requireUser()`, response shapes |
| Data | [`types-and-constants`](./site/types-and-constants) | `types/` vs `lib/`, NavItem collision |
| Data | [`utils-auth-supabase`](./site/utils-auth-supabase) | Supabase clients, cookies, CORS, auth helpers |
| Content | [`mdx-content`](./site/mdx-content) | Articles, frontmatter, demos, LLM routes |
| Content | [`article-components`](./site/article-components) | Two-column demos, rurikon article UI |
| UI | [`components-ui`](./site/components-ui) | Radix, Dialog, ConfirmDialog, Sonner, Charts |
| UI | [`services-ui`](./site/services-ui) | Lowercase copy, mono buttons, workspace shells |
| UI | [`services-confirm-toasts`](./site/services-confirm-toasts) | Destructive confirms, toast rules |
| UI | [`admin-ui`](./site/admin-ui) | Square admin tab switchers |

### JustHold

Skills for the JustHold investing workspace.

| Category | Skill | When to use |
|----------|-------|-------------|
| Foundation | [`justhold-project`](./justhold/justhold-project) | Stack, shells, defaults, skill index |
| Foundation | [`justhold-i18n`](./justhold/justhold-i18n) | next-intl locales, messages, admin exception |
| Design | [`justhold-design-tokens`](./justhold/justhold-design-tokens) | Palette, fonts, radius |
| Design | [`justhold-ui-layout`](./justhold/justhold-ui-layout) | Base UI components + public/member/admin shells |
| Content | [`justhold-mdx-content`](./justhold/justhold-mdx-content) | JustHold MDX posts — admin CMS, Supabase metadata, R2 bodies |
| Testing | [`justhold-testing`](./justhold/justhold-testing) | Unit, integration, and e2e tests (Vitest / Playwright) |

## Stack these skills target

- Next.js 16 App Router & React 19
- TypeScript, Tailwind CSS v4, MDX, Supabase
- Cursor / Claude Code / Codex and other agents via the [skills](https://www.skills.sh/) CLI
