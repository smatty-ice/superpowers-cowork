---
name: sensitivity-guard
description: "[INTERNAL ONLY] Checkpoint before sharing cross-source data; flag PII, destination risk, and context collapse."
---

# Sensitivity Guard

**One rule:** The human decides what is sensitive — never the agent. The steps below enforce that rule.

Before showing or sending any output that combines data from multiple sources or apps:

1. Name the sources: "This output includes data from [email / calendar / Drive / web]."
2. Name the destination: "It's going to [person / folder / external service]."
3. Check for these conditions — each makes step 4 mandatory:
   - PII in the output (names, addresses, account numbers)
   - Internal data (calendar entries, personal emails) going to an external recipient
   - Data from one app appearing in a context where it doesn't belong
4. Ask: "Does that look appropriate?" **Wait for explicit confirmation. Do not proceed without it.**
