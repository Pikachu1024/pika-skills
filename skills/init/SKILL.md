---
name: init
description: "Bootstrap or refresh a repo's AGENTS.md with verified commands, layout, conventions, repo-wide prohibitions, and artifact locations. Use on a new or unfamiliar repo, when AGENTS.md is missing or stale, or 初始化项目 / 配置仓库 / 刷新 AGENTS.md."
---

# init

Goal: an AGENTS.md a fresh agent can act on in 40 lines or fewer.

## When not to use

AGENTS.md exists, is accurate, and nothing structural changed → skip, unless `docs/prd/` or `docs/spec/` are missing.

## Process

1. **Inspect before asking.** Read manifests and CI (`package.json`, `pyproject.toml`, `Makefile`, `justfile`, `.github/workflows`), existing docs (`docs/adr/`, `docs/spec/`, `openspec/`, `specs/`, `.trellis/`), and `CLAUDE.md` / `.cursor/rules`. Report what you found in 5 lines.
2. **Match existing conventions.** If the repo already has a scheme for decisions, specs, or proposals, point AGENTS.md at it and say which one won. Do not create a second scheme, and do not create `docs/prd/` or `docs/spec/`.
3. **Detect commands, then prove them.** Copy commands from the repo; never invent them. Run the test and lint commands once. A command you did not run is not done.
4. **Draft, one confirmation, then write.** Show the full AGENTS.md and ask exactly one question: "write it?". On yes: write AGENTS.md. Under pika's default scheme also create the empty directories `docs/prd/` and `docs/spec/`; when an existing scheme wins, create nothing new. If `CLAUDE.md` exists append a one-line pointer to AGENTS.md instead of duplicating content. Do not create `docs/BACKLOG.md` — that stays lazy.
5. **Refresh mode.** Diff current reality against AGENTS.md, patch only stale sections, keep the user's wording. Re-create `docs/prd/` and `docs/spec/` if missing, but only in repos that use pika's default scheme.

## Template

```md
# <repo>

## Commands
install: | dev: | test: | test one: | lint: | typecheck:

## Layout
3–6 lines: where the important things live.

## Conventions
≤10 bullets, only non-obvious or enforced ones.

## Never
Repo-wide prohibitions, one line each.

## Ask first
Destructive or shared actions: pushes, migrations, deploys, deletions.

## Artifacts
PRD: docs/prd (ephemeral work orders; delete when done)
Spec: docs/spec (durable: behavior, constraints, interfaces, decisions)
Backlog: docs/BACKLOG.md (optional; one line per deferred task; delete when done)

## Decisions
≤5 one-liners: cross-cutting, irreversible choices. Details live in the spec.
```

## Done when

AGENTS.md exists, `docs/prd/` and `docs/spec/` exist (default scheme only), every command listed in AGENTS.md was executed successfully, and the artifact section matches what is actually on disk. Name the next command: `grill` or `build`.
