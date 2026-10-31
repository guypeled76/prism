# T1. Rust as the main language

[← Technology validation](README.md)

**Verdict: Go.** Rust fits Prism's local-first, on-device-AI design well: one core library can compile to iOS, Android, desktop and WebAssembly, and every AI runtime Prism needs has a usable Rust binding. The cost is a thin layer of Swift and Kotlin for OS features, and a smaller hiring pool than TypeScript or Kotlin.

## What the design needs from the language

- Share all domain rules (planning constraints, XP arithmetic, FSRS intervals, momentum, nudge caps, plan diffs) across phone, desktop and web, so behaviour is identical everywhere.
- Run a local database and a conflict-safe sync engine on every platform, including the browser.
- Call native AI runtimes (llama.cpp, ONNX Runtime) with low overhead and tight memory control.
- Respond to any tap in under 100 ms locally ([architecture](../design/architecture.md)).

## Findings

| Need | Rust option | Maturity | Notes |
| --- | --- | --- | --- |
| Compile one core for all targets | `aarch64-apple-ios`, `aarch64-linux-android`, desktop, `wasm32-unknown-unknown` | High | Standard Rust targets; Android via `cargo-ndk`, iOS via XCFramework |
| Bindings to Swift / Kotlin | UniFFI (Mozilla), or Tauri's plugin bridge | High | UniFFI is used in production by Firefox and Matrix clients |
| Local database | `rusqlite` (native); SQLite WASM with OPFS storage in the browser | High native, medium web | Web persistence needs a spike, see [app-and-web](app-and-web.md) |
| Spaced repetition | `fsrs` crate (from the Anki / open-spaced-repetition project) | High | Same algorithm Anki uses |
| LLM inference | `llama-cpp-2` (llama.cpp bindings), `mistral.rs`, `candle` | Medium to high | See [local-llm](local-llm.md) |
| ONNX models (TTS, embeddings) | `ort` (ONNX Runtime bindings) | High | CoreML and NNAPI / QNN execution providers on phones |
| CRDT / sync | `automerge`, `yrs` (Yjs port), or a custom op log with hybrid logical clocks | High | See [google-sync](google-sync.md) |
| HTTP, OAuth | `reqwest` (works in WASM), `oauth2` | High | Google sign-in on mobile uses the native SDK through a plugin |
| JSON schema validation of LLM output | `serde` + `schemars` + `jsonschema` | High | Schemas generated from the same Rust types the database uses |

## Risks and mitigations

| Risk | Mitigation |
| --- | --- |
| OS features with no Rust API: exact alarms, lock-screen notification actions, widgets, Live Activities, watch apps, calendar access, Focus / DND integration | Accept a small native layer per platform (Swift, Kotlin), kept free of business logic; Tauri plugins already cover notifications and some of the rest |
| Compile times and binary size with AI runtimes linked in | Feature flags per target; ship the image model runtime only where it is enabled |
| Smaller hiring pool; slower UI iteration than React | Keep the UI layer thin; consider a TypeScript UI inside Tauri if Rust UI velocity becomes a problem (core stays Rust) |
| C++ dependencies (llama.cpp, ONNX Runtime) complicate WASM builds | Web uses a separate inference path (WebGPU runtimes), behind the same Rust trait |

## Spike

Build `prism-core` with a `Task`, `Step` and `Event` model, FSRS scheduling and SQLite storage. Compile and run its test suite on iOS simulator, Android emulator, macOS and `wasm32` in a headless browser.

Pass: same tests green on all four targets; core library adds under 10 MB to the app before models; a list of 1,000 tasks loads in under 50 ms on the mid-range Android.
