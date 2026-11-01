# 14. Journal

[← Design index](../design.md)

The journal is a free-form log where the user writes or says anything about their day; the app acknowledges each entry right away, learns from it quietly, and proposes profile changes only when several entries show the same pattern.

**Why it exists.** The rest of the app learns from behaviour: what was started, abandoned or finished, and when. That shows *what* happened but not *why*, and it sees nothing outside the app: friends, interests, good and bad days. The [trait profile](trait-profile.md) already lists reflections as an input; the journal is where they come from.

**Core rule: log now, propose later.** An entry is never answered with "Should I update your profile?". Per-entry analysis would turn a diary into an assessment, trigger demand avoidance, and over-read single moments. It would also bypass the trait profile's weekly cap and confirmation step. So each entry passes through four layers, from fast to slow:

| Layer | When | What happens | Visible to the user |
| --- | --- | --- | --- |
| Acknowledge | Immediately | One warm, short line. No analysis, no advice unless asked | The reply under the entry |
| State | Immediately, silently | An *inferred* state snapshot (mood, energy, social battery, sensory load) when the entry clearly implies one | As a guessed state with "tap to correct", like a skipped check-in |
| Evidence | Nightly job | The entry is tagged (people, activities, feelings, interests) and linked as candidate evidence to traits and supports it may bear on | In the entry's *Why?* view and in trait evidence lists |
| Insight | Weekly card | A trait or support change is proposed only when several entries (and/or behaviour) point the same way | In "What I learned about you", each insight linking to the entries behind it |

**Worked example.** Entry: *"Today was a good day, I learned with a friend and it was really nice."*

- **Acknowledge:** "Sounds like a really good one."
- **State:** mood good; social battery slightly used. Shown as an inferred snapshot.
- **Evidence:** tagged *studying, with a friend, positive*. Linked as candidate evidence for the *social* interest driver, *social / masking cost* (leaning "social energises"), and body doubling as a support.
- **Planning:** if a study quest is active, one optional offer at most: "Want to plan another session with your friend this week?" Never added without the user saying yes.
- **Insight (weeks later, only if repeated):** "You've written 4 times about studying with someone going well. Body doubling seems to work for you. Want me to suggest it more often?"

## How entries get in

- **The capture box.** The existing capture classifier gains a fifth class, *note*, alongside event, deadline, task and quest. There is one input everywhere, so the user never has to decide where a thought belongs. Mixed input is split: "good day, also need to email the professor" becomes a note chip and a task chip.
- **The journal screen.** A timeline of entries with a plain input at the top, for users who want a dedicated diary.
- **Wind-down prompt (optional).** The existing [wind-down nudge](nudges-and-colour.md) can add "Anything you want to remember about today?". Off by default for the reflective part, never repeated if ignored.
- **Formats.** Text, voice (transcribed on device; Gemma 4 E2B / E4B accept short audio, to be validated in [T3](../technology/local-llm.md)), a single emoji or mood face, or a photo with an optional caption.

## What the journal is used for

| Use | How it works | Why it fits AuDHD |
| --- | --- | --- |
| Trait evidence | Tagged entries feed nightly inference alongside behaviour events, within the existing bounded updates | Adds context and the user's own words to behavioural signals |
| Free-text check-in | An entry like "exhausted, too many people today" counts as a check-in when it clearly implies state | Some users find sliders harder than a sentence |
| Wins memory | Positive entries are kept as "wins". On low-battery days or after a setback, the app may resurface one: *"Last Tuesday you wrote: 'learned with a friend and it was really nice.'"* | Counters weak recall of past success and rejection-sensitive negativity bias |
| Agent memory | Entries are retrievable context for planning, nudges and tutoring ("you mentioned you like studying with Dana") | Externalises memory; the app remembers so the user does not have to |
| Pattern mirror | A monthly, optional card of observations across entries and state: "Your hardest days followed back-to-back social events 3 times this month" | Makes patterns visible without labelling them; the user decides what they mean |
| Planning hints | Entries can suggest framings or supports for upcoming tasks; offered once, never applied silently | Interest-based motivation, using what actually worked |

## Rules

- **Never interrogate.** At most one follow-up question per entry, and only when it changes something (e.g. "Should I add 'email the professor' as a task?"). No "How did that make you feel?".
- **Negative entries get support, not productivity.** A hard-day entry never triggers task suggestions; the reply is supportive, and the app may offer low-battery mode. Entries are not framed as friction points.
- **Crisis handling first.** Journal text passes through the same deterministic crisis checks as all free text ([safety](safety-privacy.md)) before the LLM sees it.
- **"Just for me".** Any entry can be marked private: stored and shown in the timeline, but never analysed, never used as evidence, never resurfaced. A global switch turns analysis off for the whole journal.
- **Deletion removes evidence.** Editing an entry re-runs its analysis; deleting it removes it from every trait's evidence list and from agent memory.
- **No game layer.** No XP, streaks or counts for writing, so journaling never becomes a chore. At most a quiet "Reflection" support badge, like other [supports](gamification.md).
- **Grounded insights only.** An insight must cite the entries behind it; the LLM may not infer traits from a single entry or from entries the user marked private.

## Data

A new `JournalEntry` entity (see [data model](data-model.md)): text and/or media reference, time, source (capture / journal / wind-down / voice), linked inferred `StateSnapshot`, extracted tags, analysis status, `private` flag, `win` flag. `TraitInsight.evidence[]` may reference journal entries as well as events.

## Open questions

- [ ] Should journal entries appear in the phase 4 coach / therapist share view? Likely valuable, but only with explicit per-entry sharing.
- [ ] How often may wins be resurfaced before it feels manipulative? Start with at most once per low-battery day, and measure.
- [ ] Is a monthly pattern mirror welcome, or should it only appear when the user asks for it?
