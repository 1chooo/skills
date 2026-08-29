---
name: git-commits
description: >-
  Write git commits as a step narrative — many small conventional commits that
  show how we got here. Use when the user asks to commit, split commits, write
  a commit message, or review commit history style. Covers monorepo app scopes
  (feat(web), feat(mobile)) and tickets as scope (feat(JH-142)).
license: MIT
metadata:
  author: 1chooo
  version: "1.1.0"
---

# Git Commits

Also follow the always-on rule `.cursor/rules/git-commits.mdc`.

## Principle

We are not afraid of many commits. Each commit is a **step**. Reading `git log` should reconstruct the path that brought us here — not a single opaque dump.

## When to use

- User asks to commit, amend (only if allowed), or draft commit messages
- Splitting a large diff into reviewable steps
- Matching existing commit voice before opening a PR

## Format

```
type(scope): imperative summary

Optional body: why this step matters, or what remains intentional.
```

With a ticket, the ticket **is** the scope:

```
type(<TICKET-NUM>): <imperative verb> rest of summary
```

| Piece | Convention |
|-------|------------|
| Type | `feat` `fix` `refactor` `style` `docs` `test` `chore` |
| Scope | Ticket ID when known; otherwise the app/package (monorepo) or area of the change. Infer from `git log`. |
| Subject | Imperative, concise, no trailing period |
| Body | 1–2 sentences of **why** / intent — not a file inventory |

## Monorepo scope

In a monorepo (or multi-app repo), **scope is the app / package**, not a restatement of the type:

```
feat(web): ...
feat(mobile): ...
fix(api): ...
```

Pick the app that actually changed. Infer names from the repo layout and recent `git log` (`web`, `mobile`, `api`, `tokens`, …). If a ticket is known, use the ticket as scope instead (see Tickets).

**Do not duplicate type as scope.** `docs(docs):`, `test(test):`, `chore(chore):` add no information.

| Situation | Use | Not |
|-----------|-----|-----|
| Feature in the web app | `feat(web):` | `feat(feat):` |
| Docs about the web app | `docs(web):` | `docs(docs):` |
| Repo-wide docs (README, contributing) | `docs:` (omit scope) | `docs(docs):` |
| Docs package / `docs` app with no better target | `docs:` | `docs(docs):` |
| Tests for mobile | `test(mobile):` | `test(test):` |

Scope the thing being documented or tested, not the `docs`/`test` folder. Omit scope when nothing more specific than the type exists.

**Scope examples**

| Repo | Typical scopes |
|------|----------------|
| JustHold | `web`, `api`, `tokens`, `agents`, `mobile` |
| 1chooo.com | `bio`, `projects`, `services`, `admin`, `articles` |

## Tickets

When a ticket / issue / Linear / Jira ID is known, it occupies the scope slot. The summary still starts with an imperative verb:

```
feat(JH-142): add member checkout
fix(MOB-88): prevent double tap on submit
docs(API-12): document webhook retries
```

- Keep the ticket token as the team writes it (`JH-142`, `ABC-123`, `#884`).
- No ticket → use app/package scope (monorepo) or omit scope; do not invent IDs.
- Ticket wins over app name. If both matter, name the app in the summary (`feat(JH-142): add web member checkout`) or the body.
- Prefer one ID per commit. Extra context goes in the body, not a second ticket in the subject.

## Splitting workflow

When the working tree mixes concerns:

1. Group by **one step** (one UI surface, one API layer, one docs pass).
2. Stage only that group; commit with a message that names the step.
3. Repeat until the tree is clean.
4. Prefer a short chain over one mega-commit.

## Message checklist

- [ ] Subject alone makes the step clear in `git log --oneline`
- [ ] Monorepo with no ticket: scope is the app/package; type is not repeated as scope
- [ ] Ticket (if any) is the scope: `type(<TICKET-NUM>):` then an imperative verb
- [ ] Body answers "why this step?" when non-obvious
- [ ] No unrelated files in the same commit
- [ ] Still only commit when the user explicitly asks

## Anti-patterns

| Avoid | Prefer |
|-------|--------|
| `update stuff` / `misc fixes` | Named step + scope |
| `docs(docs):` / `test(test):` | `docs(web):` or `docs:` |
| `feat(web): JH-142 add checkout` (ticket after colon) | `feat(JH-142): add checkout` |
| `feat(JH-142): checkout flow` (no verb) | `feat(JH-142): add checkout flow` |
| One commit for 5 pages | One commit per page/step |
| Body that lists every file | Body that states intent |
| Squashing away intermediate steps before PR (unless asked) | Keep the narrative |

## Examples

**Good — JustHold step chain**

```
feat(web): add member desktop top-nav item list
feat(web): make member account menu header-ready
feat(web): add desktop top navbar to member site header
feat(web): switch member shell from sidebar to top navbar
docs(web): document member desktop top-nav shell
```

```
refactor(web): drop Tools page desktop intro

Start with the tool cards; nav already labels the page.
```

**Good — tickets**

```
feat(JH-142): add member checkout
fix(MOB-88): prevent double tap on submit
```

```
fix(API-12): reject stale webhook signatures

Replay window is 5 minutes; unsigned retries were still accepted.
```

**Good — 1chooo.com step chain**

```
feat(bio): restyle work history with company logos
fix(cursor): scale heatmap to the content column
chore(services): remove big money from the navbar
docs: add contributing guide
```

```
fix(cursor): scale /cursor heatmap to the content column

The year grid was a fixed-width HTML layout clipped by overflow-x-hidden.
```

**Bad**

```
chore: cleanup
```

```
docs(docs): update readme
```

```
feat(web): JH-142 add checkout
```

```
refactor(web): remove headers and fix layout and update docs
```
