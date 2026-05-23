# 03 — Design System (principles + generation)

The build's design system is **generated per build** from the
Design Direction in [`02-brand-decisions.md`](02-brand-decisions.md). This
doc holds the invariant principles, the quality bar, the installed design
skills, and the methods used to generate spacing / type / motion / colour
tokens within bounds.

## What stays constant vs. what varies

| Always constant (every build) | Always varies (every build) |
|---|---|
| Quality bar (no AI-default tells) | Palette tokens (generated via OKLCH) |
| Accessibility — WCAG AA body, focus states, reduced-motion | Type pairing + scale (generated) |
| One focal point per section | Spacing scale (chosen base + ratio) |
| Single spacing scale per build | Container width (chosen in a range) |
| Real visual hierarchy | Motion personality + timing set |
| Responsive from 320px up | Component-class names & their styles |
| Semantic HTML5 | Hero treatment + section composition |
| `next/font/google` (fonts only loaded via this) | Image direction (see [`06`](06-imagery.md)) |
| The installed design skills set the bar | The chosen *style variant* skill that anchors this build |

Hard rule: **any token value that appears unchanged in two consecutive
builds must be re-justified or replaced.** Defaults are the failure mode
this playbook exists to prevent.

---

## The design skills — load these before designing

Three design skill packs are installed in `.agents/skills/`. They are the
quality bar. Consult them *before* writing any markup or CSS, and run them
again as an audit before shipping.

### `emil-design-eng` — polish philosophy
Decides how interactions should *feel* — easing, timing, focus states,
the small transitions. Read it before building any animated component.

### `impeccable` — audit + vocabulary + OKLCH palette method
Comprehensive frontend design skill: typography, colour/contrast (OKLCH,
tinted neutrals), spatial systems, motion, interaction, responsive
behaviour, UX writing. **User-invocable** with `/audit`, `/polish`,
`/critique`, `/animate`. Use the OKLCH method to generate the palette
(Decision 2 in [`02`](02-brand-decisions.md)); use `/audit` as the
mandatory final pre-ship gate. Catches AI tells: overused fonts, grey
text on coloured backgrounds, pure blacks, card-in-card nesting, dated
easing.

### `taste-skill` suite — layout taste + style anchors
- **`design-taste-frontend`** — the all-rounder. Stronger layout,
  typography, motion, spacing instead of boilerplate UIs. Always loaded.
- **Style variants** — pick the one that matches Decision 6
  (layout archetype) in [`02`](02-brand-decisions.md):
  `minimalist-ui`, `industrial-brutalist-ui`, `high-end-visual-design`.
- **`redesign-existing-projects`** — when revising an existing build.
- **Image-direction skills** (`imagegen-frontend-web`, `brandkit`, etc.)
  — see [`06-imagery.md`](06-imagery.md).

### How to apply them per build

1. **Start:** load `impeccable` + `design-taste-frontend` + the style
   variant matching Decision 6. Load `emil-design-eng`.
2. **Build:** when a default instinct conflicts with a skill, the skill
   wins.
3. **Audit:** before deploy, run `impeccable`'s `/audit` (and `/critique`)
   over every page. Fix every flag. Mandatory.

---

## Colour tokens (generated per build)

Generate via the `impeccable` OKLCH method. Inputs in
[`02`](02-brand-decisions.md) Decision 2.

Token *roles* are constant; values are not:

| Role | Notes |
|---|---|
| background | tinted off-white or off-dark, never pure |
| foreground | near-black or near-white, never pure |
| primary accent | used sparingly — CTAs, active states |
| secondary accent | optional; at most one more |
| muted text | secondary copy |
| section band | a shifted background for alternating sections / form bg |
| line | a 1px divider colour |

Avoid the AI-default colour tells: grey-on-coloured-background, pure
greys against tinted accents, ultra-saturated primaries.

## Typography (generated per build)

Two fonts via `next/font/google`, each exposed as a `--font-{x}` CSS
variable. Choice made in [`02`](02-brand-decisions.md) Decision 3.

The **type scale is generated**:

1. Pick a **modular scale ratio** matching the design's energy:
   - 1.125 (minor second) — quiet, document-like.
   - 1.200 (minor third) — restrained editorial.
   - 1.250 (major third) — balanced, common.
   - 1.333 (perfect fourth) — confident, contrast-rich.
   - 1.500 (perfect fifth) — dramatic, magazine.
2. Pick a **base body size** (15–17px typical). Generate the scale up
   and down from the base.
3. **Hero display size** is the top of the scale (or top minus one),
   set with `clamp()` derived from the scale — not a fixed clamp from
   the playbook.
4. **Letter spacing** — display tracking tightens with scale ratio;
   body tracking is near 0. Eyebrow tracking is *chosen* per design
   between 0.12em and 0.32em — not fixed.
5. **Italic and optical-size axes** declared in the font config where
   needed.

Type rules that hold across builds:
- Italic is for emphasis in display headings and pull-quotes — never
  decoration.
- Headings `text-balance`; long body `text-pretty`.
- All paragraphs read at 320px width without breaking.

## Spacing scale (generated per build)

Pick a **base unit** (4 or 8) and a **ratio** consistent with the type
scale (use the same modular ratio where possible). Generate the scale
once and use only its steps — no off-scale values.

The scale produces:
- Section padding (vertical rhythm) — choose 2–3 steps tied to the
  archetype (centred-minimal uses tighter steps; magazine uses larger).
- Component padding, gap, and gutter — all from the same scale.
- Container width — choose in the range **~1040–1480px**, sized to the
  archetype and visual density dial (Decision 6). Wider for full-bleed
  and magazine; tighter for editorial-asymmetric and minimal.

## Motion (generated per build)

Personality picked in [`02`](02-brand-decisions.md) Decision 8. From the
personality, derive:

- **Default fade-in** — duration + easing.
- **Hover-in / hover-out** — duration + easing (often different).
- **Slider / draggable physics** — spring or non-spring.
- **Focus-shift / state-change** — duration.

Bounds that always hold:
- Respect `prefers-reduced-motion`: collapse to ≤150ms opacity fades.
- No parallax on the hero.
- One personality per build.

## Component-class utilities — purposes, not fixed styles

The site needs a small set of utility classes whose **purposes** are
constant; **class names and exact styles are generated per build** to
match the chosen typography, palette and archetype.

Purposes:

- Container — applies max-width + responsive padding.
- Hero/section title — generated from the type scale and chosen display
  face.
- Eyebrow — small uppercase tracked label.
- Lead paragraph.
- Button variants — primary, ghost, plus accent if Decision 2 produced a
  second accent.
- Link with animated hover treatment (the treatment varies — underline
  sweep, character shift, colour change, slight translate).
- Horizontal divider.

Implement in `@layer components` in `globals.css`. Name them consistently
within the build; do not import a previous build's `globals.css`.

---

## The design quality bar (non-negotiable)

A build is not done until it clears all of these — verified by running
`impeccable`'s `/audit`:

- No AI-default fonts used without a stated reason.
- No grey text on a coloured background; no pure black or pure white.
- No card-inside-card-inside-card nesting.
- No dated bounce easing; motion is calm and purposeful per its personality.
- One clear focal point per section.
- Generous, consistent spacing from a single scale.
- Responsive from 320px up; tested at 375px.
- Accessible: focus states, contrast, reduced-motion, semantic HTML5.

Premium, modern, elegant. Subtle, intentional motion. One accent colour
(or two if the design needs warm + cool). No emoji icons. No generic
gradients. The look is *of this build*, not of "AI-generated marketing
site #427".
