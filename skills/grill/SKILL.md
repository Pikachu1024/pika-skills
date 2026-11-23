---
name: grill
description: "Stress-test a plan, PRD, spec, or idea through frontier rounds of questions with recommended answers, recording PRD/spec updates only when they earn it. Use when the user wants their thinking challenged, or 挑刺 / 追问 / 评审一下 / grill / 写 PRD / 更新 spec / 记录决策 / 边聊边落文档."
---

# grill

Interview until shared understanding. Writing happens only when a decision earns it — and every file you touch is announced.

## When not to use

No artifact and no plan in mind → `think` first. Trivial question → just answer it.

## The frontier

Model the discussion as a decision tree. Work it in rounds; each round asks the **frontier** — questions whose prerequisites are already settled.

- Ask the whole frontier in one round. Number them `Q1.`, `Q2.`, each with a `Recommend:` line.
- Word each question so that answering "yes" accepts your recommendation.
- One decision per question. No riders ("and also...").
- Facts are your job: dispatch `research` instead of asking. Decisions are the user's.
- After the answers, recompute the frontier. Repeat.

## Quality bar

- Probe: behavior, boundaries, failure modes, data, migration, reversibility, what is explicitly out.
- Never ask what the code, spec, or a recorded decision already answers.
- If an answer contradicts a recorded decision (spec `## Decisions` or AGENTS.md), surface the contradiction; overturning it needs a new entry, not a silent edit.
- Do not grill trivia.

## Writing, when earned

Announce the file and section before writing; never write silently. Before the first write, say which files you expect to touch. If the user says "thinking only", write nothing.

- **PRD** `docs/prd/PRD-<slug>.md` — create when the change has ≥3 tasks or crosses sessions; ask once before creating: Change / In-Out scope / Tasks with acceptance / Acceptance command / Links, `status: draft`. Ephemeral: delete when done.
- **Spec** `docs/spec/<domain>.md` — patch `Behavior` / `Constraints` / `Interfaces` in the same round an answer changes them; bump `updated:`. No tasks, no history, no file paths.
- **Decisions** — append-only `## Decisions` in the spec. Record an entry when the choice is hard to reverse, surprising without context, or had a rejected alternative worth remembering: `- D<n> (YYYY-MM-DD) <decision>. Why: ... Rejected: <option> — ...`. Reversing = new entry + mark old `[superseded by D<n>]`; never delete. Cross-cutting irreversible decisions (max 5 lines) go in AGENTS.md instead.
- **Prohibitions:** repo-wide → AGENTS.md `Never`; domain-wide → spec `Constraints` (`Must not:` + one-line reason); this-change-only → PRD `Out`. A durable prohibition never lives only in the PRD.
- **Deferred real work** from `Out` that should outlive the PRD → one line in `docs/BACKLOG.md` (create it on first use).
- Keep PRD and spec disjoint: PRD says "this change does X", spec says "the system does X". Link, never copy.
- Every PRD task traces to a behavior, constraint, or acceptance check. Cut tasks that trace to nothing.

Templates: `references/artifacts.md` in the `pika` skill.

## Exit

Stop when the frontier is empty and the user confirms shared understanding. Output settled decisions as 5–10 bullets, remaining unknowns, and every file touched. Name the next command — normally `build`.
