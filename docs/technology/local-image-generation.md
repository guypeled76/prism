# T6. Local image generation

[← Technology validation](README.md)

**Verdict: Partial. Feasible as an optional extra, not as a core feature.** Distilled diffusion models can make a 512 × 512 image in a few seconds on recent phones and laptops, but the download (1 to 2 GB), memory use (cannot share RAM with the LLM), heat, and weak browser support make it a poor fit for anything the user must wait on. The design does not depend on it today.

## Where images could help

| Use | Value | Can be done without generation? |
| --- | --- | --- |
| Companion creature / plant variations and cosmetic drops ([gamification](../design/gamification.md)) | Novelty, surprise | Yes: a large hand-made or pre-generated asset set, combined procedurally |
| Interest-bridged analogy illustrations in [study mode](../design/study-mode.md) | Makes abstract ideas concrete | Partly: diagrams from code (Mermaid, SVG) are more accurate for most concepts |
| Visual cues for steps ("open the laptop") | Helps low working memory | Yes: icon library |
| Quest map art personalised to the quest | Motivation | Yes: themed templates |

Generated images are also a risk for accuracy (wrong diagrams in study content) and for sensory load (busy, high-contrast imagery), which the calm-by-default principle argues against.

## Options

| Option | Model size | Speed (planning numbers) | Runtime | Notes |
| --- | --- | --- | --- | --- |
| **SD-Turbo / SDXL-Turbo class (1 to 4 steps)** | ~1 to 2 GB (fp16 / quantized) | A few seconds per 512 px image on recent phone NPUs and Apple Silicon | `candle` (has Stable Diffusion and Turbo examples), stable-diffusion.cpp (Rust bindings exist), Core ML on iOS, ONNX Runtime | Best speed / quality for on-device |
| Stable Diffusion 1.5 + LCM / distilled variants | ~1 GB | Seconds with 4 to 8 steps | Same | Huge ecosystem of styles; older quality |
| FLUX.1-schnell, SD3-class | 6 GB+ | Too slow / large for phones | Desktop GPU only | Desktop-only "high quality" option at most |
| WebGPU in the browser (ONNX Runtime Web) | ~1 to 2 GB download | Tens of seconds on many machines | JS interop | Experimental; desktop Chrome only in practice |

Licences differ (the Turbo models carry non-commercial or community licences at time of writing); a commercial-use licence is a hard requirement and must be checked first.

## Recommendation

1. **MVP: no on-device generation.** Ship a curated asset set for the companion, drops and quest maps, combined procedurally for variety.
2. **Later, opt-in:** "creative pack" download on capable devices that generates new cosmetic variations overnight while charging, never on demand, never while the LLM is loaded.
3. **Never** for study content diagrams; use code-rendered diagrams with source citations.

## Spike (only if the opt-in pack is prioritised)

SD-Turbo class model via `candle` or Core ML: generate one 512 × 512 companion variation on the iPhone, mid-range Android and an M-series Mac.

Pass: under 10 s per image on the iPhone and Mac, under 30 s on the Android; under 1% battery per image; device stays below thermal throttling for a batch of 10; licence permits commercial use.
