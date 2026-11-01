# Prism — AuDHD Executive-Function Companion: Design

Oct 31, 2026 · @Guy Peled

## 1. Overview

Prism is a mobile companion that acts as an external executive function for AuDHD and ADHD adults: it decides *what to do next, how small to make it, and how to make starting it feel rewarding*, so the user does not have to. The app is a thin, calm client over a synced database; nearly all reasoning (planning, breaking down, adapting, tutoring, profiling) is delegated to an LLM agent.

**Problem.** Most productivity apps assume a working executive system: they store tasks and expect the user to plan, prioritise, start and sustain effort. For AuDHD people those are exactly the failing steps. The result is the familiar cycle: install, over-organise, feel shame at the red overdue list, abandon.

**Target users.** Adults (16+) with diagnosed or self-identified ADHD, autism, or both (AuDHD). Primary persona: a student or knowledge worker facing a deadline (an exam, a project) who knows *what* matters but cannot reliably get from intention to action.

**Core jobs the app does for the user**

1. Turn a vague goal or a pile of material into a sequence of tiny, concrete, startable steps.
2. Pick the single next step that fits the user's *current* energy, sensory load and time.
3. Make starting and finishing feel good immediately, not someday.
4. Learn the user over time and adapt, without ever shaming them.
5. When the goal is learning, teach the material directly inside the app.

**Design principles**

- **One thing at a time.** The home screen shows one next action, not a list. Lists exist but are one tap away.
- **Externalise, don't instruct.** The app holds time, memory and sequence so the user's working memory does not have to.
- **Interest is the fuel.** Use the interest-based nervous system (novelty, challenge, urgency, passion) deliberately, rather than importance-based motivation.
- **Predictable surface, novel content.** The autistic side needs a stable, predictable UI and routines; the ADHD side needs novelty. Keep layout and rules fixed; vary the content, framing and rewards.
- **No shame, ever.** No red overdue counters, no broken-streak punishments, no guilt copy. Missed is just rescheduled.
- **Low demand language.** Offer, don't command ("Want to try 5 minutes of X?") to reduce demand-avoidance responses.
- **Calm by default.** Minimal motion, muted palette, sensory settings first-class. Rewards are bright but brief.
- **The user is the authority.** Every inference the LLM makes about the user is visible, explained and editable.

## Contents

The design is split into linked documents, one per topic, so each can be read and changed on its own.

| # | Document | What it covers |
| --- | --- | --- |
| 1 | Overview (this page) | Problem, users, core jobs, design principles |
| 2 | [Knowledge foundation](design/knowledge-foundation.md) | The ADHD, autism and AuDHD research constructs that every feature maps to |
| 3 | [Evolving trait profile](design/trait-profile.md) | Slow-changing model of how the user's brain works, and how it learns |
| 4 | [Current state profile](design/state-profile.md) | Fast-changing energy, focus and sensory snapshot; state-aware modes |
| 5 | [Planning system](design/planning.md) | Quest → Milestone → Task → Step, capture, calendar, Now card, re-planning |
| 6 | [Study mode](design/study-mode.md) | Material ingestion, knowledge map, micro-lessons, exam schedules |
| 7 | [Gamification and reward design](design/gamification.md) | Effort XP, momentum, companion, distraction budget, anti-shame rules |
| 8 | [Nudges, notifications and colour](design/nudges-and-colour.md) | Nudge types, adaptive rules, colour system, accessibility |
| 9 | [Architecture](design/architecture.md) | Local-first client, sync, LLM agent, code vs LLM split |
| 10 | [Data model](design/data-model.md) | Core entities and who writes them |
| 11 | [Safety, privacy and ethics](design/safety-privacy.md) | Medical boundaries, crisis handling, data control |
| 12 | [MVP scope, roadmap and open questions](design/roadmap.md) | Phases, success metrics, open decisions |
| 13 | [User stories](design/user-stories.md) | Who wants what, and how the mobile app and the website serve each story |
| 14 | [Technology validation](technology/README.md) | Rust, app + web, local LLM, Google Drive sync, local TTS, local image generation |
