---
name: plan
description: Break a complex task into structured steps with checkpoints
argument-hint: "<task description>"
---

> Entry point — chains to implementation skills. Users invoke this directly.

# /plan

## Pipeline

Load and invoke the following internal skills in order:

```
brainstorming → context-awareness → planning
```

After the plan is approved, hand off to execution.

## Invoke: @$1

If no argument, ask: "What would you like to plan?" Do not proceed until you have a task.

## Steps

1. **Clarify.** You MUST invoke the `brainstorming` skill now. Understand the task. Produce a brief with scope and success criteria. Present the brief. **Do not proceed to step 2 until the human explicitly approves the brief.**
2. **Context check.** You MUST invoke the `context-awareness` skill now for only the connector categories named in the brief. Report what's available.
3. **Plan.** You MUST invoke the `planning` skill now. Break into numbered steps with "done when" criteria and checkpoints. Assign tools to each step.
4. **Present the plan.** **Do not begin executing until the human explicitly approves, adjusts, or rejects the plan. Wait.**
