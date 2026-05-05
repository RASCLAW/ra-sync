---
name: Make.com & Zapier Accounts
description: Free accounts for portfolio tool learning, MCP connection details
type: reference
related: [feedback_make_module_versions.md, feedback_make_router_paths.md, project_portfolio_rebuild.md]
---

**Make.com:**
- Zone: eu1.make.com
- Plan: Free (1,000 credits/mo, 2 active scenarios)
- API token: stored in Claude Code MCP config (user scope)
- MCP: connected via `https://eu1.make.com/mcp/u/<token>/stateless`

**Zapier:**
- Plan: Pro trial (expires Apr 17, 2026), 2,000 tasks
- MCP: registered at `https://mcp.zapier.com/api/v1/connect`, needs OAuth auth from home browser
- To authenticate: start Claude Code, run `/mcp`, select Zapier, click Authenticate

**n8n:**
- MCP: connected in docs-only mode (1,396 nodes knowledge base)
- No account needed. Full workflow mode requires n8n running locally (`npx n8n`)
