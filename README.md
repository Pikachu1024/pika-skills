# pika

A lean engineering workflow for coding agents: eight small skills, two artifact layers, three human checkpoints. Fewer documents, more evidence.

[![skills.sh](https://skills.sh/b/Pikachu1024/pika-skills)](https://skills.sh/Pikachu1024/pika-skills)

## Install

```bash
npx skills add Pikachu1024/pika-skills
```

Or copy `skills/*/` into `.claude/skills/`, `.agents/skills/`, or `.qoder/skills/`.

## Skills

| Skill | Does | Reach for it when |
|---|---|---|
| `pika` | Router and shared rules; names the next command | You are unsure which command fits |
| `init` | Writes AGENTS.md and verifies every command in it | New or unfamiliar repo |
| `research` | Facts from primary sources, version-pinned | A decision is blocked on a missing fact |
| `think` | One recommended approach, in conversation, no files | The ask is vague or several designs compete |
| `grill` | Frontier-round interview; writes PRD/adr only when earned | A plan needs stress-testing |
| `build` | Vertical slices, test-first at agreed seams, adr kept in sync | A PRD or task list exists |
| `check` | Two-axis review (Standards + ADR) with evidence | Before merge, on any branch |
| `improve` | Architecture scan with a deletion test | Every few weeks, or when the code feels heavy |

## The chain

```
init → (research) → think → grill → build → check
                          improve (periodic) → grill
```

Not linear: `check` works on any branch, `research` fires whenever a fact is missing. No command auto-chains into build, commit, or merge.

## Artifacts

| File | Layer | Lifecycle |
|---|---|---|
| `AGENTS.md` | Repo contract: commands, layout, never/ask-first | Refreshed by `init` |
| `docs/prd/PRD-<slug>.md` | Task layer: change, tasks, acceptance | Ephemeral — delete when done |
| `docs/adr/<domain>.md` | Decision layer: behavior, constraints, interfaces, append-only decisions | Durable — patched in place |
| `docs/BACKLOG.md` | Deferred work that outlives a PRD | Optional, one line per item |

## Human checkpoints

1. `grill` asks once before creating a PRD.
2. `build` agrees the test seams before writing a test.
3. `check` reports, then asks which findings to fix — it never edits code.

## Notes

- Skills assume git. `check` reviews `git diff` plus the working tree.
- Installing the `check` skill can collide with another skill of the same name; keep one.
- Install the pack together — `grill`, `build`, and `check` reference the `pika` skill's `references/artifacts.md`.
