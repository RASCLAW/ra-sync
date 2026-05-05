---
name: Chatbot Recovery Complete
description: DuberyMNL chatbot fully live on local Flask + Cloudflare tunnel + Worker fallback. Recovery steps a-g done, h-i remain.
type: project
related:
  - project_chatbot_live.md
  - reference_cloudflare_migration.md
  - reference_chatbot_image_bank.md
  - reference_dubery_crm_sheet.md
  - reference_chatbot_live_path.md
  - reference_chatbot_autostart.md
originSessionId: b1211702-4f57-4a72-a5b5-98cb55c11c8f
---
**Status: LIVE (session 117, 2026-04-13)**

**Architecture:**
- Flask on localhost:8080 → Cloudflare named tunnel `dubery-chatbot` → `chatbot.duberymnl.com`
- Cloudflare Worker `dubery-chatbot-fallback` sits in front -- transparent pass-through when origin up, sends away message when origin down
- Auto-start on logon: Task Scheduler tasks `DuberyMNL-Chatbot` + `DuberyMNL-Tunnel`
- UptimeRobot monitors `chatbot.duberymnl.com/status`

**Session 117 features added:**
- dotenv loading for local .env
- Smart flood debounce (3s normal, 8s when image keywords detected)
- Customer image vision (Gemini reads screenshots via inlineData)
- Multi-image handling (1 at a time, polite acknowledgment)
- CRM auth fixed (token.json, not ADC)
- FAQ rewritten to conversational tone
- Startup attachment warmup (48/48 pre-uploaded to Meta CDN)
- Security gates check original text only (no false positives from augmented context)

**Recovery checklist:**
- (a) Image bank 48 + captions -- DONE session 106
- (b) Cloudflare DNS migration -- DONE session 111
- (c) Named tunnel -- DONE session 111
- (d) Wire Meta webhook -- DONE session 117
- (e) Auto-start on boot -- DONE session 117
- (f) UptimeRobot -- DONE session 117
- (g) CRM cleanup -- DONE session 106
- (h) Unpause boosted ads -- PENDING (RA manual)
- (i) 1-week production data -- PENDING (starts after h)

**Why:** Chatbot recovery is the critical path to first paid client (RAS Creative SOLUTIONS). No chatbot = no case study data = no credible pitch.
**How to apply:** Steps h+i are all that remain before the DuberyMNL gate opens for RAS Creative SOLUTIONS launch prep.
