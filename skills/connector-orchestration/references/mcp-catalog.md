# MCP Tool Catalog

## Email
- **Primary:** Gmail MCP — `search_threads`, `get_thread`, `create_draft`, `list_drafts`, `label_message`, `label_thread`, `unlabel_message`, `unlabel_thread`, `list_labels`, `create_label`, `update_label`, `delete_label`
- **Fallback:** Browser automation to Gmail/Outlook (via Chrome MCP)
- **Use for:** Reading email, searching threads, drafting replies, finding attachments, organizing with labels

## Calendar
- **Primary:** Google Calendar MCP — `list_events`, `list_calendars`, `get_event`, `create_event`, `update_event`, `delete_event`, `respond_to_event`, `suggest_time`
- **Fallback:** Browser automation to Google Calendar (via Chrome MCP)
- **Use for:** Checking schedules, creating events, finding meeting context, scheduling time

## File Storage
- **Primary:** Google Drive MCP — `search_files`, `read_file_content`, `create_file`, `copy_file`, `download_file_content`, `get_file_metadata`, `get_file_permissions`, `list_recent_files`
- **Fallback:** Browser automation to Drive/Dropbox (via Chrome MCP)
- **Use for:** Finding documents, reading shared files, uploading deliverables, sharing

## Web Research
- **Primary:** WebSearch + WebFetch (always available)
- **Fallback:** Browser automation to search engines
- **Use for:** Researching topics, finding sources, checking facts

## Document Creation
- **docx skill:** Reports, memos, letters, formal documents
- **pptx skill:** Presentations, slide decks
- **xlsx skill:** Spreadsheets, data analysis, budgets
- **pdf skill:** Fixed-layout documents, printables
- **No fallback** — these skills are always available

## Scheduled Tasks
- **Primary:** Scheduled tasks MCP — `create_scheduled_task`, `list_scheduled_tasks`, `update_scheduled_task`
- **Use for:** Recurring workflows, timed delivery, reminders

## Native macOS Apps (Cowork only)
- **Apple Notes:** `add_note`, `list_notes`, `get_note_content`, `update_note_content`
- **iMessage:** `send_imessage`, `read_imessages`, `get_unread_imessages`, `search_contacts`
- **Use for:** Personal note-taking, messaging contacts, capturing quick thoughts

## Browser Automation
- **Claude in Chrome MCP:** `navigate`, `read_page`, `form_input`, `javascript_tool`, `find`, `read_console_messages`, `read_network_requests`
- **Use for:** Any web app without a dedicated MCP
- **Note:** Requires the Chrome extension to be connected — ask the human to install it if unavailable

## Everything Else
- **Desktop control (computer-use MCP):** Native macOS apps without a dedicated MCP (System Settings, Finder, third-party apps). Tier restrictions apply (browsers/IDEs/terminals are limited).
- **Ask the human:** When no automated path exists
