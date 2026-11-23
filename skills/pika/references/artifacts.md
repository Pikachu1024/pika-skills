# pika artifact templates

Two layers, plus the repo contract. A fact lives in exactly one of them.

In repos that adopt pika's default scheme, `init` creates the empty directories `docs/prd/` and `docs/spec/`; the files inside are created lazily by the command that earns them. When an existing scheme wins, nothing new is created.

## PRD — `docs/prd/PRD-<slug>.md`

Task layer. Ephemeral: delete when done, git history is the archive. One page max.

```md
# PRD-<slug>
status: draft | active | done — YYYY-MM-DD

## Change
One paragraph: what changes, for whom.

## Scope
In: ...
Out: ...

## Tasks
- [ ] <task> — acceptance: <observable check>
- [ ] <task> — acceptance: <observable check>

## Acceptance
Command(s) to run and expected result.

## Links
spec: docs/spec/<domain>.md#<section> · issue: ...
```

Rules:
- Never copy behavior text from the spec. Link to it.
- Every task traces to a behavior, constraint, or acceptance check. Cut tasks that trace to nothing.
- Created only when the change has ≥3 tasks or spans sessions.
- `build` owns `status`: `draft` when created, `active` when work starts, `done` once verified.

## Spec — `docs/spec/<domain>.md`

Decision layer. Durable. One file per domain or capability, patched in place.

```md
# <domain> — spec
updated: YYYY-MM-DD

## Behavior
What the system does now, narrated as user-visible capability.

## Constraints
Must: ...
Must not: ... (one-line reason each)

## Interfaces
Stable types, signatures, data shapes. No file paths, no code snippets.

## Decisions
- D2 (YYYY-MM-DD) <decision>. Why: ... Rejected: <option> — ...
- D1 (YYYY-MM-DD) <decision>. Why: ... [superseded by D2]

## Open
Known but undecided. Not a TODO list.
```

Rules:
- No tasks, no history, no file paths (they rot; git diff is the delta).
- `Behavior` / `Constraints` / `Interfaces` are current truth: edit in place, in the same commit as the code.
- `Decisions` is append-only. Record an entry only when the choice is hard to reverse, surprising without context, or had a rejected alternative worth remembering. Reversing a decision = new entry + mark the old one `[superseded by D<n>]`. Never delete.
- Longer than ~2 pages → split by capability, don't append.

## Backlog — `docs/BACKLOG.md` (optional)

Created lazily, only when deferred real work starts accumulating across features. One line per item; delete the line when it is done or dropped — git is the archive. Not a wish list: if nobody intends to do it, don't write it.

```md
- <what needs doing> — <why it matters>
```

## AGENTS.md

Repo contract, written and refreshed by `init`, 40 lines or fewer. Holds commands, layout, `Never`, `Ask first`, and at most 5 cross-cutting irreversible decisions as one-liners. Details link to the relevant spec.
