# T7. Cloud LLM for heavy jobs

[← Technology validation](README.md)

**Verdict: Go, as an opt-in tier for a short list of heavy jobs.** On-device Gemma 4 ([T3](local-llm.md)) handles the many small calls Prism makes every day. It cannot reliably turn a 200-page course into a knowledge map, analyse a stack of past exams, or generate a semester of lessons at good quality on a phone. For those jobs Prism calls a cloud model, Claude by default, through a thin stateless relay. The relay stores nothing, and each job sends only the material it needs. The user switches the cloud tier on per job or once in settings. Without it the app still works, more slowly and with lower quality, on the desktop model or with an overnight job on the phone.

## Which jobs go to the cloud

Only jobs that are rare, large and hard. Everything frequent, short or personal stays on the device.

| Job | Why it needs a big model | Input | Proposed model | Mode |
| --- | --- | --- | --- | --- |
| Knowledge map from a whole course (topics → concepts → facts, prerequisites, source passages) | Needs the whole course in context at once to find structure and prerequisites | 100K to 600K tokens (PDFs, slides, syllabus) | Claude Opus 5.5 | Background job; batch when not urgent |
| Exam weighting from past exams and the syllabus | Comparing many documents and reasoning about emphasis | 20K to 200K tokens | Claude Opus 5.5 | Part of the map job |
| Bulk lesson generation (bite cards, flashcards, worked examples for every concept) | Many calls with consistent quality and correct citations | One concept plus its source passages per call | Claude Sonnet 5.5 (Haiku 5.5 for flashcards) | Batch, overnight |
| Handwritten notes and diagram-heavy pages | Vision quality on messy photos and figures | Images | Claude Sonnet 5.5 | Per upload |
| Re-mapping after new material is added | Merging new content into an existing map without breaking IDs | Existing map plus new material | Claude Opus 5.5 | Background job |
| Long tutoring sessions (escalation) | Multi-turn reasoning past what E4B handles well | One concept, its passages, the chat so far | Claude Sonnet 5.5 | Interactive, opt-in per session |

Never sent to the cloud: journal entries, trait scores, state snapshots, calendar, tasks, or the event log. Lecture audio is transcribed on the device; Claude does not take audio input.

## Why Claude

- **Long context.** Opus 5.5, Sonnet 5.5 and Haiku 5.5 all have a 1M-token context window, so most whole courses fit in one request with no chunk-and-merge step. PDFs are accepted directly (up to 600 pages and 32 MB per request), including figures and scanned pages.
- **Citations.** The API can cite exact passages (page ranges for PDFs). This fits the study-mode rule that every question cites its source.
- **Structured outputs.** Responses can be constrained to a JSON schema generated from the same Rust types as the local database, as with llama.cpp grammars on the device.
- **Tiers in one family.** Opus for one-off structure, Sonnet for bulk generation and tutoring, Haiku for cheap high-volume cards. All use the same prompt format.
- **Batch API at 50% off.** Map building and lesson generation are not urgent and can run as batches.
- **Data terms.** Zero data retention can be arranged for API traffic, and API data is not used for training. Claude Fable 5.1 is excluded because it requires 30-day retention unless Anthropic authorises otherwise; Prism does not need it.

The client stays provider-neutral: the cloud tier is another implementation of the same Rust `LanguageModel` trait, so the provider can change without touching study mode.

**Current pricing** (Anthropic API, per million tokens, checked Oct 2026; re-check before the spike):

| Model | Input | Output | Cached input read | Batch |
| --- | --- | --- | --- | --- |
| Claude Opus 5.5 (`claude-opus-5-5`) | $4.00 | $20.00 | $0.20 | 50% off |
| Claude Sonnet 5.5 (`claude-sonnet-5-5`) | $2.00 | $10.00 | $0.20 | 50% off |
| Claude Haiku 5.5 (`claude-haiku-5-5`) | $0.10 (≤100K prompt) | $0.50 | see pricing page | 50% off |

## How a job runs without a Prism database

The local-first stack ([T4](google-sync.md)) has no server copy of the user's data, so the cloud tier must not bring one back.

```mermaid
sequenceDiagram
    participant D as Device (prism-core)
    participant R as Prism relay (stateless)
    participant C as Claude API
    participant G as User's Google Drive
    D->>D: Extract text, OCR, chunk; compute cost estimate
    D->>D: User confirms "Build map in the cloud?" (first time or per job)
    D->>R: Job: material + task prompt + JSON schema (TLS)
    R->>R: Check account, quota and size limits
    R->>C: Messages / Batch request (zero data retention)
    C-->>R: Map JSON with citations
    R-->>D: Result (relay keeps no copy)
    D->>D: Validate against schema, store locally
    D->>G: Sync as usual (encrypted)
```

- **Relay, not direct calls.** Shipping an API key in the app would leak it. A small stateless relay holds the key, authenticates the user (a Google sign-in token or a signed device token), enforces per-user quotas and cost caps, and forwards the request. It writes no request or response bodies to disk or logs, only counts for billing and abuse control. It can run as a serverless function.
- **Long jobs.** Map building can take minutes. The device submits the job, the relay returns a job ID, and the device polls for the result or gets it by push notification. For batches, the relay keeps only the batch ID and the user ID until the results are collected, then deletes both.
- **Bring your own key (optional).** Power users can paste their own Anthropic API key and skip the relay. The key is stored in the OS keychain and never synced.
- **Prompt injection.** Uploaded material is untrusted ([architecture guardrails](../design/architecture.md)). Cloud jobs get no tools, only a structured output, so injected text can at most produce a bad map. The device validates the result and shows it to the user, who can correct it.

## Cost per user

Rough estimates to plan with. Replace them with measured numbers from the spike.

| Job | Tokens (in / out) | Model | Cost, standard | Cost, batch |
| --- | --- | --- | --- | --- |
| Map one 200-page course | ~300K / ~25K | Opus 5.5 | ~$1.70 | ~$0.85 |
| Lessons for ~150 concepts | ~150 × 4K in / 1.5K out | Sonnet 5.5 | ~$3.45 | ~$1.70 |
| Flashcards for ~150 concepts | ~150 × 3K / 0.8K | Haiku 5.5 | ~$0.10 | ~$0.05 |
| Tutoring session, 10 turns | ~60K / ~5K, mostly cached | Sonnet 5.5 | ~$0.10 | n/a |

A student with two courses a term who uses batch for everything costs about $5 to $8 per term in model fees. Lesson generation, not the map, is the biggest cost. That is the first job to move back onto the device (Gemma 4 E4B, overnight) if cost becomes a problem, with the cloud kept for the map itself. This answers the LLM cost question in the [roadmap](../design/roadmap.md): cost is bounded per course rather than per day of use, which suits a subscription with a fair-use cap.

## Privacy and consent

- **Opt-in, explained in plain words.** On first use: "This course is big. Build the map in the cloud? Your files are sent to Anthropic to process and are not stored or used for training. You can turn this off any time." There are three choices: *Yes*, *Yes, don't ask again*, and *No, do it on my device (slower)*.
- **Minimum context.** A job carries the material and the task. It carries no profile, no name, and no data about the user's state. Tutoring escalation sends the concept, its passages and the current chat only.
- **Visible log.** Settings → Data lists every cloud job: date, what was sent (file names and size), and the model used.
- **Region.** Where the market needs it, requests can be pinned to a region (`inference_geo`).
- **Changes a promise.** "Your data never leaves your devices or your Drive" becomes "…except study material you choose to process in the cloud". The [safety and privacy](../design/safety-privacy.md) copy and the store listings must say this.

## Fallbacks when the cloud tier is off or offline

1. **Desktop model.** Gemma 4 26B-A4B on a Mac or PC (256K context) builds the map, splitting very large courses by chapter.
2. **Phone, overnight.** E4B builds the map chapter by chapter while charging, then merges chapters in a second pass. Quality is lower, so the result is labelled "draft map" and the user is invited to review it.
3. **Manual seed.** The syllabus headings become the top of the map straight away, so planning can start before any LLM work finishes.

## Risks

| Risk | Mitigation |
| --- | --- |
| Breaks the privacy promise for users who expected fully local | Opt-in per job, clear copy, visible job log, local fallbacks that work |
| The relay becomes a de facto server with user data | Stateless by design; no body logging; open to audit; batch IDs deleted after collection |
| Runaway cost (huge uploads, abuse) | Size limits per job, per-user monthly token cap at the relay, cost estimate shown before the job |
| Vendor lock-in or price change | Provider behind the `LanguageModel` trait; evaluation set reused to qualify an alternative |
| Model refuses or misreads unusual material | Check refusal stop reasons, retry with a fallback model, otherwise fall back to the local path and tell the user |
| Bad map from injected or messy material | No tools, schema-validated output, source citations, user can edit the map |

## Spike

Take three real courses (a 200-page textbook PDF, a slide deck plus syllabus, and photographed handwritten notes) and two past exams. Build the knowledge map with Claude Opus 5.5 through a minimal relay, with structured output and citations, in standard and batch mode. Build the same maps with Gemma 4 26B-A4B on an M-series Mac.

Pass: the cloud map is schema-valid every time; at least 90% of concepts have a citation that a reviewer confirms supports them; reviewers rate the cloud map clearly better than the local map on at least 2 of 3 courses (otherwise drop the cloud tier); measured cost per course is under $2 in batch mode; the relay keeps no request or response content (checked by inspecting its storage and logs).
