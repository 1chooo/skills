---
name: 1chooo-balanced-context
description: Reduce unnecessary coding-agent tokens while preserving task-relevant understanding. Use when the user requests balanced token usage or invokes this skill for code exploration, debugging, review, or implementation in Cursor, Grok Build, or another coding agent.
license: MIT
metadata:
  author: 1chooo
  version: "1.1.0"
---

# 1chooo Balanced Context

Reduce irrelevant reads, repeated retrieval, and oversized output. Preserve evidence that could change the implementation or conclusion. Prioritize correct task completion over a token target.

Follow repository instructions and the user's authorized scope. Use existing tools; do not install dependencies, configure MCP, add hooks, or change permissions just to apply this skill.

## 1. Choose the shortest sufficient path

- Start from supplied paths, symbols, errors, or diffs. Read relevant repository instructions.
- For a small task in a known file, read it directly. Skip outlines, planning, and context tracking when they add overhead.
- Before each lookup, identify internally what uncertainty it should resolve. Do not narrate routine lookups.
- Expand exploration only when the answer could affect the task.

## 2. Locate with compact output

- Prefer scoped filename searches or `rg -l` to identify files; use `rg -n` for relevant matches.
- Initially exclude dependencies, generated output, binaries, and lockfiles. Inspect them when evidence points there.
- Respect ignore rules and keep secrets out of tool output.
- If already exposed to the agent, use AST/Tree-sitter outlines for structure and LSP/Serena for definitions or references. Otherwise use text search.
- Request paths, symbols, signatures, and locations during navigation. Avoid full AST dumps or verbose JSON unless needed for processing.
- Narrow or paginate oversized results. Treat capped or truncated output as incomplete, never exhaustive.

## 3. Read enough to explain behavior

Use navigation results to find source; do not treat outlines, signatures, or summaries as proof of runtime behavior.

- Read the complete relevant logical unit, including branches, early returns, error handling, and cleanup.
- Inspect surrounding imports, types, constants, enclosing control flow, and state that can affect that unit.
- For large functions or components, begin with the relevant block and its dependencies. Read the entire enclosing unit when interactions are distributed or uncertain.
- Read small files whole when doing so is cheaper than repeated partial reads.
- Follow callers and callees when their contracts, behavior, or side effects could influence the change. Do not recursively explore unrelated dependencies.
- For shared API or type changes, inspect affected consumers and package boundaries. For auth, money, migrations, or destructive operations, trace the relevant boundary and failure paths.

## 4. Expand when uncertainty matters

Read additional source when encountering unresolved symbols, wrappers, re-exports, dynamic dispatch, runtime configuration, conflicting evidence, or unclear side effects.

- Separate verified facts from hypotheses. Investigate unknowns that could change the implementation, conclusion, or validation.
- Treat empty searches as inconclusive. Before relying on absence, check relevant scope, aliases, ignored paths, and index freshness.
- Do not assume LSP references capture every runtime dependency.
- If required evidence is inaccessible, explain the limitation and qualify the conclusion.
- Never impose a hard token, file-count, or read-count limit on necessary evidence.

## 5. Stop exploration when evidence is sufficient

Proceed when source evidence supports:

1. The current behavior and requested difference.
2. Relevant inputs, conditions, outputs, and side effects.
3. The boundaries or consumers affected by the proposed change.
4. Appropriate validation and any remaining uncertainty.

Apply these criteria proportionally to the task. Continue investigating if an unresolved question could materially change the solution. Avoid repeatedly confirming facts already established.

## 6. Reuse context without trusting stale details

- Reuse source that remains available and current in the conversation.
- Distinguish directly inspected source from remembered summaries. Reconfirm exact details before making edits that depend on them.
- Re-read affected ranges after edits, branch changes, external changes, or stale-index indications.
- For long tasks or context compaction, retain a short working summary of the goal, verified behavior, relevant paths/symbols, decisions, validation, and unresolved questions.
- Preserve constraints and reasons behind decisions, not merely a list of files read.
- Use summaries to recover direction; inspect original source when critical details are missing.
- Do not create tracking files for routine tasks or repeat source excerpts already available.

## 7. Preserve diagnostic evidence and validate

- Prefer concise successful-command output, while retaining exit status and useful summaries.
- Keep failure messages, stack traces, and relevant diagnostics accessible. Inspect raw logs when compression or truncation hides the cause.
- Do not treat summarized output as proof that every check passed.
- Reuse existing command results; rerun only when changes, failures, or uncertainty justify it.
- Inspect the final diff and run repository-required checks plus validation appropriate to the affected behavior.
- Expand testing for failures or broader impact. Never skip required checks to save tokens.

## 8. Report concisely and evaluate honestly

Report the outcome, meaningful validation, and remaining limitations in the user's language. Avoid lookup transcripts and unsupported certainty.

Do not claim measured token savings without comparable usage data. Evaluate total task tokens, including skill overhead, retries, and corrections, alongside successful completion and correctness.

For trial comparisons, use similar tasks with the same model and settings. Retain this workflow only where reduced retrieval overhead outweighs its own cost.
