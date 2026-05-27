---
name: context-awareness
description: "[INTERNAL ONLY] Survey relevant connected tools and missing data after brief approval so plans account for access gaps."
---

# Context Awareness

Runs AFTER the brief is approved, chained by the command-skill. Scans only the connector categories named or implied by the approved brief — not a broad inventory of all available tools.

1. **Identify relevant connectors.** From the approved brief, determine which connector categories are needed. Check only those. Reference categories include: email (e.g., Gmail MCP), calendar (e.g., Google Calendar MCP), file storage (e.g., Google Drive MCP), project management (e.g., Linear/Asana/Jira), chat (e.g., Slack), iMessage, Apple Notes, GitHub, and any others explicitly required by the brief.
2. **Check always-available tools.** Web search, web fetch, browser control, and desktop/computer use require no connector. File creation tools (Word, PowerPoint, Excel, PDF) and scheduled-tasks (cron) are available locally. Note which apply to this task.
3. **Check what the human must provide.** Identify data the task needs that no tool can supply (e.g., private documents, internal data, context only the human has).
4. **Report in 2-3 sentences.** Example: "You have email and calendar connected. File storage is not connected, so I'll need you to share documents directly. Web search, file creation, and scheduled tasks are available."
5. **Hard stop on critical gaps.** If a connector category the task fundamentally requires is unavailable, do not proceed. Say: "This task needs [category] but it's not connected. Options: (A) connect it, (B) provide the data manually, (C) continue with reduced scope." Wait for the human's choice before continuing.
6. **Adjust the plan.** If gaps are minor or fallbacks exist, note them and proceed. Pass the environment summary to the `planning` skill so the plan accounts for what's actually available.

**Handoff stop:** When the context report is complete and gaps are resolved (or waived), report the summary to the human. **Do not invoke the `planning` skill until the human acknowledges the context report and confirms they want to proceed.**
