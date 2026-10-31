# 8. Nudges, notifications and colour

[← Design index](../design.md)

Prism sends few, well-timed, choice-based nudges (default cap: 6 per day) whose timing and wording are chosen by the LLM from the user's state and history, and it treats notification fatigue as a failure mode to measure, not a cost of doing business.

**Nudge types**

| Type | Trigger | Example copy | Default |
| --- | --- | --- | --- |
| Start nudge | Planned window opens and user is idle | "Got 5 minutes? Next tiny step: open the slides." (with Start / Later buttons) | On |
| Transition warning | 10 and 3 minutes before a fixed event | "Dentist in 10 min. Good moment to save and grab your keys." | On |
| Leave-now alert | Travel time to event (location optional) | "Leave in 5 minutes to arrive calm." | On |
| Surface check | Planned session length + 20% exceeded | "You've been deep for 70 min. Water? Keep going?" | On |
| Body check | Long gap since last food/water log, before study blocks | "Quick fuel check before you start?" | Off (opt-in) |
| Runway nudge | Deadline runway crosses a threshold | "Essay due in 5 days. One 10-min step today keeps it easy." | On |
| Wind-down | Evening, user-set time | "Want to set tomorrow's first step? It makes mornings easier." | On |
| Welcome back | No activity for 3+ days | "Hey, good to see you. Here's one small thing, no backlog." | On |

**Adaptive rules**

- The LLM learns which nudge times and phrasings lead to a Start (bandit-style experiment per user) and stops sending what is ignored.
- Ignored nudges reduce frequency automatically; three ignored in a row triggers a "too many?" check instead of another nudge.
- Quiet hours, focus-mode integration (iOS Focus / Android DND) and a "pause everything for X hours" switch.
- Every nudge is actionable from the lock screen (Start / Snooze 15 / Skip) so the user does not have to open the app and get lost.
- Escalation is optional and user-designed (e.g. a louder alarm only for leave-now on flagged events).
- Channels beyond phone, opt-in: smartwatch haptic taps (low-distraction), widget, optional message to an accountability buddy.

**Colour system.** Colour encodes *state and category*, never failure.

| Use | Colours | Notes |
| --- | --- | --- |
| App mood (current state) | Flow = deep teal, Steady = soft blue, Low-battery = warm amber, Maintenance = lavender | Whole UI tint shifts gently so the user sees their state at a glance |
| Life areas (user-chosen) | Up to 6 hue tags, e.g. study, work, home, health, social, fun | Calendar blocks and task chips; user can rename and recolour |
| Energy cost | 1 to 3 dots, not colour | Avoids overloading hue meaning |
| Time runway | Bar that fills with a single calm hue as time passes | No red; urgency shown by bar length, not alarm colour |
| Rewards | Saturated, brief colour bursts | The only bright colour in the app, so it is distinctive |

Accessibility: WCAG AA contrast, colour-blind-safe palettes, every colour paired with an icon or label, a low-stimulation theme (greyscale + one accent), reduced motion, and dyslexia-friendly font option.
