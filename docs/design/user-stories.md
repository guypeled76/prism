# 13. User stories

[← Design index](../design.md)

Each user story below is written from the primary persona's point of view and shows how the phone app and the website serve it. The phone is where things get done: the Now card, nudges and rewards. The website is "desk mode", the big screen for planning, study material and reviewing what the app has learned.

## How the two surfaces split the work

| | Mobile app (iOS, Android) | Website (PWA) and desktop app |
| --- | --- | --- |
| Role | Primary product. In the user's pocket when it is time to start | Companion for desk work: planning, study sessions, bulk upload, review |
| Home screen | The Now card, nothing else | The Now card in a narrow centre column. Today and the calendar sit in side panels that start collapsed |
| Input | Voice, camera, swipes, lock-screen actions, widget, watch | Keyboard shortcuts, drag-and-drop, long-form typing |
| Reminders | Exact alarms, actionable notifications, background re-planning | Only while the tab is open. No exact alarms. Offers to install the app ([T2](../technology/app-and-web.md)) |
| Shared | Same UI code, same data via sync, same rules for copy, colour and rewards | |

The desktop app uses the website layout but gets native notifications and background jobs while it is running. "Web" in the tables below covers both unless a cell says otherwise.

**Phase** refers to the [roadmap](roadmap.md). *Launch* means it must be in place before any public release, whatever the phase. A dash means the roadmap has not placed the story yet (see [open points](#open-points)).

## A. Onboarding and setup

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-01 | I want to set up my profile by answering a few scenarios instead of a clinical questionnaire, so that onboarding feels like a game and not an assessment | Swipeable scenario cards, one per screen, 5 minutes. I can stop and finish later | Same flow, fully keyboard-navigable. If I start on one device I can finish on the other | MVP |
| US-02 | I want to choose sensory settings (motion, sound, haptics, theme, font) before anything else, so that the app never overloads me on first use | Shown as onboarding step 1 and applied straight away. Defaults follow OS reduced-motion and text-size settings | Same. Also respects the browser's `prefers-reduced-motion` and `prefers-contrast` settings | MVP |
| US-03 | I want to connect my calendar and sync once, so that the app knows my fixed commitments without me entering them twice | Native calendar permission (EventKit / CalendarContract). Google sign-in for Drive sync | Google Calendar API only. Asks for persistent storage and warns if the browser refuses it | MVP |
| US-04 | I want to set a daily notification cap and quiet hours, so that nudges help rather than nag | Cap (default 6), quiet hours, iOS Focus / Android DND integration | Same settings, synced. A note explains that the web can only nudge while it is open | MVP |

## B. Capture

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-05 | I want to dump everything on my mind in one go, by voice or text, so that nothing is lost while I still remember it | Capture button on every screen, home-screen widget, lock-screen shortcut, voice input. Under 5 seconds from thought to saved | Global capture shortcut (e.g. Cmd/Ctrl-K) on every page. I can paste long text such as an email or syllabus | MVP |
| US-06 | I want to see how my capture was filed and fix it in one tap, so that I trust the app with my brain dump | Each fragment becomes a chip (event, deadline, task, quest) that I can tap to change. At most one clarifying question | Same chips inline under the capture box, editable with the keyboard | MVP |

## C. The next action (Now card)

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-07 | When I open the app I want to see one thing to do, so that I do not have to decide | Home is only the Now card: step, interest hook, minutes, *Start / Smaller / Not now*. A widget mirrors the card | Now card in the centre. Today list one click away in a collapsed side panel | MVP |
| US-08 | When a step still feels too big, I want to make it smaller until I can start, so that the wall of awful shrinks | *Smaller* returns instantly from the pre-split buffer and can be pressed again and again | Same, plus a keyboard shortcut (`S`) | MVP |
| US-09 | I want to skip a step without penalty, so that a bad moment does not become a failure | *Not now* swaps in another card. Nothing is counted against me | Same (`N`) | MVP |
| US-10 | I want finishing a step to feel good immediately, so that starting the next one is easier | *Done* gives haptic, sound and a colour burst (under 1.5 s), XP rises, and the next card slides in | Colour burst and optional sound (no haptics). Next card appears in place | MVP |
| US-11 | I want a visual timer instead of numbers, so that I can feel time passing | Timer that shrinks in colour, also shown as a Live Activity / ongoing notification | Same timer in the page. The tab title and favicon show time left when I switch tabs | MVP |
| US-12 | I want someone to work alongside me, so that starting is easier | Companion avatar in the focus view. Optional silent live rooms | Same. Live rooms work well at a desk with a webcam | 3 |

## D. Daily plan, calendar and quests

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-13 | I want the app to build a realistic plan for today, so that I do not overcommit | Today list one tap from the Now card: up to 3 main tasks plus a bonus shelf, grouped into morning, afternoon and evening windows | Today list next to a day or week calendar, so I can see how tasks fit around fixed events | MVP |
| US-14 | I want deadlines to feel real before they are urgent, so that I do not start the night before | Runway bars on deadline markers in the calendar and on related tasks | Week and month calendar with runway bars. Wider view of all deadlines at once | MVP |
| US-15 | I want the app to tell me when it changes my plan, and why, so that changes never surprise me | Small announcement card above the Now card. Big changes open a confirm sheet | Same. A diff view lists what was added, moved and dropped, each with its one-line reason | 2 |
| US-16 | When a deadline is at risk I want calm options, not alarms, so that I can act without panic | One card with *Cut scope / Ask for extension / Add focus block*. A drafted email opens in the share sheet | Same card. The drafted email opens in a full editor for longer edits | MVP |
| US-17 | I want to see and shape my big goals, so that far-off goals stay visible | Quest map: milestones as nodes on a path. Tap a node to rename or move its date | Main place for structural edits: drag milestones to reorder, edit dates, add milestones | MVP |

## E. State and modes

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-18 | I want to tell the app how I am doing in seconds, so that the plan fits today's brain | Three-swipe check-in (energy, focus, sensory), offered at open if the last one is over 3 hours old. Also available from a notification | Three compact sliders at the top of the page, keyboard-operable | MVP |
| US-19 | If I skip the check-in, I want the app to guess my state and say so, so that I can correct it | Banner: "Guessing you're medium energy; tap to correct" | Same banner | MVP |
| US-20 | I want to see my current mode at a glance, so that I understand why the plan looks the way it does | Whole UI tint shifts (teal, blue, amber, lavender) with a mode label | Same tint and label | MVP |
| US-21 | When I am overwhelmed I want one button that clears everything, so that I can calm down | Overwhelm button in a fixed spot on every screen. One tap shows only one calming action and one optional micro-step | Same button, same fixed spot | — |
| US-22 | When I am heading for burnout I want the app to lower its demands, so that I protect my capacity | Maintenance mode: pauses non-essential quests and shows rest as a completed task. Announced with a reason | Same. Paused quests are listed, so I can see they are safe and not lost | 3 |
| US-23 | I want hard tasks placed in my medication window, so that I use my best hours well | Log a dose from a notification or widget in one tap | Set the schedule and duration. The window is shown on the calendar | 3 |

## F. Nudges

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-24 | I want a gentle nudge when it is time to start, and to act on it without opening the app, so that I do not get lost in my phone | Actionable notification: *Start / Snooze 15 / Skip* from the lock screen. Smartwatch haptic in phase 3 | Browser notification only while the tab is open. A banner says reminders need the app | MVP |
| US-25 | I want warnings before fixed events and a leave-now alert, so that transitions do not ambush me | Exact alarms at 10 and 3 minutes before, plus leave-now (time-sensitive on iOS, exact alarm on Android) | Not supported. Events show "Reminders on your phone" | MVP |
| US-26 | When I am hyperfocused I want a soft check-in, not a hard stop, so that I remember water and deadlines | Notification after planned time plus 20%: "Water? Keep going?" | In-page banner and tab title change. Desk sessions are where hyperfocus usually happens, and the tab is open then | MVP |
| US-27 | I want to pause everything for a few hours, so that I get real rest | One switch in the notification shade tile or settings, synced | Same switch, synced to the phone | — |
| US-28 | If I keep ignoring nudges, I want the app to ask whether there are too many, so that it adjusts instead of escalating | After 3 ignored nudges in a row, a "Too many?" card offers fewer, later or different nudges | Same card on next visit | 2 |

## G. Study mode

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-29 | I want to give the app all my course material at once, so that I never face a 200-page PDF alone | Camera capture of notes and pages (OCR), share-sheet import, record a lecture | Main upload surface: drag-and-drop PDFs, slides, syllabus and past exams, paste links. Shows progress as each file is processed | MVP |
| US-30 | I want to see what the exam covers and how far I am, so that I know it is achievable | Zoomable knowledge map, or a simpler topic list that shows mastery | Full map with prerequisites and exam weights. I can set confidence per topic and correct the map | MVP |
| US-31 | I want a quick placement quiz, so that I do not waste time on what I already know | 5-minute playful quiz, one question per screen | Same quiz, keyboard answers | — |
| US-32 | I want to learn in 1 to 2 minute bites inside the app, so that study fits low-energy moments | Bite cards and flashcards, one per screen, swipe to grade. Good for commutes and queues | Keyboard-driven: `Space` reveals, `1` to `4` grades | MVP |
| US-33 | I want to explain a concept back and work through examples, so that I understand rather than memorise | Voice answer for explain-it-back. Worked examples fit on a phone screen but are cramped | Recommended surface: typing, maths input and worked → faded → solo examples side by side | 2 |
| US-34 | When I am confused I want to ask why, so that I get unstuck without leaving the app | Tutor chat scoped to one concept, 5 minutes, with a visible cap | Same chat, with the source passage open beside it | 2 |
| US-35 | I want to study by listening, so that I can learn while walking or stimming | Audio mode: plays with the screen off, answers by voice, headphone controls | Supported but secondary | 3 |
| US-36 | I want to see where an answer comes from and flag wrong questions, so that I can trust the material | *Source* link opens the passage in a sheet. Flag in one tap | Source passage opens side by side with the lesson. Flag in one click | — |
| US-37 | When I fall behind I want the app to cut by exam weight rather than make me cram, so that the plan stays humane | One card proposing cuts ("Skip chapter 7, 3% of past exams"), accept or adjust | Same proposal with the affected part of the knowledge map highlighted | — |
| US-38 | I want an optional boss battle at the end of a milestone, so that progress has a clear finish line | Timed mixed quiz, retry freely | Same | — |

## H. Rewards and companion

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-39 | I want to see my momentum without a streak I can break, so that one bad day does not end it | Momentum meter shown quietly under the Now card. It drains slowly and never resets | Same meter | MVP |
| US-40 | I want a companion that grows as I work and rests when I rest, so that I get feedback that does not judge me | Lives on one secondary screen, never on the Now card | Same secondary page | 2 |
| US-41 | I want to unlock real treats I chose myself, so that app rewards connect to real ones | Unlock card after a milestone | Manage the treat list | 3 |
| US-42 | After days away, I want a warm welcome and one small step, not a backlog, so that coming back is easy | Welcome-back notification and a fresh small plan. Missed items are re-planned silently | Same welcome screen on next visit | — |

## I. Self-knowledge, trust and control

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-43 | I want to review what the app learned about me each week, so that I stay in charge of my profile | "What I learned about you" cards: swipe to accept or reject, tap to edit | Same insights as a list, with the evidence expanded by default | 2 |
| US-44 | I want to see my strengths and friction points, so that I understand my brain without being labelled | Character sheet as a constellation. Tap a star for its description and workaround | Constellation plus a table view of every trait, score and confidence | 2 |
| US-45 | I want to ask "why?" about any decision the app made, so that it never feels arbitrary | *Why?* link on plan changes, nudges, state guesses and traits | Same links. Opens in a side panel, so I keep my place | 2 |
| US-46 | I want to export or delete all my data at any time, so that I own it | Settings → Data: export (share sheet) and delete with confirmation | Same. Export downloads a file directly | Launch |
| US-47 | If I write something that signals crisis, I want to be shown real help, so that I am safe | Crisis resources for my region appear without relying on the LLM. Productivity framing stops | Same | Launch |

## J. Across devices

| ID | Story | Mobile | Web | Phase |
| --- | --- | --- | --- | --- |
| US-48 | I want to plan at my desk and act on my phone, so that each device does what it is good at | Changes made on the web appear within seconds of sync. Works offline from the local copy | Changes from the phone appear on the next sync. Unsynced changes are flagged | MVP |
| US-49 | When I use the website I want to know what it cannot do, so that I do not rely on reminders that never arrive | n/a | A banner (shown until dismissed) explains that reminders and alarms need the app, with an install link | MVP |

## Open points

- **Stories with no phase yet:** US-21 (overwhelm button), US-27 (pause everything), US-31 (placement quiz), US-36 (sources and flagging), US-37 (cut by exam weight), US-38 (boss battle) and US-42 (welcome back). US-21, US-36 and US-42 support core design principles and are strong MVP candidates.
- **Web without reminders:** is the web useful enough to justify building it in the MVP, or should it come in phase 2 alongside study tools such as explain-it-back that suit a keyboard?
- **Keyboard shortcuts:** single-key shortcuts (`S`, `N`) must not fire while typing, and should be optional for users who press keys by accident.
