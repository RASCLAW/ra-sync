---
name: takeover-via-dashboard
description: For manual chatbot handoff, RA uses the /conversations admin dashboard exclusively -- not bookmarklets, not curl, not Page Inbox shortcuts
metadata:
  type: feedback
related:
  - reference_cc_command_center.md
  - feedback_store_handoff_persist.md
  - project_chatbot_live.md
---

When RA needs to manually take over a Messenger conversation from the bot, the go-to surface is **https://chatbot.duberymnl.com/conversations** — the admin dashboard rendered by `conversations_view()` in [chatbot/messenger_webhook.py:1912](../../../projects/DuberyMNL/chatbot/messenger_webhook.py#L1912). Don't propose bookmarklets, browser shortcuts, or curl flows for this use case.

**Why:** Single screen with PSID, last 6 messages, badges (handoff/order/policy/source/intent), and existing FLAG HANDOFF / RELEASE / MARK SALE buttons. RA reviewed it 2026-05-23 and chose it over a bookmarklet alternative — "easier to mark them in the conversations page, best that I access it from there."

**How to apply:**
- When designing takeover/handoff UX, default to dashboard-first iteration. Add features to /conversations rather than building parallel surfaces.
- Don't suggest browser bookmarklets that call `/flag/<PSID>` — RA finds them clunkier than the existing buttons.
- The `message_echoes` auto-detection path (echo arrives with `app_id != META_APP_ID` → auto-flag) is still the eventual durable fix for "bot replies after I already replied in Page Inbox" — see [[project_chatbot_live]] for the handler at `messenger_webhook.py:1151`. As of 2026-05-23 the subscription isn't on at the Meta App level (zero echo events in chatbot-server.log), so the handler never fires. Enable via Graph API curl when ready.
- Future iterations on takeover flow happen on /conversations (likely additions: mobile-friendly layout, filter by handoff state, batch actions).
