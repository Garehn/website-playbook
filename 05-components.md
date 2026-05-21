# 05 — Components

Per-component implementation notes. These are tuned across multiple builds.
Read [`emil-design-eng`](03-design-system.md) before building any animated
component — it governs how interactions should feel.

Most components are niche-agnostic. `BeforeAfter` only applies to niches
with a visible transformation (trades, landscaping, cleaning, renovation,
cosmetic). For other niches, swap it for a case-study card or a results
stat block — see the note in its section.

---

## `Header`

Constraints learned the hard way:

- **Solid backdrop from frame 1.** Never `bg-transparent` over a dark hero
  — the wordmark and nav go invisible. Use `bg-{background}/90 backdrop-blur-md`
  always.
- Height: `96px` at top → `80px` once scrolled past 24px. Don't go below
  80px or the nav cramps.
- Wordmark: `text-[2rem] md:text-[2.5rem]` at top, shrinking to
  `text-3xl md:text-[2.25rem]` on scroll.
- Wordmark structure: one word italicised inside the wordmark
  (`Brand <span italic>Name</span>`) — a signature touch.
- Eyebrow tag beside the wordmark (the niche, e.g. "Property Maintenance",
  "Family Dentistry"), uppercase, tracked `[0.28em]`, muted colour.
- Mobile menu: animated hamburger → X (middle line fades, top/bottom rotate
  to meet).
- Don't put "Home" in the desktop nav — the logo links to `/`. Slice the
  nav array `[1, -1]` for the middle items; render the last item (the
  primary CTA — "Book a visit" / "Request a quote") as a styled button.

## `BeforeAfter` (bespoke — do NOT use a library)

Only for niches with a visible transformation. For others, replace with a
results card (stat + short proof) or a case-study card.

- `clip-path: inset(0 calc(100% - var(--pos)) 0 0)` on the after-image overlay.
- Pointer events: `pointerdown` on the container starts the drag;
  `pointermove`/`pointerup` listeners live on `window`.
- Keyboard: `role="slider"`, arrow keys ±2%, Home/End jump to ends.
- Aspect ratio `16/10`.
- BEFORE label top-left, AFTER label top-right (small chips, opposite colours).
- Handle: circular accent chip with a chevron `< >` icon, 48px.

### Matched before/after pairs — use the image-edit endpoint

You cannot get a matched pair by writing two prompts. See
[`06-imagery.md`](06-imagery.md): generate the BEFORE first, then feed that
JPEG into the image-**edit** endpoint so the identity of the place/subject
is preserved. That preservation is the entire point of a before/after.

## `JobsSlider`

- Horizontal `flex` scroller, `overflow-x-auto snap-x snap-mandatory`.
- ~8 slides. Slide width: `82vw → 60vw → 46vw → 36vw → 32vw` across breakpoints.
- Each slide aspect `4/5`.
- Dark band, light text.
- Prev/Next pill buttons top-right of the section; disabled at the ends.
- Tick indicators below (active `w-12` solid, inactive `w-6` faded).
- Hide the native scrollbar (`scrollbar-width: none` + WebKit pseudo).
- Scroll listener updates `activeIndex` by nearest-to-centre.

## `SixServices`

- 3-col grid desktop, 2-col tablet, 1-col mobile.
- Cells separated by hairline borders (`border-{line}`).
- Each cell: circular icon (line SVG, 20×20 inside a 48px circle) + `0N`
  numeral + service title + one-line body + "Learn more →".
- Hover: cell bg faintly tints, icon circle inverts to accent-filled, arrow
  nudges right `translate-x-1`.
- Icons are inline SVG paths, keyed by service id. **Draw a fresh icon set
  per niche** — six simple, consistent line icons representing that
  business's six core services. Keep them one visual family (same stroke
  weight, same corner treatment).

## `QuoteForm`

- React Hook Form + Zod; schema in `lib/validation.ts`, shared with the API route.
- Fields: `name`, `email`, `phone`, `projectType` (select, 4–6 niche-specific
  options), `message`, hidden `honeypot`.
- Text inputs: floating-bottom-border style — no boxes, no rounded corners,
  `border-b border-{foreground}`, focus shifts to the accent colour.
- Textarea: a full single-colour border instead.
- Select: inline SVG chevron via a data-URL background image.
- Submit: primary CTA colour.
- Success: `AnimatePresence` swaps the form for a serif-italic confirmation
  in a section-band panel. No toast.
- Error (network / 429 / validation 400): red banner, keep the form mounted.

## `FadeIn`, `Cta`, `PullQuote`

- `FadeIn` — the scroll-reveal wrapper; timings in
  [`03-design-system.md`](03-design-system.md) under Motion.
- `Cta` — a section-band CTA: heading, one line, primary button.
- `PullQuote` — large italic display quote; used once per long page.

## Blog components

`BlogBody` (markdown render), `Breadcrumbs`, `TableOfContents` (anchor
links), `AuthorCard`. Used by every generated blog post. Anchor IDs must
match the heading-slugifier the `TableOfContents` expects — keep them in sync.
