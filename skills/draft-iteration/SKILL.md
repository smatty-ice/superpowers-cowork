---
name: draft-iteration
description: "[INTERNAL ONLY] Manage draft feedback loops, version revisions, show changes, and flag divergence."
---

# Draft Iteration

1. **Produce v1 as a discrete checkpoint.** Save as `draft-v1.md`. Present it to the human and explicitly request feedback. Do not continue past v1 without feedback or sign-off.
2. **Version every revision.** Each subsequent draft is `draft-v2.md`, `draft-v3.md`, etc. Never overwrite a previous version.
3. **Parse feedback into specific changes.** Before writing the next version, map each piece of feedback to a section or element. See `references/iteration-guide.md`.
4. **Revise and show what changed.** With every new version, explicitly state what changed: section names, word counts, additions, removals.
5. **Convergence check.** After each version, track whether feedback is getting smaller and more specific (converging) or shifting direction (diverging).
6. **Hard stop on divergence:** If the direction keeps shifting, do not continue iterating. Stop and ask: "The direction seems to be shifting. Should we update the brief first?" Do not proceed until the human answers.
7. **Hard stop on loop termination:** The loop ends only when the human explicitly signs off ("looks good," "ship it," "ready"). After each version, present it and ask "How does this look?" Do not advance to delivery until that sign-off arrives.

Feedback integration details and diff presentation: `references/iteration-guide.md`
