---
name: connector-orchestration
description: "[INTERNAL ONLY] Choose tools for each step: MCP, browser, desktop, then human fallback. Cross-cutting skill."
---

# Connector Orchestration

At each workflow step that needs an external tool:

1. **Check for MCP connector.** If one exists for this task, use it.
2. **Fall back to browser automation.** If MCP fails or isn't connected.
3. **Fall back to desktop control.** For native macOS apps with no web interface.
4. **Ask the human.** If all automated paths fail: "I need [X] but don't have access. Can you paste the relevant information?" Do not proceed until they respond.

**Retry policy:** Before escalating to the next tier, apply the error-handling retry policy — retry once if the failure is transient, skip retries if the tool is unavailable.

**When falling back:** Always tell the human. "The email connector was not responding, so I used browser automation instead." Never silently skip a step because a tool isn't available.

**When chaining across tools** (e.g. email attachment → spreadsheet): you must record where each piece of data came from. This note is required — verification cannot proceed without it.

For the full tool table: `references/mcp-catalog.md`.
