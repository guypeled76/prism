# 3. Evolving trait profile

[← Design index](../design.md)

The trait profile is a slowly changing model of *how this person's brain works*, built from onboarding, behaviour and reflections; it is shown as a character sheet of strengths and friction points, never as a diagnosis or deficit score.

**Presentation.** A "character sheet" with a constellation visual. Each trait is a star with a brightness (strength) and a short, plain-language description written by the LLM for this user. Challenges are framed as "friction points" with the workaround the app uses for each.

**Trait dimensions** (each scored 0 to 100 with a confidence value)

| Group | Trait | Strength pole | Friction pole | Evidence signals |
| --- | --- | --- | --- | --- |
| Activation | Task initiation | Starts readily | Stalls before starting | Time from task shown to task started |
| Activation | Transition cost | Switches easily | Switching drains | Abandonment right after context changes |
| Focus | Attention style | Flexible, wide | Monotropic, deep | Session length, interruption tolerance |
| Focus | Hyperfocus tendency | Deep flow, high output | Overruns needs and deadlines | Sessions far over planned length |
| Effort | Sustained effort on low-interest tasks | Can grind | Needs hooks | Completion rate by interest rating |
| Time | Time estimation | Accurate | Optimistic or blind | Estimated vs actual duration |
| Time | Urgency dependence | Self-starts early | Acts only near deadline | Work distribution before deadlines |
| Memory | Working memory load | Holds multi-step plans | Needs one step at a time | Success rate vs step size |
| Emotion | Frustration / RSD sensitivity | Resilient to setbacks | Setbacks stop momentum | Drop-off after a missed or failed task |
| Motivation | Interest drivers | Weights for novelty, challenge, urgency, passion, social | (profile, not a scale) | Which framing preceded completion |
| Motivation | Demand sensitivity | Direct instructions fine | Demands trigger avoidance | Response to imperative vs choice phrasing (A/B) |
| Sensory | Sensory sensitivity by channel (sound, light, touch, social) | Tolerant | Easily overloaded | Check-ins, environment notes |
| Energy | Social / masking cost | Social energises | Social drains | Energy after social events |
| Energy | Burnout vulnerability | Stable capacity | Capacity crashes | Rolling energy trend, sleep, load |
| Routine | Need for predictability | Enjoys spontaneity | Needs routines and warnings | Reaction to plan changes |
| Strengths | Special interests, pattern recognition, creativity, justice sense, honesty, crisis performance | Always strengths | (none) | Onboarding, reflections, completed projects |

**How the profile evolves**

1. **Seed.** A 5-minute onboarding: short, game-like scenarios ("You have an essay due in 3 days. Pick what usually happens.") rather than clinical questionnaires. Optional import of self-reported diagnoses and medication timing.
2. **Observe.** Every app event (task shown, started, abandoned, completed, rescheduled, check-in) is logged with context: time, state, framing used, step size. [Journal](journal.md) entries add the user's own words and the parts of life the app does not see.
3. **Infer.** A nightly LLM job reads the day's events and journal entries (except private ones) plus the current profile and proposes *trait updates* with evidence ("Started 4 of 5 tasks framed as challenges, 1 of 5 framed as importance → raise challenge weight"). Updates are small (bounded, e.g. at most 5 points per week) to keep the profile stable and trustworthy.
4. **Confirm.** Weekly, the user sees a short "What I learned about you" card and can accept, reject or edit each insight. Rejected inferences are stored so the LLM does not repeat them.
5. **Act.** The profile is part of the context for every planning and nudging decision (step size, framing, timing, reward type).

**Levelling.** Traits do not "level up" by changing who you are. What levels up are *skills and supports*: e.g. "Transition ritual: level 3, used 21 times". This rewards building scaffolding instead of implying the user must become neurotypical.
