# Website Playbook

A reproducible, niche-agnostic playbook for building a premium marketing
website for **any single-location service or product business** — a plumber,
a dentist, a law firm, a cafe, a landscaper, an accountant, a gym, a
boutique agency. The process is the same; only the inputs change.

Distilled from real builds (the *Mother Natures* / *Verto* landscaping sites
and the *Plumbing Co* SEO site) and generalised so one folder serves every
client.

---

## The one hard gate — read this first

**This playbook does not build anything until a keyword strategy CSV is
present.** The CSV is the spine of the whole site: it decides which pages
exist, which blog posts get written, and which service/location pages get
built. No CSV, no build.

Before scaffolding a project, generating a page, or writing a post:

1. Look for a keyword strategy CSV in the working directory (any `*.csv`
   whose columns match the spec in [`01-keyword-strategy.md`](01-keyword-strategy.md)).
2. **If it is missing — stop. Do not scaffold, do not write copy, do not
   generate images.** Tell the user:
   > "I need a keyword strategy CSV to build the site around. Drop it in
   > the project folder and I'll continue."
3. Only proceed without a CSV if the user **explicitly overrules** the gate
   (e.g. "skip the CSV, just build a placeholder home page"). If overruled,
   say so plainly and proceed.

The user supplies a different CSV every time. Never reuse a previous
client's CSV. Full rules: [`01-keyword-strategy.md`](01-keyword-strategy.md).

---

## How to use this folder

Read it top-to-bottom once. Then build in this order:

| # | Step | Doc |
|---|------|-----|
| 0 | **Confirm the keyword CSV exists** (gate) | [`01-keyword-strategy.md`](01-keyword-strategy.md) |
| 1 | Decide brand: positioning, palette, type, tagline | [`02-brand-decisions.md`](02-brand-decisions.md) |
| 2 | Scaffold project + theme; load design skills | [`03-design-system.md`](03-design-system.md), [`04-stack-and-pages.md`](04-stack-and-pages.md) |
| 3 | Generate imagery (longest task — kick off early) | [`06-imagery.md`](06-imagery.md) |
| 4 | Write `lib/content.ts` in the client's voice | [`07-content.md`](07-content.md), [`references/`](references/) |
| 5 | Build components and pages | [`05-components.md`](05-components.md), [`04-stack-and-pages.md`](04-stack-and-pages.md) |
| 6 | Wire backend (lead form → email) | [`08-backend-and-deploy.md`](08-backend-and-deploy.md) |
| 7 | Deploy and smoke-test | [`08-backend-and-deploy.md`](08-backend-and-deploy.md) |

Every keyword-driven page (blog posts, service pages, location pages) is
generated *after* the core site, one per primary keyword in the CSV.

---

## Folder map

```
website-playbook/
  README.md                  this file — overview, CSV gate, build order
  01-keyword-strategy.md      THE GATE — CSV format, how the site is built around it
  02-brand-decisions.md       brand sliders + auto-deciding theme/type/colour
  03-design-system.md         design system, design quality bar, the design skills
  04-stack-and-pages.md       tech stack, file structure, page architecture
  05-components.md            per-component implementation notes
  06-imagery.md               image generation (AI + stock), prompt patterns
  07-content.md               lib/content.ts, voice rules, copy templates
  08-backend-and-deploy.md    lead form backend, deploy pipeline, smoke test
  09-pitfalls-and-checklist.md hard-won pitfalls + per-client customisation checklist
  references/
    voice.md                  how to build a client voice profile (+ worked example)
    humour.md                 how to build a humour profile (+ worked example)
    opinions.md               how to build an opinions profile (+ worked example)
    stories.md                how to build a stories profile (+ worked example)
  .agents/skills/             installed design skills (see 03-design-system.md)
```

---

## Core principles (the whole folder in six lines)

1. **No keyword CSV, no build.** The CSV defines the site.
2. **The agent decides the brand.** Theme, palette, typography and tagline
   are inferred from the business and the keywords — not asked for. See
   [`02-brand-decisions.md`](02-brand-decisions.md).
3. **Design quality is non-negotiable.** Three installed design skills set
   the bar; AI-default looks are banned. See [`03-design-system.md`](03-design-system.md).
4. **Every word is in the client's voice.** Build a voice/humour/opinions/
   stories profile per client before writing a sentence. See [`references/`](references/).
5. **Real numbers, real specifics, honest framing.** Never round, never
   invent, tell people when *not* to hire the business.
6. **Ship it.** `npm run build` green, deployed, smoke-tested — or it's not done.
