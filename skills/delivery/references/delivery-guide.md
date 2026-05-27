# Delivery Guide

## Destination Options

1. **Save to folder** — Write to a specific folder or path on disk
2. **Email it** — Create a draft via the Gmail MCP (`create_draft`) for human review and send
3. **Upload to cloud storage** — Push to Google Drive via the Drive MCP
4. **Schedule for later** — Use `create_scheduled_task` to deliver at a specific time (hand off to `recurring-work`)
5. **Create a persistent artifact** — Save as a conversation artifact for in-thread reference
6. **Discard** — Human changed their mind. Clean up working files and stop.

## Format Conversion

If the output was produced in markdown but the brief specifies a different format:
1. Invoke format-selection to pick the right file skill
2. Convert the content
3. Verify the conversion preserved structure and content
4. Deliver the formatted file

## Recurring Check

After delivery: "Is this something that should happen again?" Good candidates: weekly summaries, monthly analyses, daily digests. If yes, hand off to recurring-work.

## Delivery Summary

After delivery, provide:

```markdown
**Delivered:** [What]
**To:** [Where]
**Format:** [docx / email draft / etc.]
**Brief:** [One-line reminder of the goal]
```
