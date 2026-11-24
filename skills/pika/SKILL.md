---
name: pika
description: "Router and shared rules for the pika workflow. Use at the start of any non-trivial engineering request, when unsure which pika command fits, or when asked 下一步做什么 / 该用哪个命令 / what should I do next. Routes to init, research, think, grill, build, check, improve."
---

# pika

Lean engineering workflow: eight commands, two artifact layers, three per-feature checkpoints (plus one repo-level confirmation in init). If the task is small and clear, use no command — just do it.

## Non-negotiables

1. **Evidence over documents.** Every finding is `file:line` + triggering input + symptom → root cause (or explicitly "unverified") → impact, or it is dropped.
2. **Verification before completion.** Run the commands. "Should work" is not done.
3. **Two artifact layers, plus an optional backlog.** Never duplicate content across them.
   - `docs/prd/PRD-<slug>.md` — task layer: what changes now, tasks, acceptance. Ephemeral: delete when done, git is the archive.
   - `docs/adr/<domain>.md` — decision layer: behavior, constraints, interfaces, and an append-only `Decisions` list. Durable, patched in place.
   - `docs/BACKLOG.md` — optional, one line per deferred task that outlives its PRD. Create lazily; delete lines when done or dropped.
4. **Prohibitions live at the cheapest always-loaded level.** Repo-wide → AGENTS.md `Never`. Domain-wide → adr `Constraints`. This-change-only → PRD `Out`. Never leave a durable prohibition only in a PRD.
5. **Decisions separate truth from history.** `Behavior` / `Constraints` are edited in place and always current. A `Decisions` entry (`D<n>` + why + rejected option) is append-only; reversing one adds a new entry and marks the old `[superseded by D<n>]`. Cross-cutting irreversible decisions (max 5 lines) go in AGENTS.md; domain decisions go in the adr.
6. **Frontier.** Work only what is unblocked; recompute after each round.
7. **At most 3 review-fix rounds**, then question the approach instead of starting round 4.

## Routing

| Situation | Command |
|---|---|
| Unfamiliar repo, or AGENTS.md missing/stale | `init` |
| Blocked on missing facts (API, version, behavior) | `research` |
| Vague ask, needs one recommendation | `think` |
| Plan or doc exists, needs stress-testing (writes PRD/adr only when earned) | `grill` |
| PRD/adr or explicit tasks exist, time to code | `build` |
| Changes are ready to review | `check` |
| "What should we improve in this codebase" | `improve` |
| Small, clear change | No command — do it |

Names are plain verbs (`/init`, `/check`, …). If another installed skill collides, disable one — pika assumes it owns these names.

## Chain

`init` → (`research`) → `think` → `grill` → `build` → `check`. `improve` runs periodically and feeds back into `grill`. The chain is not linear: `check` works on any branch, `research` can fire at any point.

Templates for every artifact: `references/artifacts.md`.

End every command by naming the next one. Never auto-chain into build, commit, or merge.
