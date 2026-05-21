# 03 — Design System

The site lives or dies on design quality. This document sets the bar, names
the installed design skills, and defines the token/type/motion system the
agent fills in from the brand brief.

---

## The design skills — load these before designing

Three design skill packs are installed in `.agents/skills/`. They are the
quality bar. **Consult them before writing any markup or CSS, and run them
again as an audit before shipping.** They override the model's default
"AI-looking" instincts, which is the entire point.

### `emil-design-eng` — the polish philosophy
Emil Kowalski's philosophy on UI polish, component design, animation
decisions, and the invisible details that make software feel considered.
**Use it for:** deciding *how* an interaction should feel — easing,
timing, focus states, the small transitions. Read it before building
`Header`, `BeforeAfter`, `QuoteForm`, and any animated component.

### `impeccable` — the audit + vocabulary
A comprehensive frontend design skill covering typography, colour/contrast
(OKLCH, tinted neutrals), spatial systems, motion, interaction, responsive
behaviour and UX writing. It is **user-invocable** with commands like
`/audit`, `/polish`, `/critique`, `/animate`.
**Use it for:** establishing the token system at the start, and as the
**final design audit** before deploy — run `/audit` over each page and fix
what it flags. It explicitly catches AI tells: overused fonts, grey text on
coloured backgrounds, pure blacks, card-in-card nesting, dated easing.

### `taste-skill` suite — layout taste + style direction
Installed as a suite of skills under `.agents/skills/`:
- **`design-taste-frontend`** — the all-rounder. Stronger layout,
  typography, motion and spacing instead of boilerplate UIs. The default
  taste skill for any build.
- **Style variants** — pick the one matching the brand brief's palette
  mood and positioning:
  - `minimalist-ui` — clean editorial, warm monochrome, flat bento grids.
  - `high-end-visual-design` — expensive agency feel; exact fonts, spacing,
    shadows, animation.
  - `industrial-brutalist-ui` — raw, gridded, high type-contrast.
- **`redesign-existing-projects`** — when revising or upgrading an existing
  build rather than starting fresh.
- **Image-direction skills** (`imagegen-frontend-web`, `brandkit`, etc.) —
  see [`06-imagery.md`](06-imagery.md).

### How to apply them

1. **Start:** read `impeccable` + `design-taste-frontend`. Pick one
   `taste-skill` style variant matching the brand mood. Read `emil-design-eng`.
2. **Build:** keep their rules beside you. When a default instinct conflicts
   with a skill, the skill wins.
3. **Audit:** before deploy, run `impeccable`'s `/audit` (and `/critique`)
   over every page. Fix every flagged AI tell. This is mandatory, not optional.

---

## Colour tokens

From the brand brief's palette mood, define **6–7 named tokens** in the
Tailwind theme. Shape (names will vary by mood):

| Role | Notes |
|---|---|
| background | tinted off-white or off-dark — never pure |
| foreground | near-black or near-white — never pure, never grey-on-colour |
| primary accent | used sparingly — CTAs, active states |
| secondary accent | optional; at most one more (warm/cool counterpart) |
| muted text | secondary copy |
| section band | a slightly shifted background for alternating sections / form bg |
| line | a 1px divider colour |

Use the `impeccable` colour guidance (tinted neutrals, OKLCH reasoning,
contrast) to set exact values. Avoid pure black/white/saturated colour.

## Typography

Two fonts via `next/font/google`, each as a `--font-{x}` CSS variable.
Declare variable axes/weights/styles in the font config (not via `<link>`).

Type rules:
- Italic is reserved for emphasis in display headings and pull-quotes —
  never decoration.
- Body letter-spacing `-0.005em` to `-0.01em`; display tracking `-0.025em`
  to `-0.035em`.
- Hero size: `clamp(2.5rem, 6.5vw, 5.75rem)` via a `.display` utility.
- Eyebrows: `text-xs uppercase tracking-[0.2em]+`.
- All headings `text-balance`; feature-block body `text-pretty`.

## Layout & spacing

- Container: `max-w-[1280px]` (`max-w-container`).
- Gutters: `px-6 sm:px-8 lg:px-16`.
- Vertical rhythm: section padding `py-24` mobile → `lg:py-32`, and
  `lg:py-40` for hero-adjacent sections.
- Hero: `h-[100svh] min-h-[640px]`.

## Motion

- **Scroll fade-up** on most sections: `y: 24 → 0`, `opacity: 0 → 1`,
  700ms `[0.22,1,0.36,1]`, `viewport={{ once: true, margin: '-80px' }}`.
- **Card image hover:** `scale 1.04`, ~1200ms ease-out.
- **Draggable handles:** `whileTap={{ scale: 1.05 }}`, no spring.
- **Hero:** no parallax — static image, slight gradient overlay. Parallax
  reads gimmicky.
- Respect `prefers-reduced-motion` (the design skills enforce this).

## Component-class utilities (`globals.css`, `@layer components`)

- `.container-x` — max-width + responsive padding.
- `.display` / `.display-italic` — hero/section title size + display font.
- `.eyebrow` — uppercase tracked label.
- `.body-lg` — large lead paragraph.
- `.btn` `.btn-primary` `.btn-ghost` — CTA button variants (add an accent
  variant if the palette has a second accent).
- `.link-underline` — animated underline-from-left hover.
- `.rule` — 1px horizontal divider.

---

## The design quality bar (non-negotiable)

A build is not done until it clears all of these — verified by running
`impeccable`'s `/audit`:

- No AI-default fonts used without a stated reason.
- No grey text on a coloured background; no pure black or pure white.
- No card-inside-card-inside-card nesting.
- No dated bounce easing; motion is calm and purposeful.
- Real visual hierarchy — one clear focal point per section.
- Generous, consistent spacing from a single scale.
- Responsive from 320px up; tested at 375px.
- Accessible: focus states, contrast, reduced-motion, semantic HTML5.

Premium, modern, elegant. Subtle animation, proper spacing, clear
hierarchy. One accent colour. No emoji icons. No generic gradients.
