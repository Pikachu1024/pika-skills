---
name: check
description: "Review changes since a fixed point on two axes — Standards (repo conventions + code smells) and ADR (does it match the PRD/adr) — with evidence-qualified findings. Use before merge, after build, on any branch, or 帮我 review / 审查一下 / 看看有没有问题 / 能不能合."
---

# check

Review and diagnose, do not rewrite. No report file, no score.

## When not to use

Docs-only or typo-level change → say so and just fix it. No diff and no uncommitted changes → nothing to review.

## Process

1. **Pin the fixed point.** Default: merge-base with the main branch. Validate the ref; the review surface is `git diff <fp>...HEAD` **plus** any staged, unstaged, or untracked files from `git status`. If both are empty → stop and ask. Never imply full coverage while excluding uncommitted work — say what was excluded.
2. **Find the intent.** Branch name → PRD slug in recent commit messages → `docs/prd/` match. If nothing found, run Standards only and say the ADR axis was skipped.
3. **Standards sources, in order:** AGENTS.md `Never` / `Conventions` → existing lint rules (tooling-enforced items are not findings) → smell baseline: Mysterious Name · Duplicated Code · Feature Envy · Data Clumps · Primitive Obsession · Repeated Switches · Shotgun Surgery · Divergent Change · Speculative Generality · Message Chains · Middle Man · Refused Bequest. Write each as "what → fix". Repo standards win over the baseline.
4. **Depth by size and risk.** ≤5 files or <100 lines: quick. 100–500 lines or 6–10 files: standard. Larger, or touches auth / payments / migrations / CI / deletion paths: deep — read the callers, not just the diff. State the depth before proceeding.
5. **Finding quality gate.** Every finding carries `file:line` + triggering input + affected caller/consumer + impact, told as **symptom → root cause → impact**. A root cause without evidence (caller unread, not reproduced) is written "root cause unverified" — never guessed. Fails the gate → drop it. Severity: `Critical` (blocks merge) / `Required` / `Optional` / `Nit`.
6. **ADR axis:** unmet PRD tasks and acceptance checks, missing requirements, partial implementation, scope creep, contradiction with adr `Constraints`, behavior changed without an adr update, a durable decision made in code but missing from `## Decisions`, prohibitions ignored.
7. **Also verify:** tests assert behavior through public interfaces (if cheap, mutate one condition and confirm red); acceptance commands actually pass — run them and paste the output; if no acceptance command exists, say so as a finding; no secrets, no debug leftovers.

## Report

`## Standards` and `## ADR` side by side, never merged or reranked. ≤400 words per axis, one-line summary each. Then:

- **Problem list** — every finding ordered by severity, across both axes; the two sections above are its classification, this is the work queue.
- **Jargon** — the first time a non-obvious term appears, explain it in one plain sentence (e.g. "Feature Envy: a function mostly reads another object's data, so it belongs on that object").
- **Verdict** — `Approve` or `Request changes`, naming only blocking items.
- **Fix gate** — ask with AskUserQuestion which findings to fix; Critical pre-selected, the rest multi-select. The chosen set goes to `build`; check edits nothing. "Just looking" ends the review here.

On approve, remind that the PRD may be deleted, and that commit/merge/push is the user's call. Name the next command: `build` for the chosen fixes; otherwise the user decides.

Max 3 review-fix rounds. If round 4 would be needed, the approach is wrong — say so.
