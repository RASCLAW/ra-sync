---
name: Version-Bump Bank Files Before Mutation
description: Before resaving an image bank file, rename the current version to -v2 (or higher) so the previous pick list survives. Avoids the silent overwrite that lost 16 picks session 126.
type: feedback
related: [project_image_bank_april.md, feedback_content_storage_rule.md]
originSessionId: 2d508059-363a-4c95-9edb-672b753e5f69
---
Always version-bump image bank files before a mutating save. The Write tool overwrites in place — no automatic backup. If you trim a bank and save over the file, the removed picks are gone forever (not in git, not in VSCode history, not in shadow copies).

**Why:** Session 126 saved 60 FB stories picks to `contents/assets/chatbot-image-bank-2026-04.json`, then later trimmed to 44 for chatbot. The trim overwrote the file. 16 picks permanently lost. RA had to re-pick all 74 fresh from the gallery for the stories pool. Cost real time + user trust.

**How to apply:**
- Before calling Write on an existing bank file, rename it first: `mv bank.json bank-v2.json` (or `-v3` etc if v2 exists)
- Or copy to a dated backup: `cp bank.json bank-2026-04-16-1330.json`
- Only safe to overwrite if the file has been committed to git first (then `git restore` is recovery)
- Applies to any curated list in `contents/assets/` — image banks, content schedules, etc.
- Does NOT apply to regenerable artifacts (`.tmp/` configs, prompt txt files, metadata.json derived from scan)

**Mechanical rule:** if a file represents curation work that took >5 min of human input, treat as precious. Version-bump before save.
