---
name: draft
description: Managed drafting workflow with iteration and feedback loops
argument-hint: "<what to draft>"
---

> Entry point — chains to implementation skills. Users invoke this directly.

# /draft

## Pipeline

Load and invoke the following internal skills in order:

```
brainstorming → planning → communication-drafting → draft-iteration → verification → sensitivity-guard → delivery
```

## Invoke: @$1

If no argument, ask: "What would you like to draft?" Do not proceed until you have a drafting task.

## Steps

1. **Clarify scope.** You MUST invoke the `brainstorming` skill now. Ask: audience, tone, format, length. Produce a brief. Present it. **Do not proceed to step 2 until the human explicitly confirms the brief.**
2. **Outline.** You MUST invoke the `planning` skill now. Create a structure. Present it. **Do not begin drafting until the human explicitly approves the outline.**
3. **Draft.** You MUST invoke the `communication-drafting` skill now. Write the first version.
4. **Iterate.** You MUST invoke the `draft-iteration` skill now. Present the draft. Get feedback. Revise. Repeat. **Claude does not decide when the draft is done — only the human does. Keep iterating until the human says it's ready.**
5. **Verify.** You MUST invoke the `verification` skill now. Final check against the brief from step 1.
6. **Sensitivity check.** You MUST invoke the `sensitivity-guard` skill now. Flag any sensitive data or PII that should not appear in the final draft or its destination. **Do not deliver until the human signs off.**
7. **Deliver.** You MUST invoke the `delivery` skill now. State the destination and format explicitly. **Do not execute delivery until the human explicitly confirms.**
