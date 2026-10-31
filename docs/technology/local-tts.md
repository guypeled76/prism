# T5. Local text to speech

[← Technology validation](README.md)

**Verdict: Go.** Small neural TTS models now sound natural enough for lessons and nudges, run faster than real time on phone CPUs, and have Rust-usable runtimes on every target including the browser.

## Where the design uses speech

- **Audio mode** lessons in [study mode](../design/study-mode.md): listen and answer by voice, for walking, stimming or low visual tolerance.
- Reading the Now card, bite cards and transition warnings aloud on request.
- A body-doubling companion voice (later phase).

Speech must be calm, clear, adjustable in speed, and must never be the only channel (captions always shown).

## Options

| Engine | Model size | Quality | Languages | Runtime | Licence |
| --- | --- | --- | --- | --- | --- |
| **Kokoro-82M** | ~80 MB (quantized) to ~330 MB | High, natural prosody | English strongest; several others | ONNX via `ort`; sherpa-onnx; kokoro-js on the web | Apache 2.0 |
| **Piper** | 20 to 100 MB per voice | Good, slightly robotic | Many (30+), many voices | ONNX via `ort`; sherpa-onnx | MIT (engine); voice licences vary |
| sherpa-onnx (wrapper for Kokoro, Piper, Matcha, others) | per model | per model | per model | C API with Rust bindings (`sherpa-rs`); iOS, Android, desktop, WASM | Apache 2.0 |
| OS voices (AVSpeechSynthesizer, Android TextToSpeech, Web Speech API) | none | Varies, often good on iOS | Many | Native plugin / browser API | n/a |

**Recommendation:** Kokoro through `ort` (or sherpa-onnx if it saves integration time) as the default voice; Piper voices for languages Kokoro lacks; OS voices as the zero-download fallback and for users who prefer a familiar system voice.

## Notes

- Grapheme-to-phoneme conversion (espeak-ng in most pipelines) is the fiddly part to package for iOS and WASM; sherpa-onnx bundles it.
- Streaming by sentence keeps time-to-first-audio short even on slow devices.
- Voice input (the other half of audio mode and voice capture) is a separate question; Whisper-class models (whisper.cpp, ~40 to 150 MB for tiny / base) or OS speech recognition are the candidates, and should get their own validation.

## Risks

| Risk | Mitigation |
| --- | --- |
| Pronunciation of course-specific terms (stats, medicine) | User-editable pronunciation dictionary; fall back to OS voice |
| Sensory sensitivity to certain voices | Several voices, speed and pitch controls, preview before choosing |
| Download size per language | One default voice bundled or downloaded with the LLM; others on demand |

## Spike

Kokoro via `ort` reading three bite cards on the old Android, the iPhone and Chrome.

Pass: first audio within 500 ms of tap; synthesis faster than real time on the old Android; listening panel rates it at least as pleasant as the OS voice.
