---
name: recurring-work
description: "[INTERNAL ONLY] Prepare repeating workflows with brief, plan, tools, schedule, verification, and delivery instructions."
---

# Recurring Work

1. **Check if recurring is appropriate.** Good candidates: weekly summaries, monthly analyses, daily digests, periodic reports. Do not suggest for one-off tasks, exploratory research, or unique deliverables. **If not a good candidate, stop here.**
2. Propose: "This looks like something that could run on a schedule. Want me to set it up to run [weekly on Mondays / daily at 9am / monthly on the 1st]?"
3. **Stop. Do not configure anything until the human confirms.**
4. Capture from the current session:
   - The brief (what the task produces, for whom, in what format)
   - The plan (steps to follow)
   - The tools (which connectors are used)
   - The schedule (cron expression or plain-language frequency)
5. Create a scheduled task or Dispatch-ready recurring task with the full workflow prompt and schedule.
6. Confirm: "Scheduled. It will run [description] and deliver to [destination]."

Recurring runs re-execute, re-verify, and re-deliver. They do not repeat blindly — if sources or data change, the output should reflect that.

For full setup details, see `references/setup-guide.md`.
