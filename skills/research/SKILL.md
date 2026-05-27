---
name: research
description: Structured research session with source evaluation and synthesis
argument-hint: "<research topic or question>"
---

> Entry point — chains to implementation skills. Users invoke this directly.

# /research

## Pipeline

Load and invoke the following internal skills in order:

```
context-awareness → research-methodology → verification → output-review → sensitivity-guard → delivery
```

## Invoke: @$1

If no argument, ask: "What would you like to research?" Do not proceed until you have a topic.

## Steps

1. **Context check.** You MUST invoke the `context-awareness` skill now for research sources named or implied by the topic.
2. **Define questions.** Refine the topic into specific, answerable questions. **Do not proceed to step 3 until the human confirms the questions are right.**
3. **Research.** You MUST invoke the `research-methodology` skill now. Search from multiple angles. Evaluate sources. Extract findings with citations.
4. **Verify.** You MUST invoke the `verification` skill now. Source-check every claim. Flag conflicts between sources. **Do not proceed if key claims cannot be verified — report gaps and ask how to handle.**
5. **Review.** You MUST invoke the `output-review` skill now. Audit the synthesis for completeness and accuracy. **Do not proceed if critical gaps or errors are found — report and ask how to handle.**
6. **Sensitivity check.** You MUST invoke the `sensitivity-guard` skill now. Flag any findings that include PII or data inappropriate for the intended recipient. **Do not deliver until the human signs off.**
7. **Deliver.** You MUST invoke the `delivery` skill now. Present findings with citations. State the destination explicitly. **Do not execute delivery until the human explicitly confirms.**
