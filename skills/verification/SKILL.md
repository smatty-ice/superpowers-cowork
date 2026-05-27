---
name: verification
description: "[INTERNAL ONLY] Verify outputs against the brief's success criteria and block delivery on critical issues."
---

# Verification

Verify phase. The brief defined "done." Check whether we got there.

1. Load the brief's success criteria. **If no brief exists or success criteria are missing, stop and ask the human to supply them before continuing. Do not proceed without criteria to check against.**
2. If the success criteria are vague ("make it good"), flag that immediately and ask for specifics. Verification only works when criteria are specific enough to check.
3. Check each criterion against the output.
4. Run type-specific checks from `references/verification-checklist.md`.
5. Flag issues by severity: **Critical** (blocks delivery), **Moderate** (worth fixing), **Minor** (optional polish).
6. Fix moderate/minor yourself.
7. **For any Critical issue: stop. Explain the issue clearly. Do not proceed to delivery until all critical issues are resolved.**

**Terminal stop:** After completing all checks and fixing any moderate/minor issues, report findings to the human: "Verification complete. [N critical / N moderate / N minor]. [Summary]." **Stop. Do not proceed to output-review until the human directs the next step. If any critical issues exist, do not proceed at all — wait for the human to decide how to handle them.**
