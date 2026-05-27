# Dispatch Guide

## Dispatch Prompt Template

Use this template when composing the prompt for each Agent tool call. Fill in the bracketed sections exactly — subagents have no other context.

```
## Your assignment

**Brief:**
[Paste the full brief here — goals, audience, format, success criteria, constraints.]

**Your specific step:**
[Copy the step title and instructions from the plan exactly.]

**Tools to use:**
[List each tool or MCP connector the step requires, e.g. WebSearch, Gmail MCP, docx skill.]

**Done when:**
[Copy the "done when" criterion from the plan step verbatim.]

## Instructions

Complete only the step above. Do not attempt other steps. Return your output directly — do not summarize what you did, just return the result.
```

## Dispatch Process

For each parallel step:
1. Fill in the template above with the brief, the specific step, the tools, and the "done when" criterion.
2. Dispatch via the Agent tool. Each subagent works independently.
3. When results return, run the two-stage review.

## Two-Stage Review

### Stage 1: Completeness
Does the output cover everything the step asked for? Check against the "done when" criterion.
See `completeness-reviewer.md` for the review prompt.

### Stage 2: Quality
Is the output accurate, well-structured, and appropriately toned for the audience?
See `quality-reviewer.md` for the review prompt.

If either stage fails, provide specific feedback and re-dispatch the task.

## Merge Strategy

When parallel results need to be combined:
1. Collect all parallel outputs
2. Check for contradictions between outputs
3. Synthesize into a unified result, noting where sources agree and disagree
4. The merge step is always sequential

## Sequential Fallback

If subagents are not available:
1. Execute steps sequentially in the current session
2. After each step, briefly verify the output before moving on
3. Group steps into batches of 2-3, with a checkpoint after each batch

## Progress Tracking

Use the task system to track each parallel step. Mark tasks as in_progress when dispatched, completed when the two-stage review passes.
