# 09 — Pitfalls & Customisation Checklist

Hard-won lessons, a per-client build checklist, what must change every
build, and what to leave alone.

---

## Pitfalls discovered the hard way

1. **No keyword CSV = no build.** The most important rule in the folder.
   See [`01-keyword-strategy.md`](01-keyword-strategy.md). Don't "be
   helpful" and build a generic site without it.

2. **Running `npm run build` while `npm run dev` is up corrupts `.next/`.**
   The dev server then serves stale HTML with broken CSS hashes and the page
   renders unstyled. Fix: stop dev, `rm -rf .next`, restart. Build only when
   dev is not running.

3. **A transparent header over a dark hero makes the nav invisible.** Solid
   translucent backdrop from frame 1. See [`05-components.md`](05-components.md).

4. **A database on Vercel breaks on every cold start.** The serverless
   filesystem is ephemeral. For a lead form, email-via-Resend is correct.
   See [`08-backend-and-deploy.md`](08-backend-and-deploy.md).

5. **The image-edit endpoint may return PNG from a JPEG input.** Next.js
   `<Image>` reads file headers, not extensions — leave it, don't convert.

6. **Stock photography is mostly mismatched for service niches.** Generate
   imagery by default. See [`06-imagery.md`](06-imagery.md).

7. **The Zod honeypot field must be `.optional()`, never `.max(0)`.**
   Rejecting the honeypot in Zod `400`s the bots and tips them off. The
   route does the silent-200. See [`08-backend-and-deploy.md`](08-backend-and-deploy.md).

8. **The hero `<section>` needs top padding equal to the header height on
   mobile** when the header is solid — otherwise the hero subject is occluded.

9. **Load fonts via `next/font/google`, not `<link>` tags.** Variable axes
   and italic styles must be declared in the font config.

10. **Image generation rate-limits around 4–5 parallel requests.** Generate
    sequentially. See [`06-imagery.md`](06-imagery.md).

11. **`gpt-image-1` cannot render legible text or precise logos.** Always
    add "no text, no logos, no watermarks" to the style prefix.

12. **Faces are uncanny — generate the work and the result, not people.**
    See [`06-imagery.md`](06-imagery.md).

13. **Generic copy sinks a premium design.** Build the `references/` voice
    profile before writing. See [`07-content.md`](07-content.md).

14. **AI-default design tells make a site look cheap.** Run `impeccable`'s
    `/audit` and fix every flag before shipping. See
    [`03-design-system.md`](03-design-system.md).

15. **Copying a previous site's design is the single biggest cause of
    sameness.** Use the *stack*, never the *look*. Reusing
    `lib/validation.ts`, the email lib, the form route, the build config
    is fine — those are tuned infrastructure. Reusing tokens, components,
    a page set, motion timings, or `globals.css` from a previous build is
    a failure of the Design Direction step. See
    [`02-brand-decisions.md`](02-brand-decisions.md).

16. **Picking from the menus in `02` without justification produces
    sameness too.** "Premium specialist + editorial-asymmetric + Fraunces"
    every time *is* the AI default in this playbook's clothing. Each of
    the eight decisions must be a real decision, with a stated reason
    grounded in the CSV and brief — not the first menu item that fits.

---

## What must change every build

If you finish a build and *none* of these has changed from the previous
build, the Design Direction step in [`02`](02-brand-decisions.md) has been
skipped. Every one of these is a per-build choice:

- **Page architecture** — the actual list of pages and how the site is
  composed from the archetype menu in [`04`](04-stack-and-pages.md).
- **Layout archetype** (Decision 6) — editorial / minimal / bento /
  magazine / full-bleed / brutalist / split / list / scroll-narrative.
- **Palette tokens** — generated via OKLCH per `impeccable`, not picked
  from a fixed table.
- **Type pairing + scale** — display + body faces, modular ratio, base
  size, eyebrow tracking, optical-size / italic axes.
- **Hero treatment** — full-bleed / typographic / split / video / minimal
  / editorial-magazine / asymmetric-overlap.
- **Motion personality** — calm / lively / dramatic / mechanical, with
  generated easing and durations.
- **Component variant choices** — Header, Services section, Proof, CTA,
  Form, Carousel, PullQuote treatment.
- **Spacing scale** — base unit + ratio, generated; container width
  chosen in range.
- **Heading shapes** — picked from the menu in
  [`07-content.md`](07-content.md), not the same shape as last build.
- **Image direction** — one of the four prefix options in
  [`06-imagery.md`](06-imagery.md), consistent across the build.
- **Tagline shape** — picked from the menu in [`02`](02-brand-decisions.md),
  not the same shape as last build for the same niche.
- **Tailwind tokens, generated `globals.css`, component class names** —
  these are *output* of the design choices and must regenerate every build.

---

## What to leave alone

These are tuned *non-visual* internals. Reuse them across builds; don't
touch them unless the client explicitly needs a change. **None of these
affect how the site looks.**

- `BeforeAfter.tsx` pointer math (the clip-path + pointermove/pointerup
  logic). The *visual* treatment of `BeforeAfter` is per-build; the
  pointer physics are tuned.
- `QuoteForm` wiring — RHF + Zod resolver setup, the silent-200 honeypot
  trick, the rate-limiting map, the `AnimatePresence` swap pattern. The
  *visual* form variant is per-build.
- `lib/validation.ts` regex and length limits — phone/email patterns
  generalised across locales.
- `lib/email.ts` Resend `fetch` shape and `reply_to` wiring. The HTML
  email *template skeleton* (text + html, header / field-list / footer
  structure) stays; the styling matches the build's design.
- The `app/api/quote/route.ts` flow (validate → honeypot → send).
- SSG-safety patterns (`generateStaticParams` for dynamic routes, no
  `cookies()` / `headers()`).
- The `next.config.mjs` shape (no remote images, no `output: 'export'`).
- The deploy pipeline in [`08`](08-backend-and-deploy.md).

If you find yourself reusing anything *outside* this list from a
previous build — that's the sameness creeping in. Stop, go back to
[`02`](02-brand-decisions.md).

---

## Customisation checklist for a new client

In order, with rough time estimates:

| Step | Time | Where |
|---|---|---|
| **Confirm the keyword CSV is present** (gate) | — | [`01`](01-keyword-strategy.md) |
| Read the CSV; report intent split, niche, top keywords | 10 min | [`01`](01-keyword-strategy.md) |
| **Commit the 8-decision Design Direction** (positioning, palette, type, tagline, page architecture, layout archetype, hero treatment, motion personality) | 30 min | [`02`](02-brand-decisions.md) |
| **Scaffold fresh** — `npx create-next-app` etc. from the stack. Reuse only the non-visual internals from "what to leave alone". **Do not copy a previous site.** | 10 min | [`04`](04-stack-and-pages.md) |
| Generate the build's design system (tokens, type scale, spacing, motion timings) | 20 min | [`03`](03-design-system.md) |
| Wire fonts via `next/font/google` per the type choice | 5 min | [`03`](03-design-system.md), [`04`](04-stack-and-pages.md) |
| Build the `references/` voice profile + stats | 30 min | [`references/`](references/) |
| Write `lib/business.ts` + `lib/content.ts` for the actual page list | 60 min | [`07`](07-content.md) |
| Pick the image direction; write prompts; run the gen script | 15 + 15 min | [`06`](06-imagery.md) |
| Run the before/after edit pass (if the build uses it) | 5 min | [`06`](06-imagery.md) |
| Visual audit; reroll any weak images | 10–20 min | [`06`](06-imagery.md) |
| Pick component variants; assemble pages per the layout archetype | varies | [`05`](05-components.md), [`04`](04-stack-and-pages.md) |
| Generate keyword pages (blog / service / location) | varies | [`01`](01-keyword-strategy.md), [`07`](07-content.md) |
| `impeccable` `/audit` + `/critique` pass; fix flags | 15 min | [`03`](03-design-system.md) |
| **Divergence self-check** — compare this build's brief side-by-side with the last build. If more than 3 of the 8 axes match, re-derive. | 10 min | [`02`](02-brand-decisions.md) |
| Build, test routes, test form | 5 min | [`08`](08-backend-and-deploy.md) |
| `gh repo create … --push`; Vercel import | 6 min | [`08`](08-backend-and-deploy.md) |
| Smoke test | 5 min | [`08`](08-backend-and-deploy.md) |

Core site ≈ 4–5 hours; keyword pages are then incremental.

---

## Definition of done

- The keyword CSV gate was honoured.
- The 8-decision Design Direction was committed and is *materially
  different* from the last build's brief on every axis.
- `npm run build` is green; every route shows `○ (Static)`.
- `impeccable` `/audit` is clean; the design quality bar in
  [`03-design-system.md`](03-design-system.md) is met.
- Copy is in the client's voice; the AI-tells checklist is clean.
- Deployed, and all smoke-test items pass.

Never say "done" while the build fails, the console errors, the design
audit flags issues, the copy reads as AI, or the build's design brief
overlaps the last build's by more than three of the eight axes.
