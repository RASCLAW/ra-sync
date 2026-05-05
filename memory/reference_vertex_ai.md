---
name: Vertex AI Setup
description: GCP project dubery -- ADC auth, Gemini 3.1 Flash image gen, Veo 3.1 video gen, pricing, available models
type: reference
related: [feedback_naturalism_prompting.md, feedback_plaintext_prompts.md, feedback_simple_prompts.md, project_veo_video_gen.md, reference_gcloud_cli.md, feedback_google_api_client_broken.md, reference_vertex_rate_limits.md]
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---
## GCP Account
- Account: ronaldadriansarinas@gmail.com
- Project: dubery (ID: 371181189379)
- $300 free credits, expires July 5, 2026
- Billing account: 019082-3259C7-AAC8BD

## Authentication
- **ADC (Application Default Credentials)** -- primary auth method (as of session 90)
- Set up via `gcloud auth login` + `gcloud auth application-default login`
- Old API key method (`GOOGLE_API_KEY`) still works but deprecated
- SDK: `google-genai` v1.70.0

## Image Generation
```python
from google import genai
from google.genai.types import GenerateContentConfig, Modality

client = genai.Client(vertexai=True, project='dubery', location='global')
response = client.models.generate_content(
    model='gemini-3.1-flash-image-preview',
    contents=parts,
    config=GenerateContentConfig(response_modalities=[Modality.IMAGE]),
)
```
- **IMPORTANT:** location='global' for Gemini 3.x models (us-central1 gives 404)
- Model: `gemini-3.1-flash-image-preview` (best available image model)
- Reference images: pass as `Part(inline_data=Blob(...))` before text prompt
- Pricing: ~$0.067/image (1K), ~$0.10/image (2K)

## Video Generation (Veo)
```python
from google.genai.types import GenerateVideosConfig, Image

client = genai.Client(vertexai=True, project='dubery', location='us-central1')
operation = client.models.generate_videos(
    model='veo-3.1-fast-generate-001',
    prompt='...',
    image=Image(image_bytes=ref_bytes, mime_type='image/png'),
    config=GenerateVideosConfig(
        aspect_ratio='9:16',
        number_of_videos=1,
        generate_audio=True,
    ),
)
```
- **IMPORTANT:** location='us-central1' for Veo (not global)
- `image` param = image-to-video (animate from this frame)
- `reference_images` in config = use as visual reference (ASSET or STYLE type)
- `generate_audio=True` enables speech + ambient sounds (Veo 3.1 only)

### Veo Model Tiers
| Model | Audio | Speech | Cost/8s video | Use for |
|-------|-------|--------|---------------|---------|
| veo-3.1-generate-001 | Yes | Yes | ~$3-4 | Hero content |
| veo-3.1-fast-generate-001 | Yes | Yes | ~$1-1.50 | Default/daily |
| veo-3.1-lite-generate-001 | Yes | Yes | ~$0.50-1 | Budget/bulk |
| veo-2.0-generate-001 | No | No | varies | DON'T USE |

## Other Available Models
- Imagen 4 Ultra/Generate/Fast (text-to-image only, no refs)
- Gemini 2.5 Flash/Pro TTS (text-to-speech)
- Chirp 3 (speech synthesis)
- Cloud TTS API (already enabled, 4M chars free/month)

## Available Infrastructure
- Cloud Run (serverless hosting, 2M req/month free)
- Cloud Functions (2M invocations/month free)
- Cloud Scheduler (3 cron jobs free)
- Cloud Storage (5GB free, CDN URLs)
- BigQuery (1TB queries/month free)

## UGC Pipeline Flow
1. **Context load** -- read recent entries from ugc_pipeline.json, avoid repeats
2. **Caption gen** -- theme, mood, scenario_hint, product_ref -> CAPTION_APPROVED
3. **Prompt writer** (`/dubery-ugc-prompt-writer`) -- caption -> prompt -> PROMPT_READY
4. **Fidelity gatekeeper** -- 8 checks, PASS/REJECT (one rewrite allowed)
5. **Image generation** -- `generate_vertex.py` -> Gemini 3.1 Flash -> image
6. **Review** -- view image, pass/fail
