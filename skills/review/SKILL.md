---
name: review
description: Audit an artifact against its goals before sharing
argument-hint: "<artifact or context about what to review>"
---

> Entry point — chains to implementation skills. Users invoke this directly.

# /review

## Pipeline

Load and invoke the following internal skills in order:

```
output-review → verification → sensitivity-guard
```

## Invoke: @$1

If no artifact provided, ask: "What would you like me to review?" Do not proceed until you have the artifact.

## Steps

1. **Review.** You MUST invoke the `output-review` skill now. Audit the artifact: completeness, accuracy, structure, tone, format.
2. **Fact-check.** You MUST invoke the `verification` skill now. Verify factual claims against available sources.
3. **Sensitivity check.** You MUST invoke the `sensitivity-guard` skill now. Flag private or sensitive data inappropriate for the intended recipient.
4. **Report.** Present findings by severity: critical / moderate / minor. **Do not proceed past critical issues — stop and ask how to handle before continuing.**
