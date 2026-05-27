# Plugin Audit & Improvement — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Audit and improve all skills in the superpowers-cowork plugin so they are structurally correct, correctly chained, and pass the token/enforcement test.

**Architecture:** 6 parallel agents (Tasks 1–6) each own a domain group of skills. A coordinator agent (Task 7) does a final consistency pass. Tasks 1–6 can run simultaneously.

**Tech Stack:** Markdown (SKILL.md files), YAML frontmatter, no code dependencies.

---

## Design Principle (every agent must internalize)

**Minimum tokens, maximum enforcement.**

- `SKILL.md` = compact enforcement contract. Numbered steps, hard stops, "do not proceed until…" gates. No prose that belongs in `references/`.
- `references/` = detailed how-to. Checklists, rubrics, examples. Loaded only when needed.

**The test for every file you touch:**
1. Can you cut any sentence without losing enforcement? Cut it.
2. Does every decision point have an explicit hard stop? Add one if not.
3. Is prose that belongs in `references/` still in the SKILL.md body? Move it.

**Why this matters:** v0.1 burned tokens with verbose skills. v0.2 compacted them. v0.3 added hard stops because compact skills were getting skipped. The current state should be lean + un-skippable.

---

## Audit Checklist (run on every SKILL.md)

| # | Check | Pass condition |
|---|-------|---------------|
| 1 | Step enforcement | Steps numbered or bulleted, none written as optional suggestions |
| 2 | Hard stops | At least one explicit "stop and wait" gate at each decision point |
| 3 | Token efficiency | No sentence removable without losing enforcement |
| 4 | References offloaded | Detail/examples live in `references/`, not SKILL.md body |
| 5 | Frontmatter `name:` | Exactly matches directory name |
| 6 | Frontmatter `description:` | ≤ 25 words; specific enough to trigger correctly |
| 7 | Cross-references | Every named skill/file actually exists in `skills/<name>/` |

---

## Source Material

- Spec: `docs/specs/2026-05-26-superpowers-cowork-design.md`
- Audit design: `docs/superpowers/specs/2026-05-26-agent-audit-design.md`
- v0.1 reference content: `/tmp/cowork-v01/superpowers-cowork/` (pre-compression, good raw content)
- v0.3 reference content: `/tmp/cowork-v03/superpowers-cowork/` (post-compression structure)

---

## File Map

**Create (4 new standalone skills):**
- `skills/connector-orchestration/SKILL.md`
- `skills/artifact-management/SKILL.md`
- `skills/error-handling/SKILL.md`
- `skills/sensitivity-guard/SKILL.md`

**Modify:**
- `skills/methodology-core/SKILL.md` — update cross-cutting references from inline to external skills
- All 7 command-entry-point skills: `superpowers`, `plan`, `research`, `review`, `deliver`, `brief`, `draft`
- All 12 implementation skills: `brainstorming`, `planning`, `parallel-execution`, `verification`, `output-review`, `delivery`, `format-selection`, `recurring-work`, `research-methodology`, `communication-drafting`, `draft-iteration`, `context-awareness`, `cross-source-synthesis`, `data-transformation`
- All reference files under `skills/*/references/`

---

## Task 1 — Agent A: Methodology Core + Promote 4 Cross-Cutting Skills

**Skills owned:**
- `skills/methodology-core/SKILL.md` + its 4 reference files
- Create: `skills/connector-orchestration/SKILL.md`
- Create: `skills/artifact-management/SKILL.md`
- Create: `skills/error-handling/SKILL.md`
- Create: `skills/sensitivity-guard/SKILL.md`

**Background:** The 4 cross-cutting skills currently exist only as reference docs under `methodology-core/references/`. Command pipelines reference them by name (e.g. `skills/superpowers/SKILL.md` says "connector-orchestration" is always active) but they're not invocable as standalone skills. This breaks the pipeline.

---

- [ ] **Step 1.1: Create `skills/connector-orchestration/SKILL.md`**

```markdown
---
name: connector-orchestration
description: Pick the right tool at each step. MCP first, then browser, then desktop, then ask. Degrade transparently. Cross-cutting — active throughout any workflow.
---

# Connector Orchestration

At each workflow step that needs an external tool:

1. **Check for MCP connector.** If one exists for this task, use it.
2. **Fall back to browser automation.** If MCP fails or isn't connected.
3. **Fall back to desktop control.** For native macOS apps with no web interface.
4. **Ask the human.** If all automated paths fail: "I need [X] but don't have access. Can you paste the relevant information?"

**When falling back:** Always tell the human. "Gmail MCP wasn't responding — using browser automation instead." Never silently skip a step because a tool isn't available.

**When chaining across tools** (e.g. email attachment → spreadsheet): track data lineage — note where each piece of data came from. Verification needs this.

For the full tool table: `references/mcp-catalog.md`.
```

- [ ] **Step 1.2: Create `skills/connector-orchestration/references/mcp-catalog.md`**

Copy from `skills/methodology-core/references/mcp-catalog.md`. No changes needed — content is complete.

```bash
cp skills/methodology-core/references/mcp-catalog.md skills/connector-orchestration/references/mcp-catalog.md
```

Verify the file exists:
```bash
head -3 skills/connector-orchestration/references/mcp-catalog.md
```
Expected: `# MCP Tool Catalog`

- [ ] **Step 1.3: Create `skills/artifact-management/SKILL.md`**

```markdown
---
name: artifact-management
description: Track where files live during workflows. Never overwrite originals. Version drafts, keep intermediates, clean up after delivery. Cross-cutting — active throughout any workflow.
---

# Artifact Management

1. **Create a working directory** per workflow: `superpowers-[topic]-[date]/`
2. **Never overwrite originals.** If the human shared a file, copy it — never modify in place.
3. **Version, don't replace.** Use `draft-v1.md`, `draft-v2.md` — not one file that gets overwritten.
4. **Keep intermediates accessible.** Verification needs to trace claims back to source files.
5. **After delivery:** "Want me to clean up the working files, or keep them for reference?"
```

- [ ] **Step 1.4: Create `skills/error-handling/SKILL.md`**

```markdown
---
name: error-handling
description: Recover from tool failures. Try primary, try fallback, ask the human. Max 2 retries. Never silently skip a failed step. Cross-cutting — active throughout any workflow.
---

# Error Handling

When a tool fails:
1. Retry once.
2. Try the fallback (browser automation if primary was MCP).
3. If fallback fails, ask the human with options: "(A) retry, (B) skip this step and work from what you tell me, (C) try browser automation."

**Never silently swallow failures.** Even when you recover, tell the human: "Gmail MCP failed — used browser automation instead."

**Max 2 retries** before escalating. Retrying forever stalls the workflow.
```

- [ ] **Step 1.5: Create `skills/sensitivity-guard/SKILL.md`**

```markdown
---
name: sensitivity-guard
description: Checkpoint before combining or sharing data across sources. Flag PII and context collapse. Always ask the human — never decide what's sensitive autonomously. Active during Verify and Deliver phases.
---

# Sensitivity Guard

Before delivering output that combines data from multiple sources, or before any action that sends data externally:

1. Name the sources: "This output includes data from [email / calendar / Drive / web]."
2. Name the destination: "It's going to [person / folder / external service]."
3. Ask: "Does that look appropriate?"
4. **Wait for explicit confirmation before proceeding.**

**Flag these specifically:**
- PII in the output (names, addresses, account numbers)
- Internal data (calendar entries, personal emails) going to an external recipient
- Data from one source appearing in a context where it doesn't belong

**The rule:** The human decides what's sensitive. Always.
```

- [ ] **Step 1.6: Update `skills/methodology-core/SKILL.md` — cross-cutting references**

The current file references cross-cutting skills via inline text pointing to `references/` files. Now that they're standalone skills, update the "Cross-Cutting Behaviors" section to reference them as external skills:

Find this section in `skills/methodology-core/SKILL.md`:
```
## Cross-Cutting Behaviors

Four behaviors are always active during a workflow. They support every phase but don't have their own. Read the references for detailed guidance when needed:

- **Connector orchestration** (`references/connector-orchestration.md`): ...
- **Error handling** (`references/error-handling.md`): ...
- **Artifact management** (`references/artifact-management.md`): ...
- **Sensitivity guard** (`references/sensitivity-guard.md`): ...
```

Replace with:
```
## Cross-Cutting Skills

Four skills are always active. They don't have their own phase — they support every phase:

- **connector-orchestration** — MCP first, then browser, then desktop, then ask
- **error-handling** — try primary, try fallback, ask the human; max 2 retries
- **artifact-management** — don't overwrite originals; version drafts; track intermediates
- **sensitivity-guard** — before any cross-source delivery, checkpoint with the human
```

- [ ] **Step 1.7: Audit `methodology-core/SKILL.md` against the checklist**

Run the 7-item audit checklist from the plan header. Confirm:
- `description:` frontmatter ≤ 25 words (current: 29 words — trim it)
- Hard stops present: ✅ (approval gates section covers this)
- Token efficiency: The brief template and verification-driven-work section are both useful enough to keep. The cross-cutting section is already trimmed in step 1.6.

Trim the `description:` to: `"Behavioral contract for all superpowers workflows. Defines the Clarify → Plan → Execute → Verify → Deliver pipeline and checkpoint protocol. Do not invoke directly."`

- [ ] **Step 1.8: Verify all 4 new skills have correct structure**

```bash
for skill in connector-orchestration artifact-management error-handling sensitivity-guard; do
  echo "=== $skill ==="
  head -4 skills/$skill/SKILL.md
  echo ""
done
```

Expected: Each shows `---`, `name: <skill-name>`, `description:`, `---`.

---

## Task 2 — Agent B: Command Entry-Point Skills

**Skills owned:**
- `skills/superpowers/SKILL.md`
- `skills/plan/SKILL.md`
- `skills/research/SKILL.md`
- `skills/review/SKILL.md`
- `skills/deliver/SKILL.md`
- `skills/brief/SKILL.md`
- `skills/draft/SKILL.md`

**Background:** These are the user-facing entry points. Each invokes a pipeline of implementation skills. Key problems: (1) pipelines may reference skill names that don't exist as standalone skills; (2) command-skills and implementation-skills have confusingly similar names (`plan` vs `planning`, `deliver` vs `delivery`); (3) frontmatter needs an `argument-hint:` field.

**Naming convention to enforce:**
- Command-skills (`plan`, `deliver`, etc.): user invokes these. Short names. They set up context and chain to implementation skills.
- Implementation skills (`planning`, `delivery`, etc.): invoked internally by command-skills. Never invoked directly by the user.
- Each command-skill's SKILL.md must say which implementation skills it chains to — not just list the pipeline abstractly.

---

- [ ] **Step 2.1: Audit each command-skill pipeline for broken references**

For each command-skill, read the pipeline and check that every named skill has a corresponding `skills/<name>/` directory:

```bash
for cmd in superpowers plan research review deliver brief draft; do
  echo "=== $cmd pipeline ==="
  grep -A5 "Pipeline" skills/$cmd/SKILL.md | head -10
  echo ""
done
```

Expected broken references (verify):
- `sensitivity-guard` — now fixed by Task 1 ✅
- `artifact-management` — now fixed by Task 1 ✅
- `connector-orchestration` — now fixed by Task 1 ✅

- [ ] **Step 2.2: Add `argument-hint` frontmatter to all 7 command-skills**

Each command-skill frontmatter should have `argument-hint`. Add to each:

| Skill | Add `argument-hint:` |
|-------|---------------------|
| `superpowers` | `"<describe the task>"` |
| `plan` | `"<describe the task>"` |
| `research` | `"<research question or topic>"` |
| `review` | `"<describe what to review>"` |
| `deliver` | `"<what to deliver and where>"` |
| `brief` | `"<topic or question>"` |
| `draft` | `"<what to draft>"` |

- [ ] **Step 2.3: Add naming clarity comment to each command-skill**

Each command-skill SKILL.md should have a line that makes the command-vs-implementation distinction explicit. Add to each command-skill after the pipeline block:

```markdown
*This is the entry-point skill. It sets context and chains to implementation skills. Users invoke this; implementation skills run automatically.*
```

- [ ] **Step 2.4: Audit all 7 command-skills against the checklist**

Run the 7-item checklist on each. Key things to verify for command-skills:
- Hard stop: does it say "wait for the human before proceeding to execution"? (Critical — this is why steps got skipped in v0.3.)
- Description ≤ 25 words
- All named skills in pipeline exist as `skills/<name>/` directories

Fix any issues found.

- [ ] **Step 2.5: Verify `skills/superpowers/SKILL.md` has the strongest hard stops**

`/superpowers` is the full pipeline — it has the most phase transitions to enforce. Verify it explicitly says "stop and wait" at:
- After Clarify (brief approval)
- After Plan (plan approval)
- Before Deliver (sensitivity confirmation)

If any are missing, add them. Example language:
```markdown
**After step 2 (Clarify):** Present the brief. Ask: "Does this capture what you need?" Do not proceed to step 3 until you receive explicit approval.
```

---

## Task 3 — Agent C: Workflow Engine Skills

**Skills owned:**
- `skills/brainstorming/SKILL.md` + `references/brainstorming-guide.md`
- `skills/planning/SKILL.md` + `references/planning-guide.md`
- `skills/parallel-execution/SKILL.md` + `references/completeness-reviewer.md`, `dispatch-guide.md`, `quality-reviewer.md`
- `skills/verification/SKILL.md` + `references/verification-checklist.md`

**Background:** These are the core workflow engine skills. `brainstorming` and `planning` are already strong (v0.3 tightened them well). Main work is token audit, verifying hard stops are explicit, and ensuring references hold detail that belongs there.

---

- [ ] **Step 3.1: Audit `skills/brainstorming/SKILL.md`**

Current state is good. Check:
- Description frontmatter: 19 words ✅
- Hard stop present ✅ ("Stop. Do nothing else.")
- Token efficiency: scan for any prose that could be cut without losing enforcement
- Reference: `references/brainstorming-guide.md` exists ✅

If the SKILL.md body has content that belongs in the guide (full process, anti-patterns), confirm it's in `references/brainstorming-guide.md`. If the guide is thin, strengthen it rather than bloating the SKILL.md.

- [ ] **Step 3.2: Audit `skills/planning/SKILL.md`**

Current state is good. Check:
- Description ≤ 25 words (current: 23 ✅)
- Hard stop present ✅ ("Do not begin step 1 until you have explicit approval")
- `references/planning-guide.md` holds the plan template and effort table — verify it does

Read `references/planning-guide.md` and confirm it contains:
- The plan template (full markdown structure)
- Task granularity guidance (what constitutes a single step)
- Effort estimation table (light/medium/heavy)

If any of these are missing, add them to the guide.

- [ ] **Step 3.3: Audit `skills/parallel-execution/SKILL.md`**

Read the current file and check:
- Does it say "use the Agent tool" to dispatch subagents?
- Does it describe the two-stage review (completeness → quality)?
- Does it describe sequential fallback when subagents aren't available?
- Hard stop: does it say to wait for all subagents to complete before merging?

Key gap to verify: the skill should include the sequential fallback explicitly, since Cowork may not always have subagent support. If the fallback is missing or vague, add:

```markdown
**Sequential fallback:** If subagents are not available, execute steps sequentially in the current session. After each step, verify the output against the "done when" criterion before proceeding. Group into batches of 2-3 with a human checkpoint after each batch.
```

Check `references/dispatch-guide.md` — it should hold the full dispatch prompt template. If it's thin or missing content, strengthen it.

- [ ] **Step 3.4: Audit `skills/verification/SKILL.md`**

Read current file. Verify:
- Loads success criteria from the brief before doing anything else (hard stop if brief is missing)
- Runs type-specific checks (factual / document / data / communication / presentation / multi-source)
- Reports by severity: critical (blocks delivery) / moderate / minor
- Hard stop at critical issues: "Do not proceed to delivery until critical issues are resolved"

Check `references/verification-checklist.md` — it should have the full type-specific checklist. If the SKILL.md body contains the checklist inline, move it to the reference file.

---

## Task 4 — Agent D: Review & Delivery Skills

**Skills owned:**
- `skills/output-review/SKILL.md` + `references/review-dimensions.md`
- `skills/delivery/SKILL.md` + `references/delivery-guide.md`
- `skills/format-selection/SKILL.md`
- `skills/recurring-work/SKILL.md` + `references/setup-guide.md`

---

- [ ] **Step 4.1: Audit `skills/output-review/SKILL.md`**

Read current file. Verify:
- Loads the brief before starting review
- Reviews 5 dimensions: completeness, accuracy, structure, tone, format
- Reports by severity: critical / moderate / minor
- Independent review mode (subagent gets brief + output only, not process context) — is this present?
- Hard stop at critical findings: "Do not proceed to delivery until critical issues are resolved"

Check `references/review-dimensions.md` — it should hold the full dimension checklist. If it's in the SKILL.md body, move it.

- [ ] **Step 4.2: Audit `skills/delivery/SKILL.md`**

Current state is good — includes "confirm before executing" language for irreversible actions. Check:
- Decision tree is complete: save / email draft / Drive upload / scheduled task / conversation artifact / discard
- "Confirm before executing" covers all irreversible actions
- Mentions recurring-work handoff
- `references/delivery-guide.md` holds the full decision tree and delivery summary template

If `delivery-guide.md` is thin, add the full decision tree and summary template there.

- [ ] **Step 4.3: Audit `skills/format-selection/SKILL.md`**

Read current file. Verify:
- Decision table covers: report/memo/letter → docx, presentation → pptx, data/analysis → xlsx, fixed-layout → pdf, quick sharing → markdown, email → Gmail MCP draft
- Handles ambiguous cases (report with numbers → ask whether data-heavy or narrative-heavy)
- Token efficiency: if the decision table is in the SKILL.md body, that's fine (it's a reference table, not prose)

- [ ] **Step 4.4: Audit `skills/recurring-work/SKILL.md`**

Read current file. Verify:
- Describes when to suggest recurring (weekly status, monthly analysis, daily digest, etc.)
- Captures: brief, plan, tools, schedule
- Uses `create_scheduled_task` integration
- Has explicit "don't suggest for one-off tasks" anti-pattern

Check `references/setup-guide.md` — it should hold the full recurring task setup process.

---

## Task 5 — Agent E: Research & Communications Skills

**Skills owned:**
- `skills/research-methodology/SKILL.md` + `references/research-process.md`, `source-evaluation.md`
- `skills/communication-drafting/SKILL.md` + `references/drafting-guide.md`
- `skills/draft-iteration/SKILL.md` + `references/iteration-guide.md`
- `skills/context-awareness/SKILL.md`

---

- [ ] **Step 5.1: Audit `skills/research-methodology/SKILL.md`**

Read current file. Verify the 6-step process is present and enforced:
1. Define questions (specific, not vague)
2. Search — minimum 3 query formulations per question
3. Evaluate sources (authority, recency, bias, corroboration)
4. Extract findings with attribution
5. Synthesize — distinguish facts vs. analysis vs. opinion
6. Cite — every claim traceable to a source

Hard stop: does the skill say "do not proceed to synthesis until you have at least 3 independent sources per key claim"? If not, add it.

Check `references/source-evaluation.md` — should hold the source evaluation rubric (authority/recency/bias/corroboration scoring). If `references/research-process.md` duplicates the SKILL.md body, consolidate.

- [ ] **Step 5.2: Audit `skills/communication-drafting/SKILL.md`**

Read current file. Verify:
- Audience adaptation process (who, relationship, what they need, medium)
- Tone mapping table: professional email / casual email / formal document / quick message
- Multi-audience drafting: draft versions separately, don't try to make one message work for everyone
- Key principles: front-load the key point, match the medium, respect the reader's time, specific asks

Token efficiency: if the tone table is inline and detailed, move to `references/drafting-guide.md`.

- [ ] **Step 5.3: Audit `skills/draft-iteration/SKILL.md`**

Read current file. Verify:
- Version tracking (draft-v1, draft-v2, etc.) — mentions artifact-management
- Feedback integration: parse into specific changes, map to sections, present what changed
- Convergence check: iterating on typos = healthy, fundamental direction shifts = flag and revisit the brief

Hard stop: "If feedback suggests the brief itself was wrong, stop iteration and revisit the brief before continuing."

- [ ] **Step 5.4: Audit `skills/context-awareness/SKILL.md`**

Read current file. Verify:
- Checks 3 categories: MCP connectors (email/calendar/Drive), always-available tools (web/browser/desktop), human-provided data
- Output: 2-3 sentence environment summary (not a detailed audit)
- Identifies gaps and adjusts plan: "For this task, I'll need [X] — you'll have to paste it since I don't have Drive access"
- Runs at start of every workflow and when a tool unexpectedly fails

Token efficiency: this skill is likely already short. Verify it stays that way.

---

## Task 6 — Agent F: Synthesis Skills + All Reference Files

**Skills owned:**
- `skills/cross-source-synthesis/SKILL.md` + `references/synthesis-process.md`
- `skills/data-transformation/SKILL.md` + `references/transformation-patterns.md`

**Plus a cross-cutting reference audit:** Read every `references/` file across the plugin and verify they hold the right content for their parent skill.

---

- [ ] **Step 6.1: Audit `skills/cross-source-synthesis/SKILL.md`**

Read current file. Verify the 5-step process:
1. Survey available sources (what's connected, what time range)
2. Pull in parallel — specific query per source
3. Reconcile conflicts — present both sides, ask human which is correct
4. Synthesize with source attribution on every claim
5. Highlight gaps — "I checked X, Y, Z but couldn't find [info]"

Hard stop: "When sources conflict, do not silently pick one. Present both and ask."

Check `references/synthesis-process.md` — holds detailed pull-in-parallel instructions.

- [ ] **Step 6.2: Audit `skills/data-transformation/SKILL.md`**

Read current file. Verify the 3 common patterns are present:
1. Email attachment → spreadsheet (email MCP → pdf skill → xlsx skill)
2. Multiple sources → combined analysis (normalize → combine → validate → analyze)
3. Research → presentation (findings → key points → visualizations → pptx skill)

Verify data lineage tracking is explicit: "At every step, note what came from where."

Verify validation steps: "After each transformation, check: did we lose data? Did formats shift?"

Check `references/transformation-patterns.md` — should hold detailed pattern guides with exact tool sequences.

- [ ] **Step 6.3: Cross-cutting reference file audit**

For each `references/` file across the plugin, verify it holds the right content for its parent skill. Run a quick read of each and flag:
- Empty or near-empty files
- Files that duplicate SKILL.md body content (should be moved to the reference)
- Files that are comprehensive and correctly scoped ✅

Reference files to check:
```
skills/brainstorming/references/brainstorming-guide.md
skills/communication-drafting/references/drafting-guide.md
skills/cross-source-synthesis/references/synthesis-process.md
skills/data-transformation/references/transformation-patterns.md
skills/delivery/references/delivery-guide.md
skills/draft-iteration/references/iteration-guide.md
skills/methodology-core/references/mcp-catalog.md  (also in connector-orchestration now)
skills/output-review/references/review-dimensions.md
skills/parallel-execution/references/completeness-reviewer.md
skills/parallel-execution/references/dispatch-guide.md
skills/parallel-execution/references/quality-reviewer.md
skills/planning/references/planning-guide.md
skills/recurring-work/references/setup-guide.md
skills/research-methodology/references/research-process.md
skills/research-methodology/references/source-evaluation.md
skills/verification/references/verification-checklist.md
```

For any file that fails, either add missing content or note it as a gap for the coordinator.

---

## Task 7 — Agent G: Coordinator (Run AFTER Tasks 1–6 complete)

**Scope:** Read-only consistency check across the full plugin. Fix any issues found.

---

- [ ] **Step 7.1: Verify all 4 new standalone skills exist**

```bash
for skill in connector-orchestration artifact-management error-handling sensitivity-guard; do
  test -f skills/$skill/SKILL.md && echo "$skill ✅" || echo "$skill ❌ MISSING"
done
```

Expected: all 4 ✅

- [ ] **Step 7.2: Verify frontmatter `name:` matches directory name for every skill**

```bash
for f in skills/*/SKILL.md; do
  dir=$(basename $(dirname $f))
  name=$(grep '^name:' $f | head -1 | sed 's/name: //' | tr -d '"')
  if [ "$dir" = "$name" ]; then
    echo "$dir ✅"
  else
    echo "$dir ❌ name mismatch: got '$name'"
  fi
done
```

Expected: all ✅. Fix any mismatches.

- [ ] **Step 7.3: Verify all cross-references resolve**

Check that every skill named in a pipeline or description exists:

```bash
# Extract all skill names referenced in pipelines across command-skills
grep -h "→" skills/superpowers/SKILL.md skills/plan/SKILL.md skills/research/SKILL.md \
  skills/review/SKILL.md skills/deliver/SKILL.md skills/brief/SKILL.md skills/draft/SKILL.md \
  | tr '→' '\n' | sed 's/[^a-z-]//g' | sort -u | while read skill; do
    [ -z "$skill" ] && continue
    test -d "skills/$skill" && echo "$skill ✅" || echo "$skill ❌ NOT FOUND"
done
```

Fix any broken references (wrong name or missing directory).

- [ ] **Step 7.4: Check for duplicate skills (same job, different name)**

Scan for skills that do the same thing. Known naming pairs that need distinction:
- `plan` (command entry) vs `planning` (implementation) — both exist by design ✅ (distinct roles)
- `deliver` (command entry) vs `delivery` (implementation) — both exist by design ✅
- `research` (command entry) vs `research-methodology` (implementation) — both exist by design ✅

Verify each pair has clearly different `description:` frontmatter that makes the distinction obvious.

- [ ] **Step 7.5: Validate `plugin.json`**

```bash
python3 -c "import json; d=json.load(open('.claude-plugin/plugin.json')); print('✅ valid JSON:', d['name'], 'v' + d['version'])"
```

Expected: `✅ valid JSON: superpowers-cowork v0.1.0`

- [ ] **Step 7.6: Run the 7-item audit checklist on any skill not covered by Tasks 1–6**

Verify no skill was missed. List all SKILL.md files:

```bash
find skills -name "SKILL.md" | sort
```

Cross-reference against the tasks above. Any file not covered by an agent, audit now.

- [ ] **Step 7.7: Final count — verify all expected skills exist**

Expected standalone skills (19 from spec + 7 command-entry-points = 26 total SKILL.md files):

```bash
find skills -name "SKILL.md" | wc -l
```

Expected: 26 (or explain any discrepancy).

List them:
```bash
find skills -name "SKILL.md" | sort
```

---

## Self-Review

**Spec coverage:** All 19 implementation skills covered across Tasks 1, 3, 4, 5, 6. All 7 command-skills covered in Task 2. Coordinator (Task 7) covers full-plugin consistency. All 4 structural fixes are in Task 1. ✅

**Placeholder scan:** No TBDs or TODOs. All new SKILL.md content written inline in Task 1 steps. ✅

**Type consistency:** Skill names used in pipeline checks (Task 7.3) match the names created in Task 1 and existing skills. ✅

**One gap noted:** The `methodology-core/references/` files for the 4 cross-cutting skills (`connector-orchestration.md`, `artifact-management.md`, `error-handling.md`, `sensitivity-guard.md`) are now redundant once Task 1 creates the standalone skills. Task 7.6 will catch any reference to the old paths. Consider whether to delete the old reference files or keep them for backward compatibility — the coordinator should decide.
