---
name: superpowers
description: Full structured workflow — clarify, plan, execute, verify, deliver
argument-hint: "<task description>"
---

> Entry point — chains to implementation skills. Users invoke this directly.

<HARD-GATE>
Do NOT skip, reorder, merge, or abbreviate pipeline steps. Every gate is mandatory
regardless of task complexity, how much context was provided upfront, or how
well-specified the request appears. Well-specified input is raw material for the
brief — it is not approval of the brief.
</HARD-GATE>

## Common Rationalizations — All Wrong

| Thought | Reality |
|---------|---------|
| "The task is fully specified — I can skip the brief" | A detailed description is input *to* the brief, not approval *of* it. |
| "I already know what the plan will be" | Present it and wait. The gate exists to catch assumptions. |
| "The human seems ready to go" | Wait for an explicit "yes" on the brief and plan. Eagerness is not approval. |
| "Verification/review would just confirm it's fine" | Run them anyway. If fine, cost is one minute. If not, you prevented a bad delivery. |

# /superpowers

## Pipeline

Load and invoke the following internal skills in order:

```
brainstorming → context-awareness → planning → parallel-execution → verification → output-review → sensitivity-guard → delivery
```

Cross-cutting skills active throughout: `connector-orchestration`, `error-handling`, `artifact-management`.

## Invoke: @$1

If no argument, ask: "What would you like to work on?" Do not proceed until you have a task.

## Steps

1. **Clarify.** You MUST invoke the `brainstorming` skill now. Ask questions to understand the task. Produce a brief with success criteria. Present the brief. **Do not proceed to step 2 until the human explicitly approves the brief.**

> **GATE 1 — Do not proceed to step 2 until the human has said yes to the brief.**

2. **Context check.** You MUST invoke the `context-awareness` skill now for only the connector categories named in the brief. Report what's available.

> **GATE 2 — Do not proceed to step 3 until the context report is complete and any critical gaps are resolved or explicitly waived.**

3. **Plan.** You MUST invoke the `planning` skill now. Break the task into numbered steps with tool assignments and "done when" criteria. Present the plan. **Do not begin execution until the human explicitly approves the plan.**

> **GATE 3 — Do not proceed to step 4 until the human has said yes to the plan.**

4. **Execute.** You MUST invoke the `parallel-execution` skill now. Work through the plan. Use subagents for parallel steps. Track progress against the plan.

> **GATE 4 — Do not proceed to step 5 until all planned execution steps are complete (or have been explicitly waived).**

5. **Verify.** You MUST invoke the `verification` skill now. Check every output against the success criteria from the brief. Flag issues. **Do not proceed if verification fails — report and ask how to handle.**

> **GATE 5 — Do not proceed to step 6 until all critical verification issues are resolved or explicitly waived by the human.**

6. **Review.** You MUST invoke the `output-review` skill now. Pre-delivery audit: completeness, accuracy, structure, tone. **Do not proceed if critical issues found — stop and ask how to handle.**

> **GATE 6 — Do not proceed to step 7 until the human directs you to.**

7. **Sensitivity check.** You MUST invoke the `sensitivity-guard` skill now. Present: "This output includes data from [sources]. Going to [destination]. Anything look off?" **Do not deliver until the human signs off.**

> **GATE 7 — Do not proceed to step 8 until the human explicitly signs off on the sensitivity check.**

8. **Deliver.** You MUST invoke the `delivery` skill now. Choose format and destination. State exactly what will be sent, where, and in what format. **Do not execute delivery until the human explicitly confirms.**
