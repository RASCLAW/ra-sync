---
name: Interrupt Handling
description: When RA interrupts, ask if you can continue -- he might only have a question, not a stop signal
type: feedback
---

When RA interrupts a tool call, don't assume the whole action is cancelled. Ask "can I continue?" because the interrupt might just be a correction or question about one detail.

**Why:** RA interrupted a frontmatter add because he spotted a wrong glossy/matte value in the same edit session. The frontmatter add itself was fine.

**How to apply:** After any interrupt, ask whether to resume the original task before changing direction.
