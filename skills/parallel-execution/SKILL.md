---
name: parallel-execution
description: "[INTERNAL ONLY] Run independent plan steps in parallel when available, review results, or fall back to sequential execution."
---

# Parallel Execution

Execute phase. Run independent plan steps through Dispatch/sub-agents when available.

1. Compose a self-contained prompt for each parallel step (brief + step + tools + "done when" criterion).
2. Start all independent steps simultaneously through Dispatch/sub-agents when available.
3. **Wait for all parallel tasks to complete before merging results. Do not begin the merge step until every dispatched agent has returned.**
4. Run **two-stage review** on each result: completeness (`references/completeness-reviewer.md`), then quality (`references/quality-reviewer.md`). If either stage fails, provide specific feedback and re-dispatch.
5. Merge sequentially. Check for contradictions across outputs.

**Sequential fallback:** If Dispatch/sub-agents are not available, execute steps one by one in the current session. Every 2-3 steps, stop and report: "Completed steps [N-M]. Here's what I have so far: [summary]. Continue?" Wait for the human to say yes before proceeding.

For the dispatch prompt template and merge strategy, read `references/dispatch-guide.md`.
