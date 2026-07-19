# 1chooo/skills

[![skills.sh](https://skills.sh/b/1chooo/skills)](https://skills.sh/1chooo/skills)

Skills by [1chooo](https://github.com/1chooo) for building [1chooo.com](https://1chooo.com) and [JustHold](https://github.com/1chooo/justhold) with AI agents — Next.js, Supabase, MDX, and the rest of the stack.

- skills.sh: [1chooo/skills](https://www.skills.sh/1chooo/skills) · [1chooo.com](https://www.skills.sh/1chooo/skills/1chooo.com) · [justhold-mdx-content](https://www.skills.sh/1chooo/skills/justhold-mdx-content) · [justhold-testing](https://www.skills.sh/1chooo/skills/justhold-testing)
- Browse on the site: [1chooo.com/skills](https://1chooo.com/skills)
- Background: [AI Agent](https://1chooo.com/thoughts/ai-agent)

## Install

```bash
npx skills add 1chooo/skills
```

Or a specific skill:

```bash
npx skills add 1chooo/skills --skill 1chooo.com
npx skills add 1chooo/skills --skill justhold-mdx-content
npx skills add 1chooo/skills --skill justhold-testing
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

| Skill | When to use |
|-------|-------------|
| [`1chooo.com`](./1chooo.com) | Working on the 1chooo.com codebase — App Router, API routes, Supabase auth, MDX, services UI, admin UI |
| [`justhold-mdx-content`](./justhold-mdx-content) | JustHold MDX posts — admin CMS, Supabase metadata, R2 bodies |
| [`justhold-testing`](./justhold-testing) | JustHold unit, integration, and e2e tests (Vitest / Playwright) |

## Stack these skills target

- Next.js 16 App Router & React 19
- TypeScript, Tailwind CSS v4, MDX, Supabase
- Cursor / Claude Code / Codex and other agents via the [skills](https://www.skills.sh/) CLI
