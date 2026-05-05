---
name: Conversation Store Handoff Persists Across Restarts
description: handoff_flagged=True in conversation_store.json survives bot restarts, silently blocks sender IDs
type: feedback
related: [feedback_crm_adc_scope.md, feedback_google_api_cache_discovery.md]
originSessionId: 4a520b66-408c-4017-b042-95e3c5f75811
---
`conversation_store.json` is written to disk on every `flag_handoff()` call. On restart, `ConversationStore.load()` reads it back -- so any sender_id with `handoff_flagged: True` is silently blocked from the first message, forever.

**Why:** Discovered 2026-04-19 after provincial order test set `should_handoff=true` for RA's own sender_id. Bot appeared to "hang" at "Running generate..." but was actually returning `blocked: handoff` immediately and staying silent. Took multiple restart cycles to diagnose.

**How to apply:**
- After any test that triggers `should_handoff=true` for a real sender_id, manually clear the flag: `python -c "import json; ...` or hit the `/release-handoff` endpoint if one exists
- Never use a real production sender_id for provincial/handoff E2E tests -- use `TEST_*` prefix instead (already skips CRM, also skips store persistence for test-only flows)
- Check `.tmp/conversation_store.json` before debugging "bot not responding" issues -- look for `handoff_flagged: true`
