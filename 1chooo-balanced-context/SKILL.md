---
name: 1chooo-balanced-context
description: Reduce redundant code-reading tokens while preserving implementation context. Use when the user requests balanced token usage, or explicitly invokes this skill for code exploration, debugging, review, or implementation in Cursor, Grok Build, or another coding agent.
license: MIT
metadata:
  author: 1chooo
  version: "1.0.0"
---

# 1chooo Balanced Context

Favor modest token savings and verified understanding. Use existing tools; do not install dependencies, configure MCP, add hooks, or change permissions just to apply this skill. Follow repository instructions and the user's authorized scope.

## Navigate, then understand

1. Start from the task's paths, error, symbol, or diff. Inspect relevant repository instructions and package scripts. For a small known file, read it directly; skip navigation overhead.
2. Prefer scoped filename searches and `rg -l` to locate candidates; use `rg -n` for relevant matches. Skip generated output, dependencies, lockfiles, and binaries during initial exploration; inspect them if the issue points there. Respect ignore rules and keep secrets out of tool output.
3. If already available, use AST/Tree-sitter outlines to locate declarations, or LSP/Serena to find definitions and references. Treat these as navigation evidence, not complete behavior. Never assume editor LSP features are accessible to the agent; use only exposed tools. Fall back to scoped text search without blocking the task.
4. Read the complete target function/component and the surrounding imports, types, constants, and enclosing control flow that affect it. Read the whole file when small or when behavior is spread across it. Before editing, inspect relevant callers, callees, contracts, configuration, and existing tests to the depth needed for the change.

## Expand when evidence is incomplete

Expand immediately for truncated output, unresolved symbols, conflicting evidence, wrappers, re-exports, dynamic dispatch, runtime configuration, or uncertain side effects.

For auth, money, migrations, shared APIs, or public types, trace the affected boundary and representative consumers before changing it. In a monorepo, check package boundaries when shared contracts change; do not scan unrelated apps by default.

Treat empty search results as inconclusive until scope, ignored paths, aliases, and tool/index freshness have been checked where relevant. Do not infer absence from a partial search.

Distinguish verified facts from hypotheses; investigate task-critical unknowns instead of inventing implementations. Do not impose a hard token or file-count limit on necessary evidence.

## Reuse context and validate

- Reuse source already visible in this session. Keep only a small working map of paths, symbols, and unresolved questions when useful; do not create tracking files for routine tasks. Re-read affected ranges after edits, branch changes, external changes, stale indexes, or context compaction when source details are no longer available.
- Bound initial search output, then narrow or paginate deliberately. Do not silently discard results with `head` or mistake a capped output for exhaustive coverage.
- Keep raw failure details accessible. If an existing output compressor hides causes, inspect the original log. Preserve exit status; do not rerun expensive commands solely to shorten output.
- Inspect the final diff. Run repository-required checks and focused tests/type checks appropriate to the change. Expand validation for failures or shared impact. Token savings never justify skipping required checks.
- Report the result, meaningful validation, and remaining uncertainty concisely in the user's language. Never claim measured token savings without comparable usage data. Avoid narrating every lookup.

For a trial comparison, use similar tasks with the same model and settings; compare total tokens, successful completion, and corrections needed. Include the skill's own context overhead when judging benefit.
