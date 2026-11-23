---
name: build
description: "Implement a PRD or explicit task list in vertical slices, test-first at agreed seams, keeping the spec in sync. Use when a PRD or spec exists and it is time to write code, or 开始实现 / 按 PRD 做 / 执行任务 / 写代码."
---

# build

Small, clear task → skip this command and just do it.

## Before the first line of code

1. Read the PRD plus the linked spec. State the PRD title (or the task-list source when there is no PRD) back in one line, list the current task frontier, and set the PRD `status: active`.
2. **Seam agreement — the one gate.** Name the seams under test: highest, fewest, ideally one. Each gets a one-line `catches / misses`. Ask once: "seams OK?" Write no test before the answer.
3. Confirm the working tree is clean enough to commit per slice.

## Loop, per frontier task

- **Red:** one failing test for one behavior, through the public interface. Expected values come from an independent source, never from the implementation.
- **Green:** minimal code. One slice per cycle — no horizontal slicing (all tests, then all code).
- **Commit** one logical change: `<slug>: <slice>`.
- Typecheck and run the touched test file after each slice. Full suite once at the end.

## Rules

- More than ~100 lines written before any test run is a red flag: stop and test.
- Refactoring belongs to `check`, not the loop.
- **Spec sync:** if behavior or constraints changed, patch the spec in the same commit and bump `updated:`. A task is not done while the spec lies.
- Stage only files you changed — never `git add -A`. Foreign changes in the tree stay untouched; if they block you, stop and ask.
- **Don't silently overturn a recorded decision** (spec `## Decisions` or AGENTS.md `## Decisions`). If the code needs to, stop and reopen it through `grill`.
- Notice something unrelated: say `Noticed, not touching:` and append one line to `docs/BACKLOG.md` (create it on first use), then continue.
- Wide refactor: expand → migrate → contract, one commit each. Never delete-and-replace in one step.
- Pause and ask if >5 files are affected in a way the PRD did not predict, or if unexpected state appears (failing baseline tests, foreign changes in the tree).

## Parallel mode

Only on request, and only when ≥5 tasks are independent. Then one worktree/subagent per task with fresh context, and you do the merging. Default is single-threaded.

## Done when

Every PRD task is checked, acceptance commands were run and their output shown, the full suite is green, PRD/spec agree with the code, and the PRD `status` is `done`. Print "PRD can be deleted" and suggest `check`. Never auto-run check, commit-and-push, or merge.
