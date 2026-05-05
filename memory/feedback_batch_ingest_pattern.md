---
name: Batch ingest via parallel subagents
description: When ingesting N videos/articles at once, dispatch one Sonnet subagent per source in parallel for raw+summary. Main Opus thread consolidates INDEX + log + memory index + backlog after.
type: feedback
related:
  - feedback_sonnet_delegation.md
  - project_llm_wiki_system.md
originSessionId: 3c07aaf7-382b-42b7-9e77-196737529157
---
When RA asks to ingest multiple sources (e.g., "ingest all"), dispatch one Sonnet subagent per source in parallel. Each subagent owns: (1) fetch/verify transcript, (2) write raw file with metadata header, (3) write opinionated summary, (4) return structured report (key insight, cross-refs added, cross-refs needed back, new memory suggested, backlog item suggested).

The main Opus thread consolidates AFTER all subagents return:
- Update `references/summaries/INDEX.md` (one batched Edit)
- Update `references/ingest-log.md` (one batched entry for the whole batch)
- Create new memory files for suggested references
- Update `MEMORY.md` index
- Update `current-priorities.md` backlog with actionable items
- Apply reverse-side bidirectional cross-refs that subagents couldn't touch

**Why:** Validated 2026-04-15 on a 9-video batch in ~8 minutes wall time. Pure serial ingest in main Opus would have blown ~500K tokens and taken ~2 hours. Parallel Sonnet subagents stayed at ~500K combined but ran concurrently; main thread consumed ~50K for synthesis. Subagents also wrote better opinionated summaries because each got a focused briefing about which existing knowledge to cross-reference.

**Why NOT let subagents touch INDEX.md / ingest-log.md / MEMORY.md:** concurrent writes conflict. Even serialized, subagents can't see the full batch context, so entries drift in format. Main thread must be the single writer for these consolidation files.

**How to apply:**
- For 1-2 sources: ingest directly in main Opus (subagent briefing overhead dominates).
- For 3+ sources: always batch. Fetch all transcripts first in parallel background Bash calls, then dispatch parallel subagents with transcript paths + full context briefing.
- Briefing MUST include: RA's positioning (DuberyMNL + RAS Creative locked), current priorities, already-ingested related summaries to cross-ref, relevant memory files, quality rules (opinionated not neutral, concrete action items, flag contradictions).
- Each subagent should return the structured report block (RAW_FILE / SUMMARY_FILE / KEY_INSIGHT / CROSS_REFS_ADDED / BIDIRECTIONAL_REFS_NEEDED / NEW_MEMORY_SUGGESTED / BACKLOG_ITEM_SUGGESTED) so main thread can consolidate without re-reading outputs.
- If a subagent's summary looks thin, read the summary file and its transcript to verify before moving on — trust but verify.
