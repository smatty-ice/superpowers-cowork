# Superpowers for Cowork — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a Cowork marketplace plugin that brings superpowers methodology (clarify → plan → execute → verify → deliver) to knowledge workers via slash commands.

**Architecture:** Monolithic Cowork plugin with 19 skills (3 tiers), 7 commands, and personal-productivity connectors. Skills are markdown-based behavioral instructions (SKILL.md). Commands are markdown files with YAML frontmatter that orchestrate skill chains. No code dependencies — the entire plugin is markdown + JSON.

**Tech Stack:** Markdown (SKILL.md files), JSON (plugin.json manifest), Cowork plugin format (`.claude-plugin/` convention)

---

## File Structure

```
superpowers-cowork/
├── .claude-plugin/
│   └── plugin.json                          # Plugin manifest
├── CLAUDE.md                                # Bootstrap — command registration
├── CONNECTORS.md                            # Tool-agnostic connector categories
├── README.md                                # User-facing documentation
├── commands/
│   ├── superpowers.md                       # /superpowers — full pipeline
│   ├── plan.md                              # /plan — structured workflow
│   ├── research.md                          # /research — structured research
│   ├── review.md                            # /review — artifact review
│   ├── deliver.md                           # /deliver — delivery checkpoint
│   ├── brief.md                             # /brief — cross-source synthesis
│   └── draft.md                             # /draft — managed drafting
├── skills/
│   ├── methodology-core/SKILL.md            # Behavioral contract
│   ├── brainstorming/SKILL.md               # Scope clarification
│   ├── planning/SKILL.md                    # Task decomposition
│   ├── parallel-execution/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── completeness-reviewer.md
│   │       └── quality-reviewer.md
│   ├── verification/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── verification-checklist.md
│   ├── output-review/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── review-dimensions.md
│   ├── delivery/SKILL.md                    # Delivery decision tree
│   ├── connector-orchestration/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── mcp-catalog.md
│   ├── research-methodology/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── source-evaluation.md
│   ├── recurring-work/SKILL.md
│   ├── format-selection/SKILL.md
│   ├── data-transformation/SKILL.md
│   ├── artifact-management/SKILL.md
│   ├── error-handling/SKILL.md
│   ├── sensitivity-guard/SKILL.md
│   ├── cross-source-synthesis/SKILL.md
│   ├── communication-drafting/SKILL.md
│   ├── draft-iteration/SKILL.md
│   └── context-awareness/SKILL.md
└── docs/
    └── specs/
        └── 2026-05-26-superpowers-cowork-design.md
```

---

## Task 1: Plugin Scaffolding

**Files:**
- Create: `.claude-plugin/plugin.json`
- Create: `CLAUDE.md`
- Create: `CONNECTORS.md`
- Create: `README.md`

- [ ] **Step 1: Create plugin manifest**

Create `.claude-plugin/plugin.json`:

```json
{
  "name": "superpowers-cowork",
  "version": "0.1.0",
  "description": "Structured workflows for knowledge work — stop before acting, plan before executing, verify before declaring done. Brings the superpowers methodology to Cowork via slash commands.",
  "author": {
    "name": "Winfield Smathers"
  }
}
```

- [ ] **Step 2: Create CLAUDE.md bootstrap**

Create `CLAUDE.md`:

```markdown
# Superpowers for Cowork

Structured workflows for knowledge work. Use slash commands to engage:

- `/superpowers` — Full pipeline for complex tasks
- `/plan` — Break a task into structured steps
- `/research` — Structured research with source evaluation
- `/review` — Audit an artifact before delivery
- `/deliver` — Format, verify, and ship
- `/brief` — Cross-source status synthesis
- `/draft` — Managed drafting with iteration

Type `/` to see available commands. The plugin does nothing unless you invoke it.
```

- [ ] **Step 3: Create CONNECTORS.md**

Create `CONNECTORS.md`:

```markdown
# Connectors

## How tool references work

Plugin files use `~~category` as a placeholder for whatever tool the user connects in that category. For example, `~~email` might mean Gmail, Outlook, or any other email provider with an MCP server.

This plugin is **tool-agnostic** — it describes workflows in terms of categories rather than specific products.

## Connectors for this plugin

| Category | Placeholder | Primary servers | Alternatives |
|----------|------------|----------------|-------------|
| Email | `~~email` | Gmail | Outlook |
| Calendar | `~~calendar` | Google Calendar | Outlook Calendar |
| File storage | `~~file storage` | Google Drive | Dropbox, OneDrive |
| Job search | `~~job search` | (if available) | Browser automation |

## Tools always available

These don't require connector setup:

| Tool | What it does |
|------|-------------|
| WebSearch / WebFetch | Research any topic on the web |
| Browser automation | Interact with any web app |
| Desktop control | Control native macOS apps |
| File skills (docx/pptx/xlsx/pdf) | Create formatted documents |
| Scheduled tasks | Set up recurring workflows |
```

- [ ] **Step 4: Create README.md**

Create `README.md`:

```markdown
# Superpowers for Cowork

Structured workflows for knowledge work. The superpowers methodology — stop before acting, plan before executing, verify before declaring done — adapted from software development to everything you do in Cowork.

## Installation

```bash
claude plugins add superpowers-cowork
```

## Commands

| Command | What it does |
|---------|-------------|
| `/superpowers` | Full pipeline — clarify → plan → execute → verify → deliver |
| `/plan` | Break a complex task into structured steps with checkpoints |
| `/research` | Define questions, search, evaluate sources, synthesize findings |
| `/review` | Audit an artifact against its goals before sharing |
| `/deliver` | Pick format, verify completeness, send/save/schedule |
| `/brief` | Pull context from email + calendar + docs into a coherent picture |
| `/draft` | Managed drafting with iteration and feedback loops |

## How it works

Every command follows the same methodology:

1. **Clarify** — What are we doing, for whom, and what does "done" look like?
2. **Plan** — Break it into steps with tools, checkpoints, and success criteria
3. **Execute** — Do the work, using subagents in parallel where possible
4. **Verify** — Check the output against the success criteria defined in step 1
5. **Deliver** — Pick the right format and destination, confirm sensitivity, ship it

Commands enter the pipeline at different points. `/superpowers` runs the full thing. `/review` enters at Verify. `/deliver` enters at Deliver.

## Philosophy

- **Verification-driven work** — Define what "done" looks like before you start. Check against it when you finish. The TDD principle, generalized.
- **Checkpoint with the human** — At every phase transition, stop and confirm. The human can redirect, skip ahead, or abort.
- **Tool-aware execution** — The plugin knows which MCP connectors you have and chooses the right tool for each step.
- **Stays out of the way** — Nothing auto-triggers. If you don't invoke a command, you don't get the methodology.

## Connectors

The plugin works standalone, but gets better with connected tools. See [CONNECTORS.md](CONNECTORS.md) for details.
```

- [ ] **Step 5: Create all skill and command directories**

```bash
mkdir -p superpowers-cowork/.claude-plugin
mkdir -p superpowers-cowork/commands
mkdir -p superpowers-cowork/skills/{methodology-core,brainstorming,planning}
mkdir -p superpowers-cowork/skills/parallel-execution/references
mkdir -p superpowers-cowork/skills/verification/references
mkdir -p superpowers-cowork/skills/output-review/references
mkdir -p superpowers-cowork/skills/{delivery,connector-orchestration/references}
mkdir -p superpowers-cowork/skills/research-methodology/references
mkdir -p superpowers-cowork/skills/{recurring-work,format-selection,data-transformation}
mkdir -p superpowers-cowork/skills/{artifact-management,error-handling,sensitivity-guard}
mkdir -p superpowers-cowork/skills/{cross-source-synthesis,communication-drafting}
mkdir -p superpowers-cowork/skills/{draft-iteration,context-awareness}
```

- [ ] **Step 6: Verify structure**

```bash
find superpowers-cowork -type d | sort
```

Expected: all directories from the file structure above.

---

## Task 2: Skill — methodology-core

**Files:**
- Create: `skills/methodology-core/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/methodology-core/SKILL.md`:

```markdown
---
name: methodology-core
description: Behavioral contract for superpowers workflows. Loaded when any superpowers command activates. Defines the Clarify → Plan → Execute → Verify → Deliver pipeline, checkpoint protocol, and brief format. Do not invoke directly — commands invoke this automatically.
---

# Methodology Core

This skill defines the behavioral contract for all superpowers workflows. When a superpowers command activates, this is the foundation.

## The Pipeline

Every workflow follows five phases. Phases execute in order. The agent checkpoints with the human at every phase transition.

```
Clarify → Plan → Execute → Verify → Deliver
```

**Clarify:** Understand what the human wants. Ask questions. Define success criteria. Produce a brief.

**Plan:** Break the work into steps. Assign tools to each step. Mark dependencies and parallelism. Present the plan for approval.

**Execute:** Do the work. Use subagents for parallel tasks when available. Track progress via the task system.

**Verify:** Check every output against the success criteria from the brief. Flag issues by severity. Fix what you can, escalate what you can't.

**Deliver:** Choose the right format and destination. Run the sensitivity guard. Ship it.

## Phase Discipline

- Phases cannot be skipped, but they can be fast-tracked. If the human's request is clear enough that the Clarify phase would be a formality, say so: "Your request is clear — here's what I understand: [brief]. Want me to skip to planning, or should we refine?"
- The human can redirect at any checkpoint. "Actually, skip the research and just draft something from what you know" is a valid instruction.
- The human can abort at any checkpoint. If they say stop, stop.

## Checkpoint Protocol

At each phase transition, present your work and wait for approval:

- **After Clarify:** "Here's the brief. Does this capture what you need?"
- **After Plan:** "Here's the plan — [N] steps, estimated [effort]. Ready to execute?"
- **After Execute:** "Work complete. Running verification next."
- **After Verify:** "Verification done. [N] issues found. Here's the review."
- **After Deliver:** "Delivered to [destination]. Here's a summary."

Keep checkpoints concise. One or two sentences plus the artifact. The human is busy — don't make them read a wall of text to say "yes, continue."

## The Brief

The Clarify phase produces a brief — a lightweight document that all subsequent phases reference. Use this template:

```markdown
## Brief: [Task Title]
**Goal:** [One sentence]
**Audience:** [Who will consume the output]
**Deliverable:** [Format and where it goes]
**Success criteria:**
- [What "done" looks like — specific, verifiable]
- [Another criterion]
**Sources:** [What data/tools to use]
**Constraints:** [Deadline, tone, length, etc.]
```

The brief is the contract. Everything downstream checks against it. If the human changes their mind mid-workflow, update the brief first, then adjust the plan.

## Verification-Driven Work

This is the most important principle in the methodology. It comes from test-driven development: define what "done" looks like before you start. Check against it when you finish.

The success criteria in the brief are your tests. They should be specific enough to verify. "Make it good" is not a success criterion. "Covers all three competitors, includes pricing comparison, fits in 10 slides" is.

## Cross-Cutting Skills

Three skills are always active during a workflow — they don't have their own phase, they support every phase:

- **connector-orchestration:** Choose the right tool for each step
- **error-handling:** Recover from tool failures gracefully
- **artifact-management:** Track where files and intermediates live
```

- [ ] **Step 2: Verify file exists and is well-formed**

```bash
head -5 superpowers-cowork/skills/methodology-core/SKILL.md
```

Expected: YAML frontmatter starting with `---`.

---

## Task 3: Skill — brainstorming

**Files:**
- Create: `skills/brainstorming/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/brainstorming/SKILL.md`:

```markdown
---
name: brainstorming
description: Scope clarification for knowledge work. Prevents premature execution by asking the right questions before starting. Produces a brief with goals, audience, deliverable format, success criteria, and constraints. Invoked automatically as the Clarify phase of superpowers workflows.
---

# Brainstorming

Before producing anything, understand what you're producing and why. This skill runs during the Clarify phase of any superpowers workflow.

## Process

1. **Assess scope.** Read the human's request. Is it clear enough to act on, or does it need refinement?
   - If the request specifies audience, format, and success criteria, fast-track: summarize your understanding as a brief and ask for confirmation.
   - If the request is vague or complex, ask questions.

2. **Ask questions one at a time.** Prefer multiple choice when possible. Focus on:
   - **Goal:** What are we trying to accomplish?
   - **Audience:** Who will see/use the output?
   - **Format:** What form should the deliverable take?
   - **Sources:** What information or tools should we use?
   - **Success criteria:** What does "done" look like?
   - **Constraints:** Deadline, length, tone, budget, sensitivity?

3. **Propose approaches.** Once you understand the scope, propose 2-3 approaches with trade-offs. Lead with your recommendation.
   - Example: (A) Comprehensive report with primary web research — thorough but takes longer. (B) Executive summary from the information you've already shared — fast but less complete. (C) Data-driven analysis with visualizations — best for quantitative topics.

4. **Write the brief.** After the human picks an approach, write the brief using the template from methodology-core. Present it for confirmation.

## Key Principles

- **One question at a time.** Don't overwhelm with a wall of questions. Each message has one question.
- **Multiple choice when possible.** "Should the report be (A) a 2-page executive summary, (B) a detailed 10-page analysis, or (C) a slide deck?" is easier to answer than "What format do you want?"
- **YAGNI.** If the human says "just a quick email," don't propose a multi-phase research project. Scale the methodology to the task.
- **Fast-track when clear.** If the request already has enough detail, don't ask questions for the sake of asking. Summarize, confirm, move on.

## Anti-Pattern: Premature Execution

If you catch yourself writing the deliverable before the brief is done, stop. The brief is the contract — without it, you're guessing. "I assumed you wanted a formal report" is the knowledge-work equivalent of "I assumed you wanted a microservices architecture."
```

---

## Task 4: Skill — planning

**Files:**
- Create: `skills/planning/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/planning/SKILL.md`:

```markdown
---
name: planning
description: Break a knowledge-work task into structured steps with tool assignments, checkpoints, and success criteria. Invoked automatically as the Plan phase of superpowers workflows. Produces a task list the human approves before execution begins.
---

# Planning

Turn the brief into an ordered, actionable plan. Each step has a clear deliverable, assigned tools, and a "done" criterion.

## Plan Structure

```markdown
## Plan: [Task Title]

**Brief:** [Link or inline reference to the brief]
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

### Step 3: [Action]
...
```

## Task Granularity

Each step should be one coherent action:
- "Research competitor X's pricing via web search" — one step
- "Draft the executive summary section" — one step
- "Format the findings as a pptx deck" — one step

Group related steps between checkpoints. A checkpoint is where the human reviews progress and can redirect.

## Tool Mapping

Every step that uses an external tool specifies which one:
- "Search for recent articles" → WebSearch
- "Pull emails about the topic" → ~~email (Gmail MCP)
- "Check calendar for meeting context" → ~~calendar
- "Create the final report" → docx skill

Consult the connector-orchestration skill for tool selection when multiple options exist.

## Dependencies and Parallelism

Mark steps that can run simultaneously:

```markdown
### Steps 2-4 (parallel)
These three research steps are independent. Run them simultaneously if subagents are available.

### Step 2: Research competitor A
...

### Step 3: Research competitor B
...

### Step 4: Research competitor C
...

### Step 5: Merge research findings
- **Depends on:** Steps 2-4
```

## Effort Estimation

- **Light:** A few minutes. Simple lookup, quick draft, format conversion.
- **Medium:** 10-20 minutes. Research with multiple sources, drafting with iteration, data analysis.
- **Heavy:** 30+ minutes. Deep research, complex multi-source synthesis, large document creation.

These are rough guides for the human's expectations, not commitments.

## Presenting the Plan

After writing the plan, present it with a summary: "[N] steps, [M] checkpoints, estimated [effort]. The main work is in steps [X-Y]. Ready to start?"

Wait for approval. The human may want to reorder, skip steps, or add constraints.
```

---

## Task 5: Skill — parallel-execution

**Files:**
- Create: `skills/parallel-execution/SKILL.md`
- Create: `skills/parallel-execution/references/completeness-reviewer.md`
- Create: `skills/parallel-execution/references/quality-reviewer.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/parallel-execution/SKILL.md`:

```markdown
---
name: parallel-execution
description: Dispatch independent tasks to subagents for parallel execution. Uses a two-stage review (completeness then quality) before accepting results. Falls back to sequential execution when subagents are unavailable. Invoked during the Execute phase of superpowers workflows.
---

# Parallel Execution

When the plan marks steps as parallel, dispatch them to subagents. Each subagent gets the brief, the plan, and its specific task. Review results before merging.

## Dispatch

For each parallel step:
1. Compose a self-contained prompt with: the brief, the specific step instructions, the tools to use, and the "done when" criterion.
2. Dispatch via the Agent tool. Each subagent works independently.
3. When results return, run the two-stage review.

## Two-Stage Review

### Stage 1: Completeness
Does the output cover everything the step asked for? Check against the "done when" criterion.

See `references/completeness-reviewer.md` for the review prompt.

### Stage 2: Quality
Is the output accurate, well-structured, and appropriately toned for the audience in the brief?

See `references/quality-reviewer.md` for the review prompt.

If either stage fails, provide specific feedback and re-dispatch the task.

## Merge Strategy

When parallel results need to be combined (e.g., three research threads into one synthesis):
1. Collect all parallel outputs
2. Check for contradictions between outputs
3. Synthesize into a unified result, noting where sources agree and disagree
4. The merge step is always sequential — it requires seeing all outputs together

## Sequential Fallback

If subagents are not available:
1. Execute steps sequentially in the current session
2. After each step, briefly verify the output before moving on
3. Group steps into batches of 2-3, with a checkpoint after each batch

## Progress Tracking

Use the task system to track each parallel step. Mark tasks as in_progress when dispatched, completed when the two-stage review passes.
```

- [ ] **Step 2: Write completeness-reviewer.md**

Create `skills/parallel-execution/references/completeness-reviewer.md`:

```markdown
# Completeness Review Prompt

You are reviewing a task output for completeness. You have the brief and the specific step that was assigned.

## Check

1. Does the output address every part of the step's instructions?
2. Does it meet the "done when" criterion specified in the plan?
3. Is anything missing that the step explicitly asked for?

## Output

- **Pass** if the output covers everything the step asked for.
- **Fail** with specific items that are missing or incomplete.

Be strict about coverage. Partial outputs that skip parts of the instructions fail.
```

- [ ] **Step 3: Write quality-reviewer.md**

Create `skills/parallel-execution/references/quality-reviewer.md`:

```markdown
# Quality Review Prompt

You are reviewing a task output for quality. You have the brief (which defines the audience and tone) and the output to review.

## Check

1. **Accuracy:** Are claims supported? Do numbers add up? Are sources credible?
2. **Structure:** Is the output well-organized and logical?
3. **Tone:** Does it match the audience specified in the brief?
4. **Clarity:** Could the intended audience understand this without additional context?

## Output

- **Pass** if the output meets quality standards for the audience.
- **Fail** with specific quality issues and suggestions for improvement.

Be constructive. Point to specific problems, not vague complaints.
```

---

## Task 6: Skill — verification

**Files:**
- Create: `skills/verification/SKILL.md`
- Create: `skills/verification/references/verification-checklist.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/verification/SKILL.md`:

```markdown
---
name: verification
description: Check outputs against the success criteria defined in the brief. The knowledge-work equivalent of running your test suite. Verifies factual accuracy, document completeness, data integrity, and communication clarity. Invoked during the Verify phase of superpowers workflows.
---

# Verification

The brief defined what "done" looks like. Verification checks whether we got there.

## Process

1. **Load the brief.** Pull the success criteria. These are your test cases.
2. **Check each criterion.** For each success criterion in the brief, verify the output meets it.
3. **Run type-specific checks.** Based on what kind of work this is, run additional verification. See the checklist in `references/verification-checklist.md`.
4. **Report findings.** Flag issues by severity:
   - **Critical:** Blocks delivery. Factual errors, missing sections, wrong audience.
   - **Moderate:** Worth fixing. Formatting issues, weak sourcing, inconsistent tone.
   - **Minor:** Optional polish. Wording tweaks, minor formatting.
5. **Fix or escalate.** Fix moderate/minor issues yourself. For critical issues, explain the problem and ask the human how to proceed.

## The Principle

This is verification-driven work — the knowledge-work equivalent of TDD. The success criteria were defined before the work started, not after. That's what makes this verification and not just "does this look okay."

If the success criteria are vague ("make it good"), flag that. Verification only works when the criteria are specific enough to check against.
```

- [ ] **Step 2: Write verification-checklist.md**

Create `skills/verification/references/verification-checklist.md`:

```markdown
# Verification Checklist by Output Type

## Factual Research
- [ ] Every claim has a cited source
- [ ] Sources are credible and recent (check publication dates)
- [ ] Numbers and statistics are accurate (cross-reference where possible)
- [ ] Conflicting sources are noted, not silently resolved
- [ ] No hallucinated citations or facts

## Documents (reports, memos, letters)
- [ ] Structure matches what the brief specified
- [ ] All sections from the plan are present
- [ ] Tone matches the audience
- [ ] Length is within constraints
- [ ] No placeholder text or TODO markers remain

## Data Work (spreadsheets, analysis)
- [ ] Formulas produce correct results (spot-check a few)
- [ ] Totals and subtotals reconcile
- [ ] Column headers are clear
- [ ] Data sources are attributed
- [ ] No empty cells where data should be

## Communications (emails, messages)
- [ ] Tone matches the recipient
- [ ] Key points are clear in the first paragraph
- [ ] Call to action is specific (if applicable)
- [ ] No sensitive information that shouldn't be shared
- [ ] Appropriate level of formality

## Presentations (slide decks)
- [ ] Narrative flow makes sense slide-to-slide
- [ ] Key message is clear without reading speaker notes
- [ ] Data visualizations are labeled and sourced
- [ ] Slide count is within constraints
- [ ] No wall-of-text slides

## Multi-Source Synthesis
- [ ] All requested sources were consulted
- [ ] Conflicting information is flagged
- [ ] Source attribution is clear
- [ ] Gaps in coverage are noted
```

---

## Task 7: Skill — output-review

**Files:**
- Create: `skills/output-review/SKILL.md`
- Create: `skills/output-review/references/review-dimensions.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/output-review/SKILL.md`:

```markdown
---
name: output-review
description: Pre-delivery audit of any artifact. Reviews against the brief for completeness, accuracy, structure, tone, and format. Can use a subagent for independent review. Trigger with "review this before I send it", "check this output", "is this ready", or during the Review phase of superpowers workflows.
---

# Output Review

A dedicated review pass before delivery. Think of this as code review for knowledge work — a fresh look at the output before it ships.

## Process

1. **Load the brief.** What were the goals, audience, and success criteria?
2. **Review each dimension.** See `references/review-dimensions.md` for the full checklist.
3. **Report findings.** Use severity levels:
   - **Critical:** Blocks delivery. Must be fixed.
   - **Moderate:** Should be fixed. Deliverable is usable but not polished.
   - **Minor:** Optional. Nice to fix if time allows.
4. **Present to the human.** "Review complete. [N critical / N moderate / N minor] findings. [Summary of most important issues]."

## Independent Review Mode

For important deliverables, use a subagent for independent review. The subagent gets only:
- The brief
- The output
- The review dimensions

It does NOT get the process context (how the output was created, what challenges were encountered). This forces a fresh-eyes evaluation — the reviewer judges the output on its own merits.

## When to Use

- Always as part of `/superpowers` and `/review` commands
- Before any delivery step where the output will be seen by others
- When the human asks "is this ready?" or "check this before I send it"
```

- [ ] **Step 2: Write review-dimensions.md**

Create `skills/output-review/references/review-dimensions.md`:

```markdown
# Review Dimensions

## 1. Completeness
- Does the output cover everything in the brief?
- Are any sections missing or incomplete?
- Were all success criteria addressed?

## 2. Accuracy
- Are facts correct and verifiable?
- Are sources cited where needed?
- Do numbers add up?
- Are there any claims that feel unsupported?

## 3. Structure
- Is it well-organized?
- Does it flow logically from start to finish?
- Are headings, sections, or slides in a sensible order?
- Can the reader find what they need quickly?

## 4. Tone
- Does it match the audience specified in the brief?
- Is the formality level appropriate?
- Is it consistent throughout (no sudden shifts)?

## 5. Format
- Does the output format match what the brief requested?
- Is formatting clean (no broken tables, misaligned content)?
- Are visuals (charts, images) properly labeled?
- Is the length within constraints?
```

---

## Task 8: Skill — delivery

**Files:**
- Create: `skills/delivery/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/delivery/SKILL.md`:

```markdown
---
name: delivery
description: Delivery checkpoint — choose the right destination and format for an output, run sensitivity checks, and ship it. Handles file saving, email drafts, Google Drive uploads, scheduled tasks, and persistent artifacts. Invoked during the Deliver phase of superpowers workflows or via the /deliver command.
---

# Delivery

The last phase. The output is verified and reviewed. Now get it where it needs to go.

## Decision Tree

Ask the human where the output should go:

1. **Save locally** — Write to a specific folder or path on disk
2. **Email it** — Create a draft via ~~email (Gmail MCP) for the human to review and send
3. **Upload to cloud storage** — Push to ~~file storage (Google Drive)
4. **Schedule for later** — Set up a scheduled task to deliver at a specific time
5. **Create a persistent artifact** — Save as a conversation artifact for easy reference
6. **Discard** — The human changed their mind. Clean up and move on.

If the brief already specifies a destination, propose it: "The brief says this goes to [recipient] via email. Want me to draft that?"

## Format Conversion

If the output was produced in markdown/plain text but the brief specifies a different format (docx, pptx, xlsx, pdf):
1. Invoke the format-selection skill to pick the right file skill
2. Convert the content
3. Verify the conversion preserved structure and content
4. Deliver the formatted file

## Recurring Check

After delivery, consider: "Is this something that should happen again?" If the workflow looks repeatable (weekly reports, monthly analyses, daily digests), mention it: "Want me to set this up as a recurring task?"

If yes, hand off to the recurring-work skill.

## Delivery Summary

After delivery, provide a brief summary:

```markdown
**Delivered:** [What]
**To:** [Where]
**Format:** [docx / email draft / etc.]
**Brief:** [One-line reminder of the goal]
```
```

---

## Task 9: Skill — connector-orchestration

**Files:**
- Create: `skills/connector-orchestration/SKILL.md`
- Create: `skills/connector-orchestration/references/mcp-catalog.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/connector-orchestration/SKILL.md`:

```markdown
---
name: connector-orchestration
description: Decision framework for choosing the right tool at each workflow step. Covers MCP connectors, browser automation, desktop control, and file skills. Handles tool chaining and graceful degradation. Cross-cutting skill — active throughout any superpowers workflow, not a standalone phase.
---

# Connector Orchestration

Cowork has many ways to interact with external tools — MCP connectors, browser automation, desktop control, file creation skills. Choosing wrong wastes entire turns. This skill provides the decision framework.

## Priority Order

1. **MCP connector** — fastest, most reliable, structured data. Always prefer if available.
2. **Browser automation** — works for any web app. Slower, requires screen context.
3. **Desktop control** — for native macOS apps. Slowest, most fragile.
4. **Ask the human** — when no automated path exists, ask them to provide the data.

## Decision Table

See `references/mcp-catalog.md` for the full table of tools and their capabilities.

## Chaining

When a workflow step needs data from one tool pushed to another:
1. Pull data from source tool (e.g., email attachment via ~~email)
2. Transform if needed (e.g., extract text from PDF)
3. Push to destination tool (e.g., insert into xlsx via xlsx skill)

Track what came from where — data lineage matters for verification.

## Degradation

If a preferred tool isn't available:
1. Try the fallback from the catalog
2. Tell the human what's degraded: "I couldn't access Gmail directly, so I'm using browser automation. This may be slower."
3. If no fallback exists: "I need [X] but don't have access. Can you paste the relevant information?"

Never silently skip a step because a tool isn't available. Always inform the human.
```

- [ ] **Step 2: Write mcp-catalog.md**

Create `skills/connector-orchestration/references/mcp-catalog.md`:

```markdown
# MCP Tool Catalog

## Email
- **Primary:** ~~email (Gmail MCP) — `search_threads`, `get_thread`, `create_draft`, `label_message`
- **Fallback:** Browser automation to Gmail/Outlook
- **Use for:** Reading email, searching threads, drafting replies, finding attachments

## Calendar
- **Primary:** ~~calendar (Google Calendar MCP) — `list_events`, `create_event`, `suggest_time`
- **Fallback:** Browser automation to Google Calendar
- **Use for:** Checking schedules, creating events, finding meeting context

## File Storage
- **Primary:** ~~file storage (Google Drive MCP) — `search_files`, `read_file_content`, `create_file`
- **Fallback:** Browser automation to Drive/Dropbox
- **Use for:** Finding documents, reading shared files, uploading deliverables

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
- **Primary:** `create_scheduled_task` (always available)
- **Use for:** Recurring workflows, timed delivery, reminders

## Everything Else
- **Browser automation:** Any web app without a dedicated MCP
- **Desktop control:** Native macOS apps (Finder, Notes, etc.)
- **Ask the human:** When no automated path exists
```

---

## Task 10: Skill — research-methodology

**Files:**
- Create: `skills/research-methodology/SKILL.md`
- Create: `skills/research-methodology/references/source-evaluation.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/research-methodology/SKILL.md`:

```markdown
---
name: research-methodology
description: Structured web research process — define questions, search from multiple angles, evaluate source credibility, extract findings with attribution, synthesize, and cite. Teaches triangulation and distinguishes facts from analysis from opinion. Invoked via /research or when a plan step involves gathering information.
---

# Research Methodology

Structured process for turning a question into a well-sourced answer.

## Process

### 1. Define Questions
Before searching, write down exactly what you're trying to learn. Vague browsing wastes time. "What are the current trends in AI regulation?" is too broad. Better: "What new AI regulations have been proposed or enacted in the US and EU in the last 12 months? What do they require? Who do they affect?"

### 2. Search — Multiple Angles
Run at least 3 different queries per question. Vary the angle:
- Direct query: "AI regulation 2025 2026"
- Institutional: "EU AI Act implementation timeline"
- Critical: "AI regulation criticism challenges"

Use WebSearch for each. Follow up with WebFetch on the most promising results to get full content.

### 3. Evaluate Sources
For each source, assess:
- **Authority:** Who published this? Are they credible on this topic?
- **Recency:** When was it published? Is it current?
- **Bias:** Does the source have a known perspective? (Trade publication vs. regulatory body vs. advocacy group)
- **Corroboration:** Do other independent sources agree?

See `references/source-evaluation.md` for the detailed rubric.

### 4. Extract Findings
Pull key findings from each source. For every claim, note:
- The specific finding
- The source (title, publisher, date, URL)
- Whether it's a fact, analysis, or opinion

### 5. Synthesize
Combine findings into a coherent answer to the original questions:
- Lead with the consensus view
- Note where sources disagree and why
- Identify gaps — what couldn't you find?
- Distinguish between well-supported conclusions and tentative ones

### 6. Cite
Every claim in the synthesis should trace back to a source. The verification skill will check these.

## Key Principles

- **Triangulate.** One source is an anecdote. Three independent sources are evidence.
- **Flag conflicts.** Don't silently pick sides when sources disagree. Present both.
- **Date everything.** "Recent study shows..." means nothing without a date. Always include when the source was published.
- **Know what you don't know.** "I couldn't find reliable data on X" is more honest and useful than filling the gap with a guess.
```

- [ ] **Step 2: Write source-evaluation.md**

Create `skills/research-methodology/references/source-evaluation.md`:

```markdown
# Source Evaluation Rubric

## Authority
- **Strong:** Government agencies, peer-reviewed journals, established news organizations, recognized domain experts
- **Moderate:** Industry publications, think tanks, well-known blogs by credentialed authors
- **Weak:** Anonymous sources, user-generated content, obvious marketing material, sources with no editorial oversight

## Recency
- **Current:** Published within the last 12 months (for fast-moving topics like tech/policy)
- **Recent:** Published within the last 2 years (for slower-moving topics)
- **Stale:** Older than 2 years — use with caution, note the date prominently

## Bias Assessment
- **Low bias:** Academic research, government data, balanced journalism
- **Moderate bias:** Industry reports (may favor their sector), advocacy organizations (clear perspective but substantive)
- **High bias:** Marketing material, partisan sources, anything with a financial interest in the conclusion

## Corroboration
- **Strong:** 3+ independent sources agree
- **Moderate:** 2 independent sources agree
- **Weak:** Only one source makes this claim — flag it as unverified
```

---

## Task 11: Skill — recurring-work

**Files:**
- Create: `skills/recurring-work/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/recurring-work/SKILL.md`:

```markdown
---
name: recurring-work
description: Identify workflows that should repeat and set them up as scheduled tasks. Captures the workflow definition (brief, plan, tools, format) so recurring runs follow the same methodology. Integrates with Cowork's scheduled tasks system.
---

# Recurring Work

When a workflow completes and the delivery phase suggests this might recur, this skill captures the pattern and sets it up.

## When to Suggest

After delivery, check: would this workflow make sense on a schedule?

Good candidates:
- Weekly status summaries
- Monthly research updates
- Daily email digests
- Periodic competitive analysis
- Regular report generation

Don't suggest recurring for: one-off tasks, exploratory research, unique deliverables.

## Capture the Workflow

To set up a recurring run, capture:
1. **The brief** — what the task produces, for whom, in what format
2. **The plan** — what steps to follow
3. **The tools** — which connectors to use
4. **The schedule** — how often (daily, weekly, monthly, custom)

## Setting Up

Use `create_scheduled_task` (via Cowork's schedule skill) with:
- The full workflow prompt (brief + instructions)
- The cron schedule
- A descriptive name

The recurring run should follow the same methodology — it re-executes, re-verifies, and re-delivers. It doesn't just repeat blindly. If sources have changed or data is different, the output should reflect that.

## Present to the Human

"This looks like something that could run on a schedule. Want me to set it up to run [weekly on Mondays / daily at 9am / monthly on the 1st]?"

If yes, configure and confirm: "Scheduled. It will run [description] and deliver to [destination]."
```

---

## Task 12: Skill — format-selection

**Files:**
- Create: `skills/format-selection/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/format-selection/SKILL.md`:

```markdown
---
name: format-selection
description: Choose the right output format (docx, pptx, xlsx, pdf, markdown, email) based on the deliverable type and invoke the corresponding file skill. Handles translation from working format (usually markdown) to the target format.
---

# Format Selection

When the brief specifies a deliverable format, or when the delivery phase needs to convert working content into a final format, this skill picks the right tool.

## Decision Table

| Deliverable type | Format | Skill to invoke |
|-----------------|--------|----------------|
| Report, memo, letter, formal document | .docx | docx skill |
| Presentation, deck, slides | .pptx | pptx skill |
| Data, analysis, budget, tracker | .xlsx | xlsx skill |
| Printable, fixed-layout, formal submission | .pdf | pdf skill |
| Quick sharing, internal notes, conversation artifact | Markdown | None — use native markdown |
| Email to someone | Email draft | ~~email (Gmail MCP) `create_draft` |

## Ambiguous Cases

If the deliverable type doesn't map cleanly:
- "Report" without further context → default to .docx
- "Analysis" with numbers → .xlsx if data-heavy, .docx if narrative-heavy
- "Summary" → markdown unless the human specifies otherwise
- Ask if genuinely unclear: "Should this be a Word doc or a slide deck?"

## Translation

Most workflow execution produces markdown or plain text. To convert:
1. Invoke the appropriate file skill with the content
2. Specify formatting requirements from the brief (headers, page numbers, branding)
3. Verify the conversion preserved all content and structure
4. If anything was lost in translation, fix it before delivery
```

---

## Task 13: Skill — data-transformation

**Files:**
- Create: `skills/data-transformation/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/data-transformation/SKILL.md`:

```markdown
---
name: data-transformation
description: Move data between formats and tools in multi-step workflows. Handles extraction from PDFs and emails, cleaning and restructuring, format conversion, and cross-tool handoffs. Tracks data lineage and validates at each transformation step.
---

# Data Transformation

The hard part of multi-step knowledge work: getting data from where it is to where it needs to be, in the right format, without losing or corrupting it.

## Common Patterns

### Email attachment → Spreadsheet
1. Find the email via ~~email
2. Download the attachment
3. If PDF: extract tables using pdf skill
4. Clean and structure the data
5. Write to xlsx via xlsx skill

### Multiple sources → Combined analysis
1. Pull data from each source (web, email, Drive, etc.)
2. Normalize formats (dates, currencies, units)
3. Combine into a single dataset
4. Validate: no duplicates, no missing fields
5. Analyze and visualize

### Research → Presentation
1. Gather research findings (text + data)
2. Identify key points for slides
3. Create visualizations from data
4. Build deck via pptx skill

## Data Lineage

At every transformation step, track where the data came from:
- "Revenue figures from Q2-report.xlsx (emailed by accountant on 2026-05-15)"
- "Market share data from Statista article (published 2026-04-20)"

This lineage feeds into the verification skill — it can check sources.

## Validation

After each transformation:
- **Did we lose data?** Row counts, column counts, spot-check values
- **Did formats shift?** Dates still dates? Numbers still numbers? Currency symbols intact?
- **Is the structure right?** Headers in the right place? Data aligned to correct columns?

If validation fails, fix before proceeding. Don't compound errors across transformation steps.
```

---

## Task 14: Skill — artifact-management

**Files:**
- Create: `skills/artifact-management/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/artifact-management/SKILL.md`:

```markdown
---
name: artifact-management
description: Track where outputs and intermediate files live during multi-step workflows. Manages working directories, version tracking, and cleanup. Cross-cutting skill — active throughout any superpowers workflow.
---

# Artifact Management

Multi-step workflows produce intermediate files — research notes, draft documents, extracted data, converted formats. This skill tracks where everything lives.

## Principles

- **Don't overwrite originals.** If the human shared a file, never modify it in place. Work on a copy.
- **Keep intermediates accessible.** If the verification phase needs to check a source, it should be able to find the intermediate file.
- **Version, don't replace.** When iterating on a draft, keep previous versions (draft-v1.md, draft-v2.md) rather than overwriting.
- **Clean up on completion.** After delivery, offer to clean up intermediate files or keep them for reference.

## Working Directory

Create a working directory for each workflow:
- Use a descriptive name: `superpowers-competitive-analysis-2026-05-26/`
- Store intermediates here
- The final deliverable may be copied elsewhere during delivery

## What to Track

- **Source files:** Anything the human provided or that was downloaded
- **Intermediates:** Research notes, extracted data, draft versions
- **Final output:** The delivered artifact
- **Brief and plan:** The workflow's steering documents
```

---

## Task 15: Skill — error-handling

**Files:**
- Create: `skills/error-handling/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/error-handling/SKILL.md`:

```markdown
---
name: error-handling
description: Recover gracefully from tool failures during workflows. Tries fallbacks, degrades transparently, and surfaces options to the human when no automated recovery exists. Cross-cutting skill — active throughout any superpowers workflow.
---

# Error Handling

Tools fail. MCP servers go down. Browser automation hits unexpected pages. This skill defines how to recover without derailing the workflow.

## Recovery Pattern

```
1. Try primary tool (MCP connector)
2. If fail → Try fallback (browser automation or alternative MCP)
3. If fail → Try manual path (ask the human to provide the data)
4. If all fail → Surface the problem with options
```

## Key Behaviors

**Never silently swallow errors.** If a tool fails, the human needs to know. Even if you recover via fallback, mention it: "Gmail MCP wasn't responding, so I used browser automation instead — may be slightly slower."

**Adjust the plan.** If a critical tool is down and fallbacks aren't working, the plan may need to change. Surface this: "I can't access your email right now. We can (A) wait and try again, (B) skip the email research and work from what you tell me, or (C) try browser automation."

**Track failures.** If multiple tools fail in the same workflow, that's a pattern worth reporting: "Several of your connectors seem to be having issues. You might want to check your MCP server status."

**Don't retry forever.** One retry is reasonable. Two is the maximum. After that, move to fallback or escalate.
```

---

## Task 16: Skill — sensitivity-guard

**Files:**
- Create: `skills/sensitivity-guard/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/sensitivity-guard/SKILL.md`:

```markdown
---
name: sensitivity-guard
description: Checkpoint before combining or sharing data from different sources. Flags when personal or private data might end up in a shared deliverable. Always checkpoints with the human — never makes sensitivity decisions autonomously. Active during the Verify and Deliver phases.
---

# Sensitivity Guard

When a workflow combines data from multiple sources or prepares output for sharing, pause and check: is it appropriate to combine this data and share it with this audience?

## When to Fire

- Before delivering any output that combines data from multiple connected tools
- Before emailing content that includes information pulled from other sources
- When PII (names, addresses, account numbers) appears in a synthesis
- When the deliverable audience differs from the source audience (e.g., internal data going to an external recipient)

## What to Check

1. **Source sensitivity:** Where did this data come from? Is any of it private, personal, or confidential?
2. **Audience appropriateness:** Is the intended recipient appropriate for all the data included?
3. **PII exposure:** Are there names, email addresses, phone numbers, or other personal information that shouldn't be shared?
4. **Context collapse:** Is internal context (personal emails, calendar details) being presented to someone who shouldn't see it?

## How to Checkpoint

Present the check clearly and concisely:

"This [report/email/deck] includes data from [sources]. It's going to [destination/recipient]. Everything look appropriate?"

If something feels off, be specific: "This report references your personal calendar entries. Should those be included in a document you're emailing to your accountant?"

## The Rule

Never make sensitivity decisions autonomously. Always checkpoint with the human. A false alarm is much better than a data leak.
```

---

## Task 17: Skill — cross-source-synthesis

**Files:**
- Create: `skills/cross-source-synthesis/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/cross-source-synthesis/SKILL.md`:

```markdown
---
name: cross-source-synthesis
description: Pull information from multiple connected tools (email, calendar, Drive, web) and weave a coherent picture. Powers the /brief command. Handles source conflicts, highlights gaps, and attributes everything.
---

# Cross-Source Synthesis

Turn scattered information across multiple tools into a unified, coherent picture.

## Process

### 1. Survey Available Sources
What's connected? What time range is relevant?
- ~~email → search for relevant threads
- ~~calendar → check for related events, meetings, deadlines
- ~~file storage → search for related documents
- WebSearch → check for external context if relevant

### 2. Pull in Parallel
Query each available source simultaneously. For each source, define a specific query:
- Email: "threads mentioning [topic] in the last [timeframe]"
- Calendar: "events related to [topic] in the last [timeframe]"
- Drive: "documents containing [topic]"

### 3. Reconcile
Information from different sources may conflict:
- Email says the deadline is Friday, calendar has it as Thursday
- A document says budget is $10K, an email thread discusses $15K

Don't silently resolve conflicts. Present both: "Your email from May 20 mentions a Friday deadline, but your calendar shows Thursday. Which is correct?"

### 4. Synthesize
Produce a unified summary with clear source attribution:
- "Based on your email with [person] on [date], the project status is..."
- "Your calendar shows [N] related meetings this week, including..."
- "The most recent document on Drive ([filename], last modified [date]) says..."

### 5. Highlight Gaps
What couldn't you find?
"I checked email, calendar, and Drive, but couldn't find pricing information. You might need to check [another source]."
```

---

## Task 18: Skill — communication-drafting

**Files:**
- Create: `skills/communication-drafting/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/communication-drafting/SKILL.md`:

```markdown
---
name: communication-drafting
description: Audience-aware messaging. Adapts content for different recipients and contexts — formal emails, casual messages, professional documents. Handles tone-matching, format adaptation, and context-appropriate framing.
---

# Communication Drafting

The same information, framed differently for different audiences and contexts.

## Audience Adaptation

Before drafting, understand:
- **Who** is the recipient?
- **What** is their relationship to you?
- **What** do they need from this message?
- **What** medium is this for? (email, document, message)

## Tone Mapping

| Context | Tone | Length | Structure |
|---------|------|--------|-----------|
| Email to a professional contact | Professional, concise | Short — 3-5 paragraphs max | Greeting, key point, context, ask, sign-off |
| Email to someone you know well | Warm but clear | Short | Key point first, context if needed |
| Formal document or letter | Formal, precise | As needed | Standard document structure |
| Quick message or note | Casual, direct | 1-3 sentences | Just the point |

## Multi-Audience Drafting

When the same content needs to reach different audiences:
1. Identify what each audience needs to know
2. Draft each version separately — don't try to make one message work for everyone
3. Verify each version independently (tone, content, sensitivity)

## Key Principles

- **Front-load the key point.** Don't bury the lead. The most important thing goes first.
- **Match the medium.** An email is not a report. A quick message is not an email.
- **Respect the reader's time.** Shorter is almost always better. Cut ruthlessly.
- **Be specific about asks.** "Let me know your thoughts" is weaker than "Can you confirm the budget by Friday?"
```

---

## Task 19: Skill — draft-iteration

**Files:**
- Create: `skills/draft-iteration/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/draft-iteration/SKILL.md`:

```markdown
---
name: draft-iteration
description: Manage feedback loops for documents and communications. Track versions, integrate human feedback into specific changes, show diffs between versions, and flag when iterations are diverging instead of converging.
---

# Draft Iteration

Manage the cycle: draft → feedback → revise → feedback → finalize.

## Version Tracking

Keep every version accessible:
- `draft-v1.md` — initial draft
- `draft-v2.md` — after first round of feedback
- `draft-v3.md` — after second round

Use artifact-management to store versions in the working directory.

## Feedback Integration

When the human gives feedback:
1. Parse it into specific changes: "Make the intro shorter" → shorten the introduction section
2. Map each piece of feedback to a section or element in the draft
3. Make the changes
4. Present what changed: "Updated the intro (cut from 200 to 80 words) and added the pricing table you asked for."

## Diff Presentation

Show the human what changed between versions:
- For small changes: quote the before and after inline
- For large changes: summarize the changes as a bulleted list
- Always be specific: "Changed the second paragraph" not "Updated the document"

## Convergence Check

Track whether iterations are converging (feedback getting smaller and more specific) or diverging (fundamental direction problems).

- **Converging:** "Fix the typo in paragraph 3" → "Looks good, just capitalize the header" → "Ship it." This is healthy.
- **Diverging:** "Actually make it more formal" → "No, too formal, make it casual" → "Actually, maybe a different format entirely." This suggests the brief needs updating. Flag it: "It seems like the direction is shifting. Should we revisit the brief before continuing?"
```

---

## Task 20: Skill — context-awareness

**Files:**
- Create: `skills/context-awareness/SKILL.md`

- [ ] **Step 1: Write SKILL.md**

Create `skills/context-awareness/SKILL.md`:

```markdown
---
name: context-awareness
description: Survey connected tools and available data at workflow start. Reports what's accessible and what's missing so the plan can account for gaps. Runs at the start of any superpowers workflow.
---

# Context Awareness

At the start of a workflow, survey the environment. What tools are connected? What data is accessible? What will the human need to provide manually?

## Process

1. **Check MCP connectors.** Which are available and responsive?
   - ~~email (Gmail) — connected?
   - ~~calendar (Google Calendar) — connected?
   - ~~file storage (Google Drive) — connected?
   - WebSearch / WebFetch — always available

2. **Report the environment.** Brief, clear summary:
   > "You have Gmail and Calendar connected. Google Drive is not connected, so for any documents I'll need you to share them directly. Web research is always available."

3. **Identify gaps.** Based on the task, what data sources would be useful but aren't available? What will the human need to provide?
   > "For this competitive analysis, I'll research online and check your email for any relevant threads. If you have internal documents on this topic, share them since I can't access your Drive."

4. **Adjust the plan.** The planning skill should account for what's available. Steps that require unavailable tools need a fallback or manual input.

## When to Run

- At the start of every superpowers workflow (before brainstorming)
- When a workflow step discovers a tool is unexpectedly unavailable
- When the human asks "what can you access?"

## Keep It Brief

The environment report should be 2-3 sentences, not a full audit. The human wants to know what's available and what's missing, not a detailed inventory of every MCP server.
```

---

## Task 21: Command — /superpowers

**Files:**
- Create: `commands/superpowers.md`

- [ ] **Step 1: Write command file**

Create `commands/superpowers.md`:

```markdown
---
description: Full structured workflow — clarify, plan, execute, verify, deliver
argument-hint: "<describe the task>"
---

# /superpowers

Run the full superpowers methodology on a complex task.

## Pipeline

```
context-awareness → brainstorming → planning → parallel-execution
    → verification → output-review → sensitivity-guard → delivery
```

Cross-cutting skills active throughout: connector-orchestration, error-handling, artifact-management.

## How to Use

```
/superpowers Prepare a competitive analysis of three solar panel companies
```

If no argument is provided, ask: "What would you like to work on?"

## What Happens

1. **Context check.** Survey connected tools, report what's available.
2. **Clarify.** Ask questions to understand the task. Produce a brief with success criteria.
3. **Plan.** Break the task into steps with tool assignments and checkpoints. Present for approval.
4. **Execute.** Work through the plan. Use subagents for parallel steps when available. Track progress.
5. **Verify.** Check every output against the success criteria from the brief. Flag issues.
6. **Review.** Pre-delivery audit for completeness, accuracy, structure, tone.
7. **Sensitivity check.** If the output combines data from multiple sources, checkpoint with the human.
8. **Deliver.** Choose format and destination. Ship it.

The human checkpoints at each phase transition and can redirect, skip ahead, or abort.

## Invoke: @$1

Treat the argument as the initial task description and begin with context-awareness.
```

---

## Task 22: Command — /plan

**Files:**
- Create: `commands/plan.md`

- [ ] **Step 1: Write command file**

Create `commands/plan.md`:

```markdown
---
description: Break a complex task into structured steps with checkpoints
argument-hint: "<describe the task>"
---

# /plan

Create a structured plan for a complex task. Clarifies scope, breaks into steps, assigns tools, and sets checkpoints — then hands off to execution.

## Pipeline

```
context-awareness → brainstorming → planning → [hand off to execution]
```

## How to Use

```
/plan Organize my tax documents for the accountant
```

If no argument is provided, ask: "What would you like to plan?"

## What Happens

1. **Context check.** What tools are available?
2. **Clarify.** Understand the task. Produce a brief.
3. **Plan.** Break into steps with "done when" criteria and checkpoints.
4. **Present the plan.** The human approves, adjusts, or rejects.

After approval, offer: "Ready to start executing this plan?"

## Invoke: @$1

Treat the argument as the task description and begin with context-awareness.
```

---

## Task 23: Command — /research

**Files:**
- Create: `commands/research.md`

- [ ] **Step 1: Write command file**

Create `commands/research.md`:

```markdown
---
description: Structured research session with source evaluation and synthesis
argument-hint: "<research question or topic>"
---

# /research

Structured research — define questions, search from multiple angles, evaluate sources, synthesize findings with citations.

## Pipeline

```
context-awareness → research-methodology → verification (source-check)
    → output-review → delivery
```

## How to Use

```
/research What are the best strategies for refinancing a mortgage in 2026?
```

If no argument is provided, ask: "What would you like to research?"

## What Happens

1. **Context check.** What sources are available?
2. **Define questions.** Refine the topic into specific, answerable questions.
3. **Research.** Search from multiple angles, evaluate sources, extract findings.
4. **Verify.** Source-check every claim. Flag conflicts.
5. **Review.** Audit the synthesis for completeness and accuracy.
6. **Deliver.** Present findings with citations, offer to save or send.

## Invoke: @$1

Treat the argument as the research topic and begin with context-awareness.
```

---

## Task 24: Command — /review

**Files:**
- Create: `commands/review.md`

- [ ] **Step 1: Write command file**

Create `commands/review.md`:

```markdown
---
description: Audit an artifact against its goals before sharing
argument-hint: "<describe what to review, or share the artifact>"
---

# /review

Review an artifact before delivery. Checks completeness, accuracy, structure, tone, and format.

## Pipeline

```
output-review → verification (fact-check) → sensitivity-guard
```

## How to Use

```
/review Check this email before I send it
/review Is this report ready for my accountant?
```

If no artifact is provided, ask: "What would you like me to review?"

## What Happens

1. **Review.** Audit the artifact across all dimensions (completeness, accuracy, structure, tone, format).
2. **Fact-check.** Verify any factual claims.
3. **Sensitivity check.** Flag any private or sensitive data that might not be appropriate for the intended recipient.
4. **Report.** Present findings by severity (critical / moderate / minor).

## Invoke: @$1

Treat the argument as context about what to review.
```

---

## Task 25: Command — /deliver

**Files:**
- Create: `commands/deliver.md`

- [ ] **Step 1: Write command file**

Create `commands/deliver.md`:

```markdown
---
description: Format, verify completeness, and send/save/schedule an output
argument-hint: "<what to deliver and where>"
---

# /deliver

Delivery checkpoint — pick the right format and destination, run sensitivity checks, ship it.

## Pipeline

```
sensitivity-guard → format-selection → artifact-management → delivery
```

## How to Use

```
/deliver Save this as a Word doc in my Documents folder
/deliver Email this report to my accountant
/deliver Schedule this to send Monday morning
```

If no argument is provided, ask: "What would you like to deliver, and where should it go?"

## What Happens

1. **Sensitivity check.** Any concerns about the content or destination?
2. **Format.** Convert to the right format if needed (docx, pptx, xlsx, pdf, email draft).
3. **Artifact management.** Organize files, track versions.
4. **Deliver.** Save, email, upload, schedule, or create a persistent artifact.
5. **Summary.** Confirm what was delivered, where, and in what format.

## Invoke: @$1

Treat the argument as delivery instructions.
```

---

## Task 26: Command — /brief

**Files:**
- Create: `commands/brief.md`

- [ ] **Step 1: Write command file**

Create `commands/brief.md`:

```markdown
---
description: Cross-source synthesis — pull context from email, calendar, and docs
argument-hint: "<topic or question>"
---

# /brief

Pull information from your connected tools and synthesize a coherent picture. Great for "what's the status of X?" or "what do I know about Y?"

## Pipeline

```
context-awareness → cross-source-synthesis → verification
    → output-review → delivery
```

## How to Use

```
/brief What's going on with my house refinance?
/brief Summarize everything I have about the Johnson project
/brief What meetings do I have this week and what should I prep for?
```

If no argument is provided, ask: "What would you like me to brief you on?"

## What Happens

1. **Context check.** Which tools are connected?
2. **Pull from sources.** Search email, calendar, Drive, and web for relevant information.
3. **Synthesize.** Weave the information into a coherent summary with source attribution.
4. **Verify.** Flag any conflicts between sources.
5. **Review.** Ensure the brief is complete and accurate.
6. **Deliver.** Present the brief, offer to save or send.

## Invoke: @$1

Treat the argument as the topic to brief on and begin with context-awareness.
```

---

## Task 27: Command — /draft

**Files:**
- Create: `commands/draft.md`

- [ ] **Step 1: Write command file**

Create `commands/draft.md`:

```markdown
---
description: Managed drafting workflow with iteration and feedback loops
argument-hint: "<what to draft>"
---

# /draft

Start a managed drafting workflow. Clarifies scope, outlines, drafts, and iterates based on your feedback until you're satisfied.

## Pipeline

```
brainstorming (scope) → planning (outline) → communication-drafting or parallel-execution
    → draft-iteration (feedback loop) → verification → delivery
```

## How to Use

```
/draft An email to my landlord about the lease renewal
/draft A cover letter for the senior analyst position at Acme Corp
/draft A report summarizing my investment portfolio performance this quarter
```

If no argument is provided, ask: "What would you like to draft?"

## What Happens

1. **Clarify scope.** Who's the audience? What tone? What format? How long?
2. **Outline.** Create a structure and present it for approval.
3. **Draft.** Write the first version.
4. **Iterate.** Present the draft. Get feedback. Revise. Repeat until you're satisfied.
5. **Verify.** Final check against the brief.
6. **Deliver.** Format and send/save.

The iteration loop is the heart of this command — draft → feedback → revise until you say it's ready.

## Invoke: @$1

Treat the argument as the drafting task and begin with brainstorming.
```

---

## Task 28: Integration Testing

**Files:**
- Verify: All files in the plugin structure

- [ ] **Step 1: Verify all files exist**

```bash
find superpowers-cowork -type f | sort
```

Expected: All files from the file structure section. Count should be approximately 35 files.

- [ ] **Step 2: Verify all SKILL.md files have valid frontmatter**

```bash
for f in $(find superpowers-cowork/skills -name "SKILL.md"); do
  echo "=== $f ==="
  head -3 "$f"
  echo
done
```

Expected: Every SKILL.md starts with `---` and has `name:` and `description:` fields.

- [ ] **Step 3: Verify all command files have valid frontmatter**

```bash
for f in $(find superpowers-cowork/commands -name "*.md"); do
  echo "=== $f ==="
  head -3 "$f"
  echo
done
```

Expected: Every command file starts with `---` and has `description:` and `argument-hint:` fields.

- [ ] **Step 4: Verify plugin.json is valid JSON**

```bash
python3 -c "import json; json.load(open('superpowers-cowork/.claude-plugin/plugin.json'))"
```

Expected: No error.

- [ ] **Step 5: Cross-reference spec coverage**

Read the spec at `docs/specs/2026-05-26-superpowers-cowork-design.md` and verify:
- All 19 skills have SKILL.md files
- All 7 commands have command files
- All connector categories from the spec are in CONNECTORS.md
- The command chaining in each command file matches the spec's chaining diagrams

- [ ] **Step 6: Verify internal references**

Check that skills referencing other files (`references/` subdirectories) have those files present:
- `parallel-execution/references/completeness-reviewer.md` — exists?
- `parallel-execution/references/quality-reviewer.md` — exists?
- `verification/references/verification-checklist.md` — exists?
- `output-review/references/review-dimensions.md` — exists?
- `connector-orchestration/references/mcp-catalog.md` — exists?
- `research-methodology/references/source-evaluation.md` — exists?

---

## Self-Review Findings

**Spec coverage:** All 19 skills mapped to tasks (Tasks 2-20). All 7 commands mapped to tasks (Tasks 21-27). Connector table, CLAUDE.md bootstrap, CONNECTORS.md, and README.md all covered in Task 1.

**Placeholder scan:** No TBDs, TODOs, or "implement later" markers. All SKILL.md files contain complete content.

**Type consistency:** Skill names in command chaining pipelines match the `name:` fields in SKILL.md frontmatter. Connector placeholders (`~~email`, `~~calendar`, `~~file storage`) are consistent across CONNECTORS.md and skill files.

**One gap found and addressed:** The spec mentions `data-transformation`, `artifact-management`, and `error-handling` as Tier 2 skills but the original plan draft didn't fully flesh out how they integrate with commands. Verified: they're listed as cross-cutting in methodology-core and referenced by other skills that invoke them. Coverage is adequate — they don't need their own command entries.
