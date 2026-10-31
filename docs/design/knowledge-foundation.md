# 2. Knowledge foundation

[← Design index](../design.md)

Every dimension in Prism maps to an established construct from ADHD, autism or AuDHD research, so profiles and interventions are explainable and not invented. The table lists each construct, its source, and the product mechanism it drives. Sources are cited from memory and should be verified by a clinical advisor before launch.

| Construct | Origin | What it means for the user | What Prism does about it |
| --- | --- | --- | --- |
| Executive function as self-regulation | Barkley's EF model (inhibition, working memory, self-motivation, emotional self-regulation, planning) | The gap is between knowing and doing, at the point of performance | Puts cues, time and next step at the point of performance (the home card, nudges) |
| Six EF clusters | Brown model: activation, focus, effort, emotion, memory, action | Different people struggle in different clusters | The trait profile scores each cluster separately ([trait profile](trait-profile.md)) |
| Interest-based nervous system | Dodson; often summarised as INCUP (Interest, Novelty, Challenge, Urgency, Passion) | Importance alone does not produce action | Each task is framed with an interest hook; the LLM picks the hook that works for this user |
| Time blindness / temporal discounting | Barkley; delay-aversion research (Sonuga-Barke) | Future deadlines feel unreal until they are urgent | Visual time (shrinking bars), artificial near-deadlines, immediate rewards |
| Task initiation / "wall of awful" | Clinical ADHD coaching literature (Brendan Mahan) | Starting costs more than doing; emotional residue from past failures | First step under 2 minutes, "just open the file" steps, body doubling |
| Monotropism | Murray, Lesser and Lawson (2005) | Attention flows into few channels deeply; switching is costly | Protect focus tunnels, warn before transitions, batch similar tasks |
| Hyperfocus | ADHD and autism literature | Strong asset that can overrun needs and deadlines | Gentle "surfacing" check-ins, hyperfocus harnessed for high-value tasks |
| Autistic burnout | Raymaker et al. (2020) | Chronic overload leads to loss of skills and capacity for weeks or months | Burnout risk tracking, automatic load reduction, "maintenance mode" |
| Spoon / energy accounting | Spoon theory (Miserandino), used widely by autistic and chronically ill communities | Capacity is finite and varies daily | Daily energy budget; tasks carry an energy cost |
| Sensory processing differences | Autism diagnostic criteria (DSM-5-TR); sensory profile research | Noise, light, texture affect capacity | Sensory load in the state profile; calm UI; environment suggestions |
| Emotional dysregulation and RSD | Barkley (deficient emotional self-regulation); RSD is a clinical, not diagnostic, term | Criticism or failure hits hard and stops action | Shame-free copy, no failure framing, celebrating partial progress |
| Demand avoidance (PDA profile) | Newson; ongoing research on pathological / persistent demand avoidance | Direct demands trigger resistance, even for wanted tasks | Choice-based phrasing, "pick one of three", user can switch demand style |
| Interoception | Autism and ADHD interoception research (e.g. Garfinkel) | Hunger, thirst, fatigue are noticed late | Body check nudges tied to state check-ins |
| Masking / camouflaging | Hull et al. (2017) | Social masking drains energy invisibly | Social events carry an energy cost and recovery buffer |
| Working memory limits | Both ADHD and autism EF research | Multi-step instructions get lost | Every step is self-contained; never more than one instruction per card |
| Need for predictability | Autism criteria (insistence on sameness) | Surprise changes cause distress | Plan changes are announced, explained, and confirmable; stable routines |
| Dopamine and reward timing | Reward-deficiency and delay-aversion models of ADHD | Delayed rewards do not motivate | Rewards at completion, within seconds |

The LLM receives this table (expanded into a structured knowledge base, see [architecture](architecture.md)) as grounding, so its explanations to the user cite the construct by name: "This looks like a transition cost, common with monotropic attention."
