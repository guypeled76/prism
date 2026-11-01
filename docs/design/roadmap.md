# 12. MVP scope, roadmap and open questions

[← Design index](../design.md)

The MVP proves one loop: *capture or upload → LLM plans tiny steps → Now card → instant reward*, centred on the exam-study use case, because that has a clear deadline, measurable learning and the highest pull for the primary persona.

| Phase | Scope | Exit criteria |
| --- | --- | --- |
| 1. MVP | Onboarding seed profile; check-in; capture box (including journal notes and journal timeline); calendar import; quests → tasks → steps; Now card with Smaller / Not now; study mode (PDF upload, knowledge map, bite cards, flashcards, daily plan); effort XP + momentum; core nudges; local-first sync | 30 testers complete one exam prep cycle; most report lower start friction |
| 2. Learning loop | Nightly trait inference + weekly insight cards (using journal evidence); journal wins memory and pattern mirror; adaptive nudge timing; explain-it-back and tutor chat; companion; plan diffs | Trait insights accepted at a high rate; nudge ignore rate falling |
| 3. Supports | Body doubling rooms; smartwatch; medication window; maintenance mode; audio lessons; real-world rewards | Retention holds through low-energy weeks |
| 4. Ecosystem | Accountability buddy; coach / therapist share view (user-controlled); integrations (Notion, Google Classroom, Canvas) | Partner pilots |

**Success metrics** (no time-in-app metric)

- Initiation rate: share of shown steps that get started within 10 minutes.
- Median time from Now card shown to Start.
- Study outcome: self-reported exam result vs expectation; concept mastery at exam date.
- 30-day return rate, *including after a gap* (return after absence is a key success).
- Wellbeing: short weekly self-report on overwhelm and self-compassion.
- Notification health: ignore rate and opt-out rate.

**Open questions**

- [ ] Platform: proposed answer is a shared Rust core with a Tauri 2 shell for app and web, plus thin Swift / Kotlin plugins for alarms, widgets and watch; pending spikes in [technology validation](../technology/README.md).
- [ ] Should the companion be a creature, a plant, or user-chosen? Needs user testing for sensory and "cringe" tolerance.
- [ ] How much autonomy should the agent have to reschedule without asking? Likely a user setting with a conservative default.
- [ ] Business model that avoids engagement incentives: subscription, student pricing, or institutional (university disability services) licences?
- [ ] Which clinical / lived-experience advisors validate the knowledge base and trait wording?
- [ ] LLM cost per active user per month at the target model mix, and the cap that keeps it sustainable. Largely replaced by on-device cost (download size, battery) if the [local LLM](../technology/local-llm.md) validation passes; still applies to opt-in cloud jobs, estimated at $5 to $8 per student per term in [T7](../technology/cloud-llm.md).
- [ ] Encryption key recovery for Drive sync: passphrase vs keychain + recovery code (see [Google sync](../technology/google-sync.md)).
- [ ] Local-only mode for users without a Google account?
- [ ] Live body-doubling rooms: worth the moderation and safety cost in phase 3?
- [ ] Journal: open questions on sharing, resurfacing frequency and the pattern mirror are tracked in [journal](journal.md).
