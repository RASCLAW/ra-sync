---
name: career-ops Job Search System
description: santifer/career-ops -- viral Claude Code job search pipeline (19K stars), scoring + portal scanner + ATS PDF gen, adapt for RA's job hunt
type: reference
related: [project_portfolio_rebuild.md, project_virtudesk_application.md]
---

## santifer/career-ops (GitHub, 19K stars)

AI-powered job search pipeline built on Claude Code slash commands. Author evaluated 740+ offers, generated 100+ tailored CVs, landed Head of Applied AI role.

### 3 Core Systems

1. **Scoring engine** -- 5 dimensions (CV match, North Star alignment, comp, culture, red flags), 1-5 scale, archetype detection (LLMOps, Agentic, AI PM, etc.), 6-block evaluation report per JD
2. **Portal scanner** -- Playwright + Greenhouse API + WebSearch, 60+ companies, title filtering, liveness checks, dedup
3. **ATS PDF generator** -- 120-line Node.js + Playwright, extracts JD keywords, rewrites summary + reorders bullets per JD, never fabricates

### Tech Stack
- Node.js (vanilla ESM), Playwright, Go + Bubble Tea TUI, markdown/YAML data, no database
- 14 skill modes in `modes/` folder, single `/career-ops {mode}` entry point
- Batch processing via `batch/batch-runner.sh` spawning parallel `claude -p` workers
- Two-layer data contract: user files (cv.md, profile.yml) never auto-updated, system files safely updatable

### Adaptation Plan for RA
- **Path A (fast):** Fork, customize profile.yml + cv.md, swap company list for remote AI/automation companies
- **Path B (clean):** Cherry-pick scoring + PDF gen into EA-brain skills, keep existing scout.py + market_intel.py
- **Recommended:** Path A to start, Path B over time
- **Customize archetypes to:** AI Automation Builder, Agentic System Designer, Sales Funnel Engineer
- **Key gap it fills:** per-JD CV customization, Interview Story Bank (STAR+R), structured scoring, batch eval

### Key Patterns Worth Studying
- Self-customizing agent (Claude edits its own config files)
- Mode routing via single skill file with lookup table
- Auto-detect JD input and trigger evaluation pipeline
- Resumable batch state tracking with atomic locks
