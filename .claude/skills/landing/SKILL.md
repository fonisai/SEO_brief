---
name: landing
description: Generate a new commercial-intent landing page for 1 Tap Cut. Picks an unused Commercial keyword from `VDOediting-keyword.csv`, researches the top-3 SERP, builds an honest comparison page, fetches Pexels images, renders with the `/v4` template, and marks the keyword used. Use when the user types `/landing` or asks for a new landing / comparison / "best X" page.
---

# `/landing` — Commercial-intent landing page generator

Creates a production-ready landing page end-to-end. Output: a new route at `/app/services/<slug>/page.tsx`, a content file at `/content/landing-<slug>.ts`, Pexels images under `/public/images/services/<slug>/`, and updated `references/used-keywords.md`. Must pass `npm run build` before reporting done.

> **This skill replaced a city+service local-SEO generator.** 1 Tap Cut is a global mobile app with no locations, no phone number, and no service areas — there is no local NAP, no `LocalBusiness` schema, and no map. If you find yourself parsing a city out of a keyword, you are following the old skill. Stop.

The commercial keywords in this category are **comparison and "best X for Y" queries**. The page that wins them is the one that answers honestly, including where a competitor is the better pick.

---

## Inputs

- No args → pick the highest-value **unused Commercial** primary from `VDOediting-keyword.csv`.
- User-supplied keyword (e.g. `/landing best video editing apps for iphone`) → use that exact phrase. Verify it exists in the CSV and is not already in `references/used-keywords.md`. If either check fails, stop and ask.

---

## Read these files before doing anything

1. `CLAUDE.md` — project rules (SSG constraints, voice rules, which files are LEGACY).
2. `on-page-seo.md` — every applicable item, **plus the adapted conversion checklist in step 8 below**, which supersedes the file's local-business "Conversion Elements" section.
3. `references/voice.md` — especially "Naming competitors" and "Calls to action".
4. `references/humour.md` — landing pages run drier than blog posts. Zero humour near a price is the rule; zero humour on the whole page is acceptable.
5. `references/stats.md` — pricing, limits, privacy, and the **"Do not claim"** list. This is the file that governs everything on a commercial page.
6. `references/opinions.md` — "when not to use 1 Tap Cut" is a required element here, not an optional one.
7. `references/used-keywords.md` — the 12-row landing-page inventory and what's taken.
8. `content/landing-baldwin-park.ts` — reference content **shape** only. LEGACY sample from a different business; copy the structure, never the copy, and delete every local-NAP field.
9. `app/v4/page.tsx` — the template you will copy, and heavily strip.

**Calibration read:** `app/blog/content/opusclip-vs-submagic-vs-descript-comparison.mdx` in the root project. That post is the model for how we handle competitor pricing — verified, dated, and with rows we lose.

---

## Step-by-step workflow

### 1. Pick the primary keyword

Read `VDOediting-keyword.csv` and `references/used-keywords.md` → "Landing-page inventory".

If no arg:
- Parse the CSV, skip the header. Column order: `Database,Keyword,Seed keyword,Page,Topic,Page type,Tags,Volume,Keyword Difficulty,CPC (USD),Competitive Density,Number of Results,Intent,SERP Features,Trend,Click potential,Content references,Competitors`.
- Keep `Intent` = `Commercial`. There are **12 such rows**; they are the entire inventory.
- Drop anything already a primary in `used-keywords.md`, and check "Near-neighbour warnings".
- Rank by `CPC (USD)` descending (commercial intent: high CPC = high buyer value), tie-break by `Volume / (KD + 1)`.
- Announce: `Picking "<keyword>" (vol <V>, KD <KD>, CPC $<C>).` — in auto mode, proceed immediately.

**Watch the near-duplicates.** `best iphone video editing app` / `best iphone video editing apps` and `best video editing app for ipad` / `best video editing software ipad` are the same page. Cluster them; never build two.

### 2. Classify the page type

Commercial keywords in this category resolve to one of three shapes. Pick one and commit:

| Shape | Trigger | Structure |
|---|---|---|
| **Best-X-for-Y** | `best video editing apps for iphone` | Ranked shortlist, 4–6 tools, one "best for" verdict each |
| **Beginner / entry** | `video editing software for beginners` | What to actually learn first, then tools sorted by learning curve |
| **Price-led** | `affordable video editing software`, `low cost video editing software` | Real cost table, free tiers and what their watermarks cost you |

`slug` = kebab-case of the keyword, under 60 chars (e.g. `best-video-editing-apps-for-iphone`).

### 3. Build the keyword cluster

Same-intent commercial variations, max 5. CSV matches first (mark `✓ CSV`), then invented from typical Related-Searches patterns (mark `(invented)`). Same-intent test applies: would this searcher want this same page?

### 4. Research the top-3 SERP

`WebSearch` the primary keyword, then `WebFetch` the top 3 organic results. Extract:

- **How many tools they list**, and whether the list is ranked or unordered.
- **Whether they show real prices** — most don't, or they're stale. That's the gap.
- **Whether they disclose affiliate relationships.** Many of these pages are affiliate roundups where the ranking is bought. Naming that dynamic once, plainly, is a legitimate angle.
- **Their publish and update dates.** Anything describing Sora as live, or quoting 2025 pricing, is out of date.
- **Any tool category they skip entirely** (on-device editors, mobile-native tools).
- **FAQ blocks and People-Also-Ask questions.**

### 5. Verify every price on the page

This is the step that decides whether the page is worth publishing.

- **Our numbers** → `references/stats.md`. Starter $0, Pro $15/mo or $12/mo annually ($144/yr), Studio $39/mo or $30/mo annually ($360/yr). Quote monthly and annual together.
- **Every competitor's numbers** → `WebFetch` their own pricing page **today**. Record the URL and the date. Do not reuse the July 2026 figures in `stats.md` without re-checking — see "Perishable numbers".
- **Put the verification date on the page**, visibly, near the table. "Pricing verified <Month Year>."
- If a price can't be verified, the tool goes in the table with the cell left as "see vendor" and a link. Never guess.

### 6. Build the comparison table honestly

The table is the page. Rules:

- **We do not win every row.** If the table has no row where a competitor is the better answer, it's fake and readers can tell. Include a row we lose — 4K on the free tier, desktop timeline control, multicam, colour.
- **One "best for" verdict per tool**, in plain language: CapCut for templates, Resolve for colour and long-form, Descript for transcript-driven editing, OpusClip for pulling clips from long uploads, 1 Tap Cut for phone footage to finished vertical on-device.
- **Include the billing model as a column**, not just the price. Per-minute-of-source billing behaves very differently from flat monthly — see `stories.md` → "The billing-model trap".
- **Watermark status on every free tier.** All of them have one, ours included. Say so.
- **Platform row:** 1 Tap Cut is iOS and Android native; OpusClip and Submagic are browser tools; Descript is Mac and Windows desktop. On an iPhone/iPad keyword this row is the whole argument, so it must be accurate.

### 7. Write the content file

Create `/content/landing-<slug>.ts`. Shape (derived from `landing-baldwin-park.ts`, with every local field removed):

```ts
// <slug> — commercial-intent landing page targeting "<primary keyword>".
// Competitor pricing verified <Month Year>. Re-verify before any edit.

export const <camelCaseSlug>Landing = {
  slug: "<slug>",
  pageType: "best-x-for-y" | "beginner" | "price-led",
  primaryKeyword: "<primary>",
  keywordCluster: [ /* 5 entries */ ],
  title: "<50–60 chars, primary keyword near the start>",
  description: "<150–160 chars>",
  publishedAt: "<YYYY-MM-DD>",
  pricingVerifiedOn: "<YYYY-MM>",
  hero: { h1, subhead, primaryCta: "Join the waitlist" },
  tools: [
    {
      name, bestFor, platform,
      price: { monthly, annual, freeTier },
      billingModel: "flat monthly" | "per minute of source" | "credits" | "free",
      watermarkOnFree: true | false,
      sourceUrl, verifiedOn,
    },
    // ... 4–6 tools, one of which is 1 Tap Cut, not in first position
  ],
  whenNotToUseUs: [ /* 3–5 concrete cases, from opinions.md */ ],
  faqs: [ /* 6–8, schema-ready */ ],
};

export type <PascalSlug>Landing = typeof <camelCaseSlug>Landing;
```

**Never add:** `phone`, `address`, `serviceAreas`, `hours`, `licenseNumber`, `jobsCompleted`, `review`, `testimonials`. None of them exist for this product, and inventing them is exactly what `stats.md` → "Do not claim" forbids.

### 8. Conversion elements — adapted checklist

`on-page-seo.md` → "Conversion Elements" was written for a local service business. **This list supersedes it for landing pages.** (The file itself still needs updating; flag it, don't silently rewrite it.)

- [ ] **Primary CTA above the fold** — join the waitlist. Not `tel:`, there is no phone number.
- [ ] **Multiple CTA placements** — hero, after the comparison table, closing section.
- [ ] **Beta status stated near every CTA** — the app is in closed beta. Never imply instant availability.
- [ ] **The waitlist offer stated once** — free week of Pro, 20% off the first year.
- [ ] **Trust signals that are actually true** — flat pricing shown in full, the on-device privacy model, the verification date on the pricing table, and the "when not to use us" section. That's the set.
- [ ] **No testimonials.** The homepage ones are placeholder copy (`stats.md`). Do not reuse, rewrite, or invent any.
- [ ] **No ratings, user counts, awards, or "trusted by"** — none exist.
- [ ] **App Store / Google Play badges** where the root site uses them.

### 9. Fetch Pexels images

Edit `scripts/fetch-pexels.mjs`, adding an entry keyed by slug. Landing pages need fewer images than posts:

- `hero` — landscape, someone filming or editing on a phone.
- `comparison` — optional, a desk/laptop editing setup for the table section.
- `waitlist` — closing CTA image.

Useful queries: `smartphone filming`, `creator vlogging`, `video editing laptop`, `phone tripod recording`.

The script writes to `/public/images/blog/<slug>/` by default. Either generalise it (add a `type` field so it writes to `/public/images/<type>s/<slug>/` and namespaces `content/pexels.json`) or reference the images from the blog path — the second is purely cosmetic and fine.

Run `node scripts/fetch-pexels.mjs`, then verify every key is present and no `✗` lines appeared.

### 10. Create the route

Copy `app/v4/page.tsx` to `app/services/<slug>/page.tsx`, then **strip it hard** — v4 is a local service page and most of it does not apply:

- Import the new content file (alias it `page` or similar).
- **Delete** the local NAP block, the service-areas list, the business-hours block, every `tel:` link, and the testimonials section.
- **Replace** the `Plumber` / `LocalBusiness` JSON-LD entirely with:
  - `SoftwareApplication` — `name: "1 Tap Cut"`, `applicationCategory: "MultimediaApplication"`, `operatingSystem: "iOS, Android"`.
  - `Offer` entries for Starter / Pro / Studio with the real prices and `priceCurrency: "USD"`.
  - `FAQPage` for the FAQ section.
  - `BreadcrumbList` — `Home › Services › <page title>`.
  - **No `aggregateRating`.** We have no ratings; emitting a fake one is schema spam and a manual-action risk.
- `metadata.title.absolute` — 50–60 chars. `metadata.description` — 150–160.
- `metadata.alternates.canonical` and `metadata.openGraph.url` — `"/services/<slug>/"`. `locale: 'en_US'`.
- Swap the feature cards for the comparison table and the "when not to use us" section.

SSG guard: no `cookies()`, `headers()`, `searchParams`, no `dynamic = 'force-dynamic'`.

### 11. Update the services index

Edit `app/services/page.tsx`. Add an entry pointing at `/services/<slug>/` with the page title and a one-line description.

### 12. Mark the keyword used

Append a section to `references/used-keywords.md` under "Active primaries": primary + CSV source line (volume, KD, CPC, intent), the route, published date, the secondary cluster with `✓ CSV` / `(invented)` tags, and the pricing verification month.

### 13. Verify

```
npm run build
```

Every route shows `○ (Static)`, no build errors. Then check the generated HTML:

- `<title>` 50–60 chars with the primary keyword; `<meta name="description">` 150–160.
- H1 contains the primary keyword.
- JSON-LD present for `SoftwareApplication`, `Offer`, `FAQPage`, `BreadcrumbList` — and **no `LocalBusiness`, no `aggregateRating`**.
- **Grep the output for `tel:`, for any street address, and for the legacy business name — all must return nothing.**
- Every price in the table has a source link and the verification date is visible on the page.
- The comparison table contains at least one row where a competitor wins.
- The "when not to use 1 Tap Cut" section renders.
- "Closed beta" appears near the primary CTA.

Then `npm run dev`, browse `/services/<slug>/` at mobile width: CTA thumb-reachable, no horizontal scroll, table scrolls inside its own container.

---

## Output summary (report at the end, ~10 lines)

- Primary keyword + volume/KD/CPC.
- Page type chosen.
- Cluster.
- Top-3 organic competitors + what they skip that we cover.
- Tools compared, and which row we lose.
- Date all competitor pricing was verified.
- Slug + URL: `/services/<slug>/`.
- Files created/changed.
- Build status.

---

## Anti-patterns (do not do any of these)

- **Don't** parse a city out of a keyword or generate local NAP. There are no locations.
- **Don't** emit `LocalBusiness`, `Plumber`, or any local schema, and never emit `aggregateRating`.
- **Don't** invent a keyword that isn't in `VDOediting-keyword.csv`, or reuse a primary from `used-keywords.md`.
- **Don't** publish a competitor price without a link and a verification date.
- **Don't** build a comparison table where 1 Tap Cut wins every row.
- **Don't** invent testimonials, ratings, user counts, or results, and never reuse the homepage placeholder testimonials.
- **Don't** put a phone number, `tel:` link, address, business hours, or service area on the page.
- **Don't** imply general availability. The app is in closed beta.
- **Don't** use puns, exclamation marks, emoji, or any banned word from `voice.md`.
- **Don't** ship without running `npm run build`.
- **Don't** mutate `/app/v4/page.tsx` or `/content/landing-baldwin-park.ts` — copy, don't edit.
- **Don't** introduce runtime APIs or any SSG-breaking pattern.
