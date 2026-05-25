---
name: dubery-trailer-v1
description: "DuberyMNL motion-graphics trailer in Hyperframes — 8-scene Dubery-native storyboard, RA-approved direction 2026-05-20"
metadata: 
  node_type: memory
  related: 
    - feedback_no_skeleton_reskin.md
    - project_video_dissection_validated.md
    - reference_studio_tunnel.md
    - project_dubery_v2_brand_identity.md
    - project_redflash_ph.md
  type: project
  originSessionId: 4ac03bd6-5087-46e3-92ef-cc91a144e7a7
---

`~/projects/hyperframes/duberymnl-trailer-v1/` — 30s, 9:16 vertical, motion-graphics trailer for DuberyMNL. Built in Hyperframes + GSAP, accessed via studio.duberymnl.com. RA verdict 2026-05-20: "not bad" — direction approved, iteration ongoing.

**Why:** First DuberyMNL motion-graphics trailer that earns the brand. Built after recreating the ElevenLabs Scribe v2 trailer (validation of the dissection method — see [[project_video_dissection_validated.md]]). First attempt was a lazy reskin (corrected in [[feedback_no_skeleton_reskin.md]]). This v1 is Dubery-native: scene list designed from what sunglasses need to sell, not borrowed from a dev-tool trailer.

**How to apply:**

**8-scene arc (30s, no codecard/coverflow/dome — those were ElevenLabs idioms):**
- S1 (0–3s) POLARIZED OPEN — sun-blast bloom + lens-flare streaks + Dubery-serif caption "The sun's gone feral" → Outback Black product flies in from right → WIPE: clarity layer fades up underneath, sells the polarized claim AS experience
- S2 (3–5s) D HIT — D glyph back-out → warm gold halo → motion-blur extrude into DUBERYMNL wordmark + italic subtitle "Polarized for the labas life."
- S3 (5–9s) LINEUP ORBIT — 3 products in formation (Bandits Tortoise left, Outback Black center, Rasta Red right), all rotating 360° on Y-axis simultaneously, then SNAP flat to face camera together
- S4 (9–12s) PRICE PUNCH — kinetic typography stack: Oakley ₱9000, Ray-Ban ₱8000, Knockaround ₱1500 (all struck through, grey) → SLAM: DUBERY ₱499 in 280px Dubery serif with elastic bounce + warm-gold glow → "Same spec. Filipino price."
- S5 (12–17s) SPEC SHEET — film-reel card slides up from bottom, RedFlash Outback tattoo art at 18% opacity as backdrop texture, rows stagger-reveal (LENS / FRAME / DELIVERY / SHIPPING / PRICE)
- S6 (17–22s) WALL OF MOMENTS — 2×2 lifestyle grid pops in diagonally (RIDE / CITY / DAY OUT / DRIVE HOME) → camera dollies INTO the RIDE tile, it grows to fill frame as others fade → tagline "Made for the labas life."
- S7 (22–27s) DMs — live red dot + "FROM OUR DMs · Last 7 days" + 6 stacked bubbles slide in pair-by-pair (Tagalog customer Q + Dubery reply × 3) + "+ 42 MORE THIS WEEK"
- S8 (27–30s) CLOSE — Rasta Red tattoo art backdrop, Outback Black floats in scaled + slow rotation, DUBERYMNL wordmark + italic tagline + warm-gold CTA pill "₱499 · SHOP NOW"

**Assets in project (`assets/`):**
- `fonts/` — `dubery-regular.woff2`, `dubery-italic.woff2` (custom Dubery serif, from dubery-landing-v2)
- `products/` — `outback-black-hero.png`, `outback-black-kraft.png`, `rasta-red-hero.png`, `bandits-tortoise-hero.png`, 4 lifestyle shots (`lifestyle-outback-1/2`, `lifestyle-rasta`, `lifestyle-bandits`)
- `art/` — RedFlash PH tattoo art for Outback Black + Rasta Red (used as background textures in S5 + S8, see [[project_redflash_ph.md]])

**Color palette:** `--ink:#050505`, `--warm:#f4c785`, `--gold:#d4a04a`, `--red:#e63946`, `--grey:#8a8a8a`. Dark cinematic per [[project_dubery_v2_brand_identity.md]].

**Open iteration points (not yet flagged by RA):**
- S1 sun-blast is CSS gradients only — could use an actual phone-POV reference photo for more grit
- S3 lineup orbit uses kraft hero shots; might benefit from removing kraft bg to pure black, or layering a dynamic backdrop
- S6 wall-dolly hits the RIDE tile by default; pick a stronger lifestyle shot if the current `lifestyle-outback-1.png` doesn't feel cinematic
- S7 DMs are placeholder copy in style of real ones; if RA wants, pull actual recent DM screenshots from chatbot/conversation_store.json
- No audio yet — Veo or Suno track could elevate; pacing might shift if synced to beat

**Studio preview:** http://localhost:3002 (local) or https://studio.duberymnl.com (phone/external, see [[reference_studio_tunnel.md]]). Render to MP4: `cd ~/projects/hyperframes/duberymnl-trailer-v1 && npm run render`.
