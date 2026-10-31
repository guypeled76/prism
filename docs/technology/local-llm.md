# T3. Local LLM

[← Technology validation](README.md)

**Verdict: Go for light, frequent tasks; hybrid for heavy ones.** Phones from about 2022 can run 1.5B to 4B parameter models at interactive speed, which is enough for most of what Prism asks of the LLM many times a day. They are not good enough, alone, for building a knowledge map from a 200-page course or for long tutoring sessions; those need a larger desktop model or an opt-in cloud model.

## What the design asks of the LLM

The [architecture](../design/architecture.md) delegates many judgements to the LLM. Sorted by difficulty:

| Task | Frequency | Input size | Difficulty for a small model | Proposed home |
| --- | --- | --- | --- | --- |
| Classify captured text into event / task / quest | Many per day | Short | Low | On device |
| "Smaller": split a step | Many per day | Short | Low | On device |
| Nudge and reward copy, interest hooks | Several per day | Short | Low | On device |
| Bite cards and flashcards from one passage | Daily | 1 to 3 pages | Medium | On device |
| Grade a short free answer ("explain it back") | Daily | Short | Medium | On device, with a source passage |
| Build today's plan | Daily | Medium (profile, calendar, tasks) | Medium to high | On device, with deterministic rules doing the constraint work |
| Trait inference from a day's events | Nightly | Medium | Medium | On device, overnight, while charging |
| Socratic tutoring chat | Occasional | Long context | High | On device for short sessions; bigger model preferred |
| Knowledge map from a whole course | Rare | Very long | High | Desktop model or opt-in cloud |

Key design change: move constraint-heavy reasoning (60% capacity rule, max three main tasks, windows, batching) into deterministic Rust code, and let the LLM rank and phrase. Small models are much better at choosing among valid options than at satisfying many constraints at once.

## Runtime options

| Runtime | Language | iOS | Android | Desktop | Web | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| **llama.cpp** via `llama-cpp-2` crate | C++ with Rust bindings | Metal | CPU, Vulkan, OpenCL (Adreno) | Metal, CUDA, Vulkan, CPU | via wllama (WASM) | Most proven on phones; GGUF models; grammar / JSON-schema constrained output |
| mistral.rs | Rust | Not a documented target | Not a documented target | Metal, CUDA, CPU | No | Pure Rust, strong desktop performance, tool calling built in; good desktop option |
| candle (Hugging Face) | Rust | Metal | CPU | Metal, CUDA | WASM (CPU) | Pure Rust; slower than llama.cpp on phones today |
| WebLLM (MLC) | TypeScript / WebGPU | Safari WebGPU | Chrome WebGPU | Browser | Yes | Best browser option; called from Rust WASM through JS interop |
| Apple Foundation Models | Swift | iOS 26+, Apple Intelligence devices | No | macOS 26+ | No | ~3B model built into the OS; no download; guided generation for typed output |
| Gemini Nano (AICore / ML Kit GenAI; Chrome built-in AI) | Kotlin / JS | No | Selected devices | No | Chrome | No download; limited availability and API surface |

**Recommendation:** llama.cpp through `llama-cpp-2` as the primary runtime on all native targets; WebLLM on the web; platform models as a zero-download fallback; all behind one Rust `LanguageModel` trait. Keep mistral.rs as a desktop alternative to measure in the spike.

## Candidate models (to be re-checked at spike time)

Small open-weight instruction models with tool or JSON output support, in the 1.5B to 4B range at 4-bit quantization: Qwen3 (1.7B, 4B), Gemma 3 / Gemma 3n (E2B, E4B), Llama 3.2 (3B), Phi-4-mini. Licences differ (Apache 2.0 for Qwen3; custom terms for Gemma and Llama) and must be reviewed before shipping.

Expected on a 2023 to 2025 flagship phone at 4-bit: roughly 15 to 40 tokens per second generation for 1.5B to 4B models; a 3B model needs about 2 GB of RAM. Mid-range phones are roughly half that speed. These are planning numbers from public benchmarks, to be replaced by spike measurements.

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
| 1 to 2.5 GB download before the app is useful | Ship with rule-based fallbacks for the first session; download in background; use the platform model while downloading |
| Battery and heat | Short generations; batch heavy jobs to charging windows; unload the model when idle |
| iOS memory limits kill the app | Request the increased memory limit entitlement; prefer 1.5B to 3B on 6 GB devices |
| Safety: crisis detection must not depend on model quality | Already deterministic in the design ([safety](../design/safety-privacy.md)); keep it that way |

## Spike

Embed llama.cpp in the Tauri shell on the mid-range Android and the iPhone. Run the evaluation set for capture classification and Smaller with two models (one ~1.7B, one ~4B), with JSON-schema constrained output.

Pass: 100% schema-valid outputs; at least 90% correct classification on the evaluation set; Smaller returns in under 2 s on the mid-range Android; 30 minutes of mixed use costs under 5% battery.
