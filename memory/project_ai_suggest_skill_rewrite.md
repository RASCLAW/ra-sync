---
name: ai-suggest-skill-rewrite-2026-05-25
description: AI Suggest caption agent rewritten from "3 options template" to a thinking skill that reads the image's visual register and matches caption voice. Captures the brand voice evolution from local commuter Filipino DTC to premium commercial editorial while staying 499-affordable.
metadata:
  type: project
related:
  - reference_schedule_chat_vs_agent_chat
  - project_cc_dashboard_overhaul_2026_05_25
  - feedback_chatbot_language
  - feedback_no_skeleton_reskin
---

System prompt lives in `_build_sched_chat_system_prompt()` in [command-center/app.py](../../../projects/DuberyMNL/command-center/app.py). This memory captures the design INTENT so future iterations don't drift back into a transactional caption-vending machine.

## What changed

**Before:** Prompt said "format your reply as 3 numbered options with labels like (product-first), (conversational), (lifestyle)". The agent converged fast on a fixed-menu structure. RA found it transactional vs his ChatGPT brainstorms which were more creative-director — exploring vibes, riffing on themes (Minecraft -> isekai -> rare loot), building concepts iteratively.

**After:** Prompt frames the agent as a thinking SKILL with a 4-step framework:

1. **READ THE IMAGE.** Name what's actually there in one sentence -- who, production register (snapshot / editorial / gritty / polished / retro / cinematic / studio / lifestyle), mood/world, color story, props. Specific to THIS image; generic reads fail the brief.
2. **MATCH THE CAPTION REGISTER TO THAT READ.** Premium editorial -> confident concept-driven. Filipino barkada slice-of-life -> warm Taglish. Retro/themed -> mine adjacent-world vocabulary (gaming, anime, music, sports, Y2K, streetwear) only if image already lives there. Clean studio product -> product-led restraint. Cinematic hero -> minimal mood-first.
3. **GENERATE 5-8 OPTIONS** spanning different angles. Labels EMERGE from the image (not picked from a fixed menu). Example types: product-led, story-hook, scene-anchored, taglish-warmth, cinematic-minimal, social-bait, duo/character-dynamic, place/setting, price-led-CTA, mood-only.
4. **CLOSE WITH A PICK.** One favorite + one sentence on why it fits THIS image best. Then invite next move: "Lean harder into [X], pivot to [Y], or want a Taglish layer / CTA version?"

## Brand voice evolution (baked into prompt)

"DuberyMNL is repositioning. Same affordable 499 polarized sunglasses, but the imagery has leveled up -- from casual Filipino DTC snapshots toward premium editorial / commercial-grade visuals. The caption voice should follow the image without losing accessibility. The tension to manage: premium feel + affordable price + Filipino DTC honesty."

This is the load-bearing line. It tells the agent the brand is in motion, and the caption register should track the image's register at that moment rather than apply one fixed voice.

## Iteration rule

When RA pushes a direction in followup ("lean into the duo dynamic", "more premium", "make it Taglish", "use a CTA"), the agent GOES DEEPER into that vein. It does NOT reset to a generic menu. Each turn either deepens a thread or deliberately opens a new angle. Chat memory is on; the agent references earlier turns.

## `duberymnl.com` CTA cadence

Explicit rule in the prompt:
- Surface `duberymnl.com` as a soft CTA in roughly **1 of every 3-4 captions** in a brainstorm set.
- Weighted toward product-forward, price-led, or social-bait angles.
- Editorial mood / cinematic / story-hook options usually skip the URL because the silence carries.

Variations the prompt models: `"Shop duberymnl.com."`, `"duberymnl.com -- 499, free shipping pag 2+."`, `"Tap. Shop. Equip. duberymnl.com"`.

## Brand non-negotiables (also in prompt)

- 11 colorways across 3 lines: Bandits, Outback, Rasta.
- 499 per pair. Free shipping on 2+ pairs. Metro Manila primary ad market.
- English-dominant (~95%) with light Filipino sprinkles.
- Never write peso prefix or symbol -- just "499".
- Never mention internal codes (D518, D918, D008) -- product name + colorway only.

## Companion features shipped same session

- **Chat history persistence** -- every brainstorm saves to `.tmp/sched_chat_history/<session_id>.json` after each turn. Includes Claude `resume_id` so model context survives CC restarts. `GET /api/schedule/chat/sessions` lists past brainstorms.
- **Emoji picker** in the chat composer (40 emojis, 5 groups: shades / outdoor / action / shop / reactions). Click inserts at cursor.

## How to apply / iterate

- When tweaking the prompt: keep the 4-step framework intact; the labels-emerge-from-image rule is load-bearing. Don't add a "default to gaming/isekai" exception (that was an early draft regression; RA explicitly rejected it -- "we don't need to copy the verbatim").
- When testing: paste an image RA's used recently, ask for "draft captions" without any thematic hint. Reply should start with a one-sentence read of the image, then 5-8 options whose labels are tied to that specific image, then a pick + invite.
- When extending: model is `claude-sonnet-4-6` via `SCHED_CHAT_MODEL` env var. Per [[reference_schedule_chat_vs_agent_chat]], this is the per-tab narrow chat (not the shared full-skills agent). `allowed_tools=['Read']` only so the agent can actually look at images.
