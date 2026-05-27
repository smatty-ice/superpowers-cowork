# Superpowers for Cowork — Design Spec

**Date:** 2026-05-26
**Status:** Draft
**Plugin name:** superpowers-cowork
**Architecture:** Monolithic marketplace plugin
**Audience:** Knowledge workers broadly (PMs, analysts, marketers, anyone using Cowork)

---

## Vision

Bring the superpowers methodology to Claude Cowork as a marketplace plugin for knowledge workers. The core insight: what makes superpowers work isn't the specific skills (TDD, git worktrees, code review) — it's the methodology: **stop before acting, design before building, plan before executing, verify before declaring done, checkpoint with the human at decision points.** That's universal. This plugin translates those principles from a coding context to knowledge work.

## Key Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Plugin name | `superpowers-cowork` | Clear lineage, distinct identity |
| Architecture | Monolithic (single plugin) | Matches how superpowers works and how Cowork marketplace plugins are structured |
| Audience | Knowledge workers broadly | The methodology is universal; domain-specific skills can be layered |
| Activation model | Slash-command only | No always-on bootstrap. Nothing auto-triggers until the user invokes a command. Skills chain automatically within an active workflow |
| Distribution | Cowork marketplace | Standard install path for Cowork users |

## Activation Model

The plugin adds slash commands to the `/` menu. Nothing auto-triggers. The agent behaves normally until the user invokes a command.

Once a command starts a workflow, the underlying skills chain automatically. `/plan` invokes brainstorming → planning → parallel-execution → verification. The user doesn't invoke each skill manually — they start the workflow and checkpoint at each phase transition.

**The plugin stays out of the way for:** Quick questions, simple lookups, calendar checks, one-shot tasks. If you don't invoke a command, you don't get the methodology.

---

## Commands (User-Facing Surface)

| Command | Purpose |
|---------|---------|
| `/superpowers` | Full pipeline — plan → execute → review → deliver |
| `/plan` | Structured workflow for a complex task — clarify scope, break into steps, execute with checkpoints |
| `/research` | Structured research session — define questions, search, evaluate sources, synthesize |
| `/review` | Review an artifact against its goals before delivering |
| `/deliver` | Delivery checkpoint — format, verify, send/save/schedule |
| `/brief` | Cross-source synthesis — "what's the status of X?" pulls from multiple connected tools |
| `/draft` | Start a managed drafting workflow with iteration support |

---

## Skills (19 total, 3 tiers)

### Tier 1: Core Methodology (adapted from superpowers)

| # | Skill | Superpowers Origin | Cowork Adaptation |
|---|-------|-------------------|-------------------|
| 1 | `methodology-core` | `using-superpowers` | Behavioral contract for active workflows — defines the Clarify → Plan → Execute → Verify → Deliver pipeline |
| 2 | `brainstorming` | `brainstorming` | Nearly 1:1. Prevents premature execution. "You asked me to write a report — before I do: who's the audience, what's the format, what sources matter?" |
| 3 | `planning` | `writing-plans` | Task granularity changes: research steps, drafting steps, formatting steps, delivery steps instead of coding tasks with file paths |
| 4 | `parallel-execution` | `subagent-driven-development` / `dispatching-parallel-agents` | Same architecture, different payload. Dispatch subagents to research topics, draft sections, or process data sources in parallel. Two-stage review becomes completeness check → accuracy/tone check |
| 5 | `verification` | `verification-before-completion` + TDD principles | TDD → verification-driven work. Define "done" criteria before doing the work. After completion, verify against those criteria. For factual work: source-checking. For documents: re-read against stated goals. For data: validation passes |
| 6 | `output-review` | `requesting-code-review` | Before delivering any artifact, review against the plan. Check facts, formatting, tone, completeness. Can use a subagent specifically for fact-checking |
| 7 | `delivery` | `finishing-a-development-branch` | Instead of "merge, PR, or discard" → "save to folder, email it, post to Slack, assign in Linear, schedule it, create a persistent artifact, or discard." Includes collaboration primitives as delivery targets |

### Tier 2: Cowork-Specific Skills (no Code equivalent)

| # | Skill | Purpose |
|---|-------|---------|
| 8 | `connector-orchestration` | Decision framework for choosing the right tool at each step. Covers all MCP categories. Handles chaining when a step needs data from one tool pushed to another |
| 9 | `research-methodology` | Structured process: define questions → search (multiple angles) → evaluate source credibility → extract with attribution → synthesize → cite. Teaches triangulation and distinguishing facts vs. analysis vs. opinion |
| 10 | `recurring-work` | When a workflow completes, checks if it should repeat. Captures the workflow definition so recurring runs follow the same methodology. Integrates with `create_scheduled_task` |
| 11 | `format-selection` | Meta-skill that selects and invokes the right file-creation skill based on deliverable type (docx/pptx/xlsx/pdf/markdown/email draft) |
| 12 | `data-transformation` | Handles moving data between formats and tools. The hard problem: extract table from PDF email attachment → clean → put in xlsx → chart → embed in pptx. Manages format transitions and tool handoffs |
| 13 | `artifact-management` | Where outputs and intermediates live. Local file vs. Google Drive folder vs. email attachment. Manages intermediate artifacts during multi-step workflows |
| 14 | `error-handling` | Detect MCP/tool failures → fall back to alternative → tell the user what's degraded. Pattern: try primary → catch failure → try fallback → if no fallback, surface to user with options |
| 15 | `sensitivity-guard` | Checkpoint before combining cross-source data. "This report combines data from [sources]. Appropriate for [audience]?" Flags when internal data might end up in a public-facing deliverable |

### Tier 3: Knowledge Work Skills (new capabilities)

| # | Skill | Purpose |
|---|-------|---------|
| 16 | `cross-source-synthesis` | Pull from multiple connected tools (email + calendar + Drive + web research) and weave a coherent picture. Powers the `/brief` command |
| 17 | `communication-drafting` | Audience-aware messaging. Tone-matching, format adaptation, context-appropriate framing. "Draft this for my manager vs. for the blog post vs. for the email" |
| 18 | `draft-iteration` | Manage feedback loops: draft 1 → get feedback → incorporate → draft 2. Tracks what changed between versions and why. The git-worktrees equivalent for documents |
| 19 | `context-awareness` | Survey connected tools and available data at workflow start. "You have Gmail and Calendar but not Slack — here's what we'll miss" |

---

## Connector Orchestration — Decision Table

Scoped for personal productivity use. Enterprise connectors (project trackers, monitoring, incident management) are out of scope.

| Need | First Choice | Fallback |
|------|-------------|----------|
| Read/send email | Gmail MCP (`search_threads`, `create_draft`) | Browser automation to Gmail |
| Find a document | Google Drive MCP (`search_files`) | Browser automation to Drive |
| Schedule something | Calendar MCP (`create_event`) | Browser automation to Calendar |
| Research a topic | WebSearch → WebFetch | Browser automation to search engines |
| Job search | Job search MCP (if available) | Browser automation to job sites |
| App with no MCP | Browser automation / desktop control | Ask the user to paste it |
| Create a document | File skills (docx/pptx/xlsx/pdf) | Write markdown + convert |

---

## Command Chaining

### `/superpowers` (full pipeline)

```
context-awareness → brainstorming → planning → parallel-execution
    → verification → output-review → sensitivity-guard → delivery
```

Cross-cutting skills active throughout: `connector-orchestration`, `error-handling`, `artifact-management`

### `/plan`

```
context-awareness → brainstorming → planning → [hand off to execution]
```

### `/research`

```
context-awareness → research-methodology → verification (source-check)
    → output-review → delivery
```

### `/review`

```
output-review → fact-checking via verification → sensitivity-guard
```

### `/deliver`

```
sensitivity-guard → format-selection → artifact-management → delivery
```

### `/brief`

```
context-awareness → cross-source-synthesis → verification
    → output-review → delivery
```

### `/draft`

```
brainstorming (scope) → planning (outline) → communication-drafting or parallel-execution
    → draft-iteration (feedback loop) → verification → delivery
```

---

## Plugin Structure

```
superpowers-cowork/
├── .claude-plugin/
│   └── plugin.json
├── CLAUDE.md
├── CONNECTORS.md
├── README.md
├── skills/
│   ├── methodology-core/
│   │   └── SKILL.md
│   ├── brainstorming/
│   │   └── SKILL.md
│   ├── planning/
│   │   └── SKILL.md
│   ├── parallel-execution/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── completeness-reviewer-prompt.md
│   │       └── quality-reviewer-prompt.md
│   ├── verification/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── verification-checklist.md
│   ├── output-review/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── review-dimensions.md
│   ├── delivery/
│   │   └── SKILL.md
│   ├── connector-orchestration/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── mcp-catalog.md
│   ├── research-methodology/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── source-evaluation.md
│   ├── recurring-work/
│   │   └── SKILL.md
│   ├── format-selection/
│   │   └── SKILL.md
│   ├── data-transformation/
│   │   └── SKILL.md
│   ├── artifact-management/
│   │   └── SKILL.md
│   ├── error-handling/
│   │   └── SKILL.md
│   ├── sensitivity-guard/
│   │   └── SKILL.md
│   ├── cross-source-synthesis/
│   │   └── SKILL.md
│   ├── communication-drafting/
│   │   └── SKILL.md
│   ├── draft-iteration/
│   │   └── SKILL.md
│   └── context-awareness/
│       └── SKILL.md
└── commands/
    ├── superpowers/
    ├── plan/
    ├── research/
    ├── review/
    ├── deliver/
    ├── brief/
    └── draft/
```

---

## Skill Detail: Core Methodology

### 1. methodology-core

The behavioral contract loaded when any command activates a workflow. Establishes:

- **Phase discipline:** Every workflow follows Clarify → Plan → Execute → Verify → Deliver. Phases cannot be skipped (but can be fast-tracked if the scope is small).
- **Checkpoint protocol:** At each phase transition, the agent presents its work and waits for human approval before proceeding. The human can redirect, skip ahead, or abort.
- **Brief as artifact:** The brainstorming phase produces a "brief" — a lightweight markdown document capturing scope, audience, deliverable format, success criteria, and constraints. All subsequent phases reference the brief. Default brief template:
  ```
  ## Brief: [Task Title]
  **Goal:** [One sentence]
  **Audience:** [Who will consume the output]
  **Deliverable:** [Format and where it goes]
  **Success criteria:** [Bulleted list — what "done" looks like]
  **Sources:** [What data/tools to use]
  **Constraints:** [Deadline, tone, length, etc.]
  ```
- **Verification-driven work:** Success criteria are defined during the Clarify phase, before any work begins. The Verify phase checks against those criteria. This is the TDD principle generalized.

### 2. brainstorming

Adapted from superpowers brainstorming with these changes:

- **Trigger conditions:** No longer "when you see a coding task." Instead: when a command starts a workflow and the scope isn't yet clear.
- **Question types:** Instead of "what framework, what testing library, what architecture," the questions are: "who's the audience, what's the format, what sources matter, what does done look like, what's the deadline?"
- **Approach proposals:** Still proposes 2-3 approaches with trade-offs before settling. For knowledge work, approaches might be: (A) comprehensive report with primary research, (B) executive summary from existing sources, (C) data-driven analysis with visualizations.
- **Output:** A brief (not a design doc). Shorter, more structured, focused on scope + deliverable + success criteria.

### 3. planning

Adapted from superpowers writing-plans:

- **Task granularity:** Research steps, drafting steps, formatting steps, delivery steps. Not coding tasks with file paths.
- **Tool mapping:** Each task specifies which tools/connectors it will use (e.g., "Research competitor pricing — WebSearch + company website via browser").
- **Verification points:** Each task has a "done" criterion. Groups of tasks have checkpoint gates where the user reviews progress.
- **Estimated effort:** light/medium/heavy per task (not 2-5 minute estimates, because knowledge work timing is less predictable).
- **Dependencies:** Tasks that can run in parallel are explicitly marked.

### 4. parallel-execution

Adapted from superpowers subagent-driven-development:

- **Dispatch model:** Where subagents are available, independent tasks run in parallel. Each subagent gets the brief, the plan, and its specific task.
- **Two-stage review:**
  1. **Completeness check:** Does the output cover what the plan asked for?
  2. **Quality check:** Is it accurate, well-structured, appropriately toned?
- **Sequential fallback:** Where subagents aren't available, tasks execute sequentially with checkpoints between groups.
- **Merge strategy:** When parallel outputs need to be combined (e.g., three research threads into one synthesis), the merge step is explicit in the plan.

### 5. verification

Adapted from superpowers verification-before-completion + TDD principles:

- **Pre-work:** Success criteria defined during brainstorming phase, captured in the brief.
- **Post-work verification by type:**
  - **Factual work:** Source-check claims. Are citations real? Do numbers add up?
  - **Documents:** Re-read against stated goals. Does the structure match the plan?
  - **Data work:** Validation passes. Are formulas correct? Do totals reconcile?
  - **Multi-source work:** Are sources consistent with each other?
  - **Communications:** Does the tone match the audience? Is the message clear?
- **Issue reporting:** Flags issues by severity (critical/moderate/minor). Critical issues block delivery. The agent fixes what it can, escalates what it can't.

### 6. output-review

Adapted from superpowers requesting-code-review:

- **Review dimensions:**
  - **Completeness:** Does it cover everything in the brief?
  - **Accuracy:** Are facts correct? Are sources cited?
  - **Structure:** Is it well-organized? Does it flow logically?
  - **Tone:** Is it appropriate for the audience?
  - **Format:** Does it meet the deliverable requirements?
- **Review mode:** Can use a subagent for independent review (the subagent only gets the brief and the output, not the process context, so it reviews with fresh eyes).
- **Severity levels:** Critical (blocks delivery), moderate (worth fixing), minor (optional polish).

### 7. delivery

Adapted from superpowers finishing-a-development-branch:

- **Decision tree:**
  - Save to a specific folder / path
  - Email it (via Gmail connector)
  - Schedule it for later (via `create_scheduled_task`)
  - Create a persistent artifact
  - Upload to Google Drive
  - Discard and start over
- **Format conversion:** If the brief specifies a format different from what was produced, invoke format-selection to convert.
- **Recurring check:** After delivery, checks if this workflow should repeat (hands off to recurring-work if so).
- **Summary:** Produces a brief summary of what was delivered, where it went, and what the success criteria were.

---

## Skill Detail: Cowork-Specific Skills

### 8. connector-orchestration

A decision framework skill — not a workflow step, but a reference that other skills consult when choosing tools. See the Connector Orchestration table above for the full decision matrix.

Key behaviors:
- **Prefer MCP over browser automation.** MCP calls are faster, more reliable, and don't require screen context.
- **Chain gracefully.** When a step needs data from one tool pushed to another (e.g., email attachment → spreadsheet), sequence the calls and handle the data handoff.
- **Degrade transparently.** If a preferred MCP isn't connected, fall back to browser automation. If browser automation isn't available, ask the user to provide the data. Always tell the user what's degraded.

### 9. research-methodology

Structured research process:

1. **Define questions** — What specifically are we trying to learn? Prevents unfocused browsing.
2. **Search** — WebSearch with targeted queries from multiple angles. At least 3 different query formulations.
3. **Evaluate** — Source credibility (authority, recency, potential bias), relevance to the question, agreement/disagreement with other sources.
4. **Extract** — Pull key findings with attribution. Note the source for every claim.
5. **Synthesize** — Combine findings into a coherent summary that answers the original questions. Distinguish between facts, analysis, and opinion.
6. **Cite** — Track where every claim came from so verification can source-check later.

Key principles:
- **Triangulate.** Don't trust a single source. Look for corroboration from independent sources.
- **Flag conflicts.** When sources disagree, present both sides rather than picking one.
- **Date-stamp.** Note when sources were published — stale data is a common failure mode.

### 10. recurring-work

When a workflow completes, the delivery phase invokes this skill to check: "Is this something that should happen again?"

If yes:
- Capture the workflow definition (what brief, what plan, what tools, what format)
- Propose a schedule (daily, weekly, monthly, or custom cron)
- Set up via `create_scheduled_task` with the full workflow context (delegates to Cowork's built-in `schedule` skill for the actual cron creation)
- The recurring run follows the same methodology — it doesn't just re-run blindly, it re-verifies

If no: skip silently.

### 11. format-selection

Decision table for output format:

| Deliverable type | Format | Skill invoked |
|-----------------|--------|---------------|
| Report, memo, letter | .docx | docx skill |
| Presentation, deck, slides | .pptx | pptx skill |
| Data, analysis, budget, tracker | .xlsx | xlsx skill |
| Printable, fixed-layout, formal | .pdf | pdf skill |
| Quick sharing, web, internal | Markdown artifact | None (native) |
| Email | Draft via Gmail MCP | Gmail connector |

Handles the translation when work was done in plain text during execution but needs to be formatted for delivery.

### 12. data-transformation

Manages format transitions and tool handoffs in multi-step workflows. The hard problems:

- Extract table from PDF email attachment → clean → put in xlsx → chart → embed in pptx
- Pull data from multiple xlsx files → combine → validate → summarize in docx
- Scrape web data → structure → validate → export to csv/xlsx

Key behaviors:
- Track data lineage (where did this number come from?)
- Validate at each transformation step (did we lose data? Did formats shift?)
- Keep intermediate artifacts accessible for debugging

### 13. artifact-management

Manages where outputs and intermediates live during a workflow:

- **Working directory:** Where intermediate files are created during execution
- **Final destination:** Where the deliverable ends up (local, Drive, email, etc.)
- **Version tracking:** Keep previous versions accessible (the git-worktrees principle — don't overwrite, don't lose previous state)
- **Cleanup:** After delivery, offer to clean up intermediates or keep them for reference

### 14. error-handling

Cross-cutting skill for tool failure recovery:

```
try primary tool (MCP)
  → if fail: try fallback (browser automation)
    → if fail: try alternative (different tool, manual input)
      → if fail: surface to user with options
```

Key behaviors:
- Never silently swallow errors
- Tell the user what's degraded ("I couldn't access your Gmail, so I'm working from the information you've provided")
- Suggest fixes ("Want me to try browser automation instead, or can you paste the email content?")
- Track which tools failed so the plan can be adjusted

### 15. sensitivity-guard

Checkpoint before combining data from different sources or audiences:

- "This report combines data from [your personal email] and [public web sources]. Appropriate for [this recipient]?"
- "This document references information from [your calendar/Drive]. Should this be shared externally?"
- Flag when personal or private data might end up in a shared deliverable
- Flag when PII appears in a cross-source synthesis
- Always checkpoint with the user — never make sensitivity decisions autonomously

---

## Skill Detail: Knowledge Work Skills

### 16. cross-source-synthesis

Powers the `/brief` command. Pulls from connected personal tools and weaves a coherent picture:

- **Survey available sources:** What's connected? What time range is relevant?
- **Pull in parallel:** Query each source for relevant information simultaneously (email threads, calendar events, Drive documents, web research)
- **Reconcile:** Handle conflicting information across sources
- **Synthesize:** Produce a unified summary with source attribution
- **Highlight gaps:** "Based on your connected tools, I couldn't find information about [X]. You might check [Y]."

### 17. communication-drafting

Audience-aware messaging skill:

- Adapt the same content for different audiences or contexts ("draft this for my manager" vs. "for the blog post" vs. "for the email")
- Tone-matching based on recipient and medium (formal email vs. casual message vs. professional document)
- Format-awareness (email length norms, document structure expectations)

### 18. draft-iteration

Manage feedback loops for documents and communications:

- **Version tracking:** Draft 1 → feedback → Draft 2. Track what changed and why.
- **Feedback integration:** Parse human feedback and map it to specific sections/changes
- **Diff presentation:** Show what changed between versions in a readable format
- **Convergence:** Track whether iterations are converging (feedback getting smaller) or diverging (fundamental direction problems). Flag divergence early.

### 19. context-awareness

Runs at the start of any workflow to survey the environment:

- What MCP connectors are available?
- What tools are connected and responsive?
- What data can we access vs. what would we need the user to provide?
- What's the likely best approach given the available tools?

Outputs a brief environment summary:
> "You have Gmail and Calendar connected but not Slack. I can pull email threads and meeting context, but for team discussions you'll need to share relevant messages. Google Drive is connected — I can search your documents."

---

## CONNECTORS.md Design

Uses the same `~~category` placeholder system as existing Cowork plugins:

| Category | Placeholder | Primary servers | Alternatives |
|----------|------------|----------------|-------------|
| Email | `~~email` | Gmail | Outlook |
| Calendar | `~~calendar` | Google Calendar | Outlook Calendar |
| File storage | `~~file storage` | Google Drive | Dropbox, OneDrive |
| Job search | `~~job search` | (if available) | Browser automation |

---

## CLAUDE.md Bootstrap

Lightweight — registers commands and describes what the plugin does. Does NOT auto-trigger anything.

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

---

## Testing Strategy

### Evaluation Approach

Since there's no compiler or test suite for knowledge work, evaluation is human judgment + structural checks:

1. **Methodology adherence:** Does the agent actually follow Clarify → Plan → Execute → Verify → Deliver? Or does it skip phases?
2. **Checkpoint discipline:** Does it stop and wait for human input at phase transitions?
3. **Tool selection:** Does connector-orchestration choose the right tool? Does it fall back gracefully?
4. **Output quality:** Are deliverables well-structured, accurate, and appropriately formatted?
5. **Sensitivity:** Does the sensitivity-guard fire when it should?

### Test Scenarios

| Scenario | Commands Used | Skills Exercised |
|----------|-------------|-----------------|
| "Prepare a competitive analysis deck on three companies" | `/superpowers` | All Tier 1 + research-methodology + format-selection |
| "What's going on with my house refinance?" (with email + calendar + Drive connected) | `/brief` | cross-source-synthesis + context-awareness + verification |
| "Draft an email to my accountant about Q2 expenses" | `/draft` | brainstorming + communication-drafting + sensitivity-guard + delivery |
| "Research the latest trends in AI regulation" | `/research` | research-methodology + verification + output-review |
| "Set up a weekly team status report" | `/superpowers` | Full pipeline + recurring-work |

### Metrics

- **Phase completion rate:** How often does the full pipeline complete without the user aborting?
- **Checkpoint quality:** Do checkpoints ask useful questions, or are they pro-forma?
- **Tool selection accuracy:** How often does the agent choose the right tool on first try?
- **Redo rate:** How often does the user ask the agent to redo a step?
- **Sensitivity catch rate:** Does the guard fire for cross-source sensitivity issues?

---

## Implementation Priority

Recommended build order by tier:

### Phase 1: Core Methodology (Tier 1)
Skills 1-7 + commands `/plan`, `/superpowers`, `/review`, `/deliver`

This alone is a useful plugin. It brings superpowers' disciplined methodology to any Cowork task.

### Phase 2: Cowork-Specific (Tier 2)
Skills 8-15 + no new commands (these skills enhance existing commands)

This makes the plugin Cowork-native — it understands tools, handles errors, manages artifacts.

### Phase 3: Knowledge Work (Tier 3)
Skills 16-19 + commands `/brief`, `/draft`, `/research`

This is what makes the plugin exceptional — cross-source synthesis, multi-audience drafting, iterative document management.

---

## Open Questions

1. **Command naming:** Should commands use the `superpowers:` namespace (e.g., `/superpowers:plan`) or be top-level (`/plan`)? Top-level is cleaner but might conflict with other plugins.
2. **Brief format:** What's the right structure for the "brief" artifact? Markdown? JSON? A specific template?
3. **Subagent availability:** Does Cowork reliably support subagent dispatch? If not, parallel-execution needs to degrade to sequential with explicit checkpoints.
4. **Plugin size limits:** Is there a max SKILL.md size or total plugin size for the Cowork marketplace?
5. **Hook support:** Does Cowork support pre/post task hooks the way Claude Code does? If so, methodology-core could use hooks for checkpoint enforcement.
