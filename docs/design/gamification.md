# 7. Gamification and reward design

[← Design index](../design.md)

Rewards fire within one second of any completed step, scale with *effort relative to today's state* rather than raw output, and never take anything away; the game layer is a thin, quiet frame around real work, not a second app to manage.

**Reward loop per step:** tap Done → short haptic + sound + colour burst (under 1.5 s) → XP number rises → the next step's card slides in (momentum). The burst can be toned down to haptic-only in sensory settings.

**Mechanics**

| Mechanic | How it works | Why it fits AuDHD | Guardrail |
| --- | --- | --- | --- |
| Effort XP | XP = base × difficulty for *you today* (an initiation on a low-energy day earns more) | Rewards the real cost: starting, not just output | Capped per day to discourage over-exertion |
| Starting bonus | Extra XP just for pressing Start | Initiation is the hardest step | None needed |
| Quest map | Each quest is a visual path; milestones light up as nodes | Makes far goals visible (time blindness) | One map per quest, no social comparison |
| Momentum, not streaks | A "momentum" meter that fills with activity and *slowly* drains; never resets to zero, and rest days are protected | Streak loss is a classic ADHD abandonment trigger | No streak counters anywhere |
| Surprise drops | Occasional random bonus (cosmetic item, a fun fact about a special interest) | Novelty and variable reward feed the interest system | Cosmetic only, low frequency, no loot boxes, no purchases |
| Companion | A small creature/plant that grows with completed steps and reflects state (rests when you rest) | Externalised, non-judgemental feedback; body-double feel | It never gets sick, sad or dies from neglect |
| Skill badges for supports | "Transition ritual lvl 3", "Asked for help", "Took a rest day" | Rewards scaffolding and self-care, not masking | Badges are private by default |
| Boss battles | End-of-milestone quizzes or hard tasks with a clear finish | Challenge and urgency (INCUP) | Optional, can be retried freely |
| Real-world rewards | User lists real treats; the app "unlocks" one after a milestone | Bridges app rewards to genuine dopamine | User-defined, never purchasable |

**Distraction budget.** The game layer must not become the hyperfocus target. Rules:

- No feeds, no social leaderboard, no infinite content.
- Companion and cosmetics live on one secondary screen; the home screen is just the Now card.
- Celebration animations are short and skippable; after a session ends, the app suggests closing itself.
- Time spent in non-task screens is tracked, and if it grows, the LLM tones the game layer down.
- Notifications never advertise rewards ("come collect your bonus") to pull the user in.

**Anti-shame rules for copy and visuals.** No red for overdue items. No "you failed", "you missed", "behind". Partial completion is completion ("You did 2 of 5 steps, that's 2 more than zero"). Returning after a long absence gets a warm welcome back and a fresh, small plan, never a backlog.
