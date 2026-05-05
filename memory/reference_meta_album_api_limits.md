---
name: Meta Album API Limits (create is dead)
description: POST /page/albums is permanently blocked by Meta regardless of scope — workaround is create album manually in FB UI, then API-add photos to it
type: reference
related: [reference_meta_attachment_caching.md, reference_fb_stories_api.md, reference_meta_verified.md]
originSessionId: 2d508059-363a-4c95-9edb-672b753e5f69
---
Meta killed programmatic album creation. You cannot create a Facebook Page album via Graph API anymore, no matter the token scope or app review status. Diagnosed session 126 after repeated `(#3) Application does not have the capability to make this API call` errors.

## What's blocked
- `POST /{page_id}/albums` — creates 400 `(#3)` every time
- `POST /{album_id}` for rename/description edit — same `(#3)` error
- `POST /{photo_id}` for caption edit — same `(#3)` error (though posted-via-UI photos can't be updated by apps anyway, since apps can only update content they created)

## What works
- `GET /{page_id}/albums` — list albums + their IDs, URLs, `can_upload` flag
- `GET /{album_id}` — album info incl. `link` (public URL)
- `GET /{album_id}/photos` — list photos + image sizes
- `POST /{album_id}/photos` — **add** photo to existing album with `source` + `message` + `no_story=true`. This IS supported with `pages_manage_posts` scope. No story pushed.

## Workaround pattern (silent album)
1. RA manually creates empty album in FB UI (one-time per use case)
2. Get album ID via `GET /{page_id}/albums`
3. Refactor `upload_album.py` to accept `--album-id` instead of creating one
4. Loop `POST /{album_id}/photos` per image with `no_story=true`

## Other gotchas hit session 126
- Page Access Token ≠ User Access Token. Album/photo write endpoints require page-scoped token. Get via `GET /{page_id}?fields=access_token` or Graph Explorer "Get Page Access Token".
- Graph Explorer permission popup only lists scopes that the App (not just the token) has declared. If `pages_manage_posts` doesn't show in the popup, go to App Dashboard → App Review → Permissions → Get advanced access (instant for admins in Dev mode).
- Granular scopes panel in Token Debugger shows which pages a scope applies to — confirm the target page is listed.

## Files
- `tools/facebook/upload_album.py` — built session 126, currently blocked at step 1 (album create). Needs `--album-id` refactor to be usable.
