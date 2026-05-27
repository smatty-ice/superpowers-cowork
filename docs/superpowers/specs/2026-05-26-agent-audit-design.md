# Superpowers-Cowork — Agent Audit & Improvement Design

**Date:** 2026-05-26  
**Status:** Approved  
**Scope:** Audit and improve all skills in the superpowers-cowork plugin

---

## Background

Three versions of this plugin exist (`v0.1`, `v0.2`, `v0.3` in `~/`) plus the current working directory. Each version solved a real problem:

- **v0.1 → v0.2:** Skills were too verbose → huge token burn on every invocation. Compacted skills and moved detail to `references/`.
- **v0.2 → v0.3:** Compact skills got skipped by Cowork → needed harder enforcement gates.
- **Current WD:** Commands migrated from `commands/` directory to skills (correct Cowork format), but brought structural problems along.

---

## North Star

> *"The methodology, not the skills. Stop before acting, plan before executing, verify before declaring done, checkpoint with the human."*

---

## Design Principle

**Minimum tokens, maximum enforcement.** Every SKILL.md must be as short as possible while making its steps un-skippable.

- **`SKILL.md`** = compact enforcement contract. Numbered steps, hard stops, explicit "wait for human" gates. No prose.
- **`references/`** = detailed how-to. Checklists, rubrics, decision tables. Loaded only when needed.

**The test for every skill:**
1. Can you remove any sentence without losing enforcement? If yes, cut it.
2. Does every phase have an explicit hard stop? If not, add one.
3. Is prose that belongs in `references/` still in the SKILL.md body? Move it.

---

## Structural Problems to Fix

### 1. Four cross-cutting skills are broken
`connector-orchestration`, `artifact-management`, `error-handling`, `sensitivity-guard` are referenced by name in command pipelines but only exist as reference docs in `methodology-core/references/`. They cannot be invoked. 

**Fix:** Promote each to a standalone `skills/<name>/SKILL.md` with proper frontmatter. The content in `methodology-core/references/` becomes the source — expand into full enforcement contracts. Update `methodology-core` to reference them as external skills.

### 2. Naming collision between command-skills and implementation skills
- `skills/plan` (user-facing entry point) vs `skills/planning` (internal workflow skill)
- `skills/deliver` vs `skills/delivery`  
- `skills/research` vs `skills/research-methodology`

**Fix:** Make the distinction explicit in frontmatter and content. Command-skills are entry points — they set up context and invoke implementation skills. Implementation skills do the work. No confusion about which is which.

### 3. `commands/` directory is intentionally dropped
The zipped versions (v0.1–v0.3) had a `commands/` directory. The current WD migrated these to skills. This is correct for Cowork's format. Do not restore the `commands/` directory.

---

## Quality Audit Criteria (per skill)

| Check | Pass condition |
|-------|---------------|
| Step enforcement | All steps numbered, none written as suggestions |
| Hard stops | Explicit "do not proceed until…" gates at decision points |
| Token efficiency | No sentence removable without losing enforcement |
| References offloaded | Detail/examples in `references/`, not SKILL.md body |
| Frontmatter description | Specific enough to trigger correctly; short enough to not inflate context |
| Cross-references | Named skills exist and are correctly identified |
| Frontmatter name | Matches directory name exactly |

---

## Agent Team Structure

### Parallel agents (6)

| Agent | Skills owned | Primary fix |
|-------|-------------|-------------|
| **A — Core** | `methodology-core/` + its 4 reference files | Promote 4 cross-cutting skills to standalone; compress methodology-core |
| **B — Commands** | `superpowers`, `plan`, `research`, `review`, `deliver`, `brief`, `draft` | Naming clarity, pipeline accuracy, checkpoint gates |
| **C — Workflow Engine** | `brainstorming`, `planning`, `parallel-execution`, `verification` | Token audit + hard-stop enforcement |
| **D — Review & Delivery** | `output-review`, `delivery`, `format-selection`, `recurring-work` | Token audit + delivery decision tree fidelity |
| **E — Research & Comms** | `research-methodology`, `communication-drafting`, `draft-iteration`, `context-awareness` | Token audit + reference file offloading |
| **F — Synthesis** | `cross-source-synthesis`, `data-transformation` + all reference files | Token audit + data lineage accuracy |

### Coordinator pass (sequential, after all agents complete)

Final consistency check across the full plugin:
- All named skill references resolve to real `skills/<name>/` directories
- Frontmatter `name:` matches directory name for every skill
- All 4 promoted cross-cutting skills properly wired into pipelines
- `plugin.json` is valid JSON
- No two skills do the same job under different names (dedup check)

---

## Source Material for Agents

Each agent receives:
1. This design doc
2. The original brainstorm transcript (design intent)
3. The spec: `docs/specs/2026-05-26-superpowers-cowork-design.md`
4. v0.1 content at `/tmp/cowork-v01/` (reference for pre-compression content quality)
5. Their specific skill files to own

---

## Success Criteria

- [ ] All 4 cross-cutting skills are standalone invocable skills
- [ ] No naming ambiguity between command-skills and implementation skills
- [ ] Every SKILL.md passes the token/enforcement test
- [ ] All cross-references resolve correctly
- [ ] plugin.json validates
- [ ] Coordinator pass finds no consistency issues
