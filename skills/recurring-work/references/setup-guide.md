# Recurring Work Setup Guide

## What to Capture

1. **The brief** — what the task produces, for whom, in what format
2. **The plan** — what steps to follow
3. **The tools** — which connectors to use
4. **The schedule** — how often (daily, weekly, monthly, custom)

## Setting Up

Call Cowork's `create_scheduled_task` tool with:
- The full workflow prompt (brief + instructions)
- The cron schedule
- A descriptive name

The recurring run should follow the same methodology. It re-executes, re-verifies, and re-delivers. If sources have changed or data is different, the output should reflect that.

## Presenting to the Human

"This looks like something that could run on a schedule. Want me to set it up to run [weekly on Mondays / daily at 9am / monthly on the 1st]?"

If yes, configure and confirm: "Scheduled. It will run [description] and deliver to [destination]."
