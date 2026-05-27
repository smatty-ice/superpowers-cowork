---
name: methodology-core
description: "[INTERNAL ONLY] Behavioral contract for superpowers workflows: phases, checkpoints, brief template, and cross-cutting skills."
---

# Methodology Core

The methodology: stop before acting, plan before executing, verify before declaring done, and checkpoint with the human.

## The Pipeline

1. **Clarify** — Understand the task, define success criteria, and produce a brief.
2. **Plan** — Break work into steps, assign tools, mark dependencies, and present the plan.
3. **Execute** — Do the work, using parallel workstreams when available.
4. **Verify** — Check outputs against the brief's success criteria and flag issues by severity.
5. **Deliver** — Choose format and destination, run sensitivity checks, and ship after confirmation.

## Checkpoint Protocol

1. **After Clarify:** Present the brief. Ask: "Does this capture what you need?" **Do not plan until approved.**
2. **After Plan:** Present the plan. Ask: "Ready to execute?" **Do not execute until approved.**
3. **After Verify (if critical issues):** List issues. Ask how to handle them. **Do not deliver until resolved.**
4. If the human redirects, update the brief first, then adjust the plan.

## The Brief

```markdown
## Brief: [Task Title]
**Goal:** [One sentence]
**Audience:** [Who will consume the output]
**Deliverable:** [Format and where it goes]
**Success criteria:**
- [What "done" looks like — specific, verifiable]
- [Another criterion]
**Sources:** [What data/tools to use]
**Constraints:** [Deadline, tone, length, etc.]
```

## Cross-Cutting Skills

1. **connector-orchestration** — choose MCP, browser, desktop, or human fallback.
2. **error-handling** — retry once, then fallback, then ask.
3. **artifact-management** — version drafts and track intermediates.
4. **sensitivity-guard** — checkpoint before cross-source delivery.
