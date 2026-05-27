---
name: superpowers
description: Full structured workflow — clarify, plan, execute, verify, deliver
argument-hint: "<task description>"
---

> Entry point — chains to implementation skills. Users invoke this directly.

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
2. **Context check.** You MUST invoke the `context-awareness` skill now for only the connector categories named in the brief. Report what's available.
3. **Plan.** You MUST invoke the `planning` skill now. Break the task into numbered steps with tool assignments and "done when" criteria. Present the plan. **Do not begin execution until the human explicitly approves the plan.**
4. **Execute.** You MUST invoke the `parallel-execution` skill now. Work through the plan. Use subagents for parallel steps. Track progress against the plan.
5. **Verify.** You MUST invoke the `verification` skill now. Check every output against the success criteria from the brief. Flag issues. **Do not proceed if verification fails — report and ask how to handle.**
6. **Review.** You MUST invoke the `output-review` skill now. Pre-delivery audit: completeness, accuracy, structure, tone. **Do not proceed if critical issues found — stop and ask how to handle.**
7. **Sensitivity check.** You MUST invoke the `sensitivity-guard` skill now. Present: "This output includes data from [sources]. Going to [destination]. Anything look off?" **Do not deliver until the human signs off.**
8. **Deliver.** You MUST invoke the `delivery` skill now. Choose format and destination. State exactly what will be sent, where, and in what format. **Do not execute delivery until the human explicitly confirms.**
