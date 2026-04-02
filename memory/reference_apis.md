---
name: API Keys and Credentials
description: What API keys are needed in .env and where they come from
type: reference
---

All secrets go in `/projects/DuberyMNL/.env`. Never store secrets anywhere else.

**Meta (DuberyMNL Facebook):**
- META_PAGE_ID: 111349974035733
- META_AD_ACCOUNT_ID: act_208147463
- META_APP_ID: 908271865337799
- META_APP_SECRET: (re-enter from Meta Developer Console)
- META_ADS_ACCESS_TOKEN: (re-enter)
- META_PAGE_ACCESS_TOKEN: permanent, never expires (re-generate if lost)

**Google:**
- credentials.json + token.json — OAuth for Drive MCP (gitignored, project root)
- .gdrive-server-credentials.json — MCP auth token (project root, gitignored)
- GMAIL_SENDER, GMAIL_APP_PASSWORD, REVIEW_EMAIL_RECIPIENT

**Anthropic:**
- ANTHROPIC_API_KEY (for claude --print and API calls)

**kie.ai:**
- KIE_AI_API_KEY + KIE_API_KEY

**Telegram:**
- RASCLAW_BOT_TOKEN (for @Rasclaw01_bot)
- BELLE_BOT_TOKEN (for @arabelle01_bot)

**GDrive MCP fix (Windows):** The MCP has a C:\C:\ path doubling bug. Fix: set both GDRIVE_CREDENTIALS_PATH and GDRIVE_OAUTH_PATH in ~/.claude.json mcpServers config.
