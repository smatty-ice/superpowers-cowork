---
name: artifact-management
description: "[INTERNAL ONLY] Track workflow files, version drafts, keep intermediates, and ask before cleanup. Cross-cutting skill."
---

# Artifact Management

**At the start of every workflow, before any file is created or modified:** Create a working directory: `superpowers-[topic]-[date]/`.
**Security Rule:** You must sanitize the `[topic]` variable before creating the directory. Strip out any slashes (`/`, `\`), `..`, or special characters to prevent path traversal. Keep only alphanumeric characters and hyphens. All files go into this sanitized directory. No exceptions.

Then throughout the workflow:
1. **Never overwrite originals.** If the human shared a file, copy it — never modify in place.
2. **Version, don't replace.** Save `draft-v1.md` before creating `draft-v2.md`. Increment on every substantive edit.
3. **Keep intermediates accessible.** Verification needs to trace claims back to source files.
4. **After the Deliver phase reports success:** "Want me to clean up the working files, or keep them for reference?" Wait for the answer. Do not delete any file before receiving an explicit response.
