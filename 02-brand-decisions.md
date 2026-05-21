# 02 — Brand Decisions

The agent decides the brand. Theme, palette, typography and tagline are
**inferred, not asked for**. The user supplies the business and the keyword
CSV; the agent reads them and makes the call. Only escalate to the user if
the business genuinely sits between two equally valid directions and the
choice would change the build.

Settle four things before any code. They cascade into every other choice.

---

## What the agent reads to decide

1. **The keyword CSV** — the niche, the seed keyword, the spread of intents.
2. **The business facts** the user gives — name, what it does, where, who
   it serves, price level, age, anything about its personality.
3. **The niche's conventions** — what the category's customers expect, and
   what the category's competitors all look like (so the site can be better
   than the average, not the same).

If the business facts are thin, ask **once**, briefly, for the essentials:
name, one-line description, location, rough price tier, and any sense of
personality. Then decide everything else.

---

## Decision 1 — Positioning

Where does the business sit on the premium ↔ accessible and
boutique ↔ high-volume axes? Pick the closest archetype:

| Archetype | Signals in the CSV / brief | Tone |
|---|---|---|
| **Premium specialist** | Low-volume, high-CPC keywords; "best", "luxury", "custom", "bespoke" | Restrained, confident, sparse |
| **Trusted local operator** | "near me", "[trade] [suburb]", "emergency", "same day" | Warm, plain-spoken, reassuring |
| **High-volume / value** | "cheap", "affordable", "quote", broad transactional terms | Direct, fast, proof-heavy |
| **Expert / advisory** | Lots of informational "how / why / cost of" keywords | Authoritative, generous, educational |

Positioning decides how loud the design is, how much white space, how much
proof, and how the copy frames price.

## Decision 2 — Palette mood

Pick a mood that fits the niche's emotional job, then let the design skills
(see [`03-design-system.md`](03-design-system.md)) turn it into exact tokens.

| Mood | Fits | Character |
|---|---|---|
| **Cool architectural** | design studios, tech, legal, finance, B2B | paper / ink / one cool accent; precise |
| **Warm earthy** | trades, landscaping, food, wellness, family services | sand / bark / a warm + a green accent; grounded |
| **Clinical calm** | medical, dental, allied health | off-white / slate / a single trustworthy blue or teal |
| **Bold editorial** | agencies, fitness, lifestyle, hospitality | high-contrast neutral + one saturated statement colour |

Rules that hold for **every** mood:
- 6–7 named tokens, no more.
- One accent colour used sparingly. Two at most (one warm, one cool).
- No pure black, no pure white, no fully-saturated primaries — those read
  cheap. Use a near-black, a tinted off-white, and slightly desaturated
  accents. The design skills enforce this; follow them.

## Decision 3 — Typography pairing

Two fonts via `next/font/google`, each exposed as a `--font-{name}` CSS
variable. Pick a display + body pairing that matches positioning and mood:

| Pairing feel | Display | Body | Use for |
|---|---|---|---|
| Modern editorial | a sharp optical serif (e.g. Fraunces) | a clean grotesque (e.g. Inter) | premium specialists, agencies |
| Warm classical | a humanist serif (e.g. EB Garamond) | a warm sans (e.g. DM Sans) | trusted local operators, trades |
| Clinical precise | a calm geometric sans | a neutral sans | medical, finance, tech |
| Confident contemporary | a characterful display sans/serif | a workhorse sans | bold editorial brands |

Avoid the AI-default tells the design skills call out — overused fonts used
without intent. Choosing a font is a *decision*, justified by positioning.

## Decision 4 — Tagline

Write the tagline **before any other copy**. It anchors the whole voice.

- 3–5 words. Declarative. No exclamation mark.
- It states what the business *is for*, not what it *sells*.
- Two reliable shapes:
  - **Permanence / craft** — for specialists and premium brands.
    ("Built to outlast the brief.")
  - **Reliability / care** — for local operators and trades.
    ("Done right, the first time.")

Section-heading shapes that always work:
- **H1 hero:** `[Verb], [italic emotional descriptor].`
- **H2 sections:** `[Number], [italic subject].` (e.g. "Six services, *one team.*")

---

## Output of this step

Before moving on, write the four decisions down (in the project's
`project_specs.md` or an equivalent note) as a short brand brief:

- **Positioning:** archetype + one sentence on tone.
- **Palette mood:** chosen mood + the 6–7 token names and rough hex.
- **Typography:** display + body font, with the one-line reason.
- **Tagline** + the two heading shapes.

Hand this brief to [`03-design-system.md`](03-design-system.md), which turns
it into exact tokens, and to [`07-content.md`](07-content.md) and
[`references/`](references/), which turn it into voice.
