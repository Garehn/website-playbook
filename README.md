# Website Playbook

A reproducible, niche-agnostic playbook for building a premium marketing
website for **any single-location service or product business** — a plumber,
a dentist, a law firm, a cafe, a landscaper, an accountant, a gym, a
boutique agency. The process is the same; the design is not.

---

## The hard rule about how sites built from this playbook should look

**Two sites built from this playbook must differ on every visual axis.**
What stays constant is the quality bar, the voice discipline, the keyword
CSV gate, the lead-form backend, and the SSG constraints. What changes
every build is the page architecture, the layout archetype, the
components, the palette, the typography, the motion personality and the
hero treatment. The playbook gives *principles* and *generation methods*;
it does not give a template to copy.

If you can look at two builds and tell they came from the same playbook,
something has been copied that should have been freshly chosen. See
[`02-brand-decisions.md`](02-brand-decisions.md) (the eight design
decisions that must be re-made every build) and
[`09-pitfalls-and-checklist.md`](09-pitfalls-and-checklist.md) ("what
must change every build").

---

## The one hard gate — read this first

**This playbook does not build anything until a keyword strategy CSV is
present.** The CSV is the spine of the whole site: it decides which pages
exist, which blog posts get written, and which service/location pages get
built. It also seeds the brand and design-direction choices in
[`02`](02-brand-decisions.md). No CSV, no build.

Before scaffolding a project, generating a page, or writing a post:

1. Look for a keyword strategy CSV in the working directory (any `*.csv`
   whose columns match the spec in
   [`01-keyword-strategy.md`](01-keyword-strategy.md)).
2. **If it is missing — stop. Do not scaffold, do not write copy, do not
   generate images.** Tell the user:
   > "I need a keyword strategy CSV to build the site around. Drop it in
   > the project folder and I'll continue."
3. Only proceed without a CSV if the user **explicitly overrules** the
   gate. If overruled, say so plainly and proceed.

The user supplies a different CSV every time. Never reuse a previous
client's CSV. Full rules: [`01-keyword-strategy.md`](01-keyword-strategy.md).

---

## How to use this folder

Read it top-to-bottom once. Then build in this order:

| # | Step | Doc |
|---|------|-----|
| 0 | **Confirm the keyword CSV exists** (gate) | [`01-keyword-strategy.md`](01-keyword-strategy.md) |
| 1 | Decide brand: positioning, palette, type, tagline | [`02-brand-decisions.md`](02-brand-decisions.md) |
| 2 | **Commit a Design Direction** (page architecture, layout archetype, hero treatment, motion personality) | [`02-brand-decisions.md`](02-brand-decisions.md) |
| 3 | Generate the build's design system from the direction | [`03-design-system.md`](03-design-system.md) |
| 4 | Scaffold project from the stack (not from a previous site) | [`04-stack-and-pages.md`](04-stack-and-pages.md) |
| 5 | Generate imagery in the direction's image style (longest task — kick off early) | [`06-imagery.md`](06-imagery.md) |
| 6 | Build the per-build voice profile; write `lib/content.ts` | [`07-content.md`](07-content.md), [`references/`](references/) |
| 7 | Compose pages from the archetype menu; pick component variants | [`04-stack-and-pages.md`](04-stack-and-pages.md), [`05-components.md`](05-components.md) |
| 8 | Wire backend (lead form → email) | [`08-backend-and-deploy.md`](08-backend-and-deploy.md) |
| 9 | Deploy and smoke-test | [`08-backend-and-deploy.md`](08-backend-and-deploy.md) |

Every keyword-driven page (blog posts, service pages, location pages) is
generated *after* the core site, one per primary keyword in the CSV.

---

## Folder map

```
website-playbook/
  README.md                  this file — overview, CSV gate, build order
  01-keyword-strategy.md      THE GATE — CSV format; also drives page architecture
  02-brand-decisions.md       brand + the 8-decision Design Direction commit
  03-design-system.md         principles, generation methods, the design skills
  04-stack-and-pages.md       stack (invariant) + page-archetype menu (per-build)
  05-components.md            component kit — purposes + variant menus
  06-imagery.md               image generation; image direction varies per build
  07-content.md               voice rules + heading-shape menu
  08-backend-and-deploy.md    lead form backend, deploy pipeline, smoke test
  09-pitfalls-and-checklist.md what to leave alone vs. what must change every build
  references/
    voice.md                  how to build a client voice profile (+ worked example)
    humour.md                 how to build a humour profile (+ worked example)
    opinions.md               how to build an opinions profile (+ worked example)
    stories.md                how to build a stories profile (+ worked example)
  .agents/skills/             installed design skills (see 03-design-system.md)
```

---

## Core principles (the whole folder in seven lines)

1. **No keyword CSV, no build.** The CSV defines the site and seeds the
   design direction.
2. **The agent decides the brand AND the Design Direction.** Eight decisions
   in [`02`](02-brand-decisions.md), made fresh every build. No defaults.
3. **Two sites must differ on every visual axis.** Page architecture,
   layout archetype, hero, components, palette, typography, motion.
4. **Design quality is non-negotiable.** Installed design skills set the
   bar; AI-default looks are banned. See
   [`03-design-system.md`](03-design-system.md).
5. **Every word is in the client's voice.** Build the voice profile per
   client before writing a sentence. See [`references/`](references/).
6. **Real numbers, real specifics, honest framing.** Never round, never
   invent, tell people when *not* to hire the business.
7. **Ship it.** `npm run build` green, deployed, smoke-tested, and
   `impeccable`'s `/audit` clean — or it's not done.
