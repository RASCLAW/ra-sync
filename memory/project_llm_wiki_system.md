---
name: LLM Wiki Knowledge System
description: Formalized EA-brain knowledge system based on Karpathy's LLM Wiki pattern -- ingest, lint, cross-reference operations
type: project
related: [feedback_chatbot_architecture.md, feedback_claude_code_layers.md, project_knowledgebase_informdata.md, reference_youtube_api.md, reference_youtube_skill.md, feedback_batch_ingest_pattern.md]
originSessionId: 3c07aaf7-382b-42b7-9e77-196737529157
---
Karpathy's LLM Wiki pattern formalized into three skills for the EA-brain system (session 95, 2026-04-09).

**Three operations now live:**
1. `/ingest` -- Feed a source (YouTube, article, doc) into the brain. Archives raw content, generates summary, updates existing files, logs everything. One ingest touches 3-10 files.
2. `/lint-memory` -- Periodic audit for staleness, contradictions, orphans, cross-reference gaps. Run every ~2 weeks.
3. Cross-referencing via `related:` field in memory frontmatter. Bidirectional links between files.

**Architecture matches Karpathy:**
- Raw sources (immutable) = `EA-brain/references/`
- Wiki pages (LLM-maintained) = `EA-brain/references/summaries/` + auto-memory files
- Schema = CLAUDE.md (global + project)
- Index = MEMORY.md + `references/summaries/INDEX.md`
- Log = `references/ingest-log.md` + `decisions/log.md`

**Key insight:** RA was already 70% there. The gap was no formal ingest step, no periodic lint, and no cross-references between memory files. Obsidian could be added as a graph viewer but isn't required.

**First lint results (2026-04-09):** 62 files, 3 orphans, 5 possibly stale project memories, 0/62 cross-references. Fixes saved to backlog.

**Why:** Makes the knowledge system compound over time instead of just accumulating. Each new source enriches existing knowledge, not just adds a new file.

**How to apply:** Use `/ingest` whenever deep-researching YouTube videos, articles, or docs worth keeping. Run `/lint-memory` every 2 weeks or when things feel stale.
