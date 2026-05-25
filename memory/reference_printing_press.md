---
name: reference-printing-press
description: Printing Press is a Go-based CLI factory + 144-CLI catalog at printingpress.dev. Replaces MCP servers with token-efficient CLIs. Install path validated on Windows 2026-05-20.
metadata: 
  node_type: memory
  related: 
    - feedback_claude_code_layers
    - reference_agent_sdk_credits_june15
  type: reference
  originSessionId: a2ad8656-8c9e-42a4-8e39-3be6f6de77a2
---

**What:** CLI factory + library for Claude Code. Author: mvanhorn. Inspired by Peter Steinberger's `discrawl`/`gogcli`. Launched May 9, 2026. Pitch: MCP servers use 35× more tokens than equivalent CLIs.

**Install (validated 2026-05-20 on Windows 11):**
1. Go 1.26.3 required. Winget install hit a flaky network error mid-MSI download — fallback: zip distribution from `https://go.dev/dl/go1.26.3.windows-amd64.zip` extracted to `C:\Users\RAS\go-sdk\go`.
2. **CRITICAL Windows gotcha:** `Expand-Archive` (PowerShell) silently truncates Go's stdlib due to path-length limit — `go install` then fails with "package X is not in std." Use `tar.exe -xf` instead (Windows 10+ ships with it).
3. Env vars set permanently via setx: `GOROOT=C:\Users\RAS\go-sdk\go`, `GOPATH=C:\Users\RAS\go`, PATH includes both `\bin` dirs.
4. Factory binary: `go install github.com/mvanhorn/cli-printing-press/v4/cmd/printing-press@latest` → `C:\Users\RAS\go\bin\printing-press.exe` (v4.9.0).
5. Skills repo cloned to `C:\Users\RAS\projects\cli-printing-press` (3309 files).
6. Pre-built CLIs install via: `go install github.com/mvanhorn/printing-press-library/library/<category>/<name>/cmd/<name>-pp-cli@latest`.

**How to actually use the factory:** Open a separate Claude Code session in the cloned repo with `claude --plugin-dir .`, then `/printing-press <app-or-url>` inside that session. The factory needs the agent loop — the binary's `print` command only sets up plan-file scaffolding.

**Test results — coingecko CLI on bitcoin+ethereum price query:**
- Default JSON output: 409 chars
- `--compact` flag: 409 chars (no diff — flag only filters fields you didn't explicitly request)
- `--agent` mode (sets `--json --compact --no-input --no-color --yes`): 148 chars (-64%) when not asking for extra fields
- `agent-context` command emits full CLI schema as 11KB JSON for agent discovery (this is what agents call instead of loading MCP tool definitions)

**Depth validated across 12 endpoints (all live, working):**
1. `simple price --ids X,Y --vs-currencies usd,php` — multi-coin multi-currency with 24h change + market cap
2. `coins detail bitcoin --community-data --developer-data --market-data` — full coin profile incl GitHub stars/forks/PR/commit counts
3. `coins market-chart coin bitcoin --days 7` — 169 hourly price/volume/mcap points
4. `coins ohlc coin bitcoin --days 1` — 48 OHLC candles
5. `coingecko-search-2` (trending) + `coingecko-search --query X` — coins + exchanges + categories with mcap rank
6. `global` — market overview with 17K+ active cryptos, BTC dominance %
7. `coins markets --order market_cap_desc --price-change-percentage 1h,24h,7d` — top-N with multi-period change cols
8. `sync --resources coins` — 17,406 rows to 11.4MB SQLite in 6.3s
9. `analytics --type coins --group-by symbol --limit 5` — compound queries on local data
10. `which "trending"` — capability search returns command + relevance score (the real lever vs MCP)
11. `doctor` — health check
12. `agent-context` — full machine-readable CLI schema

**Gotchas to flag next time:**
- `--compact` is aggressive in `--agent` mode — auto-strips `coins markets` results down to `{id, name}` only. Use bare `--json` (no compact) when you need full fields.
- `--data-source local` for prices fails — `sync` mirrors coin metadata only, not live/historical prices. Per-resource caveat, not documented in CLI help.
- FTS5 `search ethereum --type coins` returned empty after sync; minor, likely needs different invocation syntax.

**Where token savings actually come from:** Not per-call output compression. The real win is that MCP loads tool definitions into context on session start (~1000s of tokens × N servers). CLI loads nothing until invoked. For RA with multiple MCPs in `/context`, this is the leverage.

**144 CLIs in catalog — relevant to RA's stack:**
- `contact-goat` (LinkedIn email finder, RAS Creative cold outreach)
- `google-search-console` (SEO, post-priority-7 work)
- `clarity` (just installed CC tag on duberymnl.com)
- `producthunt` (launch + competitor research, no API app required)
- `firecrawl` (web scraping)
- `notion`, `linear`, `jira`, `github`, `airbnb`, `craigslist`, `amazon-seller`, `figma`, `google-ads`, `klaviyo`, `mailchimp`, `dominos`, `dub`, `mercury`, `stripe`, etc.

**Does NOT work for:** CapCut (original ask). Printing Press reverse-engineers HTTP traffic; CapCut is a desktop app with no network surface to grab. CapCut Web (capcut.com/editor) or CapCut's template API would be candidates.

**Dubery angle:** RA's `tools/` directory is already a CLI library pattern. Printing Press would be additive for: (a) replacing MCPs RA hasn't installed yet (Zapier MCP backlog, PageAgent MCP), (b) reverse-engineering sites RA has been blocked on by "no API," (c) the auto-generated MCP server lets us expose the same backend to both shell and IDE agents.
