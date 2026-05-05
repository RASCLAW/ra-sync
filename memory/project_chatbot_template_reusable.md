---
name: Chatbot webapp pattern is reusable for other knowledge bases
description: The Flask + Gemini + Messenger-style webapp pattern built for DuberyMNL in session 99 can be cloned for any knowledge-base chatbot — Informdata KB is the leading candidate.
type: project
related: [project_chatbot_live.md, reference_cloud_run_chatbot.md, project_knowledgebase_informdata.md, reference_ph_customer_shorthand.md, project_valor_internal_pitch.md]
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
The DuberyMNL chatbot stack built in session 99 is a clean reusable template for any domain-specific knowledge-base chatbot with a web UI. RA flagged this during testing: "the chatbot works great in its own web app. we can definitely use this as a knowledge base webapp for informdata in the future. its fast and cheap."

**The reusable stack:**
- `cloud-run/conversation_engine.py` — Gemini 2.5 Flash via Vertex AI REST (not SDK, avoids Cloud Run hangs), with IPv4 monkey-patch for Windows local hosting
- `cloud-run/messenger_webhook.py` — Flask webhook server with `/chat-test` endpoint + embedded Messenger-style HTML UI
- `cloud-run/knowledge_base.py` — pure data module that feeds the system prompt
- `cloud-run/security.py` — prompt injection defense + output leak scanning
- `cloud-run/conversation_store.py` — in-memory conversation history
- Optional: `cloud-run/crm_sync.py` — Google Sheets persistence layer

**To clone for a new domain (e.g., Informdata KB):**
1. Copy `cloud-run/` → new project folder
2. Rewrite `knowledge_base.py` content:
   - For Informdata: court record policies, tool documentation, common SOPs, escalation paths, reviewer guidelines, data validation rules
   - `get_full_knowledge()` becomes the single source of truth for the KB
3. Rewrite `SYSTEM_PROMPT` in `conversation_engine.py`:
   - New persona ("You are Informdata's research assistant...")
   - Remove DuberyMNL-specific sections (ORDER FLOW, PROVINCIAL ORDERS, DISCOUNT CODE, IMAGE BANK, Filipino shorthand)
   - Keep SECURITY RULES, VOICE (adjusted), FORMATTING, RESPONSE FORMAT
4. Strip Meta-specific code (`send_message`, attachment cache, Meta webhook routes) if the new bot is web-only
5. Strip `crm_sync.py` if persistence isn't needed, or repoint it to a different Google Sheet
6. Rebrand the `/chat-test` HTML UI (header text, colors, preset buttons)
7. Host options:
   - Local + Cloudflare Tunnel (zero cost, what DuberyMNL is doing)
   - Render.com free tier (sleeps after 15 min idle)
   - Hetzner €3.29/mo (when Oracle signup recovers, migrate there)
   - Internal-only on the work PC for Informdata team use

**Cost math at scale:**
- Gemini 2.5 Flash: ~$0.075/1M input tokens, $0.30/1M output tokens
- Typical chatbot turn: ~2000 input tokens + ~500 output tokens = ~$0.00015 + $0.00015 = ~$0.0003/turn
- 1000 turns/day = ~$0.30/day = **~$9/month** for the AI brain
- Hosting: $0 (local + tunnel) to $4/month (Hetzner)
- Total: **$9-13/month for a production-ready KB chatbot handling 1000 turns/day**

**Why this is portfolio-worthy:**
- Shows full-stack: backend (Python/Flask) + frontend (embedded HTML/JS) + AI integration (Gemini REST) + DevOps (tunnel + monitoring)
- Production-grade: security (injection defense, output sanitization), session management, lazy caching, graceful fallbacks, IPv4 patch, CRM sync
- Domain-agnostic: the same pattern works for customer service, internal tools, tech support, sales enablement, docs search, etc.
- Can be demoed to clients as "here's a chatbot I built that handles X, now imagine it trained on YOUR knowledge base"

**Next steps (when ready to pitch Informdata):**
1. Propose it as an internal productivity tool: "a chatbot that answers common QC processor questions, trained on our SOPs and court record policies"
2. Demo with a small pilot knowledge base (10-20 key policies)
3. Pitch the cost: ~$10/month for the team instead of hiring a full-time knowledge worker
4. Positioning: "I already built one for my e-commerce side project, here's how it works" — DuberyMNL chatbot becomes the proof-of-concept
