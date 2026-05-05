---
name: Version Bump + Hard Refresh Rule
description: Always bump ?v= tags after CSS/JS changes; hard refresh needed on live domain for HTML changes
type: feedback
related: [reference_v3_tunnel.md, feedback_cloudflare_edge_cache_bust.md, feedback_js_version_bump.md]
originSessionId: e313ff14-89a5-452f-a520-56686589fc6c
---
Always bump `?v=v3-NNN` version tags in HTML files after ANY CSS or JS change — even CSS-only edits. If the previous version tag was already fetched by the browser, it won't re-fetch without a new query string.

For HTML changes (structure, content), the browser also caches the HTML page itself. Bumping asset version tags isn't enough — the user needs to **hard refresh** (Shift+reload) on the live domain to get the updated HTML.

**Why:** Discovered session 150 when arrow button CSS/HTML changes weren't showing on v3.duberymnl.com despite CF cache being DYNAMIC. Browser had the old HTML + old CSS cached. Multiple iterations were wasted before the root cause was identified.

**How to apply:**
- After any CSS/JS edit: run the PowerShell version bump one-liner in README.md (bump to next number)
- After HTML content changes on the live CF tunnel domain: tell RA to Shift+reload (hard refresh) on the live URL
- CF tunnel (`cf-cache-status: DYNAMIC`) does NOT cache — browser cache is always the culprit on this site
