---
name: Token.json Scopes
description: Python OAuth token.json has 6 scopes -- drive, sheets, gmail.modify, gmail.settings.basic, calendar, youtube (full read/write)
type: reference
related: [reference_gmail_labels.md, reference_gws_cli.md, feedback_google_api_client_broken.md, reference_youtube_api.md, reference_youtube_skill.md]
originSessionId: 26a8d61f-ac8b-4c50-8af5-b988c063a4dc
---
token.json at `c:/Users/RAS/projects/DuberyMNL/token.json` has these scopes:
- `https://www.googleapis.com/auth/drive`
- `https://www.googleapis.com/auth/spreadsheets`
- `https://www.googleapis.com/auth/gmail.modify`
- `https://www.googleapis.com/auth/gmail.settings.basic`
- `https://www.googleapis.com/auth/calendar`
- `https://www.googleapis.com/auth/youtube` (full read/write -- playlists, ratings, etc.)

Re-auth script: `tools/reauth_token.py` (includes all 6 scopes).

**Critical:** Never narrow scopes on refresh (session 76 lesson). If re-authing, always include ALL scopes above.
