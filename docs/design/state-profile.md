# 4. Current state profile

[← Design index](../design.md)

The current state is a fast-changing snapshot (hours, not weeks) that decides *what kind* of task the user can do right now; a good plan for a depleted brain is a different plan.

**State dimensions**

| Dimension | Scale | Captured by | Used for |
| --- | --- | --- | --- |
| Energy (spoons) | 0 to 10 spoons left today | Check-in, decays with completed tasks by their cost | Filtering tasks by energy cost |
| Focus capacity | Scattered / Okay / Locked-in | Check-in, recent session behaviour | Step size and session length |
| Mood / emotional load | 5 faces + optional word | Check-in, [journal](journal.md) entries | Tone of copy, whether to suggest recovery |
| Sensory load | Calm / Buzzing / Overloaded | Check-in, optional ambient noise sensor | Suggest quieter tasks, offer environment changes |
| Social battery | 0 to 5 | Check-in, calendar social events, journal entries | Avoid scheduling calls after draining events |
| Body basics | Ate / drank / moved / slept (toggles) | One-tap body check | Interoception nudges before work |
| Medication window (optional) | Active / wearing off / none | Logged dose time + user-set duration | Put hard tasks in the active window |
| Time context | Free minutes until next fixed event | Calendar | Fit tasks to the available gap |
| Burnout risk | Low / Rising / High | Derived: 7-day energy trend, overload count, sleep | Triggers maintenance mode |

**Check-in UX.** A 10-second, three-swipe check-in (energy, focus, sensory), offered at app open if the last one is older than 3 hours. It is never required: if skipped, the LLM estimates state from time of day, recent activity and history, and says so ("Guessing you're medium energy; tap to correct").

**State-aware modes** (chosen by the LLM, shown as the app's colour mood)

- **Flow mode** (high energy, locked-in): bigger chunks, fewer interruptions, hyperfocus guard on.
- **Steady mode** (default): normal micro-steps, standard nudges.
- **Low-battery mode** (low energy or high sensory load): only tasks tagged low-effort or "maintenance"; suggests one tiny win, then rest.
- **Maintenance mode** (burnout risk high): pauses non-essential goals, renegotiates deadlines where possible, focuses on basics and recovery, celebrates rest as a completed task.
- **Overwhelm button**: always visible. One tap hides everything except one calming action and one optional micro-step.
