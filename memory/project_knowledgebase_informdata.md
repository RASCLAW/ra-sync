---
name: Knowledgebase Informdata
description: Work knowledge base for Informdata -- 57 encyclopedia files, Flask chatbot with Claude CLI backend, browser vs curl bug unresolved
type: project
related: [feedback_chatbot_architecture.md, project_llm_wiki_system.md, reference_claude_cli.md, project_chatbot_template_reusable.md, project_valor_internal_pitch.md]
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
Knowledgebase-informdata is a personal work tool for RA's night shift at Informdata (Criminal Data Processor).

**What it is:** Informdata guidelines extracted into 57 structured markdown encyclopedia files (from 5+ regional PDFs). Flask chatbot with Claude CLI backend for remote access from work.

**Why:** RA was digging through PDFs or asking supervisors for rule lookups mid-order. Now he can ask Claude Code directly, or use the chatbot remotely.

**Repo:** c:/Users/RAS/projects/Knowledgebase-informdata (in workspace)

**Structure:**
- `encyclopedia/` -- 57 .md files (SSG topics, states, civil, Surge, North Atlantic, North Central, Pacific NW, Southeast, Southwest)
- `.claude/skills/knowledgebase-id.md` -- the lookup skill (updated to walk all ID resolution avenues before detach)
- `documents/` -- original PDFs (gitignored)
- `app.py` -- Flask app, `ask_claude()` uses `claude -p` with `--system-prompt-file`
- `templates/index.html` -- chat UI with image paste (Ctrl+V), file upload, conversation history
- `.tmp/system_prompt.txt` -- 178K char encyclopedia loaded on startup

**Browser bug: FIXED.** Root cause was twofold: (1) 178K prompt exceeded Windows 32K command-line limit, (2) CLAUDE.md EA persona overrode system prompt. Fix: pipe full prompt via stdin (`subprocess.run(cmd, input=prompt)`), keyword-index the knowledge (78K→20-25K per query), greetings bypass Claude entirely. Screenshot support works with `--tools Read` when image attached.

**Architecture (rebuilt session 84):** DuberyMNL chatbot pattern -- knowledge_base.py (indexed loader), conversation_engine.py (Claude wrapper, stdin piping), conversation_store.py (server-side history), app.py (Flask + ngrok).

**RA's queue:** Project Surge, MidAtlantic region. Primary jurisdictions: MD and VA.

**How to apply:** When RA asks work-related questions about case processing, dispositions, charge levels, identifier policy, etc., use `/knowledgebase-id` or read the encyclopedia files directly.
