# Agent Audit Session — 2026-05-27

**Plugin:** superpowers-cowork v0.1.0  
**Goal:** Audit and improve all skills — token efficiency, enforcement gates, structural fixes, cross-reference integrity  
**Method:** 7-agent team (`cowork-audit`) using `TeamCreate` / shared task list / `SendMessage` coordination  

---

## Background

The superpowers-cowork plugin brings the superpowers methodology (stop before acting, plan before executing, verify before declaring done, checkpoint with the human) to Cowork knowledge workers. It had evolved through three prior versions:

- **v0.1 → v0.2:** Skills were too verbose → massive token burn. Compacted skills, moved detail to `references/`.
- **v0.2 → v0.3:** Compact skills got skipped by Cowork → needed harder enforcement gates.
- **Current WD:** Commands migrated from `commands/` directory to skills (correct Cowork format), but brought structural problems along.

**Design principle:** SKILL.md = compact enforcement contract (numbered steps, explicit hard stops, no prose). `references/` = detailed how-to (checklists, rubrics, examples).

---

## North Star

> *"The methodology, not the skills. Stop before acting, plan before executing, verify before declaring done, checkpoint with the human."*

---

## Audit Checklist (applied to every SKILL.md)

| # | Check | Pass condition |
|---|-------|----------------|
| 1 | Step enforcement | All steps numbered; none written as suggestions or bullets |
| 2 | Hard stops | At least one explicit "Do not proceed until…" gate |
| 3 | Token efficiency | No sentence removable without losing enforcement |
| 4 | References offloaded | Detail/examples in `references/`, not SKILL.md body |
| 5 | Description length | ≤ 25 words |
| 6 | Frontmatter name | Matches directory name exactly |
| 7 | Cross-references | All named skills resolve to real `skills/<name>/` directories |

---

## Team Structure

The audit ran as a proper agent team using Claude Code's `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` feature:

- **TeamCreate** stood up the `cowork-audit` team with a shared task list
- **6 parallel agents (A–F)** worked concurrently on non-overlapping file domains
- **1 coordinator (G)** was blocked on all 6 and ran after they completed
- Agents communicated via **SendMessage** and tracked progress via **TaskUpdate**

| Agent | Domain | Task |
|-------|---------|------|
| A | methodology-core + 4 cross-cutting skills | #1 |
| B | 7 command entry-point skills | #2 |
| C | brainstorming, planning, parallel-execution, verification | #3 |
| D | output-review, delivery, format-selection, recurring-work | #4 |
| E | research-methodology, communication-drafting, draft-iteration, context-awareness | #5 |
| F | cross-source-synthesis, data-transformation + all reference files | #6 |
| G | Final consistency pass across entire plugin | #7 (blocked on 1–6) |

---

## What Each Agent Did

### Agent A — Core + Cross-Cutting Skills

**Files owned:** `methodology-core`, `connector-orchestration`, `artifact-management`, `error-handling`, `sensitivity-guard`, `connector-orchestration/references/mcp-catalog.md`

**Fixes:**
- `methodology-core/SKILL.md` — rewrote all actionable sections from bullets to numbered steps; added explicit bolded hard stops (`**Do not plan until approved**`, `**Do not execute until approved**`, `**Do not deliver until resolved**`); embedded north-star phrasing in the opening
- `connector-orchestration/references/mcp-catalog.md` — fixed 3 strikethrough artifacts (`~~email`, `~~calendar`, `~~file storage`) that rendered as visible tildes; replaced with clean `Gmail MCP / Google Calendar MCP / Google Drive MCP` labels
- Deleted `methodology-core/references/mcp-catalog.md` — duplicate of connector-orchestration reference; removed empty `references/` dir

**Already good:** `connector-orchestration`, `artifact-management`, `error-handling`, `sensitivity-guard` — all four passed full checklist as-is

---

### Agent B — Command Entry-Point Skills

**Files owned:** `superpowers`, `plan`, `research`, `review`, `deliver`, `brief`, `draft`

**Fixes:**
- `superpowers/SKILL.md` step 8 — tightened delivery gate: "State exactly what will be sent, where, and in what format. **Do not execute delivery until the human explicitly confirms.**"
- `research/SKILL.md` step 5 — added explicit gate: "**Do not proceed if critical gaps or errors are found — report and ask how to handle.**"; step 7 tightened with explicit delivery confirmation gate
- `brief/SKILL.md` step 7 — tightened to: "State the destination explicitly. **Do not execute delivery until the human explicitly confirms.**"
- `draft/SKILL.md` step 7 — tightened to: "State the destination and format explicitly. **Do not execute delivery until the human explicitly confirms.**"

**Already good:** `plan`, `review`, `deliver` — explicit gates at every human checkpoint; all 7 had `argument-hint:` frontmatter, entry-point identity notes, descriptions ≤ 25 words, and all pipeline skill names resolving to real directories

---

### Agent C — Workflow Engine Skills

**Files owned:** `brainstorming`, `planning`, `parallel-execution`, `verification` + their `references/` subdirectories

**Fixes:**
- `brainstorming/SKILL.md` — converted main-body bullets to numbered list (steps 1–4); strengthened hard stop to "Do not proceed until the human explicitly approves"
- `planning/SKILL.md` — converted main-body bullets to numbered list (steps 1–5); moved "do not begin until approval" enforcement into the hard stop block itself

**Already good:** `parallel-execution` (explicit hard stop on merge step, sequential fallback also gated), `verification` (two hard stops — one if no brief, one if critical issues), all four `references/` files (detailed, actionable, no TBDs; `dispatch-guide.md` has full dispatch prompt template)

---

### Agent D — Review & Delivery Skills

**Files owned:** `output-review`, `delivery`, `format-selection`, `recurring-work` + their `references/` subdirectories

**Fixes:**
- `delivery/references/delivery-guide.md` — fixed 2 strikethrough artifacts; rewrote as clean 6-option decision tree: save-to-folder, email draft (Gmail MCP), Drive upload, schedule (create_scheduled_task), conversation artifact, discard
- `format-selection/SKILL.md` — was missing HTML artifact row (only 5 of 6 formats); rewrote with 4 numbered steps; added "do not silently downgrade" hard stop for content/format mismatches
- `output-review/SKILL.md` — promoted "Independent review mode" hanging paragraph to numbered step 8; reformatted Critical/Moderate/Minor handling into numbered steps 6–7; sharpened step-5 hard stop
- `recurring-work/references/setup-guide.md` — removed ambiguous reference to a non-existent "Cowork schedule skill"; correctly names `create_scheduled_task` as the tool

**Already good:** `delivery/SKILL.md`, `recurring-work/SKILL.md`, `output-review/references/review-dimensions.md`

---

### Agent E — Research & Comms Skills

**Files owned:** `research-methodology`, `communication-drafting`, `draft-iteration`, `context-awareness` + their `references/` subdirectories

**Fixes:**
- `communication-drafting/SKILL.md` — added explicit hard stop (new step 3) gating drafting on audience + tone + format being named; renumbered downstream steps
- `draft-iteration/SKILL.md` — rewrote steps to make v1 → feedback → v2 checkpoint pattern explicit; v1 is now a discrete checkpoint that cannot be passed without feedback; added second hard stop for loop termination
- `context-awareness/SKILL.md` — expanded tool inventory to full Cowork surface: named all MCP connectors (Gmail, Calendar, Drive, Linear/Asana/Jira, Slack, iMessage, Notes, GitHub) and added always-available tools (Word/PPT/XLSX/PDF file creation, scheduled-tasks/cron)

**Already good:** `research-methodology/SKILL.md` — enforces source evaluation (step 3) before synthesis (step 6), hard stop at step 5 requiring 2+ independent sources, mandates citation in step 8, conflict-handling rule included

---

### Agent F — Synthesis Skills + All Reference Files

**Files owned:** `cross-source-synthesis`, `data-transformation` + full audit of all 16 reference files across the plugin

**Fixes:**
- `connector-orchestration/references/mcp-catalog.md` — expanded tool listings: added Native macOS Apps section (Apple Notes, iMessage), Browser Automation section (Claude in Chrome MCP), and computer-use MCP section with tier-restriction note; expanded Scheduled Tasks section with full tool set

**Already good:** `cross-source-synthesis/SKILL.md` (lineage tracking, conflict hard stop, attribution required), `data-transformation/SKILL.md` (lineage tracking enforced in steps 1/4/5, validate-before-proceeding hard stop)

**All 16 reference files audited — clean:**
1. `brainstorming/references/brainstorming-guide.md`
2. `communication-drafting/references/drafting-guide.md`
3. `connector-orchestration/references/mcp-catalog.md` *(fixed — see above)*
4. `cross-source-synthesis/references/synthesis-process.md`
5. `data-transformation/references/transformation-patterns.md`
6. `delivery/references/delivery-guide.md` *(fixed by Agent D)*
7. `draft-iteration/references/iteration-guide.md`
8. `output-review/references/review-dimensions.md`
9. `parallel-execution/references/completeness-reviewer.md`
10. `parallel-execution/references/dispatch-guide.md`
11. `parallel-execution/references/quality-reviewer.md`
12. `planning/references/planning-guide.md`
13. `recurring-work/references/setup-guide.md` *(fixed by Agent D)*
14. `research-methodology/references/research-process.md`
15. `research-methodology/references/source-evaluation.md`
16. `verification/references/verification-checklist.md`

---

### Agent G — Coordinator Consistency Pass

**Scope:** Full plugin consistency check after all A–F work landed

**6 checks run — all passed, no fixes needed:**

1. **Cross-references resolve** — Every skill name mentioned in a pipeline maps to an existing `skills/<name>/` directory. 18 skill names verified.

2. **Frontmatter name matches directory** — All 27 skills pass. Zero mismatches.

3. **Cross-cutting skills wired into pipelines** — `connector-orchestration`, `error-handling`, `artifact-management` appear in `superpowers/SKILL.md` as "Cross-cutting skills active throughout." `sensitivity-guard` appears before delivery in `superpowers`, `research`, `brief`, `draft`, `deliver`, and as terminal gate in `review`. `plan` correctly has no sensitivity-guard (no delivery step).

4. **plugin.json valid** — Valid JSON; has `name`, `version`, `description`, `author.name`.

5. **No naming collisions** — `plan` vs `planning`, `deliver` vs `delivery`, `research` vs `research-methodology` — all confirmed as distinct command entry-point / implementation skill pairs with different roles.

6. **No orphaned reference files** — All 16 reference files linked from their sibling SKILL.md or transitively from another reference file in the same skill.

---

## Final State

```
27 skills total
  7  command entry-points (user-invocable via /name)
 16  implementation skills (internal pipeline steps)
  4  cross-cutting skills (active throughout any workflow)
```

**Every SKILL.md:**
- Numbered steps, no bullets
- At least one explicit "Do not proceed until…" hard stop
- Description ≤ 25 words
- `name:` frontmatter matches directory name
- Detail offloaded to `references/` not body
- All named cross-references resolve

**Cross-cutting skills (standalone invocable):**
- `connector-orchestration` — MCP → browser → desktop → ask; fallback hierarchy
- `artifact-management` — working directory, versioned drafts, no-delete-without-confirmation
- `error-handling` — one retry, then fallback, then human; no silent failures
- `sensitivity-guard` — cross-source checkpoint, PII flag, human signs off before delivery

**plugin.json:** superpowers-cowork v0.1.0 ✓

---

## Process Note

This session corrected an earlier mistake: the first audit run used fire-and-forget `Agent` tool calls (isolated subagents, no shared state, no communication). The second run used the proper `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` infrastructure: `TeamCreate` for a named team, `TaskCreate` for a shared task list with dependency tracking, `Agent` with `team_name` and `name` parameters so agents could use `SendMessage` to communicate and `TaskUpdate` to coordinate. Agent G was blocked in the task list until A–F all completed, ensuring the coordinator pass only ran on finished work.
