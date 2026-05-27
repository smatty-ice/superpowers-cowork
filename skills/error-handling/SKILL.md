---
name: error-handling
description: "[INTERNAL ONLY] Recover from tool failures with one retry, fallback, or human choice. Cross-cutting skill."
---

# Error Handling

When a tool fails:
1. **Retry once.** If the error is permanent (tool not found, permission denied, not connected), skip to step 2 immediately — do not retry.
2. **Try the fallback.** Browser automation if the primary was MCP. Desktop control if browser automation also fails.
3. **If all fallbacks fail, present options:** "(A) wait and retry, (B) skip this step and work from what you tell me." Do not proceed until the human picks one.

**Never silently swallow failures.** Even when you recover via fallback, tell the human: "The email connector failed, so I used browser automation instead."

**One retry maximum.** Retrying forever stalls the workflow.
