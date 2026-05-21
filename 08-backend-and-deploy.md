# 08 — Backend & Deploy

The only backend is a lead form that emails the business. No database.
Then: ship to GitHub + Vercel, and smoke-test before handing over.

---

## Backend — lead form

### `app/api/quote/route.ts`

- `runtime = 'nodejs'`, `dynamic = 'force-dynamic'` (this single route is
  the reason the project is not static export).
- In-memory rate-limit map: 5 submissions per IP per 10 minutes. Imperfect
  on serverless (per-instance) but better than nothing.
- Flow: Zod validate → honeypot check → `sendLeadEmail`.
- **Honeypot:** the Zod schema must allow the honeypot field as a plain
  `.optional()` string — **not** `.max(0)`. The route does the silent-200
  trick: if the honeypot is filled, return `200` and send nothing. If Zod
  rejected the honeypot you would `400` the bots, which advertises the trick
  and invites a retry.

### `lib/email.ts`

- Plain `fetch` to `https://api.resend.com/emails` — no SDK.
- Env vars: `RESEND_API_KEY`, `LEAD_TO_EMAIL`, optional `LEAD_FROM_EMAIL`.
- Default `from`: `Business Name <onboarding@resend.dev>` — works without
  domain verification, but only delivers to the Resend account holder's
  address until a domain is verified.
- Send BOTH `text` and `html` versions. The HTML uses the site's display
  font for headers and small uppercase-tracked field labels — it should
  feel like the site, not a default template.
- `reply_to: data.email` so hitting reply goes straight to the lead.

### `lib/validation.ts`

The Zod schema, shared by the client form and the API route. Covers the
form fields with sensible length limits and locale-appropriate phone/email
shapes. Tuned — leave it unless the client needs different fields.

---

## Deploy — GitHub + Vercel

```bash
# 1. Local git
cd <project>
git init -b main
git add .
git commit -m "<Business name> — initial commit"

# 2. Push to GitHub via authenticated gh CLI
gh repo create <slug> --public --source=. --remote=origin \
  --description "<one-liner>" --push

# 3. Vercel (manual — the user opens the browser)
#  • https://vercel.com/new → Import the repo
#  • Add env vars:
#      RESEND_API_KEY = re_xxx  (from https://resend.com/api-keys)
#      LEAD_TO_EMAIL  = the client's address
#  • Deploy
```

The build takes ~90 seconds; the first image-heavy build can run longer
while Vercel optimises the JPEGs.

### Hosting constraints

- **No SQLite, no file storage.** Vercel's serverless filesystem is
  ephemeral and resets on every cold start. Leads go to email — that is the
  correct architecture here, not a compromise.
- **Resend's onboarding sender works** without a verified domain, but only
  sends to the account holder's email. For production-grade delivery,
  verify a domain on Resend (≈5 DNS records).
- **Custom domain** is added in the Vercel dashboard post-deploy — SSL
  auto-issues.

---

## Technical SEO artifacts

Ship these as part of the build:

- `app/sitemap.ts` — auto-generated sitemap covering all routes, including
  every keyword-driven page.
- `app/robots.ts` — allows all crawlers, points to the sitemap.
- Canonical URLs on every page (`metadata.alternates.canonical`).
- Open Graph images, 1200×630, in `/public/og/`.
- `width`/`height` on every `<Image>` (avoids layout shift / CLS).
- Semantic HTML5 — `<header> <nav> <main> <article> <footer>`.
- Mobile viewport set in `app/layout.tsx`.
- If a search-console verification snippet is supplied, place it correctly
  (the HTML verification file in `/public/`, or the meta tag in `layout.tsx`).

---

## Final smoke test (run before handing over)

After deploy, all of these must pass:

1. `curl https://<deploy>.vercel.app/` → 200, page renders.
2. `/services`, `/process`, `/portfolio`, `/about`, `/contact`, `/blog`,
   and every generated keyword page → all 200.
3. Submit the contact form with real-looking data → the lead arrives at
   `LEAD_TO_EMAIL` within ~5 seconds.
4. Submit 6× within 10 minutes → the 6th attempt `429`s.
5. Drag the before/after slider on desktop (if present) → smooth.
6. Open at 375px → header readable, hero text not clipped, sliders drag by
   touch.
7. Chrome DevTools → no console errors; run Lighthouse — aim for green
   across Performance, Accessibility, Best Practices, SEO.
8. View source of `/` and one blog post → rendered content present,
   JSON-LD blocks present, no leaked env vars, no stack traces.

When all eight pass — and `impeccable`'s `/audit` is clean (see
[`03-design-system.md`](03-design-system.md)) — it's done.
