---
name: Chatbot Image Bank Editor
description: Visual HTML editor + batch upload script for refreshing chatbot-image-bank-2026-04.json
type: reference
related: [reference_chatbot_image_bank.md, project_chatbot_knowledge_base.md]
originSessionId: 3c798af6-9b1c-4cd3-86ae-bd50ccc92223
---
**Editor:** `.tmp/chatbot-bank-editor.html` — open directly in browser (file://), no server needed.
- Shows all 44 picks grouped by model (Bandits → Outback → Rasta)
- Hover any card → Replace button → file picker → preview updates
- Save JSON downloads `chatbot-bank-updates-YYYY-MM-DD.json` with diff only (uid, original_url, replacement_file)

**Batch upload script:** `.tmp/batch_upload_bank.py`
- Reads updates JSON + resolves files from `contents/ready/{type}/{model}/`
- Uploads to Drive folder `DuberyMNL/ChatbotImageBank`
- Patches `contents/assets/chatbot-image-bank-2026-04.json` in place (replaces url + drive_file_id per uid)
- Run from project root: `python .tmp/batch_upload_bank.py`

**Workflow:**
1. Open editor HTML, select replacements, Save JSON → `Downloads/chatbot-bank-updates-*.json`
2. Drop JSON in chat alongside replacement image files
3. Run batch upload script → bank JSON patched, Drive uploads done
4. Re-open editor HTML to verify new images show

**Note:** All bank images use `lh3.googleusercontent.com/d/{id}` CDN — not duberymnl.com paths. Domain swaps have zero impact on chatbot image URLs.
