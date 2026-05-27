---
name: brief
description: Cross-source synthesis — pull context from email, calendar, and docs
argument-hint: "<topic or question to brief on>"
---

> Entry point — chains to implementation skills. Users invoke this directly.

# /brief

## Pipeline

Load and invoke the following internal skills in order:

```
context-awareness → cross-source-synthesis → verification → output-review → sensitivity-guard → delivery
```

## Invoke: @$1

If no argument, ask: "What would you like me to brief you on?" Do not proceed until you have a topic.

## Steps

1. **Context check.** You MUST invoke the `context-awareness` skill now for the source categories needed by the topic. Report available sources.
2. **Pull from sources.** You MUST invoke the `cross-source-synthesis` skill now. Search email, calendar, Drive, and web for relevant information. Weave information into a coherent summary with source attribution.
3. **Verify.** You MUST invoke the `verification` skill now. Flag any conflicts between sources. **Do not proceed if critical conflicts exist — report and ask how to handle.**
4. **Review.** You MUST invoke the `output-review` skill now. Ensure the brief is complete and accurate.
5. **Sensitivity check.** You MUST invoke the `sensitivity-guard` skill now. Present: "This brief combines data from [sources]. Going to [destination]. Anything look off?" **Do not deliver until the human signs off.**
6. **Deliver.** You MUST invoke the `delivery` skill now. Present the brief. State the destination explicitly. **Do not execute delivery until the human explicitly confirms.**
