---
name: Nested Button Invalid HTML
description: button inside button is invalid HTML -- browser ejects inner element from DOM, breaking data-attribute targeting
type: feedback
related: [feedback_data_field_removal_crash.md, project_shop_social_edit_mode.md]
---

Never put a `<button>` inside a `<button>`. It is invalid HTML — the browser ejects the inner element from the DOM during parsing, placing it outside its intended parent. Any `data-*` attribute targeting (e.g. `data-remove="${t.id}"`) will then match the wrong element.

**Why:** Discovered in shop-social ?edit mode — X remove buttons inside tile buttons were removing the wrong tiles because the inner buttons were being ejected and reordered in the DOM.

**How to apply:** When adding an interactive element (remove, edit, share button) inside a clickable tile/card `<button>`, use `<span role="button" tabindex="0">` for the inner element instead. Wire click + keydown (Enter/Space) via JS.
