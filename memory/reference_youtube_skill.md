---
name: YouTube API Skill
description: /youtube skill for video metadata, transcripts, liked videos, and channel ops via Data API v3 + OAuth -- replaces WebSearch/WebFetch for YouTube URLs
type: reference
related: [project_llm_wiki_system.md, reference_youtube_api.md, user_youtube_profile.md]
originSessionId: 60513586-10b6-4daf-84d2-72b1a3c48439
---
YouTube skill built at `~/.claude/skills/youtube/` (session 95, 2026-04-09).

**What it does:**
- Video info via YouTube Data API v3 (curl + API key, 1 unit quota)
- Transcript via `get_transcript.py` using `youtube-transcript-api` v1.2+ (free, no quota)
- Search and channel info commands documented in SKILL.md

**OAuth capabilities (session 112, 2026-04-12):**
- Full `youtube` scope in `tools/reauth_token.py` (6 scopes total: drive, sheets, gmail, calendar, youtube)
- OAuth credentials: `credentials.json` (duberymnl-automation project)
- Token: `token.json` -- shared across all Google API tools
- Liked videos, subscriptions, playlists, channel management all possible with OAuth token
- Reauth command: `python tools/reauth_token.py` (browser consent flow)

**KNOWN ISSUE (2026-04-13):** token.json easily loses YouTube scope when other tools (Drive, Sheets) re-auth with narrower scopes. After reauth, always verify scopes include `youtube`. If missing, re-run `reauth_token.py`.

**Auto-triggers on:** YouTube URLs, video research requests, transcript requests.

**Key technical notes:**
- `youtube-transcript-api` v1.2 uses instance methods (`api.fetch()`) not class methods
- Returns `FetchedTranscriptSnippet` objects with `.start` and `.text` attributes, not dicts
- Transcript output format: `[MM:SS] text` per line

**Why:** WebFetch can't scrape YouTube (JS-rendered). WebSearch wastes time. Direct API call is instant and reliable.
