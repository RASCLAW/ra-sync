---
name: meta-use-cases-picker
description: Meta App Dashboard moved permission management from "App Review > Permissions and Features" to a "Use Cases" panel in 2024. Some perms (like read_insights) are no longer under "pages_" prefix in the picker, which causes confused token-regen attempts. This is the working path.
metadata:
  type: reference
related:
  - reference_token_scopes
  - project_cc_dashboard_overhaul_2026_05_25
  - reference_dubery_orders_sheet
---

When adding new permissions to the DuberyMNL Automation Meta App, follow this exact path. Confusion this caused 2026-05-24 -- the old `read_insights` permission isn't searchable under the "pages_" prefix in Graph API Explorer's picker, and the old `App Review > Permissions and Features` page doesn't exist anymore in the new dashboard UI.

## The new permission lifecycle

1. **App Dashboard -> Use cases** -- this is where permissions are managed for newer Meta apps. NOT App Review.
2. Add or expand the relevant use case (e.g. "Manage everything on your Page", "Access Page insights"). The use case bundles multiple permissions including the legacy ones.
3. Permissions can also be added individually via the use case Customize/Add permissions button.
4. Once added at the App level, they appear in Graph API Explorer's permission picker when generating a User Access Token.

## Finding `read_insights`

- It's NOT under the "pages_" prefix in the picker. Don't search "pages" -- you'll see only `pages_manage_metadata`, `pages_manage_engagement`, `pages_utility_messaging`.
- Clear the search box and type the literal string `read_insights`. It surfaces under R.
- If still missing: it hasn't been added to the App via Use Cases yet. Go to App Dashboard -> Use cases -> add/expand a Pages use case, then come back.

## Token regen flow (User token -> permanent Page token)

After granting the new scope in Graph API Explorer:

1. **Generate User Access Token** in GAE (Meta App = DuberyMNL Automation, type = User Access Token).
2. Token is short-lived (~1-2 hours). Don't paste into `.env` directly.
3. Exchange short user token -> long-lived user token (60 days):
   ```
   curl "https://graph.facebook.com/v21.0/oauth/access_token?grant_type=fb_exchange_token&client_id=$META_APP_ID&client_secret=$META_APP_SECRET&fb_exchange_token=<short_user_token>"
   ```
4. Use long user token to fetch permanent Page token from `/me/accounts`:
   ```
   curl "https://graph.facebook.com/v21.0/me/accounts?access_token=<long_user_token>"
   ```
   Find the page entry matching `META_PAGE_ID` -- its `access_token` field is the permanent Page token (`expires_at: 0`).
5. Swap into `.env` as `META_PAGE_ACCESS_TOKEN=...`. Back up the old token first.
6. Restart chatbot + CC so they pick up the new token.

A working one-shot script for steps 3-5 lives at `.tmp/swap_page_token.py` (gitignored; references `META_APP_ID`/`META_APP_SECRET`/`META_PAGE_ID` from `.env`). Recreate from the pattern if needed.

## Pre-deploy verification

Before swapping `.env`, verify the new token has ALL the scopes the chatbot needs (not just the new one). Critical scopes that must NOT be dropped: `pages_messaging` (Send API), `pages_read_engagement`, `pages_manage_posts`, `pages_show_list`. Use:

```
curl "https://graph.facebook.com/v21.0/debug_token?input_token=<new_token>&access_token=$META_APP_ID|$META_APP_SECRET"
```

Check the `scopes` array in the response. If `pages_messaging` is missing, the chatbot will instantly stop replying once you swap and restart. Don't skip this check.

## What unlocks with `read_insights`

In the DuberyMNL Use Cases panel as of 2026-05-25, the active scope set is 13 perms:
`read_insights, pages_show_list, ads_management, ads_read, business_management, pages_messaging, pages_read_engagement, pages_manage_metadata, pages_read_user_content, pages_manage_ads, pages_manage_posts, pages_manage_engagement, public_profile`.

With `read_insights` granted, these Page Insights metrics return real data (was returning empty arrays before):
- `page_impressions_unique` -- 28d reach (~283K for DuberyMNL Page as of 2026-05-25)
- `page_post_engagements` -- 28d reactions+comments+shares (~8.7K)
- `page_views_total` -- 28d profile views (~3.9K)
- `page_actions_post_reactions_total` -- optional 4th metric

Deprecated metric IDs (return #100 "value must be a valid insights metric"): `page_impressions`, `page_engaged_users`, `page_fans`, `page_consumptions`, `page_consumptions_unique`.

Without `read_insights`, those scopes return empty arrays silently (no error message) -- a known Meta footgun. Check token scopes first when an insights endpoint quietly returns nothing.
