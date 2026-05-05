---
name: Content storage rule -- git for code, Drive for content
description: If code or website reads a file at runtime, commit it. Everything else (generated content, archives, supplier refs) is gitignored and backed up to Drive.
type: feedback
related: [feedback_google_api_client_broken.md, project_refactor_recovery_session99.md]
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---
**Rule:** Commit to git only what code or the website reads at runtime. Everything else -- generated content, archives, supplier reference material -- is gitignored and backed up to Google Drive as cold storage.

**Why:** Session 99, RA made this decision explicit when we were about to stage ~386MB of content. His pushback: *"isn't GitHub for codes and markdown files only. stock images for websites but as storage of contents that could snowball to 100GB is not good."* His instinct was correct -- git history bloats forever, binaries compound, GitHub has a 100MB/file hard limit and a recommended <5GB repo size. The final commit was ~50MB (code + contents/assets/ which skills actually read at runtime). The remaining ~236MB (contents/new, contents/ready, contents/failed, archives, references/supplier-images) was gitignored and backed up to Drive via tools/drive/sync_folder.py.

**How to apply:** Three tiers, not two. When deciding where a file/directory belongs:

**Tier 1 -- commit to git** (code or website reads it at runtime)
- Python/JS/HTML/CSS/skill reads this file directly -> commit
- Example: `contents/assets/product-refs/`, `contents/assets/fonts/`, `contents/assets/logos/`

**Tier 2 -- gitignore + Drive backup** (stuff you'd cry about losing)
- Generated output you want to keep: `contents/ready/` -> Drive
- Pre-review staging with real work in progress: `contents/new/` -> Drive
- External material that's hard to reacquire: `references/supplier-images/` -> Drive
- Example Drive path: `My Drive/DuberyMNL/backup/<mirror-of-local-structure>`

**Tier 3 -- gitignore AND skip Drive** (local-only, disposable or redundant)
- Rejected/failed content: `contents/failed/` -- it's garbage that didn't pass review, don't back up trash
- Archives of git-tracked content: `archives/` -- git history already has the original once pushed
- Temp scratch: `.tmp/` -- the whole point is it's temporary

**Sync commands:**
- Backup: `python tools/drive/sync_folder.py --local <path> --remote "DuberyMNL/backup/<path>"`
- Cleanup: `python tools/drive/delete_folder.py --remote "DuberyMNL/backup/<path>"`

**Why three tiers and not two:** Session 99, RA caught me about to back up 87MB of archives (tier 3 — redundant with git history) and then 58MB of `contents/failed/` (tier 3 — rejected trash). Without the third tier, the rule would default to "everything non-committed goes to Drive" which wastes Drive quota on garbage. Drive is for stuff you'd cry about losing, not for everything that isn't code.

This rule means Veo-generated videos (coming soon, ~10-50MB each) will go to Drive only if they pass review. Failed video generations stay local-only and get rm'd periodically. The repo stays lean indefinitely and Drive stays focused on keeper content.
