---
name: improve
description: "Scan a codebase for architecture deepening opportunities, present a short candidate list with confidence labels, then grill the one the user picks. Use periodically, or 架构改进 / 哪里可以重构 / 代码库体检 / 设计问题."
---

# improve

Not a review of one diff — a scan of the whole design. Periodic, not per feature.

## Process

1. **Scope with YAGNI.** User names a direction → use it. Otherwise take hot spots from roughly the last 50 commits. Read spec `Constraints` and `Decisions`; do not propose what a recorded decision already rejected.
2. **Scan with a fresh context** for friction: bouncing between shallow modules, information leaking across modules, functions extracted only for tests, untestable areas, shotgun surgery. Apply the **deletion test**: would deleting this module concentrate complexity or scatter it?
3. **Candidate table**, in chat:

   ```md
   | Files | Problem | Proposed deepening | Gain | Confidence |
   ```

   Gain = locality / leverage / testability. Confidence: `Strong` / `Worth exploring` / `Speculative`. No 0–10 scores, no HTML report, no file.
4. **Do not design interfaces yet.** Ask: "which one do you want to explore?"
5. **On pick:** run the `grill` frontier on it. Only then offer to record — spec `Interfaces` / `Constraints` updates, a new `Decisions` entry if a rejection is load-bearing, or a PRD via `grill` if it becomes real work.

## Rules

- Every candidate needs a concrete consequence, or it is dropped.
- Prefer deepening existing modules over adding layers. One adapter is a hypothetical seam; two make it real.
- A clean scan is a valid result. Say so instead of padding the list.

## Done when

The user picked one candidate (or none), and the next command is named.
