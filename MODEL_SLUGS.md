# Model Vendor & Model Slugs

A shared vocabulary for naming the **model** a tool is built on, separate from the
**provider** that runs it. Two optional tool-level fields carry it:

| Field | Meaning | Granularity | Example |
|-------|---------|-------------|---------|
| `model_vendor` | The lab or company that made the model. | The org. | `black-forest-labs` |
| `model` | The specific model, scoped tightly to its parameters — the unit across which settings, LoRAs, and cross-compatibility hold. | Granular. | `flux2-klein-9b` |

Both fields are **optional** and live alongside the other [tool descriptor metadata](TOOLS_PROTOCOL.md#metadata-fields). Generic tools, built-in filters, and tools with no meaningful model omit them. Hosts use these slugs for two things: rendering consistent vendor/model branding (icons) in tool UIs, and assessing cross-compatibility between tools (e.g. whether a LoRA or setting carries from one tool to another). They are **not** `provider`: `provider` describes where a tool runs or comes from (a Stimma Cloud account, a ComfyUI instance, a built-in), while `model_vendor`/`model` describe the model itself. The two are orthogonal — the same model can be served by many providers.

There is intentionally no `family` layer between vendor and model. Vendor plus a granular model is enough, and two models that look like one "line" still split when their parameters or compatibility differ (`z-image-base` vs `z-image-turbo`, `flux2-klein-4b` vs `flux2-klein-9b`).

## Slug naming convention

- Lowercase ASCII, `-` separated, no legal suffixes (no `™`, `Inc`, etc.).
- Versions follow the model's own style — dots are fine (`rmbg-2.0`, `wan-2.6`), solid where the brand writes it solid (`flux2`).
- Split on parameter or compatibility boundaries: size (`4b` / `9b` / `3b` / `7b`), tier (`turbo` / `pro` / `base`), distilled vs. full.
- This document is the source of truth. Unknown slugs SHOULD degrade gracefully — a host that doesn't recognize a slug falls back to generic treatment.

## Vendor slugs

| `model_vendor` | Display name |
|----------------|--------------|
| `openai` | OpenAI |
| `google` | Google |
| `xai` | xAI |
| `black-forest-labs` | Black Forest Labs |
| `alibaba` | Alibaba |
| `bytedance` | ByteDance |
| `ideogram` | Ideogram |
| `krea` | Krea |
| `lightricks` | Lightricks |
| `skywork` | Skywork |
| `stability-ai` | Stability AI |
| `bria-ai` | Bria |

## Model slugs

Grouped by vendor. A `model` slug is meaningful only together with its `model_vendor`.

### `black-forest-labs`

| `model` | Display name |
|---------|--------------|
| `flux1-dev` | FLUX.1 Dev |
| `flux2-dev` | FLUX.2 Dev |
| `flux2-klein-4b` | FLUX.2 Klein 4B |
| `flux2-klein-9b` | FLUX.2 Klein 9B |
| `flux2-pro` | FLUX.2 Pro |
| `flux2-flex` | FLUX.2 Flex |
| `flux2-max` | FLUX.2 Max |

### `alibaba`

| `model` | Display name |
|---------|--------------|
| `qwen-image` | Qwen-Image |
| `qwen-image-2512` | Qwen-Image 2512 |
| `qwen-image-2.0` | Qwen-Image 2.0 |
| `qwen-image-2.0-pro` | Qwen-Image 2.0 Pro |
| `qwen-image-edit-2509` | Qwen-Image Edit 2509 |
| `qwen-image-edit-2511` | Qwen-Image Edit 2511 |
| `wan-2.2` | Wan 2.2 |
| `wan-2.6` | Wan 2.6 |
| `wan-2.6-flash` | Wan 2.6 Flash |
| `wan-2.6-image` | Wan 2.6 Image |
| `z-image-base` | Z-Image Base |
| `z-image-turbo` | Z-Image Turbo |

### `google`

| `model` | Display name |
|---------|--------------|
| `nano-banana` | Nano Banana |
| `nano-banana-pro` | Nano Banana Pro |
| `veo-3.1` | Veo 3.1 |
| `veo-3.1-fast` | Veo 3.1 Fast |

### `bytedance`

| `model` | Display name |
|---------|--------------|
| `seedream-4.5` | Seedream 4.5 |
| `seedance-1.5-pro` | Seedance 1.5 Pro |
| `seedvr2-3b` | SeedVR2 3B |
| `seedvr2-7b` | SeedVR2 7B |
| `seedvr2` | SeedVR2 |

### `xai`

| `model` | Display name |
|---------|--------------|
| `grok-imagine` | Grok Imagine |
| `grok-imagine-pro` | Grok Imagine Pro |
| `grok-imagine-video` | Grok Imagine Video |

### `openai`

| `model` | Display name |
|---------|--------------|
| `gpt-image-1.5` | GPT Image 1.5 |
| `gpt-image-2` | GPT Image 2 |

### `ideogram`

| `model` | Display name |
|---------|--------------|
| `ideogram-4` | Ideogram 4 |

### `krea`

| `model` | Display name |
|---------|--------------|
| `krea-2-large` | Krea 2 Large |
| `krea-2-medium` | Krea 2 Medium |
| `krea-2-turbo` | Krea 2 Turbo |

### `lightricks`

| `model` | Display name |
|---------|--------------|
| `ltx-2` | LTX-2 |
| `ltx-2-distilled` | LTX-2 Distilled |
| `ltx-2.3` | LTX-2.3 |
| `ltx-2.3-fast` | LTX-2.3 Fast |

### `skywork`

| `model` | Display name |
|---------|--------------|
| `skyreels-v4` | SkyReels V4 |

### `stability-ai`

| `model` | Display name |
|---------|--------------|
| `sdxl` | SDXL |

### `bria-ai`

| `model` | Display name |
|---------|--------------|
| `rmbg-2.0` | RMBG 2.0 |

## Adding a new model

When you build a tool on a model that isn't listed here:

1. Pick the obvious lowercase-dash form of the model's published name, following the [naming convention](#slug-naming-convention) above.
2. Split on parameter and compatibility boundaries — give distinct slugs to variants that differ in size, tier, or distillation, since those are the units across which settings and LoRAs hold.
3. Submit the slug (and its vendor, if new) to this list so other tool authors can reuse it.

Until a slug is established here, omit `model` rather than inventing a private one — hosts treat an absent slug gracefully, but inconsistent slugs for the same model defeat the purpose.
