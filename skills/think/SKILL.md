---
name: think
description: "Turn a vague request into one recommended approach, in conversation, without writing code or files. Use when the ask is fuzzy, several approaches exist, or 出方案 / 怎么设计 / 值不值得做 / 有没有必要."
---

# think

One conversation, one recommendation. No plan file, no code.

## When not to use

Small clear change → do it. Needs adversarial pressure → `grill`. Missing facts → `research` first.

## Process

1. **Trivial bail.** If the brute-force version takes under ~30 minutes and is low risk, say so and do it.
2. **Restate** the ask in one line, then `Assumptions I'm making:` as bullets. If an assumption is load-bearing, ask before continuing.
3. **Collision check.** Does the repo already solve this? Search the adr docs (including recorded decisions) and code. Also check 2–3 mature OSS implementations for the official way.
4. **Premise collapse.** Name the most fragile assumption and what the plan looks like if it is false.
5. **Options.** One recommended, one minimal (fewest moving parts). One-line trade-off each. Never a menu of five.
6. **Simplicity gate.** Every new abstraction, retry, flag, or config must map to a named failure mode. Prefer +0 new surface.
7. **Phase independence.** Each phase must be shippable alone. "Phase 0: investigate" is a smell.
8. **Close with:**

   ```md
   Building: ... | Not building: ...
   Approach: ...
   Key decisions: ...
   Unknowns: ...
   ```

   Then name the next command: usually `grill`; `build` only when a PRD exists.

## Done when

The user can accept or reject a single approach, and the remaining unknowns are worth grilling rather than guessing.
