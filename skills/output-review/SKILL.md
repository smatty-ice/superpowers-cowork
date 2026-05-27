---
name: output-review
description: "[INTERNAL ONLY] Audit an artifact against its brief for completeness, accuracy, structure, tone, and format."
---

# Output Review

1. Load the brief (goals, audience, success criteria). **If no brief exists, ask the human to describe the goal. Do not proceed until you have it.**
2. Review all five dimensions against the artifact (see `references/review-dimensions.md`): completeness, accuracy, structure, tone, format.
3. Classify every finding as **Critical**, **Moderate**, or **Minor**.
4. Report in this format: "Review complete. [N critical / N moderate / N minor]. [Summary of top issues]."
5. **Stop. Do not proceed to the sensitivity check or delivery until the human directs the next step.**
6. If any **Critical** findings exist, list each one and ask how the human wants to handle it. Delivery does not happen until critical issues are resolved or explicitly waived.
7. If only **Moderate/Minor** findings exist, present the list and ask: "Should I fix these before delivering, or send as-is?"
8. For high-stakes deliverables, run an independent pass: dispatch a subagent with only the brief, the artifact, and `references/review-dimensions.md` — no process context. Reconcile its findings with yours before reporting.
