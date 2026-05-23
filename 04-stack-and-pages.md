# 04 — Stack & Pages

The **stack** is invariant — same Next.js / TypeScript / Tailwind / Resend
stack every build. The **page architecture is not** — it's picked per
build from the archetype menu below, driven by the CSV intent split and
the brand brief.

---

## Stack (invariant)

- **Next.js 15** — App Router, Node runtime. **Not** static export — the
  lead-form API route needs Node runtime.
- **TypeScript** — strict.
- **Tailwind CSS 3.4** — NOT v4. Use `@tailwind` directives in `globals.css`.
- **Framer Motion 11** — scroll fade-ins, slider physics, hover transforms.
- **React Hook Form + Zod** — form with a shared client/server schema.
- **next/font/google** — fonts via `--font-{name}` CSS variables.
- **Resend** REST API — direct `fetch`, no SDK.
- **No database.** Leads → email. Vercel serverless storage is ephemeral;
  do not waste effort on SQLite or file persistence. See
  [`09-pitfalls-and-checklist.md`](09-pitfalls-and-checklist.md).

Install:
```bash
npm i next@^15.1.0 react@^19.0.0 react-dom@^19.0.0 framer-motion@^11.11.17 react-hook-form@^7.53.2 zod@^3.23.8
npm i -D typescript @types/node@^22 @types/react@^19 @types/react-dom@^19 tailwindcss@^3.4.15 postcss autoprefixer
```

`next.config.mjs`:
```js
export default { reactStrictMode: true };
```
No `images.remotePatterns` — all images are local in `/public/images/`.
No `output: 'export'` — the form route needs Node runtime.

---

## File structure (illustrative — the page set is per-build)

```
app/
  layout.tsx              fonts, header, footer, metadata, sitewide schema
  globals.css             Tailwind layers + generated type / token / component classes
  page.tsx                Home (if the architecture has one)
  api/quote/route.ts      POST handler → email via Resend (if the build has a lead form)
  sitemap.ts  robots.ts   technical SEO artifacts
  <page-archetype>/page.tsx ... one folder per page in the chosen architecture
components/
  site/   layout / chrome components (Header, Footer, etc.)
  <page-archetype>/   the section components for each page
  blog/    BlogBody, TableOfContents, etc. (if Blog is in the architecture)
  seo/     JSON-LD components
lib/
  content.ts              core-site copy (shape varies with the architecture)
  business.ts             single source of truth for the business facts
  validation.ts           Zod schema for the form
  images.ts               local image manifest, typed slot keys
  email.ts                Resend sender
  markdown.ts             minimal markdown renderer (if long-form pages exist)
references/                voice, humour, opinions, stories — built per client
public/
  images/                 locally-hosted JPGs
.env.local.example         RESEND_API_KEY, LEAD_TO_EMAIL, optional LEAD_FROM_EMAIL
```

The `app/` folders shown above are illustrative. The actual route set is
the **page architecture** committed in [`02-brand-decisions.md`](02-brand-decisions.md)
Decision 5.

**Per-build variation lives only in** `lib/content.ts`, `lib/business.ts`,
`lib/images.ts`, `lib/email.ts` (from-address), the `references/` files,
the generated Tailwind tokens, the generated `globals.css` utility
classes, and the chosen component variants. **Tuned infrastructure
internals** (form validation regex, before/after pointer math, email
deliverability template structure) stay constant.

---

## Page-archetype menu

Pick from this menu in [`02`](02-brand-decisions.md) Decision 5. **There
is no canonical page set.** Two builds rarely have the same page list.

| Archetype | Purpose | When to include | Section ingredients (compose, don't sequence) |
|---|---|---|---|
| **Home** | Landing for navigation + top-of-funnel | Almost always (omit only for single-page builds) | Hero · what-we-do · proof · evidence · CTA · social-proof · navigation aids |
| **Single-page** | Everything on one route with anchors | Single dominant product or service, single-CSV | Hero · ingredients · proof · pricing · FAQ · CTA |
| **Services hub** | List & route into service detail | Multiple service offerings, commercial CSV | Hero · service list · why-us · CTA |
| **Service-leaf** | One page per primary service keyword | Each commercial keyword in the CSV | Hero · scope · pricing · proof · related-services · CTA |
| **Location-leaf** | One page per primary location keyword | Each `[trade] [suburb]` keyword in the CSV | Hero · area served · local trust signals · price · CTA |
| **Process / How it works** | Demystify the engagement | Considered purchases, advisory niches | Steps · timelines · "what we need from you" · CTA |
| **Work / Portfolio / Case studies hub** | Showcase outputs | Visual-proof niches, agencies | Hero · gallery / index · CTA |
| **Case study (leaf)** | Deep proof for one project | Agencies, advisory, design studios | Brief · approach · outcome · supporting visuals |
| **About** | Trust, the people, the why | Most builds, but place it *after* "what we do" | Story · values · team · place · CTA |
| **Pricing** | Transparency about cost | When price is a differentiator | Tiers / flat prices · what's included · FAQ · CTA |
| **FAQ** | Answer the long tail | PAA-rich CSV, advisory niches | Grouped Q&A · CTA · links to related pages |
| **Blog / Journal / Insights** | Long-form content hub | Informational-heavy CSV | Index · taxonomy / filters · feature |
| **Blog post (leaf)** | One per informational primary keyword | Each informational keyword in the CSV | Direct answer · TL;DR · sections per SERP topic · FAQ · CTA |
| **Resources** | Lead magnets, downloads, tools | Advisory / B2B | Index · CTA per resource |
| **Pricing calculator / tool** | Interactive lead capture | Quote-driven niches | The tool · explanation · CTA |
| **Contact / Book** | Convert | Almost always | Lead form · contact info · hours · trust signals |

### How to compose pages from ingredients

Each archetype lists *section ingredients*, not a fixed flow. **Pick the
ingredients needed for this build, then arrange them per the layout
archetype** (Decision 6). The same ingredients in a centred-minimal
arrangement vs. a bento-grid vs. a brutalist-utilitarian arrangement
produce three very different pages.

### The one composition rule

For any page longer than the hero, **the page must answer "what do you
do for me" before "who are we"**. The visitor's first question is
capability. Whatever order the sections appear in, capability comes
first. This is the *only* ordering constraint — there is no canonical
seven-section home flow.

If the brand brief insists on team / "about" content high on the home
page, place it *after* the capability and proof answers as a brief note
linking to a dedicated About page.

---

## Example architectures (illustrative — never copy)

These exist only to show how different they should be.

- **Emergency trade** (commercial-heavy CSV): Home · Services hub · ~10
  Service-leaves · ~10 Location-leaves · Contact.
- **Editorial branding agency** (low-volume expertise CSV): Work · ~6
  Case studies · About · Journal · Contact.
- **Advisory / accountant** (PAA-rich informational CSV): Home · Services
  · Insights (Blog hub) · ~20 Blog posts · Pricing · About · Contact.
- **Single-product business**: Single-page · 2–3 supporting blog posts.

The right architecture is the one the CSV asked for. The wrong move is to
pick the one the *last build* used.

---

## SSG / rendering constraints — do not break these

- No `cookies()`, `headers()`, or `searchParams` in server components.
- No `fetch(..., { cache: 'no-store' })` or `dynamic = 'force-dynamic'`
  except in the single Node-runtime form route.
- No runtime API routes beyond that form handler.
- Dynamic routes (`[slug]`) must implement `generateStaticParams`.
- All page data resolved at **build time**.
- Every route pre-renders — confirm `○ (Static)` in the build log.
