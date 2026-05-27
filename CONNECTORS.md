# Connectors

## How tool references work

Plugin files use `~~category` as a placeholder for whatever tool the user connects in that category. For example, `~~email` might mean Gmail, Outlook, or any other email provider with an MCP server.

This plugin is **tool-agnostic** — it describes workflows in terms of categories rather than specific products.

## Connectors for this plugin

| Category | Placeholder | Primary servers | Alternatives |
|----------|------------|----------------|-------------|
| Email | `~~email` | Gmail | Outlook |
| Calendar | `~~calendar` | Google Calendar | Outlook Calendar |
| File storage | `~~file storage` | Google Drive | Dropbox, OneDrive |
| Job search | `~~job search` | (if available) | Browser automation |

## Tools always available

These don't require connector setup:

| Tool | What it does |
|------|-------------|
| WebSearch / WebFetch | Research any topic on the web |
| Browser automation | Interact with any web app |
| Desktop control | Control native macOS apps |
| File skills (docx/pptx/xlsx/pdf) | Create formatted documents |
| Scheduled tasks | Set up recurring workflows |
