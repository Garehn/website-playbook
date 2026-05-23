# 06 — Imagery

The site lives or dies on its photos. Stock photography for most service
niches is largely mismatched filler. **Default to generating imagery; use
curated stock only as a deliberate fallback.**

Two image-direction skills are installed (via the `taste-skill` suite) and
should be consulted before generating:
- **`imagegen-frontend-web`** — premium, conversion-aware website image
  direction; one image per section, composition variety, one consistent
  palette across all images.
- **`brandkit`** — for brand-guideline boards, logo systems and identity
  imagery if the client needs a brand mark.

---

## Cost & time

- A full core site needs ~26 hero/feature shots + ~3 matched after-edits ≈
  29 images. Generated images cost roughly **$2–4 USD total** at
  `quality: 'medium'`, ~12 minutes wall-clock at ~25s/image.
- **Run sequentially.** The image API rate-limits around 4–5 parallel
  requests — a parallel script will fail partway. One at a time.

---

## Generation — two passes

Use OpenAI `gpt-image-1`.

**Pass 1 — generate (most images):** `POST /v1/images/generations`,
`model: 'gpt-image-1'`:
- `1536x1024` (landscape) — hero, services, process steps, before/after.
- `1024x1536` (portrait) — portfolio / case-study cards.

**Pass 2 — edit (matched before/after pairs only):** `POST /v1/images/edits`
with the BEFORE jpeg as input. The edit endpoint preserves the subject's
identity; without it you get two different houses / rooms / faces.

`output_format: 'jpeg'`, `output_compression: 85` → ~250–350KB files, good
for the web. Avoid `quality: 'low'` — it breaks the realism look; `medium`
is the sweet spot.

---

## Image direction — pick one per build

Image direction varies every build. Pick **one** direction in the Design
Direction commit ([`02-brand-decisions.md`](02-brand-decisions.md)) and
apply the same prefix to every prompt in the build. Mixing directions
within a single site is the fastest way to make it look AI-generated.

Four direction prefixes. None of them is the default — choose the one
that fits the layout archetype, the niche, and the brand brief.

### A — Documentary film-grain (cinematic-amateur)

Best for: trades, hospitality, lifestyle, food, anywhere a sense of "real
work happening" carries the brand.

```
Shot on 35mm film, slight grain, natural ambient lighting, imperfect
composition, slightly overexposed highlights, muted colour palette, no
retouching, real environmental context with everyday clutter and wear,
shallow depth of field, shot by an amateur photographer documenting
their work. No legible text, no logos, no watermarks.
```

### B — Editorial clean (magazine-quiet)

Best for: agencies, advisory, premium specialists, editorial brands.

```
Editorial photograph, large-format aesthetic, calm composition with
generous negative space, controlled diffuse studio or window light,
neutral colour palette with one quiet accent, sharp focus across the
frame, no grain, considered framing — feels printed in a quarterly
magazine. No legible text, no logos, no watermarks.
```

### C — Studio-lit product (commercial-precise)

Best for: product businesses, e-commerce, single-product builds, food
where the object is the hero.

```
Studio product photograph, controlled three-point lighting on a seamless
backdrop, accurate colour, crisp microcontrast, soft shadow grounding
the subject, slight specular highlight where the material warrants it,
no environmental clutter. Single subject framed with deliberate space.
No legible text, no logos, no watermarks.
```

### D — Brutalist / utilitarian (document-flat)

Best for: industrial, technical, contrarian, brutalist-archetype builds.

```
Flat utilitarian documentation photograph, even fluorescent or daylight
flood, no atmosphere, no shallow depth of field — everything in focus,
hard shadows allowed, desaturated colour, frontal or right-angle
composition. The subject reads like a record, not a mood. No legible
text, no logos, no watermarks.
```

The 35mm-film prefix is one option, not the default — using it on every
build is the very sameness this playbook exists to prevent.

### People — avoid by default

`gpt-image-1` produces uncanny faces, hands and postures. **By default,
generate the work and the result, not the worker / staff / customer.** Show
the outcome and the tools or environment, not a person.

Append to the style prefix: `ABSOLUTELY NO PEOPLE, NO HUMAN FIGURES, NO
HANDS, NO BODY PARTS.`

If a niche genuinely needs a human presence (e.g. a gym, a salon), prefer
photographed-from-behind, hands-and-torso-only, or face-obscured framing —
or use curated stock for those specific slots.

---

## Image slot inventory (adapt the subjects to the niche)

| Slot | Aspect | Subject — written for the niche |
|---|---|---|
| `hero` | landscape | The signature scene of the business at its best |
| `intro` | landscape | A quiet, specific beauty shot — detail, light, texture |
| `service_*` (6) | landscape | One scene per core service — the result, not the worker |
| `process_*` (4) | landscape | The four process steps as still scenes |
| `about` | landscape | The premises / vehicle / workspace, no people |
| `portfolio_1..4` | portrait | Four representative results, varied in character |
| `slider_1..6` | landscape | Six recent-work moments, mostly visual |
| `ba{1,2,3}_before` | landscape | A neglected/before scene with strong identity cues |
| `ba{1,2,3}_after` | landscape | Generated by the EDIT endpoint from the matching `_before` |

For a non-transformation niche, drop the `ba*` slots and add more
`portfolio_*` or detail shots.

## Prompt patterns that work

- Ground every prompt in the real location ("a suburban [city] context").
- Name specific, real-world objects and materials the niche uses — concrete
  nouns read as authentic; generic nouns read as AI.
- Specify light: "late afternoon warm light", "soft mid-morning",
  "golden hour", "dappled light".
- Add texture: scuffs, wear, weathering, things half-done or mid-use.
- Keep one consistent palette across all images (the brand palette mood).

## Pitfalls

- **No legible text in images.** `gpt-image-1` cannot render crisp text or
  precise logos. Always include "no legible text, no logos, no watermarks".
- **Faces are uncanny.** See the People section above.
- The edit endpoint may return PNG even from a JPEG input. Next.js `<Image>`
  reads file headers, not extensions — leave it; don't convert.

---

## Blog & keyword-page imagery

For generated blog and service pages, either generate per-section images
the same way, or fetch curated stock from Pexels via a small fetch script
(`scripts/fetch-pexels.mjs` pattern). Each post needs a `hero`, one image
per H2 section, and generic "FAQ" / "contact" images. Whichever route, keep
the realism bar and the one-consistent-palette rule.

## Image manifest (`lib/images.ts`)

A typed map of `slot → { src, alt }`. `src` is `/images/{slot}.jpg`. Keep
alt text honest and descriptive — not keyword-stuffed.
