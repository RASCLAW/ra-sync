# Content Generation Rules Index

Feedback memories on prompt-writing, UGC/brand/Veo content rules, fidelity, aesthetic preferences. Loaded on-demand when the main MEMORY.md points here.

- [UGC Quality Standard](feedback_ugc_quality.md) -- naturalism-first, recreate don't paste
- [Naturalism Over Fidelity](feedback_naturalism_prompting.md) -- soft reference matching beats strict fidelity
- [Plain Text Prompts](feedback_plaintext_prompts.md) -- plain text not JSON for Gemini
- [Simple Prompts Win](feedback_simple_prompts.md) -- overloaded prompts cause 500s
- [Sequential Prompt Planning](feedback_sequential_prompt_planning.md) -- write batch prompts one at a time; parallel writing causes repetitive scene picks
- [RA Aesthetic Preference](feedback_ra_aesthetic_preference.md) -- clean premium > gritty/weathered
- [UGC Framing Rule](feedback_ugc_framing.md) -- product must be recognizable, no whole-body wides
- [Angle Ban](feedback_angle_ban.md) -- -2 and -multi refs banned, single-view only
- [Narrow Scenarios](feedback_narrow_scenarios.md) -- CAFE_TABLE etc repetitive, use sparingly
- [Carousel Format](feedback_carousel_format.md) -- wide then slice, product-anchor only
- [UGC Image Tracking](feedback_ugc_tracking.md) -- every image needs prompt JSON alongside
- [Brand Skill Rewrite](feedback_brand_skill_rewrite.md) -- WF2 fidelity rules + creative freedom zones
- [Creative Range](feedback_creative_range.md) -- variety banks for surfaces, environments, lighting
- [Reference Angle Variety](feedback_reference_angle_variety.md) -- rotate angles across batches
- [NB2 Vehicle Scenes](feedback_nb2_vehicle_scenes.md) -- car/motorcycle interiors fail, avoid
- [No Night Scenes](feedback_no_night_scenes.md) -- sunglasses are daytime product, no dark/evening/neon settings
- [No Jungle Shots](feedback_no_jungle_shots.md) -- drop loc#15 dense jungle path from rotation
- [HOLDING Product-Forward](feedback_holding_product_forward.md) -- sunglasses close to lens, dominate frame, face secondary
- [Drop Heavy-PH-Flavor](feedback_no_sarisari_market.md) -- drop loc#7 jeepney, #26 rice paddy, #29 market, #33 sari-sari
- [Explicit Box Color](feedback_explicit_box_color.md) -- write "dark DUBERY box with red branding" in hero-category placement
- [Multi-Product Randomizer](reference_multi_product_mode.md) -- omit --product for mixed batch with no-repeat
- [UGC Pipeline Rename](reference_ugc_pipeline_skill.md) -- /ugc-pipeline replaces archived /dubery-v3-pipeline
- [Brand Pipeline Research](project_brand_pipeline_research.md) -- Knockaround-inspired direction + duberymnl.com v2 in backlog
- [Veo Prompting](feedback_veo_prompting.md) -- motion-only, negative as nouns, last_frame works
- [Veo ref-image not supported](feedback_veo_ref_image_not_supported.md) -- RawReferenceImage is Imagen-only; --ref-image removed from generate_videos.py
- [Veo RAI not faces](feedback_veo_rai_not_faces.md) -- RAI filter does NOT blanket-block faces; failure was image/prompt-specific
- [Veo RAI composition trigger](feedback_veo_rai_composition.md) -- wide-angle crouching + athletic wear blocks; studio portraits + text overlays pass
- [Veo 8-second prompt structure](feedback_veo_8sec_prompt.md) -- beat-by-beat full-timeline prompt prevents Veo from improvising after initial motion
- [Chatbot Architecture](feedback_chatbot_architecture.md) -- stdin piping, keyword indexing
