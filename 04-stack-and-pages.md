# 04 — Stack & Pages

The technical foundation and the page architecture. Both are niche-agnostic
— the same stack and the same 6-page core serve every business.

---

## Stack

- **Next.js 15** — App Router, Node runtime. **Not** static export — the
  lead-form API route needs a Node runtime.
- **TypeScript** — strict.
- **Tailwind CSS 3.4** — NOT v4. Use `@tailwind` directives in `globals.css`.
- **Framer Motion 11** — scroll fade-ins, slider physics, hover transforms.
- **React Hook Form + Zod** — form with a shared client/server schema.
- **next/font/google** — variable fonts via `--font-{name}` CSS variables.
- **Resend** REST API — direct `fetch`, no SDK (one less dependency).
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

## File structure

```
app/
  layout.tsx              fonts, Header, Footer, metadata, sitewide schema
  globals.css             Tailwind layers + base type + component classes
  page.tsx                Home
  services/page.tsx
  process/page.tsx        (may be renamed: "How it works", etc.)
  portfolio/page.tsx      (may be renamed: "Gallery", "Recent work", "Case studies")
  about/page.tsx
  contact/page.tsx        (may be renamed: "Book a visit", "Get a quote")
  blog/page.tsx           blog index
  blog/<slug>/page.tsx    one per informational keyword (generated)
  <service-or-city>/page.tsx  one per commercial keyword (generated)
  api/quote/route.ts      POST handler → email via Resend
  sitemap.ts  robots.ts   technical SEO artifacts
components/
  site/Header.tsx         fixed top nav, always-solid backdrop
  site/Footer.tsx         dark band, three columns
  site/FadeIn.tsx         scroll-reveal wrapper
  site/BeforeAfter.tsx    bespoke draggable slider (omit if niche has no before/after)
  site/JobsSlider.tsx     horizontal snap-scrolling carousel
  site/SixServices.tsx    icon grid of capabilities
  site/Cta.tsx            band CTA
  site/PullQuote.tsx      large italic display quote
  portfolio/ProjectCard.tsx
  contact/QuoteForm.tsx   RHF + Zod, optimistic UI
  blog/                   BlogBody, Breadcrumbs, TableOfContents, AuthorCard
  seo/                    JSON-LD components (LocalBusiness, Article, FAQ, Breadcrumb)
lib/
  content.ts              ALL core-site copy — main file to edit per client
  business.ts             single source of truth for the business facts
  validation.ts           Zod schema for the form (shared with the API route)
  images.ts               local image manifest, typed slot keys
  email.ts                Resend sender
  markdown.ts             minimal markdown renderer for blog/service body copy
references/                voice, humour, opinions, stories — built per client
public/
  images/                 locally-hosted JPGs
.env.local.example         RESEND_API_KEY, LEAD_TO_EMAIL, optional LEAD_FROM_EMAIL
```

**Hard rule:** per-client variation lives ONLY in `lib/content.ts`,
`lib/business.ts`, `lib/images.ts`, `lib/email.ts` (from-address), the
`references/` files, and the Tailwind theme tokens. Component files are
reused verbatim across clients.

---

## Pages — the 6-page core

Every site ships the same six core pages plus a blog index. Names adapt to
the niche; structure does not.

| Path | Title | Section flow |
|---|---|---|
| `/` | Home | Hero · **SixServices** (or work gallery) · Featured proof · JobsSlider · Selected projects · Testimonial · CTA |
| `/services` | Services | Hero · 6× alternating image+text blocks · CTA |
| `/process` | Process / How it works | Hero · 4× numbered steps · CTA |
| `/portfolio` | Portfolio / Work / Case studies | Hero · 2-up grid · proof · 2-up grid · proof · CTA |
| `/about` | About | Hero · wide image · narrative · pull quote · 3 values · service area · CTA |
| `/contact` | Contact / Book / Quote | Hero · 2-col: info left, form right |
| `/blog` | Blog index | Hero · post grid (populated as posts are generated) |

All pages: the header is fixed and overlays `<main>` (no offset); inner
pages get top padding `pt-40 lg:pt-48` to clear it.

### Home page section order — services before "about us"

A standing temptation is to put an "about the team" intro right under the
hero. **Resist it.** The visitor's first question is "can you do the thing
I need?" — not "who are you?". The second section answers *what the
business does*: the SixServices grid or a work gallery.

Canonical home order:

1. **Hero** — tagline + primary CTA.
2. **SixServices** (or work gallery) — "here is what we do".
3. **Featured proof** — a before/after, a case study, a flagship result.
4. **Recent work slider** — more proof.
5. **Selected projects grid** — more proof.
6. **Testimonial** — social proof.
7. **CTA band** — convert.

If the client insists on prose about themselves on the home page, place it
*after* the testimonial as a brief two-sentence note linking to `/about`.

---

## Keyword-driven pages

Built after the core site, one per primary keyword from the CSV (see
[`01-keyword-strategy.md`](01-keyword-strategy.md)):

- **Blog post** → `app/blog/<slug>/page.tsx` + `content/blog-<slug>.ts`.
  Research the top-3 SERP, match format and length, FAQ schema from PAA.
- **Service / location page** → `app/<slug>/page.tsx`, modelled on the home
  page but rewritten for the commercial keyword and (if local) the city.

Both must keep SSG-safe patterns — see the constraints below.

---

## SSG / rendering constraints — do not break these

- No `cookies()`, `headers()`, or `searchParams` in server components.
- No `fetch(..., { cache: 'no-store' })` or `export const dynamic = 'force-dynamic'`.
- No runtime API routes beyond the single Node-runtime form handler.
- Dynamic routes (`[slug]`) must implement `generateStaticParams`.
- All page data is resolved at **build time**, not request time.
- Every route must pre-render — confirm `○ (Static)` in the build log.
