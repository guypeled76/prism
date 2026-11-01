# 10. Data model

[← Design index](../design.md)

The data model has 17 core entities; the append-only `Event` log is the source of truth for everything the agent learns, and every agent-written record carries its reason and source so the user can ask "why?".

| Entity | Key fields | Written by | Notes |
| --- | --- | --- | --- |
| User | id, timezone, settings (sensory, notification caps, demand style, theme) | User | Settings drive both UI and agent prompts |
| TraitScore | user\_id, trait\_key, score 0–100, confidence, updated\_at | Agent (bounded), User (override) | `trait_key` references the knowledge base |
| TraitInsight | id, trait\_key, proposed\_change, evidence\[\] (events and journal entries), status (proposed / accepted / rejected) | Agent proposes, User decides | Rejected insights are kept to avoid repeats |
| StateSnapshot | user\_id, at, energy, focus, mood, sensory, social, body flags, med\_window, source (check-in / inferred) | User or Agent | Inferred snapshots are marked and correctable |
| Quest | id, title, why (user's words), target\_date, status, area\_tag | User (agent can draft) | Top of the hierarchy |
| Milestone | id, quest\_id, title, order, target\_date, status | Agent, User edits |  |
| Task | id, milestone\_id?, title, est\_minutes, energy\_cost 1–3, interest\_hook, window, due\_at?, status, area\_tag | Agent, User | Standalone tasks have no milestone |
| Step | id, task\_id, text, est\_minutes, order, parent\_step\_id? (for Smaller splits), lesson\_id? | Agent | Generated just in time; split tree kept |
| CalendarEvent | id, source (device / app), start, end, kind (fixed / deadline), energy\_cost, buffer\_before/after | Import, User | Imported events are read-only mirrors |
| Nudge | id, type, scheduled\_at, copy, actions, outcome (started / snoozed / ignored) | Agent schedules, Device fires | Outcomes feed the nudge learner |
| Material | id, quest\_id, type (pdf / image / audio / link), storage\_ref, extracted\_text, chunks + embeddings | User uploads, pipeline processes | Treated as untrusted data |
| Concept | id, quest\_id, title, parent\_id, prerequisites\[\], exam\_weight, source\_chunk\_ids\[\], mastery, fsrs\_state | Agent | The knowledge map |
| Lesson / Card | id, concept\_id, format, content (JSON), source\_chunk\_ids\[\], flagged | Agent | Regenerated if flagged |
| Reward | id, kind (xp / badge / drop / companion / real-world), amount, reason, at | App code (rules) + Agent (multipliers) | Never revoked |
| JournalEntry | id, at, text?, media\_ref?, source (capture / journal / wind-down / voice), state\_snapshot\_id?, tags\[\], analysis\_status, private, win | User writes, Agent tags | Private entries are never analysed; deleting removes them from all evidence. See [journal](journal.md) |
| Event (log) | id, user\_id, at, type, entity\_ref, context (state id, framing, step size), payload | App | Append-only; feeds trait inference and analytics |
| PlanDiff | id, at, changes\[\], reason, requires\_confirmation, status | Agent | Shown as announcements |

All user content is encrypted at rest; `Material` and embeddings are stored per user with no cross-user retrieval.
