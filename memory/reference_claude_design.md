---
name: Claude Design Reference
description: Claude Design capabilities, workflow, ISP fix, and DuberyMNL use case
type: reference
related: [project_dubery_v3_landing.md, project_dubery_landing_v2.md]
originSessionId: 27face87-f412-4aab-a563-0acef8250ad4
---
Claude Design is a canvas-based visual builder at claude.ai/design (Anthropic Labs research preview, launched 2026-04-17). Powered by Opus 4.7. Free on Pro/Max/Team/Enterprise, counts toward usage limits.

**Workflow:**
1. Set up a Design System (company blurb, GitHub repo, local folder, fonts/logos, brand notes)
2. Create prototype → pick Wireframe or High Fidelity
3. Prompt in layers: vibe/tone → layout → specific content
4. Iterate in chat; use "Make tweakeable" for knobs instead of re-prompting
5. Export: standalone HTML, Canva, PPTX, PDF, or hand off to Claude Code

**Key features:**
- GitHub integration: Claude reads codebase to inherit existing styles/components
- "Save as standalone HTML" = instant working file, drop into repo as v1
- "Make tweakeable" = exposes sliders/layout controls
- Can build: landing pages, wireframes, slide decks, animated video, interactive UI

**DuberyMNL setup (done 2026-04-19):**
- Design System: "DuberyMNL" with brand notes (light Knockaround theme, product-forward copy, MM audience, Messenger CTA)
- GitHub: https://github.com/RASCLAW/DuberyMNL linked
- Assets: logo-header.png + custom WOFF2 font uploaded
- Default for all new prototypes

**ISP fix (PLDT login loop):**
- PLDT routed through Cloudflare block → login loop on claude.ai/design
- Fix: Cloudflare WARP app (1.1.1.1) or ProtonVPN Chrome extension with split tunneling

**How to apply:** Use Claude Design to prototype duberymnl.com v2/v3 sections before committing to code. One section at a time, High Fidelity mode, export as standalone HTML then integrate.
