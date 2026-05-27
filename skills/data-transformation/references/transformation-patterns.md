# Data Transformation Patterns

## Data Lineage Log

At every step, append an entry:

```
Step N — [Action]
  Input: [source tool / file name / query used], retrieved [date/time]
  Output: [file name / format / row count]
  Changes: [what was normalized, filtered, or restructured]
```

This log is required. Verification cannot proceed without it.

---

## Pattern 1: Email Attachment → Spreadsheet

**Lineage entry required at each numbered step.**

1. Search for the email via Gmail MCP (`search_threads` + `get_thread`). Log: thread ID, sender, date.
2. Download the attachment. Log: filename, size, format.
3. If PDF: extract tables using the pdf skill. Log: pages processed, tables extracted, row count.
4. Clean and structure the data: normalize column headers, fix date formats, remove empty rows. Log: what was changed and why.
5. Validate: row count matches the source (±0 unless rows were explicitly filtered). Column headers match expectations. No empty cells where data should be.
   **Hard stop: if row count is off or structure looks wrong, fix before writing output.**
6. Write to xlsx via xlsx skill. Log: output filename, sheet count, final row count.

---

## Pattern 2: Multiple Sources → Combined Analysis

**Lineage entry required at each numbered step.**

1. Pull data from each source (web, email, Drive, etc.) in parallel. Log each source: tool used, query, retrieval date, item count.
2. Normalize formats: align date formats, currency units, measurement units across all sources. Log: what was standardized and to what format.
3. Combine into a single dataset. Log: how records from each source were merged (join key, union, manual mapping).
4. Validate: no duplicate records (check join key), no missing fields on required columns, totals reconcile across sources.
   **Hard stop: resolve duplicates and missing fields before analysis.**
5. Analyze and (if needed) visualize. Log: what analysis was performed, what tool.
6. Summarize findings with source attribution on every claim.

---

## Pattern 3: Research → Presentation

**Lineage entry required at each numbered step.**

1. Gather research findings (text + data) via research-methodology skill. Log: sources used, claims extracted, gaps noted.
2. Map findings to key points: each slide or section should be one clear, supportable claim.
3. Check coverage: does every key point have a cited source in the lineage log? If not, note it as "assertion — needs verification."
4. Create visualizations from data (charts, tables). Log: data source and date for each visual.
5. Build deck via pptx skill. Every data visual includes its source label.
6. Validate: all key points traceable to logged sources. No slide makes a claim that isn't backed.

---

## Validation Checklist (after every transformation)

- **Lost data?** Row/record counts before and after match (or difference is explained).
- **Format shift?** Dates are still dates. Numbers are still numbers. Currency symbols intact.
- **Structure correct?** Headers in place. Data aligned to correct columns. No truncated values.
- **Lineage updated?** Step entry added to the log before moving on.
