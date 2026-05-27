# Pipeline Gate Enforcement Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Close the three root causes that let a model skip the `/superpowers` pipeline approval gates when a task appears well-specified.

**Architecture:** Pure text edits to existing skill Markdown files. No new files. Changes are in three independent clusters (orchestrator, brainstorming, remaining pipeline skills) that can be executed in any order and committed separately.

**Tech Stack:** Markdown, git

**Spec:** `docs/superpowers/specs/2026-05-27-pipeline-gate-enforcement-design.md`

---

## Task 1: Harden `skills/superpowers/SKILL.md`

**Files:**
- Modify: `skills/superpowers/SKILL.md`

Adds a `<HARD-GATE>` block, a rationalization table, and inline STOP markers between pipeline steps. This is the primary fix for RC-1 (orchestrator lacks semantic barriers).

- [ ] **Step 1: Confirm current state**

```bash
grep -n "fast-track\|HARD-GATE\|rationali" skills/superpowers/SKILL.md
```

Expected: no output (none of these patterns exist yet).

- [ ] **Step 2: Replace the file contents**

Replace the full content of `skills/superpowers/SKILL.md` with:

```markdown
---
name: superpowers
description: Full structured workflow — clarify, plan, execute, verify, deliver
argument-hint: "<task description>"
---

> Entry point — chains to implementation skills. Users invoke this directly.

<HARD-GATE>
Do NOT skip, reorder, merge, or abbreviate pipeline steps. Every gate is mandatory
regardless of task complexity, how much context was provided upfront, or how
well-specified the request appears. Well-specified input is raw material for the
brief — it is not approval of the brief.
</HARD-GATE>

## Common Rationalizations — All Wrong

| Thought | Reality |
|---------|---------|
| "The task is fully specified — I can skip the brief" | A detailed description is input *to* the brief, not approval *of* it. |
| "I already know what the plan will be" | Present it and wait. The gate exists to catch assumptions. |
| "The human seems ready to go" | Wait for an explicit "yes" on the brief and plan. Eagerness is not approval. |
| "Verification/review would just confirm it's fine" | Run them anyway. If fine, cost is one minute. If not, you prevented a bad delivery. |

# /superpowers

## Pipeline

Load and invoke the following internal skills in order:

```
brainstorming → context-awareness → planning → parallel-execution → verification → output-review → sensitivity-guard → delivery
```

Cross-cutting skills active throughout: `connector-orchestration`, `error-handling`, `artifact-management`.

## Invoke: @$1

If no argument, ask: "What would you like to work on?" Do not proceed until you have a task.

## Steps

1. **Clarify.** You MUST invoke the `brainstorming` skill now. Ask questions to understand the task. Produce a brief with success criteria. Present the brief. **Do not proceed to step 2 until the human explicitly approves the brief.**

> **GATE 1 — Do not proceed to step 2 until the human has said yes to the brief.**

2. **Context check.** You MUST invoke the `context-awareness` skill now for only the connector categories named in the brief. Report what's available.

> **GATE 2 — Do not proceed to step 3 until the context report is complete and any critical gaps are resolved or explicitly waived.**

3. **Plan.** You MUST invoke the `planning` skill now. Break the task into numbered steps with tool assignments and "done when" criteria. Present the plan. **Do not begin execution until the human explicitly approves the plan.**

> **GATE 3 — Do not proceed to step 4 until the human has said yes to the plan.**

4. **Execute.** You MUST invoke the `parallel-execution` skill now. Work through the plan. Use subagents for parallel steps. Track progress against the plan.

5. **Verify.** You MUST invoke the `verification` skill now. Check every output against the success criteria from the brief. Flag issues. **Do not proceed if verification fails — report and ask how to handle.**

> **GATE 5 — Do not proceed to step 6 until all critical verification issues are resolved or explicitly waived by the human.**

6. **Review.** You MUST invoke the `output-review` skill now. Pre-delivery audit: completeness, accuracy, structure, tone. **Do not proceed if critical issues found — stop and ask how to handle.**

> **GATE 6 — Do not proceed to step 7 until the human directs you to.**

7. **Sensitivity check.** You MUST invoke the `sensitivity-guard` skill now. Present: "This output includes data from [sources]. Going to [destination]. Anything look off?" **Do not deliver until the human signs off.**

> **GATE 7 — Do not proceed to step 8 until the human explicitly signs off on the sensitivity check.**

8. **Deliver.** You MUST invoke the `delivery` skill now. Choose format and destination. State exactly what will be sent, where, and in what format. **Do not execute delivery until the human explicitly confirms.**
```

- [ ] **Step 3: Verify HARD-GATE block is present**

```bash
grep -n "HARD-GATE\|GATE 1\|GATE 3\|GATE 7\|rationali" skills/superpowers/SKILL.md
```

Expected output (exact line numbers may differ):
```
8:<HARD-GATE>
13:</HARD-GATE>
16:## Common Rationalizations — All Wrong
...
(lines for GATE 1, GATE 2, GATE 3, GATE 5, GATE 6, GATE 7)
```

- [ ] **Step 4: Verify fast-track language is absent**

```bash
grep -n "fast-track\|well-specified.*skip\|skip.*checkpoint" skills/superpowers/SKILL.md
```

Expected: no output.

- [ ] **Step 5: Commit**

```bash
git add skills/superpowers/SKILL.md
git commit -m "fix: add HARD-GATE block and rationalization table to superpowers orchestrator

Closes RC-1: orchestrator now has semantic barriers that prevent models from
rationalizing past pipeline steps when a task appears well-specified.
Each gate is labeled inline between the relevant steps."
```

---

## Task 2: Remove fast-track clause from brainstorming skill and guide

**Files:**
- Modify: `skills/brainstorming/SKILL.md`
- Modify: `skills/brainstorming/references/brainstorming-guide.md`

Removes the clause that gave explicit permission to skip brainstorming questions when context is rich — the rationalization pathway for RC-2 and RC-3.

- [ ] **Step 1: Confirm fast-track clause exists in SKILL.md**

```bash
grep -n "fast-track" skills/brainstorming/SKILL.md
```

Expected:
```
10:1. Assess scope. If the request already specifies audience, format, and success criteria, fast-track to writing the brief — skip the question-asking, not the checkpoint.
```

- [ ] **Step 2: Remove the fast-track clause from SKILL.md**

Replace the `## Brainstorming` numbered list so step 1 becomes the scope assessment without the fast-track clause. Replace this exact block:

Old content:
```
# Brainstorming

Clarify phase. Before producing anything, understand what and why.

1. Assess scope. If the request already specifies audience, format, and success criteria, fast-track to writing the brief — skip the question-asking, not the checkpoint.
2. Ask questions **one at a time**. Prefer multiple choice.
3. Propose 2-3 approaches with trade-offs. Lead with your recommendation.
4. Write the brief using the template from `methodology-core`. Present for confirmation.

## The Hard Stop

After writing the brief, do exactly this:

1. Show the brief.
2. Ask: "Does this capture what you need?"
3. **Stop. Do nothing else. Do not proceed until the human explicitly approves.**

No file reading. No searching. No drafts. Not even a small exploratory step. Wait for the human to say yes before any further action. Rich context from a form, a long task description, or prior conversation is input *to* the brief — it is not approval *of* the brief.
```

New content:
```
# Brainstorming

Clarify phase. Before producing anything, understand what and why.

1. Assess scope. Ask questions **one at a time** to understand goal, audience, format, sources, success criteria, and constraints. Prefer multiple choice.
2. Propose 2-3 approaches with trade-offs. Lead with your recommendation.
3. Write the brief using the template from `methodology-core`. Present for confirmation.

## The Hard Stop

After writing the brief, do exactly this:

1. Show the brief.
2. Ask: "Does this capture what you need?"
3. **Stop. Do nothing else. Do not proceed until the human explicitly approves.**

No file reading. No searching. No drafts. Not even a small exploratory step. Wait for the human to say yes before any further action. Rich context from a form, a long task description, or prior conversation is input *to* the brief — it is not approval *of* the brief. A detailed task description is not approval of the brief.
```

- [ ] **Step 3: Verify fast-track clause is gone from SKILL.md**

```bash
grep -n "fast-track" skills/brainstorming/SKILL.md
```

Expected: no output.

- [ ] **Step 4: Confirm "Fast-track when clear" bullet exists in guide**

```bash
grep -n "Fast-track" skills/brainstorming/references/brainstorming-guide.md
```

Expected:
```
23:- **Fast-track when clear.** If the request already has enough detail, don't ask questions for the sake of asking. Summarize, confirm, move on.
```

- [ ] **Step 5: Remove "Fast-track when clear" bullet from guide**

In `skills/brainstorming/references/brainstorming-guide.md`, under `## Key Principles`, remove this line:

```
- **Fast-track when clear.** If the request already has enough detail, don't ask questions for the sake of asking. Summarize, confirm, move on.
```

The section should go from 4 bullets to 3. The Anti-Pattern sections lower in the file already explain the correct behavior and can stand alone.

- [ ] **Step 6: Verify fast-track bullet is gone from guide**

```bash
grep -n "Fast-track\|fast-track" skills/brainstorming/references/brainstorming-guide.md
```

Expected: no output.

- [ ] **Step 7: Commit**

```bash
git add skills/brainstorming/SKILL.md skills/brainstorming/references/brainstorming-guide.md
git commit -m "fix: remove fast-track clause from brainstorming skill and guide

Closes RC-2 and RC-3: the 'fast-track' language in SKILL.md and the
'Fast-track when clear' Key Principle in the guide were the rationalization
pathway that allowed models to skip the brief checkpoint entirely when a
task appeared well-specified. The anti-pattern sections in the guide already
explain the correct behavior."
```

---

## Task 3: Audit and strengthen unconditional stops in remaining pipeline skills

**Files:**
- Modify: `skills/context-awareness/SKILL.md`
- Modify: `skills/verification/SKILL.md`
- Modify: `skills/output-review/SKILL.md`
- Modify: `skills/sensitivity-guard/SKILL.md`
- Modify: `skills/delivery/SKILL.md`

Ensures every pipeline skill has an unconditional terminal stop that prevents the model from self-advancing to the next stage.

### 3a — context-awareness

- [ ] **Step 1: Read and assess current terminal step**

```bash
tail -5 skills/context-awareness/SKILL.md
```

Expected current ending:
```
6. **Adjust the plan.** If gaps are minor or fallbacks exist, note them and proceed. Pass the environment summary to the `planning` skill so the plan accounts for what's actually available.
```

Step 6 currently says "note them and **proceed**" — this is the only pipeline skill that actively instructs proceeding. Add a terminal stop.

- [ ] **Step 2: Add terminal stop to context-awareness**

Append to the end of `skills/context-awareness/SKILL.md` (after step 6, as a new paragraph):

```markdown

**Handoff stop:** When the context report is complete and gaps are resolved (or waived), report the summary to the human. **Do not invoke the `planning` skill until the human acknowledges the context report and confirms they want to proceed.**
```

- [ ] **Step 3: Verify**

```bash
grep -n "Handoff stop\|Do not invoke.*planning" skills/context-awareness/SKILL.md
```

Expected: one matching line.

### 3b — verification

- [ ] **Step 4: Read current Hard Stop language**

```bash
grep -n "stop\|Stop\|block\|do not proceed" skills/verification/SKILL.md
```

Expected: the existing conditional stop is on critical issues only. Add an unconditional terminal that fires regardless of findings.

- [ ] **Step 5: Add unconditional terminal to verification**

Append to end of `skills/verification/SKILL.md`:

```markdown

**Terminal stop:** After completing all checks and fixing any moderate/minor issues, report findings to the human: "Verification complete. [N critical / N moderate / N minor]. [Summary]." **Stop. Do not proceed to output-review until the human directs the next step. If any critical issues exist, do not proceed at all — wait for the human to decide how to handle them.**
```

- [ ] **Step 6: Verify**

```bash
grep -n "Terminal stop\|do not proceed to output-review" skills/verification/SKILL.md
```

Expected: one matching line.

### 3c — output-review

- [ ] **Step 7: Read current stop language**

```bash
grep -n "stop\|Stop\|Do not proceed" skills/output-review/SKILL.md
```

Expected: step 5 currently reads "Stop. Do not proceed to delivery until the human directs the next step." — **this wording is a bug**: the next step after output-review is `sensitivity-guard`, not delivery directly. A model reading this could skip the sensitivity-guard step. Fix it.

- [ ] **Step 8: Fix the incorrect next-step reference in output-review**

In `skills/output-review/SKILL.md`, find and replace step 5:

Old:
```
5. **Stop. Do not proceed to delivery until the human directs the next step.**
```

New:
```
5. **Stop. Do not proceed to the sensitivity check or delivery until the human directs the next step.**
```

- [ ] **Step 9: Verify**

```bash
grep -n "Do not proceed\|sensitivity check" skills/output-review/SKILL.md
```

Expected: one match containing "sensitivity check".

### 3d — sensitivity-guard

- [ ] **Step 9: Read current stop language**

```bash
grep -n "Wait\|Do not proceed\|stop\|Stop" skills/sensitivity-guard/SKILL.md
```

Expected: step 4 has "Wait for explicit confirmation. Do not proceed without it." — sufficient if present. Confirm.

If present and clear: no change needed, proceed to 3e.

If missing or conditional: append:

```markdown

**Terminal stop:** **Do not invoke `delivery` until the human has explicitly confirmed the sensitivity check. "Looks fine" counts. Silence does not.**
```

- [ ] **Step 10: Verify**

```bash
grep -n "Do not proceed\|Do not invoke.*delivery\|Wait for explicit" skills/sensitivity-guard/SKILL.md
```

Expected: at least one match.

### 3e — delivery

- [ ] **Step 11: Read current stop language**

```bash
grep -n "confirm\|Confirm\|Do not execute\|Stop" skills/delivery/SKILL.md
```

Expected: step 3 has "require explicit confirmation every time ... Do not execute until the human says yes." — sufficient if present. Confirm.

If present and clear: no change needed.

If missing or conditional: append:

```markdown

**Terminal stop:** **Do not execute any delivery action until the human explicitly confirms the destination and format in this conversation turn. Prior approval in the brief does not count.**
```

- [ ] **Step 12: Verify all five skills have terminal stops**

```bash
grep -rn "Terminal stop\|Handoff stop\|Do not invoke.*planning\|Do not proceed to output-review\|Do not execute.*delivery" skills/context-awareness/ skills/verification/ skills/output-review/ skills/sensitivity-guard/ skills/delivery/
```

Expected: at least one match per file (5 total across the 5 paths).

- [ ] **Step 13: Commit**

```bash
git add skills/context-awareness/SKILL.md skills/verification/SKILL.md skills/output-review/SKILL.md skills/sensitivity-guard/SKILL.md skills/delivery/SKILL.md
git commit -m "fix: add unconditional terminal stops to all remaining pipeline skills

Each pipeline skill now has a clear unconditional stop at its handoff point.
context-awareness was the only one actively instructing 'proceed'; that is
replaced with a human-acknowledgment gate. verification, output-review,
sensitivity-guard, and delivery stops confirmed or added."
```

---

## Integration Verification

After all three tasks are committed:

- [ ] **Step 1: Confirm no fast-track language anywhere in skills/**

```bash
grep -rn "fast-track\|fast track" skills/
```

Expected: no output.

- [ ] **Step 2: Confirm HARD-GATE present in orchestrator**

```bash
grep -c "HARD-GATE" skills/superpowers/SKILL.md
```

Expected: `2` (opening and closing tag).

- [ ] **Step 3: Confirm every pipeline skill has a stop**

```bash
grep -rln "Stop\.\|Do not proceed\|Do not invoke\|Do not execute\|Wait for explicit\|Terminal stop\|Handoff stop\|GATE [0-9]" \
  skills/superpowers/SKILL.md \
  skills/brainstorming/SKILL.md \
  skills/context-awareness/SKILL.md \
  skills/planning/SKILL.md \
  skills/verification/SKILL.md \
  skills/output-review/SKILL.md \
  skills/sensitivity-guard/SKILL.md \
  skills/delivery/SKILL.md
```

Expected: all 8 filenames printed.

- [ ] **Step 4: Final commit if any loose changes**

```bash
git status
# If clean: done.
# If dirty: git add -A && git commit -m "fix: integration cleanup"
```
