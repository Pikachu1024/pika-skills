---
name: research
description: "Answer a factual question from primary sources — official docs, source code, specs, changelogs — with version-pinned citations. Use when a decision is blocked on missing facts, or 查一下 / 调研 / 这个 API 怎么用 / 最新版本是哪个."
---

# research

Find facts. Leave decisions to the user.

## When not to use

The repo or established knowledge already answers it → answer directly, no research.

## Process

1. **State the question in one line**, plus which decision it unblocks. Vague question → sharpen it first, or route to `think`.
2. **Delegate when possible.** Spawn a fresh-context agent so the main thread keeps working. Give it the question, not your guesses.
3. **Primary sources only:** official documentation, the library's own source and tests, spec text, first-party changelog. Trace every claim to its owner. Pin the version from the manifest, not from memory.
4. **Answer format**, in conversation, ≤10 lines:

   ```md
   Question: ...
   Answer: ...
   Evidence: <page-level URL> (vX.Y)
   Implication for this repo: ...
   ```

5. **Persist only what will be re-read.** Durable constraints → adr `Constraints` / `Interfaces` with the source link, when that adr exists; otherwise hand them to the caller to record. Task-scoped facts → PRD `Links`. No standalone research file unless the user asks.

## Done when

The claim that unblocked the decision has a primary source and a version. Contradictory sources are reported, not averaged. Name the next command, or say the question is answered and nothing further is needed.
