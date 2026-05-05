---
name: Make.com Module Versions
description: Always use latest module versions when building Make.com scenarios, never use legacy/deprecated modules
type: feedback
related: [feedback_make_router_paths.md, reference_make_zapier.md]
---

Always use the latest version of Make.com modules when building scenarios. The old Google Forms module used legacy scopes that didn't include Forms permissions, causing connection issues.

**Why:** Session 79 -- built a scenario with the old Google Forms module, wasted time debugging scope issues that would have been avoided with the newer module version.

**How to apply:** When creating or modifying Make.com scenarios via MCP, check for module version upgrades. Use v2/latest versions of app modules, not legacy ones.
