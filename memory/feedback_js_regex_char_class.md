---
name: js-regex-char-class
description: "JS regex char classes treat `-` as a range operator. `[-:--]+` errors with 'Range out of order'. Place `-` first/last in the class or escape it. This killed the entire schedule_chat.js IIFE silently for ~30 min during the v2 build."
metadata: 
  node_type: memory
  type: feedback
  related: 
    - project_schedule_v2_shipped.md
  originSessionId: d328227d-c1ef-40e6-9cfe-488271df6193
---

Inside a JS character class, `-` between two characters is interpreted as a range. `[-:--]+` is parsed as `-`, `:` to `-` (invalid: `:` > `-`), then another `-`. The interpreter throws `SyntaxError: Invalid regular expression: Range out of order in character class`.

**Why:** I wanted to match the separator after `OPTION 1` (which could be `-`, `--`, or `:` in Claude's reply). The intent was "one or more of `-` or `:`". The correct write is `[-:]+` (or `[:-]+`) — put `-` at the start or end of the char class so it's literal, not a range delimiter.

**How to apply:**
- When building a JS regex with `-` inside `[ ]`, place `-` at the very start or very end of the class, or backslash-escape it (`\-`). This was a 30-second fix once spotted — the real cost was diagnostic time.
- The whole `schedule_chat.js` IIFE failed silently when this regex parsed at script load. Symptoms RA reported: AI Suggest tab thumbs not showing + Ask button dead. Both fail when the IIFE never assigns `window.__schedChat`.
- **Detection pattern:** parse the file via `node -e "new Function(require('fs').readFileSync('path/to/file.js','utf8'))"` — prints the exact line + error. Faster than guessing browser-side. Use this before committing any sizeable JS edit.
- Add a quick `node -e "new Function(...)" && echo PARSE OK` step to JS file edits, especially for IIFE-pattern modules where one syntax error kills everything.
