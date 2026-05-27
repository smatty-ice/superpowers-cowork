---
name: delivery
description: "[INTERNAL ONLY] Confirm destination and format, run delivery safeguards, execute delivery, and summarize the result."
---

# Delivery

1. Ask where the output should go. Options: save locally / email draft / Drive upload / scheduled task / conversation artifact / discard.
2. If the brief specifies a destination, propose it — **confirm before executing**. Example: "I'll save this as a Word doc to your Documents folder. Go ahead?"
3. **For all irreversible actions (sending email, uploading to Drive, scheduling a task): require explicit confirmation every time, even if the brief specified this destination. Do not execute until the human says yes.**
4. If format conversion is needed, invoke the `format-selection` skill.
5. Execute delivery.
6. Provide delivery summary (see `references/delivery-guide.md` for template).
7. Check: should this recur? If yes, mention the `recurring-work` skill.

For the full decision tree and delivery summary template, see `references/delivery-guide.md`.
