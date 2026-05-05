---
name: Make.com Router Fires All Paths
description: Make.com routers fire ALL matching paths, not first-match -- fallback routes need explicit exclusion filters
type: feedback
related: [feedback_ai_prompt_verbatim.md, feedback_make_module_versions.md, reference_make_zapier.md]
---

Make.com BasicRouter fires every path whose filter matches, not just the first match. A fallback path with no filter will ALWAYS fire alongside any other matching path.

**Why:** During Client Engagement Pipeline testing, consulting leads received both the consulting proposal email AND the fallback email because the fallback had no filter.

**How to apply:** Always add explicit exclusion filters on fallback/catch-all routes (e.g., "does not contain consulting AND does not contain automation"). Never leave a fallback path unfiltered if other paths exist.
