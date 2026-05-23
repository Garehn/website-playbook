# 02 — Brand & Design Direction

This is the most important doc in the folder. **Eight decisions are made
here, fresh, every build.** They are not picked from a small menu of
defaults — they are *derived* from the niche, the keyword CSV and the
business brief, using the methods below.

The agent decides all eight. Escalate to the user only if the business
genuinely sits between two equally valid directions and the choice would
change the build.

The eight decisions, in order:

1. Positioning
2. Palette (generated)
3. Typography (generated)
4. Tagline
5. **Page architecture**
6. **Layout archetype**
7. **Hero treatment**
8. **Motion personality**

Decisions 5–8 are the per-build *Design Direction* — the thing that
guarantees two sites from this playbook don't look the same.

---

## What the agent reads to decide

1. **The keyword CSV** — niche, seed keyword, intent split (informational
   vs commercial vs single-dominant), volume profile, locations.
2. **The business brief** the user gives — name, what it does, where, who
   it serves, price tier, age, personality.
3. **The niche's category conventions** — what its competitors all look
   like, so this site is better, not the same.

If the brief is thin, ask **once**, briefly, for the essentials and then
decide everything else.

---

## Decision 1 — Positioning

Where the business sits on premium ↔ accessible and boutique ↔ high-volume.
Pick the closest archetype. **Positioning is an input to every later
decision, not an output that fixes them.** A premium specialist can be
editorial OR brutalist; a trusted local operator can be warm OR cool —
the rest of the decisions pick which.

| Archetype | Signals in the CSV / brief | Tone |
|---|---|---|
| **Premium specialist** | Low-volume, high-CPC; "best", "luxury", "custom", "bespoke" | Restrained, confident, sparse |
| **Trusted local operator** | "near me", "[trade] [suburb]", "emergency", "same day" | Warm, plain-spoken, reassuring |
| **High-volume / value** | "cheap", "affordable", "quote", broad transactional | Direct, fast, proof-heavy |
| **Expert / advisory** | Lots of "how / why / cost of"; PAA-rich | Authoritative, generous, educational |

---

## Decision 2 — Palette (generate, don't pick)

**Do not pick from a fixed table.** Generate the palette via OKLCH using
the `impeccable` skill (installed in `.agents/skills/impeccable/`).

Inputs to the generation:
- The niche's category convention (and the urge to break it where helpful).
- Positioning archetype from Decision 1.
- The mood the brand needs to evoke (calm, urgent, trustworthy, playful,
  serious, optimistic — pick one or two, not a generic "professional").
- The competitor scan — what colours dominate the SERP top-10? Choose a
  palette that holds its own next to them, not one that blends in.

Output: **6–7 named tokens** with reasoning per token.

Bounds (these always hold):
- Use OKLCH; pick a base hue and derive tinted neutrals from it (never
  pure greys against tinted accents).
- No pure black, no pure white, no fully-saturated primaries.
- One accent used sparingly. A second accent only if the design needs
  warm + cool contrast — never more than two.
- Contrast meets WCAG AA for body text; AAA for primary headings where
  achievable.

If the generated palette would land within ΔE ~10 of a palette the agent
has used recently for a similar niche, push the hue, chroma or lightness
further before committing. The point is divergence.

## Decision 3 — Typography (generate, don't pick)

**Do not default to popular pairings.** Inter, Fraunces, EB Garamond,
DM Sans are fine fonts but reaching for them as defaults is itself the
problem — every "AI-built" site uses them. Justify every choice.

Method:

- Decide the **display–body relationship**: contrast (serif + sans,
  display-sans + grotesque, mono + serif), companion (two harmonious
  serifs, two sans of different x-heights), or single-family (one
  superfamily across both — sometimes the strongest choice).
- Decide what the **display face** has to do: assert a personality,
  carry headlines at large size, be quiet and let the layout speak.
  Pick a face whose character matches that job.
- Decide what the **body face** has to do: long-form reading, dense UI,
  tight tabular numbers. Pick for x-height fit, weight range (need ≥4
  weights), italic availability, and number-feature support.
- Pick from a broad pool — `next/font/google` has ~1500 families.
  Treat any candidate as eligible if it serves the job. Some
  under-used-but-excellent display choices: Boldonse, Instrument
  Serif, Bricolage Grotesque, Fraunces, Newsreader, Vollkorn,
  Crimson Pro, Geist, Albert Sans, Familjen Grotesk, Manrope,
  Tomorrow, Source Serif 4, Spectral, Lora, Cormorant, Old Standard
  TT, Unbounded, Bebas Neue, Anton, Cinzel, Marcellus, Italiana.
  This is a prompt, not a menu — use any family that fits.
- Set up both fonts via `next/font/google`, expose as `--font-{name}`
  CSS variables, declare variable axes/weights/styles in the font config.

Output: display family + body family + the one-sentence reason each was
chosen and a list of any optical-size or italic axes needed.

## Decision 4 — Tagline

Write the tagline **before any other copy**. It anchors the voice.

Principle: 3–5 words, declarative, no exclamation mark. States what the
business *is for*, not what it *sells*. Sounds like the named person from
the voice profile.

Shape menu (riff, don't copy):

- **Permanence / craft** — "Built to outlast the brief."
- **Reliability / care** — "Done right, the first time."
- **Speed / response** — "On site in under an hour."
- **Specificity / scope** — "One street. One trade. Done."
- **Quiet expertise** — "Plumbing, without the surprises."
- **Contrarian** — "Cheaper isn't cheaper."
- **Outcome-led** — "Houses, sold faster."
- **Anti-promise** — "We'll tell you when you don't need us."

Do not use the same shape as the last build for the same niche.

---

## Decision 5 — Page architecture (new)

Derived from the CSV intent split and the niche. **The page set is not
fixed.** Two real examples of how different it can be:

- **Emergency trade** (commercial-heavy CSV, location-rich): Home, a
  thin Services hub, a dozen service-leaf pages, a dozen location-leaf
  pages, Contact. No Process page. No Blog initially. A small Tips/FAQ
  section grows over time.
- **Editorial branding agency** (low-volume, expertise queries): Work,
  Case-studies (deep), About, Journal (sparse, long-form), Contact.
  No Services page, no FAQ.
- **Single-product business** (one dominant query): a single long-page
  site with anchor navigation, plus 2–3 long-form articles supporting it.
- **Advisory / professional services** (PAA-rich informational CSV):
  Home, About, Services, Insights (blog-led), Pricing, Contact.

Pick from the page-archetype menu in
[`04-stack-and-pages.md`](04-stack-and-pages.md). Output: the actual
page list this build needs, with a one-line reason per page. If the
output looks identical to a previous build, the CSV signal has been
ignored — go back.

## Decision 6 — Layout archetype (new)

Pick **one** from the menu below. Each anchors how every page is composed.

| Archetype | When it fits | Reference skill |
|---|---|---|
| **Editorial-asymmetric** | Premium specialists, agencies, editorial brands | `high-end-visual-design` |
| **Centred-minimal** | Quiet expertise, advisory, "less is more" | `minimalist-ui` |
| **Bento-grid** | Productised services, dashboards, multi-offer | `design-taste-frontend` |
| **Magazine** | Content-heavy, advisory, journal-led | `high-end-visual-design` |
| **Full-bleed-image-led** | Hospitality, trades with visual proof, food | `design-taste-frontend` |
| **Brutalist-utilitarian** | Industrial, technical, contrarian | `industrial-brutalist-ui` |
| **Classic-split** | Trusted local operators, family services | `design-taste-frontend` |
| **List-driven** | Single-page businesses, document-style | `minimalist-ui` |
| **Scroll-narrative** | Case-study or origin-led brands | `high-end-visual-design` |

Then set the three **taste-skill dials**:

- **Design variance** — low (orderly, predictable) ↔ high (asymmetric,
  layered, experimental).
- **Motion intensity** — subtle ↔ choreographed (this feeds Decision 8).
- **Visual density** — spacious ↔ dense.

Output: archetype + the three dial positions, with one sentence on each.

## Decision 7 — Hero treatment (new)

Pick **one**. The hero sets the building's facade — make it specific to
the brand, not the playbook's default.

| Hero type | Character | Pairs well with |
|---|---|---|
| **Full-bleed image** | Confident, visual-proof-led | Trades, hospitality, lifestyle |
| **Typographic-only** | Quiet expertise, brand-led | Agencies, advisory |
| **Split (text \| image)** | Balanced, dependable | Trusted local operators |
| **Video / motion loop** | Live, contemporary, kinetic | Editorial, fitness, food |
| **Minimal — no image** | Restrained, anti-brochure | Premium specialists |
| **Editorial-magazine** | Long, layered, scrollable hero | Long-form / case-study brands |
| **Asymmetric-overlap** | Confident, design-forward | Agencies, design studios |

The hero must not be a full-bleed image by default. Default is the enemy.

## Decision 8 — Motion personality (new)

Pick **one** personality, then **generate** the timing set from it. Do
not reuse a single canonical easing curve across builds.

| Personality | Feel | Easing family | Duration range |
|---|---|---|---|
| **Calm** | Confident, restrained | gentle ease-out (e.g. `(0.16, 1, 0.3, 1)`) | 500–800ms |
| **Lively** | Friendly, responsive | spring-like / quick ease-out | 250–500ms |
| **Dramatic** | Editorial, choreographed | overshoot / staged delays | 700–1200ms |
| **Mechanical** | Industrial, deliberate | linear or step-eased | 200–400ms, no delays |

Bounds (always):
- Respect `prefers-reduced-motion` — collapse all animation to opacity
  fades under 150ms.
- No parallax on the hero — gimmicky.
- Easing is *chosen*, not the same `[0.22, 1, 0.36, 1]` every time.
- One personality per build. No mixing.

Output: personality + the actual durations and easing curves you'll use
for fade-in, hover-in/out, slider physics, focus shifts.

---

## The hand-off — the Design Direction brief

Write the eight decisions down as the brand brief (in
`project_specs.md` or an equivalent). One page. This brief is the input
to [`03-design-system.md`](03-design-system.md) (token generation),
[`04-stack-and-pages.md`](04-stack-and-pages.md) (page composition),
[`05-components.md`](05-components.md) (variant choices),
[`06-imagery.md`](06-imagery.md) (image direction) and
[`07-content.md`](07-content.md) (voice + heading shapes).

If two consecutive briefs look identical on more than two of the eight
axes — that's a signal the playbook is being used as a template, not a
method. Go back and re-derive.
