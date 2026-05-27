# Planning Guide

## Plan Template

```markdown
## Plan: [Task Title]

**Brief:** [Reference to the brief]
**Steps:** [N]
**Estimated effort:** [light | medium | heavy]

### Step 1: [Action]
- **Do:** [What to do]
- **Tools:** [Which MCP connectors or skills to use]
- **Done when:** [Specific, verifiable criterion]
- **Effort:** light

### Step 2: [Action]
...

### Checkpoint: [Review point]
- **Review:** [What the human should check]
- **Before continuing:** [What needs approval]
```

## Task Granularity

Each step should be one coherent action:
- "Research competitor X's pricing via web search"
- "Draft the executive summary section"
- "Format the findings as a pptx deck"

## Tool Mapping

Every step that uses an external tool specifies which one:
- "Search for recent articles" → WebSearch
- "Pull emails about the topic" → Gmail MCP
- "Check calendar for meeting context" → Calendar MCP
- "Create the final report" → docx skill

## Dependencies and Parallelism

Mark parallel steps:

```markdown
### Steps 2-4 (parallel)
These three research steps are independent. Run simultaneously if subagents are available.

### Step 5: Merge research findings
- **Depends on:** Steps 2-4
```

## Effort Estimation

- **Light:** A few minutes. Simple lookup, quick draft, format conversion.
- **Medium:** 10-20 minutes. Research with multiple sources, drafting with iteration, data analysis.
- **Heavy:** 30+ minutes. Deep research, complex multi-source synthesis, large document creation.

These are rough guides for the human's expectations, not commitments.
