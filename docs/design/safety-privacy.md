# 11. Safety, privacy and ethics

[← Design index](../design.md)

Prism handles health-adjacent, emotionally sensitive data for a vulnerable group, so it is designed as a support tool, never a diagnostic or treatment device, with privacy and user control as hard requirements.

- **Not a medical device.** No diagnosis, no medication advice. Trait language is descriptive and strengths-based. Medication timing is a user-entered schedule only.
- **Crisis handling.** Deterministic keyword and classifier checks on free text (outside the LLM's discretion) route to local crisis resources; the LLM is also instructed to recognise distress and step back from productivity framing.
- **Burnout over productivity.** When burnout risk is high, the agent's objective switches from "complete tasks" to "protect capacity". Success metrics include rest, not only output.
- **No dark patterns.** No streak guilt, no purchasable rewards, no loot boxes, no engagement-maximising notifications. Time-in-app is not a success metric.
- **Journal privacy.** Journal entries are the most personal data in the app. Any entry can be marked "just for me" (never analysed or used as evidence), analysis can be switched off for the whole journal, and deleting an entry removes it from all inferences.
- **Transparency.** Every inference (trait, state, plan change) has a "why?" view with the evidence. The user can edit or delete any of it.
- **Data minimisation and control.** Location, microphone and health data are opt-in, per feature. Full export and full delete at any time. No data sold or used for ads. LLM provider configured with zero data retention and no training on user data.
- **Encryption.** At rest and in transit; uploaded study material is private to the user.
- **Neurodiversity-affirming review.** Copy, traits and mechanics are reviewed with autistic and ADHD advisors and a clinician before launch; the knowledge base cites sources and is versioned.
- **Age.** 16+ at launch; a teen version would need guardian and school considerations.
- **Regulatory.** GDPR / UK GDPR and equivalent; health-data handling assessed per market (e.g. special category data under GDPR).
