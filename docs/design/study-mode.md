# 6. Study mode

[← Design index](../design.md)

Study mode turns uploaded learning material and an exam date into a daily plan of 2 to 10 minute learning steps that are completed *inside the app*, so the user never has to face a 200-page PDF and decide where to begin.

**Pipeline**

1. **Ingest.** User adds material: PDFs, slides, photos of notes or textbook pages (OCR), web links, lecture recordings (transcribed), past exams, the syllabus. Plus the exam date and, optionally, how confident they feel per topic.
2. **Map.** The LLM (by default the opt-in cloud model, [T7](../technology/cloud-llm.md)) builds a *knowledge map*: topics → concepts → atomic facts/skills, with prerequisites between them and an exam-weight estimate (from syllabus, past exams, emphasis in material). Each concept stores source passages so every question can cite where its answer came from.
3. **Diagnose (optional).** A 5-minute playful placement quiz marks concepts as known, shaky or new, so time is not wasted on known material.
4. **Schedule.** Working back from the exam date: new concepts in prerequisite order, spaced reviews of earlier ones (FSRS-style spaced repetition), and 2 to 3 full practice sessions in the final week. A buffer of about 20% of days is left empty for bad days. Study steps are merged into the daily plan with everything else.
5. **Learn.** Each step is one interactive micro-lesson (below).
6. **Adapt.** Results update concept mastery; the schedule re-plans nightly. If the user falls behind, the LLM cuts by exam weight ("Skip chapter 7, it's 3% of past exams") rather than cramming.

**Micro-lesson formats** (the LLM picks by concept type, state and profile)

| Format | Length | Best for | Low-energy friendly |
| --- | --- | --- | --- |
| Bite card: 2 to 4 sentence explanation + one check question | 1 to 2 min | New facts and definitions | Yes |
| Recall flashcard (spaced repetition) | Seconds each, 5 to 10 per set | Reviews | Yes |
| "Explain it back" (type or voice; LLM gives feedback) | 3 to 5 min | Deep understanding (Feynman technique) | No |
| Worked example → faded example → solo problem | 5 to 10 min | Procedures, math | No |
| Socratic tutor chat, scoped to one concept | 5 min, capped | Confusion, "why" questions | Medium |
| Interest-bridged analogy (uses the user's special interests) | 2 min | Hard abstract concepts | Yes |
| Boss battle: timed mixed quiz | 10 min | End of milestone, exam practice | No |
| Audio mode: listen and answer by voice | 5 min | Walking, stimming, low visual tolerance | Yes |

**Learning-science basis.** Retrieval practice, spaced repetition, interleaving, worked examples and elaborative interrogation are all well-supported by cognitive psychology (e.g. Dunlosky et al., 2013). Prism uses them because they also happen to fit ADHD: short, active, immediately scored.

**Grounding and accuracy.** The tutor answers only from the user's material plus general knowledge flagged as such. Each lesson shows a "source" link to the passage. Users can flag a wrong question in one tap; flagged items are regenerated and down-weighted.

**Example daily study plan** (7 days to exam, steady state, 45 minutes of study budget)

- [ ] 2 min: Warm-up, 5 flashcards from yesterday
- [ ] 4 min: Bite card, "What a p-value is (and isn't)"
- [ ] 3 min: Explain p-value back in your own words
- [ ] 8 min: Worked example, one-sample t-test
- [ ] Break: 5 min, move or snack
- [ ] 6 min: Faded example, two-sample t-test
- [ ] 2 min: Analogy card linking to your interest (e.g. football stats)
- [ ] 10 min: Mini boss, mixed quiz on week 1 topics
- [ ] 2 min: Close-out, rate confidence, see XP and map progress
