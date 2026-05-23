# 01 — Keyword Strategy (the gate)

The keyword strategy CSV is the **single required input**. It is the spine
of the site. This document is the gate: nothing gets built before the CSV
is confirmed present and read.

---

## The gate — enforce this before any build action

A "build action" is: scaffolding a project, creating a route, writing a
content file, generating images, or producing a blog/service/location page.

**Before any build action:**

1. **Look for the CSV.** Scan the working directory for a `*.csv` file
   whose header matches the schema below. The user supplies a fresh file
   for every site — never assume a previous client's CSV applies.
2. **If no matching CSV is found — STOP.** Do not scaffold. Do not write
   placeholder copy. Do not generate images. Respond with exactly this
   intent:
   > "I can't build the site yet — I need a keyword strategy CSV to build
   > it around. Please drop the CSV into the project folder and tell me
   > the business name and what it does, and I'll continue."
3. **If the user explicitly overrules** ("skip the CSV", "just build a
   one-page placeholder", "I'll add keywords later") — acknowledge the
   override out loud, then proceed with a clearly-marked placeholder build.
   The gate yields to an explicit instruction; it never yields to silence.
4. **If a CSV is found** — read it fully, confirm the column mapping, and
   summarise back to the user what you found (row count, intent split,
   top keywords) before building.

Do not guess keywords, invent a niche, or proceed "to be helpful" when the
CSV is absent. An empty-handed build is worse than a paused one.

---

## What the CSV is for

The CSV is a keyword research export (typically from Semrush, Ahrefs, or a
similar tool). Each row is a search query a real person types. The site is
architected so that **every high-value keyword has a page that targets it.**

The CSV drives four things:

| CSV signal | Drives |
|---|---|
| The **niche / seed keyword** | The whole brand: positioning, palette, voice, imagery |
| **Intent split across the CSV** | The site's **page architecture** (see below) |
| **Informational** intent rows | Blog posts (one post per primary keyword cluster) |
| **Commercial / transactional** intent rows | Service pages and city/location landing pages |
| **Volume** and **Keyword Difficulty** | Build priority — high volume ÷ low difficulty first |

One CSV may contain everything, or the user may supply separate files
(e.g. a blog-keyword CSV and a service-keyword CSV). Handle either.

### The CSV also decides what pages exist

The CSV doesn't just feed individual pages; it also drives the **whole
page architecture** of the site, which feeds into
[`02-brand-decisions.md`](02-brand-decisions.md) Decision 5.

- A **commercial-heavy CSV** (lots of `[trade] [suburb]`, "near me",
  "emergency [service]") implies a build with a thin core + many
  service-leaf and location-leaf pages — and possibly no Blog at all.
- An **informational-heavy CSV** (questions, "how to", "cost of",
  "best …") implies a blog-led structure: a small core site + a deep
  Blog with topic clusters.
- An **even split** implies both: core + service leaves + a blog.
- A **single dominant query** (e.g. one product, one service) implies a
  single-page site with a deep FAQ and one or two long-form articles.

Summarise the intent split when you report the CSV back (next section)
and hand the implied architecture to [`02`](02-brand-decisions.md).

---

## Expected schema

The playbook is built against a Semrush-style export. Expected columns
(case-insensitive; extra columns are fine, missing ones are tolerated):

| Column | Required | Use |
|---|---|---|
| `Keyword` | **yes** | The exact search phrase the page targets |
| `Intent` | strongly preferred | `Informational` → blog · `Commercial`/`Transactional` → service/location page |
| `Volume` | preferred | Monthly searches — ranks build priority |
| `Keyword Difficulty` (`KD`) | preferred | Ranking difficulty — ranks build priority |
| `Seed keyword` | preferred | The niche anchor — feeds brand inference |
| `SERP Features` | optional | `People Also Ask` present → mine PAA for FAQ schema |
| `CPC` | optional | Commercial value signal — high CPC = worth a dedicated page |
| `Page` / `Page type` / `Topic` / `Tags` | optional | Pre-grouping, if the user has done it |

If the supplied CSV uses different column names, **map them explicitly and
confirm the mapping with the user** before building. Do not silently guess.

If the CSV has no `Intent` column, infer intent from the phrase:
- Contains a place name, "near me", "in [city]", "[trade] [suburb]" →
  treat as **commercial** (service/location page).
- A question or "how to / what is / why does / cost of" →
  treat as **informational** (blog post).

---

## How the CSV becomes a site

### 1. Brand inference (once, up front)

Read the `Seed keyword` / dominant niche and the spread of keywords. This
is the raw material for [`02-brand-decisions.md`](02-brand-decisions.md) —
the agent infers positioning, palette, typography and tagline from it.

### 2. Core site (built once)

The 6-page core site (Home, Services, Process, Portfolio/Work, About,
Contact) plus a Blog index. See [`04-stack-and-pages.md`](04-stack-and-pages.md).

### 3. Keyword-driven pages (one per primary keyword)

For each primary keyword, in priority order (`Volume ÷ (KD + 1)` descending):

- **Informational keyword** → a blog post. Build a cluster of one primary
  + 4–5 same-intent secondaries (from the CSV first; invent the rest from
  People-Also-Ask patterns). Research the top-3 SERP, match format and
  length, cover every topic the top-3 share plus 1–2 they miss.
- **Commercial keyword** → a service page or a city/location landing page,
  modelled on the home page but rewritten for that keyword.

### 4. Track what's used

Maintain a `used-keywords.md` (or equivalent) in the project. Every time a
primary keyword is turned into a page, record it: the keyword, its
volume/KD, the page URL, and the secondary cluster. **Never target the
same primary twice.** Before picking a keyword, check this file.

---

## Choosing the next keyword

When the user asks for "another page" / "another post" without naming a
keyword:

1. Read the CSV and the used-keywords log.
2. Drop every keyword already used.
3. Drop keywords whose intent doesn't match the page type requested
   (informational for blog, commercial for service/location).
4. Rank the rest by `Volume ÷ (KD + 1)` descending. Prefer rows whose
   `SERP Features` lists `People Also Ask` (free FAQ-schema questions).
5. Announce the pick in one line — `Picking "<keyword>" (vol <V>, KD <KD>)` —
   and proceed unless the user has signalled a preference.

If the user names a keyword, verify it exists in the CSV (case-insensitive
exact match on `Keyword`) and is not already used. If either check fails,
stop and ask.

---

## Reporting back

After reading a fresh CSV, before building, give the user a 4–5 line summary:

- Row count and the niche/seed keyword detected.
- Informational vs commercial split.
- The 3 highest-priority keywords by `Volume ÷ (KD + 1)`.
- What you propose to build first.
