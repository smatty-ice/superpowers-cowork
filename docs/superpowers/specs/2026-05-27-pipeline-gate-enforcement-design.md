# Pipeline Gate Enforcement — Design Spec

**Date:** 2026-05-27  
**Status:** Approved  
**Scope:** superpowers-cowork plugin (v0.4.0+)

---

## Problem

The `/superpowers` pipeline has 8 mandatory steps with explicit approval gates at steps 1 (brief), 3 (plan), 5 (verification critical issues), 7 (sensitivity), and 8 (delivery). A real invocation skipped all of them.

**Root causes identified (three, distinct):**

### RC-1 — Orchestrator lacks semantic barriers
`skills/superpowers/SKILL.md` uses `"You MUST invoke..."` language but no structural barrier prevents a model from rationalizing past it when the task appears well-specified. The official superpowers plugin uses `<HARD-GATE>` blocks for exactly this purpose; this plugin does not.

### RC-2 — Fast-track clause in brainstorming created a skip pathway
`skills/brainstorming/SKILL.md` line 10:
> "If the request already specifies audience, format, and success criteria, fast-track to writing the brief — skip the question-asking, not the checkpoint."

A detailed task description triggered "fast-track" and the model extended "skip the questions" to "skip the checkpoint entirely." The clause was the rationalization door.

### RC-3 — Brainstorming guide reinforced the loophole
`skills/brainstorming/references/brainstorming-guide.md` includes "Fast-track when clear" as a Key Principle. This contradicts the anti-pattern section in the same file and reinforces RC-2.

**Note:** The verification/output-review/sensitivity-guard/delivery skills already contain hard stops internally. The model never reached them because the orchestrator pipeline was skipped wholesale.

---

## Solution: Approach A — HARD-GATE injection + fast-track removal

Three independent changes, parallelizable across three agents.

### Change 1 — Harden `superpowers/SKILL.md`

Add a `<HARD-GATE>` block at the top of the file:

```
<HARD-GATE>
Do NOT skip, reorder, merge, or abbreviate pipeline steps. Every gate is mandatory
regardless of task complexity, how much context was provided upfront, or how
well-specified the request appears. Well-specified input is raw material for the
brief — it is not approval of the brief.
</HARD-GATE>
```

Add a rationalization table (same pattern as `using-superpowers`):

| Thought | Reality |
|---------|---------|
| "The task is fully specified — I can skip the brief" | A detailed description is input *to* the brief, not approval *of* it. |
| "I already know what the plan will be" | Present it and wait. The gate exists to catch assumptions. |
| "The human seems ready to go" | Wait for an explicit "yes" on the brief and plan. Eagerness is not approval. |
| "Verification/review would just confirm it's fine" | Run them anyway. If they confirm fine, cost is one minute. If they don't, you prevented a bad delivery. |

Add explicit inline STOP markers between each step in the pipeline (e.g., between step 1 and 2: `**GATE: Do not proceed to step 2 until the human has explicitly approved the brief.**`).

### Change 2 — Clean up `brainstorming/SKILL.md` and its guide

In `skills/brainstorming/SKILL.md`:
- Remove the fast-track clause (line 10) entirely. The guide's anti-pattern section already explains this correctly; the SKILL.md shortcut contradicts it.
- Add to the Hard Stop section: "Rich context, long task descriptions, and prior conversation are input *to* the brief — not approval *of* it. Do not proceed without an explicit 'yes.'"

In `skills/brainstorming/references/brainstorming-guide.md`:
- Remove the "Fast-track when clear" bullet from Key Principles.
- The Anti-Pattern: Treating Form Responses as Approval section already handles this correctly and can stand alone.

### Change 3 — Audit remaining pipeline skills for unconditional stops

Audit `context-awareness`, `verification`, `output-review`, `sensitivity-guard`, `delivery` to confirm each has a clear unconditional stop at the handoff point. Specifically:

- **context-awareness**: Step 6 currently says "proceed" on minor gaps — add a terminal: "Pass the environment summary to planning. Do not invoke the planning skill until this report is complete and any critical gaps are resolved or explicitly waived."
- **verification, output-review, sensitivity-guard, delivery**: Confirm existing stops are unconditional and visible (not buried in conditional logic). Strengthen phrasing where needed.

---

## Files Changed

| File | Change | Agent |
|------|--------|-------|
| `skills/superpowers/SKILL.md` | Add HARD-GATE block, rationalization table, inline STOP markers | Agent 1 |
| `skills/brainstorming/SKILL.md` | Remove fast-track clause, strengthen Hard Stop | Agent 2 |
| `skills/brainstorming/references/brainstorming-guide.md` | Remove "Fast-track when clear" bullet | Agent 2 |
| `skills/context-awareness/SKILL.md` | Add unconditional terminal stop | Agent 3 |
| `skills/verification/SKILL.md` | Audit and strengthen stop visibility | Agent 3 |
| `skills/output-review/SKILL.md` | Audit and strengthen stop visibility | Agent 3 |
| `skills/sensitivity-guard/SKILL.md` | Audit and strengthen stop visibility | Agent 3 |
| `skills/delivery/SKILL.md` | Audit and strengthen stop visibility | Agent 3 |

---

## Success Criteria

1. A model invoking `/superpowers` with a fully-specified task description MUST pause after writing the brief and wait for explicit human approval before any file reads, searches, or actions.
2. A model invoking `/superpowers` MUST pause after presenting the plan and wait for explicit human approval before any execution.
3. No skill file in the pipeline contains language that could be read as permission to skip a checkpoint.
4. The rationalization table in `superpowers/SKILL.md` explicitly covers the "task is well-specified" and "I already know the plan" patterns.

---

## Out of Scope

- `visualize` elicitation module (separate feature request; not a bugfix)
- Changes to skills outside the pipeline (`draft`, `research`, `brief`, etc.)
- Changes to the official superpowers plugin (separate repo)
