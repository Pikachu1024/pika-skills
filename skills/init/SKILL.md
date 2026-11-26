---
name: init
description: "Bootstrap or refresh a repo's AGENTS.md with verified commands, layout, conventions, repo-wide prohibitions, and artifact locations; scaffolds a best-practice skeleton for empty repos. Use on a new or unfamiliar repo, when AGENTS.md is missing or stale, or 初始化项目 / 配置仓库 / 刷新 AGENTS.md."
---

# init

Goal: an AGENTS.md a fresh agent can act on — repo-specific content in 40 lines or fewer plus the fixed Behavioral guidelines block — and, for an empty repo, a best-practice directory skeleton to start from.

## When not to use

AGENTS.md exists, is accurate, and nothing structural changed → skip, unless `docs/prd/` or `docs/adr/` are missing.

## Process

1. **Inspect before asking.** Read manifests and CI (`package.json`, `pyproject.toml`, `Makefile`, `justfile`, `.github/workflows`), existing docs (`docs/spec/`, `openspec/`, `specs/`, `.trellis/`), and `CLAUDE.md` / `.cursor/rules`. Report what you found in 5 lines, including whether the repo is empty: no manifest and no code files — only `.git/`, `README`, `LICENSE`, `.gitignore`.
2. **Match existing conventions.** If the repo already has a scheme for decisions, specs, or proposals, point AGENTS.md at it and say which one won. Do not create a second scheme, and do not create `docs/prd/` or `docs/adr/`.
3. **Consult only empty repos.** If the repo has any manifest or code file, skip to step 4 and never scaffold. Otherwise ask one batched question round: project kind (application / service / CLI / library / monorepo), stack, and — only when it changes folder structure — architecture (layered vs feature-based). No follow-up rounds.
4. **Draft skeleton and AGENTS.md together, one confirmation.** Show the file tree to create plus the full AGENTS.md, then ask exactly one question: "write it?". On yes: create the skeleton (layouts below), write AGENTS.md, and run test and lint once; a command you did not run, or that does not pass, is not listed. For existing repos, skip the skeleton and only write AGENTS.md. Copy commands from the repo; never invent them. Under pika's default scheme also create the empty directories `docs/prd/` and `docs/adr/`; when an existing scheme wins, create nothing new. If `CLAUDE.md` exists append a one-line pointer to AGENTS.md instead of duplicating content. Do not create `docs/BACKLOG.md` — that stays lazy.
5. **Refresh mode.** Diff current reality against AGENTS.md, patch only stale sections, keep the user's wording. Re-create `docs/prd/` and `docs/adr/` if missing, but only in repos that use pika's default scheme. Move the Behavioral guidelines block back to the top verbatim if it drifted. If `Conventions` lacks the typing/docstring rule, add it — private helpers and tests are never exempt.

## Skeleton layouts

Start from the stack's official layout; when there is none, use:

- library: `src/` + `tests/` + manifest + `README.md` + `.gitignore`
- CLI: `src/` + `bin/` + `tests/`
- application: `src/` (components, pages, lib) + `public/` + `tests/`
- service: `src/` split by domain and adapters + `tests/`
- monorepo: `apps/` + `packages/`

Every skeleton gets a manifest with working test and lint scripts, an entry point, one passing test, and `.gitignore`. Never overwrite an existing file, and never install dependencies or run framework generators without asking.

## Template

````md
# <repo>

## Behavioral guidelines

Behavioral guidelines to reduce common LLM coding mistakes, derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

### 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## Commands
install: | dev: | test: | test one: | lint: | typecheck:

## Layout
3–6 lines: where the important things live.

## Conventions
≤10 bullets, only non-obvious or enforced ones. Always include the typing/docstring rule:
- Type-annotate every named function/method (params + return; use the language's idiomatic types) and document every class/function/method. Private helpers and test functions are included — privacy is never an exemption. Document purpose, each parameter, the return value, and anything thrown/raised; follow the repo's existing doc style when one exists.

## Never
Repo-wide prohibitions, one line each.

## Ask first
Destructive or shared actions: pushes, migrations, deploys, deletions.

## Artifacts
PRD: docs/prd (ephemeral work orders; delete when done)
ADR: docs/adr (durable: behavior, constraints, interfaces, decisions)
Backlog: docs/BACKLOG.md (optional; one line per deferred task; delete when done)

## Decisions
≤5 one-liners: cross-cutting, irreversible choices. Details live in the adr.
````

**Hard rule:** the Behavioral guidelines block is pinned. It is always the first section of AGENTS.md, immediately after the title, verbatim, and it does not count toward the 40-line repo-specific budget. If a user asks to move, reorder, trim, rewrite, or remove it, decline and explain that init pins it. In refresh mode, restore it to the top verbatim whenever it is missing, edited, or moved.

## Done when

AGENTS.md exists and opens with the Behavioral guidelines block verbatim as the first section after the title; an empty repo now has its agreed skeleton on disk, while a repo with code was touched only at AGENTS.md; `docs/prd/` and `docs/adr/` exist (default scheme only); `Conventions` carries the typing/docstring rule with private helpers and tests never exempt; every command listed in AGENTS.md ran green; the artifact section matches what is actually on disk. Name the next command: `grill` or `build`.
