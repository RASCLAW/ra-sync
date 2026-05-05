---
name: Settings Self-Modification Auth
description: Editing settings.json (model, hooks, etc.) triggers a permission gate -- needs explicit RA authorization each session
type: feedback
related: [reference_autocompact_window.md]
originSessionId: 35152885-5b0b-4d59-98ce-4e9fe9f77f26
---
Edits to `~/.claude/settings.json` are blocked as self-modification unless RA explicitly authorizes them in that session.

**Why:** Claude Code treats changes to its own config as a privileged action. A general "yes lets do it" is not enough -- the gate requires a statement specifically authorizing the edit.

**How to apply:** When RA asks to change a setting, confirm he wants me to edit settings.json specifically before attempting. Phrase like "I'll edit settings.json line X to Y -- authorized?" if unclear.
