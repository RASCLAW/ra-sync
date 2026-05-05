---
name: v3 PDP Gallery Editor
description: How the item-editor.js gallery workflow works + the auto-process script pattern for dropping data.json files
type: reference
related: [reference_v3_tunnel.md, feedback_blob_url_not_base64.md, reference_catalog_hover_workflow.md, project_shop_social_edit_mode.md]
originSessionId: bc5e6338-53fa-448a-99ae-c946ebdd0c60
---
**Editor lives at:** `dubery-landing-v3/products/item-editor.js`
**Activated by:** `?edit` in URL, or the floating `✎ Edit` button on any PDP (added to item.html)

## Editor features
- **Add** photos via + button (multi-select)
- **Remove** via × button (hover thumb to reveal)
- **Reorder** via drag-and-drop on thumb strip
- **Replace** main or thumb image via click or drag-drop
- **Save data.json** — patches gallery array for current slug, downloads clean JSON
- **Save HTML** — static snapshot download

## Save data.json workflow
1. RA edits gallery in browser → clicks Save data.json → file downloads
2. RA drops file to Claude
3. Claude runs auto-process script:
   - Detects changed slug by diffing against project data.json
   - For each new image path, checks if file exists in `assets/catalog/`
   - If missing, searches `contents/ready/` recursively and copies
   - Merges updated gallery into project data.json
4. Any still-missing files: RA provides path, Claude copies manually

## Key files
- `dubery-landing-v3/products/data.json` -- gallery arrays for all 11 SKUs
- `dubery-landing-v3/assets/catalog/` -- all gallery images
- `contents/ready/` -- approved image bank, auto-searched on each drop

## Catalog card hero/thumb
The catalog card (`/products/`) uses `hero` and `thumb` fields, NOT `gallery[0]`. If RA reorders the gallery and wants the card to reflect it, update `hero` and `thumb` in data.json manually (as done for outback-black → `hero-outback-black.png`).

**How to apply:** Any time RA is editing PDP galleries or dropping a new data.json file.
