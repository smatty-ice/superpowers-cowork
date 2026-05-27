---
name: deliver
description: Format, verify completeness, and send/save/schedule an output
argument-hint: "<delivery instructions: what, where, format>"
---

> Entry point — chains to implementation skills. Users invoke this directly.

# /deliver

## Pipeline

Load and invoke the following internal skills in order:

```
sensitivity-guard → format-selection → artifact-management → delivery
```

## Invoke: @$1

If no argument, ask: "What would you like to deliver, and where should it go?" Do not proceed until you have both the content and the destination.

## Steps

1. **Sensitivity check.** You MUST invoke the `sensitivity-guard` skill now. Present: "Sending [content] to [destination]. Anything look off?" **Do not proceed until the human signs off.**
2. **Format.** You MUST invoke the `format-selection` skill now. Convert to the right format if needed (docx, pptx, xlsx, pdf, email draft).
3. **Artifact management.** You MUST invoke the `artifact-management` skill now. Organize files, track versions if applicable.
4. **Confirm destination.** State exactly what will be delivered, where, and in what format. **Do not execute delivery until the human confirms.**
5. **Deliver.** You MUST invoke the `delivery` skill now. Execute: save, email, upload, schedule, or create the artifact.
6. **Summary.** Confirm what was delivered, where, and in what format.
