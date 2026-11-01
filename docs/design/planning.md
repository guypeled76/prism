# 5. Planning system

[← Design index](../design.md)

Planning is a four-level hierarchy, Quest → Milestone → Task → Step, where the user writes only the top levels in their own words and the LLM generates and continuously re-plans everything below.

| Level | Example | Size | Who creates it | Where it shows |
| --- | --- | --- | --- | --- |
| Quest (big project / goal) | "Pass the statistics exam" | Weeks to months | User, in free text or voice | Quest map |
| Milestone | "Understand hypothesis testing" | Days to a week | LLM, user can edit | Quest map, calendar |
| Task | "Learn what a p-value means" | 15 to 60 min | LLM | Today list |
| Step | "Read this 3-sentence definition and tap Got it" | 1 to 10 min | LLM, generated just in time | Now card |

**Capture.** One input box, everywhere: type or speak anything ("dentist thursday 3pm, also need to finish the slides before Monday, and I want to learn guitar"). The LLM classifies each fragment as event, deadline, task, quest or note (a [journal](journal.md) entry), asks at most one clarifying question, and files it. Capture must take under 5 seconds so ideas are not lost to working memory.

**Calendar.** Fixed events (appointments, classes) are imported from device calendars and shown as solid blocks. Deadlines are shown as markers with a visible "time runway" bar that shrinks. Flexible tasks are never pinned to exact times by default; they are placed into *windows* (morning, afternoon, evening) to avoid the shame of a missed 10:00 slot. Each event carries an energy cost and a transition buffer (e.g. 15 minutes before and after, longer for social events, per profile).

**Daily plan.** Each morning (or at first open) the LLM builds "Today" from: hard events, deadlines and their runway, quest milestones, current state and the trait profile. Rules it follows:

- Plan to about 60% of available time; time blindness and transitions eat the rest.
- At most 3 "main" tasks per day; extras go to a "bonus" shelf.
- Hardest task in the user's best window (learned, or the medication window).
- Similar tasks batched to reduce switching (monotropism).
- Every task starts with a step that takes under 2 minutes.

**Now card.** The home screen shows a single card: the next step, why it matters (the interest hook), estimated minutes, and three buttons: *Start*, *Smaller* (LLM splits it further), *Not now* (pick another, no penalty). "Smaller" can be pressed repeatedly until the step is as small as "open the document".

**Re-planning.** Missed tasks are not "overdue"; the LLM silently re-plans and, if a deadline is at risk, shows one calm card with options ("Cut scope", "Ask for extension (draft email)", "Add a focus block tomorrow"). Any change to the visible plan is announced, because unannounced change is distressing for autistic users.

**Support tools in the task flow**

- **Body doubling:** a virtual co-working companion (avatar, or optional live rooms with others working silently).
- **Visual timers:** shrinking-colour timers instead of digits.
- **Transition rituals:** a 60-second wind-down/wind-up card between tasks, with a 5-minute warning before fixed events.
- **Hyperfocus guard:** gentle "surface" check after planned duration plus a margin, never a hard stop.
