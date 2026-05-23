# 07 — Content

Every word on the site is in the client's voice. Before writing a single
sentence, build the client's voice profile from the `references/` templates.
Generic copy is the fastest way to make a premium-looking site feel cheap.

---

## Step 0 — build the voice profile (do this first)

The `references/` folder holds four templates. For each new client, fill
them in — they become the client's working reference files:

| File | What it captures |
|---|---|
| [`references/voice.md`](references/voice.md) | Who is writing, sentence rhythm, words they use and never use, AI tells |
| [`references/humour.md`](references/humour.md) | How (and how much) the brand uses humour — or whether it does at all |
| [`references/opinions.md`](references/opinions.md) | The brand's specific, defensible opinions about its industry |
| [`references/stories.md`](references/stories.md) | Real recurring anecdotes the brand can draw on |

There is also a **stats discipline**: collect the client's canonical real
numbers (prices, counts, response times, years, review scores) into a
`stats.md` or `lib/business.ts`. Every number used in copy comes from there
— never rounded, never invented.

**Read all four profiles + the stats before writing any page.** They are
non-negotiable inputs, not optional colour.

---

## `lib/business.ts` — single source of truth

The canonical business facts. Everything else imports from here:

- Name, short name, tagline.
- Contact: phone, email, address (respect any "do not publish the address"
  instruction — some clients want location kept off the site).
- Service area / locations served.
- Licence / registration numbers, ABN, founding year.
- Review score + count.
- Hours.

## `lib/content.ts` — all core-site copy

The only file most clients need rewritten. It exports:

- `studio` / `business` — `{ name, short, tagline, email, phone, area }`.
- `nav` — array of `{ href, label }`.
- `home` — hero copy, intro, pullQuote, testimonial.
- `coreServices` — six `{ id, title, body, icon }` for the home grid.
- `services` — six `{ id, title, body, image }` for the services page.
- `process` — four `{ n, title, body, image }` for the process page.
- `portfolio` — four `{ id, title, location, year, image }` cards.
- `recentJobs` — eight `{ image, caption }` for the home slider.
- `beforeAfters` — three `{ id, label, before, after }` (omit for
  non-transformation niches).
- `about` — `{ lead, body[], pullQuote, values[3] }`.
- `contact` — `{ title, lead }`.

---

## Voice rules (apply to every page, every post)

These hold regardless of niche. The per-client `references/` files layer
specifics on top.

1. **Start with the answer.** No "in today's fast-paced world" preamble.
2. **Real numbers over adjectives.** Not "very fast" — "under 60 minutes".
   Not "fair prices" — the actual price. Never round, never invent.
3. **One specific opinion per page**, backed by a number or a story (from
   [`references/opinions.md`](references/opinions.md)).
4. **At most one story per page** (from [`references/stories.md`](references/stories.md)).
   Never invent a new one.
5. **Tell people when NOT to hire the business.** The single biggest tell
   that a real human wrote the copy. Generic AI never does this.
6. **Headings are statements, not labels.** Not "Our process" — "Three
   steps. No surprises."
7. **Italic in display type for emphasis only** — never decoration.
8. **Pull-quote lines stand alone** — quotable, declarative, often slightly
   contrarian.
9. **Banned everywhere:** "unlock", "leverage", "seamless", "world-class",
   "best-in-class", "cutting-edge", "in today's fast-paced world", "we
   pride ourselves on", "contact us"/"reach out", exclamation marks, emojis.
10. **Match the humour profile.** If the brand uses humour, every long-form
    page must land it at the cadence [`references/humour.md`](references/humour.md)
    sets. If the brand does not, keep it dry and let precision carry it.

Before shipping any writing, re-read [`references/voice.md`](references/voice.md)
→ "Tells that it's AI-written" and delete anything that matches.

---

## Tagline & heading shapes

**Tagline:** principle — 3–5 words, declarative, no exclamation, states
what the business is *for*. See the tagline shape menu in
[`02-brand-decisions.md`](02-brand-decisions.md) Decision 4 — riff on a
shape, don't copy one.

**Heading principle (the only invariant):** **headings are statements,
not labels.** Not "Our process" — say what's actually true about it.

Heading shape menu — pick a *different* shape per long page, and don't
default to the same shape across builds:

| Shape | Example |
|---|---|
| `[Verb], [italic descriptor].` | "We design and build gardens that *settle into a place.*" |
| `[Number], [italic subject].` | "Six services, *one team.*" |
| Question | "Why do toilets run?" |
| Imperative | "Call the kettle. Then the plumber." |
| Two-part with em dash | "Sunday emergencies — quoted in writing, on the phone." |
| Single short noun | "Pricing." |
| Contrarian | "Cheaper isn't cheaper." |
| Annotated micro-headline | "Step 03 // The handover" |
| Fragment | "Mowed. Edged. Gone before lunch." |

Do not use the same heading shape twice on one long page. Do not use the
same dominant heading shape as the last build for the same niche.

---

## Keyword-driven pages — on-page SEO

Every blog post and service page generated from the CSV must satisfy
on-page SEO. For each page:

- **Research the top-3 SERP** for the primary keyword. Match the dominant
  format (list / tutorial / guide / comparison). Match the length to within
  20% of the top-3 median.
- **Cover every topic** the top-3 all cover, plus **1–2 they miss**.
- **Answer the main query directly** in the first paragraph; primary
  keyword in the first ~100 words (featured-snippet bait).
- **TL;DR callout** near the top — 2–4 sentences, reads well standalone.
- **H2s are statements**, mirroring SERP consensus + the novel sections.
- **FAQ section** with 6–8 entries, questions mined from "People Also Ask".
- **3–5 internal links** (descriptive anchors) + **2–3 external links** to
  authoritative sources (`rel="noopener nofollow"`, `target="_blank"`).
- **Schema (JSON-LD):** `Article`, `BreadcrumbList`, `FAQPage`, `Person`
  (author). Service/location pages add `LocalBusiness`.
- **Meta:** title 55–60 chars (keyword near the start), description
  150–160 chars, Open Graph + Twitter Card, canonical URL.
- **Author byline + card**, table of contents with anchor links,
  breadcrumbs.
- One story, one opinion, one "when not to hire us" moment — same as core
  pages.

After drafting, run the AI-tells checklist from
[`references/voice.md`](references/voice.md) once more before shipping.
