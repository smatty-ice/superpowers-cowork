---
name: format-selection
description: "[INTERNAL ONLY] Choose output format and invoke the matching file skill for docs, slides, sheets, PDF, HTML, or markdown."
---

# Format Selection

1. Read the format requirement from the brief. If the brief is silent or ambiguous, ask the human and **do not proceed until the format is confirmed.**
2. Map the deliverable to a format using the table below.
3. Invoke the matching file skill with the content and any formatting constraints from the brief.
4. Verify the conversion preserved all content, structure, and visuals before handing back to `delivery`.

| Deliverable type | Format | Tool / Skill |
|-----------------|--------|--------------|
| Report, memo, letter, narrative document | .docx | `docx` skill |
| Presentation, slide deck, pitch | .pptx | `pptx` skill |
| Data, analysis, model, budget, tabular | .xlsx | `xlsx` skill |
| Printable, fixed-layout, signed | .pdf | `pdf` skill |
| Rich in-conversation preview, interactive | HTML artifact | Native artifact tool |
| Quick share, notes, code, lightweight | Markdown | Native (no conversion) |
| Email to a recipient | Email draft | ~~email (e.g., Gmail MCP) `create_draft` |

**Ambiguity defaults:** "Report" without context → .docx. "Analysis" → .xlsx if data-heavy, .docx if narrative. "Share with the team" → ask whether the destination is email, Drive, or artifact before picking format.

**Do not silently downgrade.** If the brief says .pptx and the content is not slide-shaped, surface the mismatch before converting — do not proceed until the human resolves it.
