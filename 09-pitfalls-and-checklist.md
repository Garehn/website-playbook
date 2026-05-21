# 09 — Pitfalls & Customisation Checklist

Hard-won lessons, a per-client build checklist, and the list of things to
leave alone.

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

---

## Customisation checklist for a new client

In order, with rough time estimates:

| Step | Time | Where |
|---|---|---|
| **Confirm the keyword CSV is present** (gate) | — | [`01`](01-keyword-strategy.md) |
| Read the CSV, summarise, infer the niche | 10 min | [`01`](01-keyword-strategy.md) |
| Decide positioning, palette, type, tagline | 15 min | [`02`](02-brand-decisions.md) |
| Scaffold the project (copy a reference build) | 5 min | [`04`](04-stack-and-pages.md) |
| Load the design skills; set Tailwind tokens | 15 min | [`03`](03-design-system.md) |
| Swap fonts in `app/layout.tsx` + config | 5 min | [`03`](03-design-system.md), [`04`](04-stack-and-pages.md) |
| Build the `references/` voice profile + stats | 30 min | [`references/`](references/) |
| Write `lib/business.ts` + `lib/content.ts` | 60 min | [`07`](07-content.md) |
| Write image prompts; run the gen script | 15 + 15 min | [`06`](06-imagery.md) |
| Run the before/after edit pass | 5 min | [`06`](06-imagery.md) |
| Visual audit; reroll any weak images | 10–20 min | [`06`](06-imagery.md) |
| Draw the 6-icon `SixServices` set for the niche | 15 min | [`05`](05-components.md) |
| Swap the Header wordmark + email "from" name | 5 min | [`05`](05-components.md), [`08`](08-backend-and-deploy.md) |
| Generate keyword pages (blog / service / location) | varies | [`01`](01-keyword-strategy.md), [`07`](07-content.md) |
| `impeccable` `/audit` pass; fix flags | 15 min | [`03`](03-design-system.md) |
| Build, test routes, test form | 5 min | [`08`](08-backend-and-deploy.md) |
| `gh repo create … --push`; Vercel import | 6 min | [`08`](08-backend-and-deploy.md) |
| Smoke test | 5 min | [`08`](08-backend-and-deploy.md) |

Core site ≈ 3.5–4 hours; keyword pages are then incremental.

---

## What to leave alone

These are tuned across multiple builds. Don't touch them unless the client
explicitly needs a change:

- `BeforeAfter.tsx` slider physics.
- `JobsSlider.tsx` snap-scroll math.
- `globals.css` component utilities (`.btn`, `.eyebrow`, `.link-underline`,
  `.display`).
- `FadeIn.tsx` motion timings — calibrated to feel calm.
- The 6-page core structure — it fits every business at this tier.
- The email HTML template — renders correctly in Gmail, Apple Mail, Outlook.
- `lib/validation.ts` limits — covers common phone/email shapes.

Per-client work belongs in `lib/content.ts`, `lib/business.ts`,
`lib/images.ts`, `lib/email.ts` (from-address), the `references/` files,
and the Tailwind tokens — nowhere else.

---

## Definition of done

- The keyword CSV gate was honoured.
- `npm run build` is green; every route shows `○ (Static)`.
- `impeccable` `/audit` is clean; the design quality bar in
  [`03-design-system.md`](03-design-system.md) is met.
- Copy is in the client's voice; the AI-tells checklist is clean.
- Deployed, and all eight smoke-test items pass.

Never say "done" while the build fails, the console errors, the design
audit flags issues, or the copy reads as AI.
