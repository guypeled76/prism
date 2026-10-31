# T2. One codebase for the app and the website

[← Technology validation](README.md)

**Verdict: Go, with a reduced web tier.** A Rust codebase can ship as an iOS, Android and desktop app and as a website. The website cannot match the app on the things Prism relies on most (reliable alarms, background re-planning, large local models), so it should be a full-featured companion for planning, study and desktop use, while the phone app stays the primary product.

## Framework options

| Option | How it works | Mobile | Web | Strengths | Weaknesses |
| --- | --- | --- | --- | --- | --- |
| **Tauri 2 + Leptos (or Dioxus web) UI** | Rust core; UI rendered in the system webview on mobile and desktop; the same UI and core compiled to WASM for the website | Yes (iOS, Android since Tauri 2.0) | Yes, the UI is already web tech | Mature plugin system with official notification, deep-link, biometric, file-system plugins; native code via Swift/Kotlin plugins | Two-process model (webview + Rust) on mobile; IPC boundary to design |
| **Dioxus 0.7** | One Rust codebase; `dx serve --platform web / desktop / ios / android` | Yes (webview renderer; native renderer experimental) | Yes, first-class | Pure Rust end to end; hot reload; fullstack server functions | Smaller mobile plugin ecosystem; more native glue to write ourselves |
| **Slint / egui** | Native-drawn Rust UI | Yes | Yes (canvas) | Very fast, small | Canvas-rendered web loses accessibility, text selection and screen-reader support; a poor fit for an accessibility-first app |
| **Rust core + native UIs (SwiftUI, Compose) + web UI** | UniFFI core shared; three UIs | Best native feel, widgets, watch | Separate web UI | Best platform integration | Three UIs to build and keep in sync; against the goal of one codebase |

**Recommendation:** Tauri 2 shell for iOS, Android and desktop, with a Rust UI (Leptos) and the shared `prism-core`; the same UI and core compiled to WASM and deployed as a Progressive Web App. Re-evaluate Dioxus at the spike if Tauri's IPC or webview performance gets in the way; the core is unaffected by that choice.

Reasons: accessibility (real DOM, screen readers, dyslexia fonts, reduced motion all work as on the web), the most mature mobile plugin story among Rust options, and the UI is literally the same code on the website.

## What works where

| Capability | iOS / Android app | Desktop app | Website (PWA) |
| --- | --- | --- | --- |
| Now card, planning, study, calendar views | Yes | Yes | Yes |
| Local database, offline use | SQLite file | SQLite file | SQLite WASM on OPFS; browser may evict storage unless persistent storage is granted |
| Local notifications with Start / Snooze buttons | Yes (native plugin) | Yes | Only while the browser is running; no exact timing; iOS web push requires the PWA be installed to the home screen |
| Exact alarms, leave-now alerts | Yes (Android exact alarm permission; iOS time-sensitive notifications) | Yes | **No** |
| Background nightly jobs | Yes, OS-scheduled | Yes, while running | **No**, runs when the tab is open |
| Device calendar import | Native plugin (EventKit, CalendarContract) | Via Google Calendar API | Google Calendar API only |
| Local LLM | llama.cpp, Metal / Vulkan / CPU | llama.cpp | WebGPU runtime (WebLLM, wllama); smaller models; first load is a large download |
| Local TTS | Yes | Yes | Yes (ONNX Runtime Web / transformers.js); browser speech API as fallback |
| Local image generation | Optional, recent phones only | Yes | Experimental on WebGPU desktops only |
| Widgets, watch, Live Activities | Native code only | n/a | No |

## Risks

| Risk | Mitigation |
| --- | --- |
| Web users expect the reminders that make Prism work | Website sends nudges only when open, and offers to install the app; the design should treat web as "desk mode" |
| Browser storage eviction loses unsynced data | Request persistent storage; sync to Drive promptly; warn when persistence is denied |
| Webview performance on low-end Android | Keep animations CSS-only and short (already a design rule); profile in the spike |
| WASM bundle size with core + UI | Split the AI runtime into a lazily loaded module; target under 3 MB compressed for first load |

## Spike

Tauri 2 + Leptos app with a Now card that reads from `prism-core`, built for iOS, Android, macOS and web. Schedule a local notification with Start / Snooze actions from Rust; handle the action back in Rust.

Pass: one UI source tree builds for all four targets; notification fires within 1 minute of schedule on both phones with the app killed; web build first load under 3 MB compressed; tap-to-render under 100 ms on the mid-range Android.
