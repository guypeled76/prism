# T3. Local LLM

[← Technology validation](README.md)

**Verdict: Go for light, frequent tasks; hybrid for heavy ones.** The chosen model family is **Gemma 4** (Apache 2.0): E2B and E4B on phones, 26B-A4B on desktop. Phones from about 2022 can run the E2B and E4B models at interactive speed, which is enough for most of what Prism asks of the LLM many times a day. They are not good enough, alone, for building a knowledge map from a 200-page course or for long tutoring sessions; those need the larger desktop model (Gemma 4 26B-A4B) or an opt-in cloud model.

## What the design asks of the LLM

The [architecture](../design/architecture.md) delegates many judgements to the LLM. Sorted by difficulty:

| Task | Frequency | Input size | Difficulty for a small model | Proposed home |
| --- | --- | --- | --- | --- |
| Classify captured text into event / task / quest | Many per day | Short | Low | On device |
| Acknowledge and tag a journal entry, infer state from it | Several per day | Short | Low to medium | On device |
| "Smaller": split a step | Many per day | Short | Low | On device |
| Nudge and reward copy, interest hooks | Several per day | Short | Low | On device |
| Bite cards and flashcards from one passage | Daily | 1 to 3 pages | Medium | On device |
| Grade a short free answer ("explain it back") | Daily | Short | Medium | On device, with a source passage |
| Build today's plan | Daily | Medium (profile, calendar, tasks) | Medium to high | On device, with deterministic rules doing the constraint work |
| Trait inference from a day's events | Nightly | Medium | Medium | On device, overnight, while charging |
| Socratic tutoring chat | Occasional | Long context | High | On device (E4B) for short sessions; 26B-A4B on desktop preferred |
| Knowledge map from a whole course | Rare | Very long | High | Gemma 4 26B-A4B on desktop (256K context) or opt-in cloud |

Key design change: move constraint-heavy reasoning (60% capacity rule, max three main tasks, windows, batching) into deterministic Rust code, and let the LLM rank and phrase. Small models are much better at choosing among valid options than at satisfying many constraints at once.

## Runtime options

| Runtime | Language | iOS | Android | Desktop | Web | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| **llama.cpp** via `llama-cpp-2` crate | C++ with Rust bindings | Metal | CPU, Vulkan, OpenCL (Adreno) | Metal, CUDA, Vulkan, CPU | via wllama (WASM) | Most proven on phones; GGUF models; grammar / JSON-schema constrained output |
| mistral.rs | Rust | Not a documented target | Not a documented target | Metal, CUDA, CPU | No | Pure Rust, strong desktop performance, tool calling built in; good desktop option |
| candle (Hugging Face) | Rust | Metal | CPU | Metal, CUDA | WASM (CPU) | Pure Rust; slower than llama.cpp on phones today |
| LiteRT-LM (Google AI Edge) | C++ / Kotlin / Swift | Yes | Yes (GPU / NPU) | Yes | No | Google's own runtime for Gemma 4 on device; best-tuned path for E2B / E4B on Android; no Rust bindings yet |
| WebLLM (MLC) | TypeScript / WebGPU | Safari WebGPU | Chrome WebGPU | Browser | Yes | Best browser option; called from Rust WASM through JS interop |
| Apple Foundation Models | Swift | iOS 26+, Apple Intelligence devices | No | macOS 26+ | No | ~3B model built into the OS; no download; guided generation for typed output |
| Gemini Nano (AICore / ML Kit GenAI; Chrome built-in AI) | Kotlin / JS | No | Selected devices | No | Chrome | No download; limited availability and API surface |

**Recommendation:** llama.cpp through `llama-cpp-2` as the primary runtime on all native targets; WebLLM on the web; platform models as a zero-download fallback; all behind one Rust `LanguageModel` trait. Keep mistral.rs as a desktop alternative and LiteRT-LM as an Android alternative to measure against llama.cpp in the spike.

## Model: Gemma 4

| Variant | Effective parameters | Context | Target | Use |
| --- | --- | --- | --- | --- |
| **E2B** | ~2.3B | 128K | Phones with 6 GB RAM, web | Default on-device model: capture, Smaller, nudge copy, cards |
| **E4B** | ~4.5B | 128K | Phones with 8 GB+ RAM, desktop fallback | Planning, grading, short tutoring, nightly trait inference |
| **26B-A4B** (MoE, ~4B active) | 26B total | 256K | M-series Macs and desktops with enough RAM | Knowledge maps from whole courses, long tutoring sessions |

Why Gemma 4: one family across every tier (same prompt format, tokenizer and evaluation set from phone to desktop), Apache 2.0 licence (no custom terms to review), built-in function calling and structured output, and long context on the small models. E2B and E4B also accept audio input (up to ~30 s), which opens voice capture without a separate speech-to-text model; to be evaluated, not assumed.

Distribution: GGUF quantizations for llama.cpp; LiteRT-LM packages from Google AI Edge for Android. Planning numbers to be replaced by spike measurements: E2B is ~2.6 GB as a LiteRT-LM package; E2B and E4B need roughly 5 GB of RAM at 4-bit according to Unsloth's guide, which is tight on 6 GB phones.

The previous candidate list (Qwen3, Llama 3.2, Phi-4-mini) stays as the fallback if Gemma 4 fails the spike.

## Reliability techniques for small models

- **Constrained decoding.** llama.cpp grammars generated from the same Rust types as the database make invalid JSON impossible, not just unlikely.
- **Narrow prompts.** One task per call with few-shot examples, instead of one agent loop with twelve tools.
- **Pre-computation.** The on-device buffer (pre-split next 20 steps, next 2 days of cards) is generated while idle and charging, so the Smaller button stays instant.
- **Prompt cache.** llama.cpp KV-cache reuse for the fixed system prompt and profile summary cuts time-to-first-token.
- **Evaluation set.** A fixed set of 200 to 500 real capture inputs, split requests and lessons, scored automatically, to compare models and catch regressions.

## Risks

| Risk | Mitigation |
| --- | --- |
| Quality gap vs cloud models in tutoring and planning | Rules in code; bigger model on desktop; opt-in cloud for heavy jobs |
| 2.5 to 5 GB download (E2B / E4B) before the app is useful | Ship with rule-based fallbacks for the first session; download in background; use the platform model while downloading |
| Battery and heat | Short generations; batch heavy jobs to charging windows; unload the model when idle |
| iOS memory limits kill the app | Request the increased memory limit entitlement; E2B only on 6 GB devices, E4B on 8 GB+ |
| Safety: crisis detection must not depend on model quality | Already deterministic in the design ([safety](../design/safety-privacy.md)); keep it that way |

## Spike

Embed llama.cpp in the Tauri shell on the mid-range Android and the iPhone. Run the evaluation set for capture classification and Smaller with Gemma 4 E2B and E4B, with JSON-schema constrained output. On Android, run the same set through LiteRT-LM to compare speed and battery against llama.cpp.

Pass: 100% schema-valid outputs; at least 90% correct classification on the evaluation set; Smaller returns in under 2 s on the mid-range Android; 30 minutes of mixed use costs under 5% battery.
