# AkindaCo Education and Training — training.akindaco.com

A hand-coded static site for AkindaCo's CPD training, supervision, placements and research.
Built to be found for **"family therapy training Adelaide"** and **"art therapy training
Adelaide"**, and to be easy to update when course dates and booking links change.

Plain HTML, CSS and a little JavaScript. No build step, no dependencies. Same approach as
the PBS site at `pbs.akindaco.com`.

## Pages (19)

| URL | What it is |
|---|---|
| `/` | Training hub: courses, educators, supervision, research, FAQ |
| `/family-therapy-training-adelaide/` | Family Therapy Foundations (main landing page) |
| `/art-therapy-training-adelaide/` | Art Therapy Foundations |
| `/genogram-training-adelaide/` | The Therapeutic Use of Genograms |
| `/preschool-boost-attachment-trauma-child-development/` | Preschool Boost: ATCD |
| `/preschool-boost-learn-to-play/` | Preschool Boost: Learn to Play Group |
| `/blog/` + 5 posts | Training blog |
| `/research/` + 2 papers | Peer-reviewed publications |
| `/conference-presentations/` + 3 talks | Conference presentations |

Shared files: `styles.css`, `main.js`, `logo.png`, `favicon.svg`, `robots.txt`,
`sitemap.xml`, `llms.txt`, `CNAME`.

## Updating a course

Everything you normally need to change lives in one **BOOKING PANEL** block near the top of
each course page, marked with a comment. Three things:

1. **Dates** — the `<p class="next-course-meta">` line.
2. **Prices** — the `<ul class="price-list">` items.
3. **Booking link** — every `href` pointing at `events.humanitix.com`.

Then update the same three things in the JSON-LD `<script type="application/ld+json">`
block at the bottom of the file, under `Event` and `hasCourseInstance`. Google and AI search
read the JSON-LD, so if it disagrees with the visible page the wrong dates get shown in
results.

Also update the matching **course card** on `index.html`. Each card carries a comment
saying exactly what to change.

## Turning on booking for a course that has none yet

Art Therapy Foundations and Genograms currently point at `https://www.akindaco.com/contact`
for interest registration. When a Humanitix event goes live:

1. Replace every `akindaco.com/contact` href on that course page with the event URL.
2. Fill in the dates, and add a price list by copying the markup from the family therapy page.
3. On `index.html`, change that card's class from `is-soon` to `is-open` and swap the badge
   to `<span class="badge badge-open">Booking now</span>`.
4. Add a `hasCourseInstance` block to that page's JSON-LD.

## Adding a blog post

Copy any file in `blog/`, change the heading, body and FAQ, then update the `BlogPosting`
and `FAQPage` JSON-LD at the bottom. Then add a card to `blog/index.html`, add the URL to
`sitemap.xml` and to the `blogPost` array in `blog/index.html`, and add a line to `llms.txt`.

Every post should link to at least one course page and one other post. That internal
linking is doing real work.

## Adding a paper or presentation

Same pattern in `research/` (peer-reviewed journal articles) or `conference-presentations/`
(talks). Update the `ScholarlyArticle` JSON-LD: headline, abstract, author, datePublished,
isPartOf, and put the DOI in `sameAs`. Then add a card to that section's index, plus
`sitemap.xml` and `llms.txt`.

## Preview locally

Root-relative paths mean the folder has to be served, not opened from disk. A tiny Node
server sits at `../.claude/static-server.js`:

```bash
node .claude/static-server.js
```

Then open http://localhost:8123.

## Publishing

GitHub Pages, same as the PBS site. Push to a repo, then Settings, Pages, Deploy from
branch, `main`, root. The `CNAME` file points the site at `training.akindaco.com`, so add a
CNAME DNS record for `training` pointing at `<username>.github.io`.

**Important:** the SA Government Preschool Boost listing publicly tells services to visit
`akindaco.com/training/`, which currently 404s. After publishing, add a Squarespace URL
redirect so that link works:

```
/training -> https://training.akindaco.com/ 301
```

## SEO and AEO checklist

**Technical.** Every page has a canonical, a unique title under 75 characters, a meta
description, one `h1`, ordered headings, `lang`, viewport, and no horizontal scroll at
375px. Google Fonts is loaded non-render-blocking via `preload` plus `media="print"` swap,
with a `noscript` fallback. Heaviest first load is the hub at roughly 103 KB before fonts.

**Content.** Keyword pages are the course pages, so authority is not split across a
landing page and a course page. FAQs are phrased as the literal search query.

**AEO.** JSON-LD on every page (`Course`, `EducationEvent`, `FAQPage`, `BlogPosting`,
`ScholarlyArticle`, `BreadcrumbList`, `EducationalOrganization`). `llms.txt` gives answer
engines a clean summary. `robots.txt` explicitly allows GPTBot, ClaudeBot, PerplexityBot,
OAI-SearchBot, Google-Extended and others.

**Measurement.** Not set up yet. Add the site to Google Search Console after publishing
(DNS TXT record, or drop the HTML verification file Google gives you into this folder and
commit it), then submit `sitemap.xml` and watch clicks, impressions and average position
over months rather than days.

**Backlinks.** Not something the site can do to itself. Highest-value targets, roughly in
order: the three SA Government Preschool Boost listings (already link to
`akindaco.com/training/`, so fix that redirect first), the Humanitix event page, PACFA and
AAFT member directories, the University of Adelaide, IKON, and the main `akindaco.com`
site itself.

## Links in and out

The site links out to akindaco.com, pbs.akindaco.com, Humanitix, LinkedIn, Facebook,
Instagram, the University of Adelaide, PACFA, AAFT, AASW, ANZACATA, PTPA, IKON, Carers SA,
NDIS, DEAI, ReturnToWorkSA, the SA Office for Early Childhood Development including all
three AkindaCo Preschool Boost listings, and the DOIs of both published papers.

Links coming **in** have to be added on the Squarespace side: `/education`, plus the team,
therapy and about pages.

## Still to do

- Personal LinkedIn URLs for Rana, Kate and Shane. Placeholders are marked `TODO` in the
  educators section of `index.html` and currently point at the company page.
- Clinical sign-off on the five blog posts before they go live.
- Dates and Humanitix events for Art Therapy Foundations and Genograms.
