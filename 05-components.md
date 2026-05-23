# 05 — Components (a kit of variants)

A site is not a fixed set of components — it's a set of **purposes**.
Each purpose has 2–7 distinct treatments. Pick **one variant per
purpose per build**, justified by the Design Direction in
[`02-brand-decisions.md`](02-brand-decisions.md). Two builds picking the
same variants across every purpose is a failure of design intent — go
back to `02` and re-derive.

Read [`emil-design-eng`](03-design-system.md) before building any
animated component. The interaction-feel rules in that skill are
invariant — only the visual expression varies.

## Constraints that hold across every variant (invariant)

- Accessible focus states on every interactive surface.
- Hit targets ≥ 44px on touch.
- Keyboard operable (arrow keys / Home/End where the metaphor implies it).
- Respects `prefers-reduced-motion`.
- No layout shift on hover.
- No `bg-transparent` Header over a dark hero — wordmark and nav stay
  legible from frame 1. (See Header variants.)
- Pointer-physics components (`BeforeAfter`, sliders, draggables) keep
  the tuned pointer math regardless of visual treatment.

Visual expression — colour, motion, layout, type, exact paddings — varies
per build from the chosen Design Direction.

---

## Header — pick one

| Variant | Character | Constraints |
|---|---|---|
| **Solid-backdrop horizontal** | Familiar, dependable | Solid translucent backdrop from frame 1. Height shrinks ~96→80px on scroll past ~24px. |
| **Side-rail vertical** | Editorial, agency | A thin column on desktop; folds to a top bar on mobile. |
| **Minimal-floating chip** | Quiet, premium | Small centred or top-right capsule. Background blur only; never transparent. |
| **Overlay-on-scroll-only** | Brand-led | The hero is unobstructed at top; the header materialises only after the first scroll. Must not stay transparent over content. |
| **Typographic bar** | Brutalist, document-style | Type-only, no chrome. Logo treated as set text. |

**Wordmark treatment** — also a per-build choice: an italic word inside
the wordmark is one option among many. Other treatments: small-caps,
mixed-weight, monospace, all-uppercase tracked, a tiny custom mark. Pick
one consistent with the type Decision 3.

## Hero — pick one

| Variant | Character | Pairs with |
|---|---|---|
| **Full-bleed image** | Confident, visual-proof | Trades, hospitality, lifestyle |
| **Typographic-only** | Quiet expertise, brand-led | Agencies, advisory |
| **Split (text \| image)** | Balanced, dependable | Trusted local operators |
| **Video / motion loop** | Live, kinetic | Editorial, fitness, food |
| **Minimal — no image** | Restrained, anti-brochure | Premium specialists |
| **Editorial-magazine** | Long, layered, scrollable hero | Long-form / case-study brands |
| **Asymmetric overlap** | Confident, design-forward | Agencies, design studios |

Bounds: no parallax. Hero height derived from the type scale and density
dial in [`02`](02-brand-decisions.md), not a fixed `100svh`.

## Services section — pick one

| Variant | Character | When |
|---|---|---|
| **Icon grid** (n columns, n cells — count picked per build) | Familiar, scannable | Multi-service trades |
| **Editorial 2-col rows** | Considered, magazine-like | Premium / advisory |
| **Bento grid** | Productised, dense | Multiple offerings of different scales |
| **Numbered list** | Document-style, clear | Single-page builds, brutalist |
| **Tabbed** | Comparative, dense | When services overlap and need contrast |
| **Accordion** | Compact, mobile-first | Long lists of small services |
| **Horizontal scroller** | Kinetic, editorial | Agencies, lifestyle |

The number of services shown is whatever the brief calls for — 3, 4, 6,
8, 12. The historical "six icons in a grid" is one variant, not the form.

## Proof / showcase — pick one (or layer two)

| Variant | When |
|---|---|
| **Before/after slider** | Niches with visible transformation (trades, landscaping, cleaning, cosmetic) |
| **Case-study card grid** | Agencies, advisory, design studios |
| **Results-stat strip** | Data-driven services (accountants, marketing, fitness) |
| **Testimonial-led** | Trust-led niches (legal, medical, family services) |
| **Logo marquee** | B2B, when names are the proof |
| **Interactive demo** | SaaS, tools, calculators |

`BeforeAfter` is a tuned component — keep its pointer math, but the
visual treatment (handle, label chips, aspect ratio, overlay easing) is
designed per build. Aspect ratio derived from the chosen layout density,
not a fixed `16/10`.

## CTA — pick one

| Variant | Character |
|---|---|
| **Full-bleed dark band** | Loud, conventional |
| **Contained card** | Considered, editorial |
| **Inline within a section** | Quiet, anti-pushy |
| **Sticky footer** | Single-page, conversion-led |
| **Split-panel** | Two parallel CTAs (book / call), balanced |

## Form — pick one

| Variant | Character |
|---|---|
| **Floating bottom-border** | Minimal, editorial |
| **Bordered card** | Familiar, conventional |
| **Inline** | One field at a time, conversational |
| **Modal-trigger** | When the form interrupts a flow |
| **Stepped / conversational** | Long lead-qualifying forms |

Behind every visual: React Hook Form + Zod, shared schema with the API
route. Fields and validation tuned per build (the *fields* are per-build,
the validation patterns reuse the canonical regexes in `lib/validation.ts`).

### Form internals that stay constant

- Honeypot field as `.optional()` (never `.max(0)` — see
  [`08-backend-and-deploy.md`](08-backend-and-deploy.md)).
- Success state swaps the form via `AnimatePresence` for a confirmation
  panel (the *style* of the panel varies).
- Error banner pattern on network/429/validation 400 — keep form mounted.

## Carousels / sliders — pick one if needed

If the build calls for a horizontal moment (recent work, testimonials,
logos):

- **Snap-scroll horizontal** — pointer + touch + scroll; tick indicators
  match the design language.
- **Auto-rotating marquee** — non-interactive; respects reduced-motion.
- **Drag-only with rest** — design-led, slows after the user lets go.

The historical `JobsSlider` is the snap-scroll variant — implement only
if the chosen design needs it, with slide-width / aspect / indicator
style derived from the build.

## PullQuote, FadeIn, dividers

- `PullQuote` — large display quote, used at most once per long page.
  Treatment (italic, bold, all-caps, framed, hung-punctuation) chosen
  per build.
- `FadeIn` — the scroll-reveal wrapper; uses the build's motion
  personality (Decision 8) for its timing/easing.
- Dividers — `.rule` purpose; visual treatment (1px hairline, double
  rule, oversized gap, asymmetric, none) per build.

## Blog components (used when Blog is in the architecture)

`BlogBody` (markdown render), `Breadcrumbs`, `TableOfContents`,
`AuthorCard`. Used by every generated blog post.

Constraints invariant: anchor IDs match the heading-slugifier;
breadcrumbs reflect real URL hierarchy; author card uses a real bio.
Visual treatment per build.

---

## How to commit the kit for a build

After [`02`](02-brand-decisions.md) is set, write the **variant choices**
into the brand brief — one line per purpose:

- Header: `<variant>`
- Hero: `<variant>`
- Services: `<variant>` × `<n>` items
- Proof: `<variant>`
- CTA: `<variant>`
- Form: `<variant>`
- Carousel: `<variant>` or `none`
- PullQuote treatment: `<treatment>`

Then build. If two consecutive briefs pick more than three matching
variants, the Design Direction in `02` is not doing its job — re-derive.
