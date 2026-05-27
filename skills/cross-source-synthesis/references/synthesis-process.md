# Cross-Source Synthesis Process

## 1. Survey Available Sources

Before pulling any data, establish scope:
- What tools are connected? (email, calendar, Drive, web)
- What time range is relevant? (last week, last month, specific dates)
- What topic or question is being answered?

Do not begin pulling until you can name each source you'll query and the specific time window.

## 2. Pull in Parallel

Issue all source queries simultaneously. For each source, define a specific query — no open-ended browsing:

| Source | Query template |
|--------|---------------|
| Email | "threads mentioning [topic] from [date] to [date]" — use `search_threads` |
| Calendar | "events related to [topic] between [date] and [date]" — use `list_events` |
| Drive | "documents containing [topic], modified after [date]" — use `search_files` + `read_file_content` |
| Web | "[topic] [specific angle]" — use WebSearch + WebFetch on top 2-3 results |

For each source, log:
- What query was issued
- What was returned (or "no results")
- Date/timestamp of the most recent item found

## 3. Reconcile Conflicts

After pulling, compare what each source says about the same facts (dates, numbers, decisions, names).

**When sources conflict, do not silently pick one. Present both and ask:**

> "Source A ([email from Jane, May 20]) says X. Source B ([calendar event, May 21]) says Y. Which is correct?"

**Do not proceed until the human answers.** This is a mandatory checkpoint — synthesizing over an unresolved conflict produces a subtly wrong output that is worse than an incomplete one.

Common conflict types to check:
- Dates/deadlines (email says one date, calendar says another)
- Numbers (budget, quantities, headcount)
- Decisions or commitments (one source says yes, another is silent or says no)
- Names and spellings

## 4. Synthesize with Attribution

Produce a unified summary. Every claim must name its source and date:

- "Based on your email with [person] on [date], the project status is..."
- "Your calendar shows [N] related meetings this week, including [event name] on [date]."
- "The most recent document on Drive ([filename], last modified [date]) says..."

No unattributed assertions. If you can't name the source for a claim, don't include the claim.

## 5. Highlight Gaps

End every synthesis with an explicit gaps section:

> "I checked email, calendar, and Drive, but couldn't find [specific missing information]. You may want to check [alternative source] or ask [person]."

Do not omit this step even when the synthesis appears complete. Absence of a gap section suggests nothing was missing — which is rarely true and erodes trust when something turns up later.
