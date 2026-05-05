---
name: Apify Setup
description: Apify MCP server configured via stdio, $5/month free credits, Facebook scraping for punk aggregator + research
type: reference
related: [project_punk_aggregator.md]
---

Apify account active, API token set.

**Config:**
- Token in `.env` as `APIFY_TOKEN`
- MCP server in `~/.claude.json` (stdio, `@apify/actors-mcp-server`, same pattern as GDrive)
- Restart Claude Code after adding for tools to load

**Free tier:**
- $5/month platform credits (compute-based, not run-count)
- 4GB actor RAM, 7-day data retention
- ~50-100 scrape runs/month depending on actor size

**Key Facebook actors:**
- Facebook Pages Scraper -- page info, posts, followers, contact
- Facebook Groups Scraper -- public group posts, reactions, comments
- Facebook Posts Scraper -- deep post data, engagement metrics
- All-in-One Facebook Scraper -- pages, posts, events, groups, marketplace, reviews, comments, search, reels, ads

**Use cases:** Manila punk scene research (punk aggregator project), competitor research, DuberyMNL market intel
