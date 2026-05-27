---
name: data-transformation
description: "[INTERNAL ONLY] Move data between tools and formats, track lineage, and validate after each transformation."
---

# Data Transformation

Getting data from where it is to where it needs to be, in the right format, without losing or corrupting it.

1. **Record the source.** Before touching any data, log: what tool it came from, when it was retrieved, and what format it's in. This is required — verification cannot proceed without it.
2. **Transform one step at a time.** Each step has a single, named action (extract, clean, convert, combine, analyze).
3. **Validate after each step.** Check: did we lose data? Did formats shift? Row counts, column counts, spot-check values. **Hard stop: if validation fails, fix it before proceeding. Do not compound errors across steps.**
4. **Update lineage at each step.** At every transformation, append to the lineage log: what changed, what the input was, what the output is.
5. **Deliver with the lineage log.** The final output includes or references the lineage trail. The human can verify any claim back to its source.

For the three common patterns (email attachment → spreadsheet, multiple sources → combined analysis, research → presentation), read `references/transformation-patterns.md`.
