---
name: blog
description: Generate a new long-form, SEO-optimised, voice-matched blog post for 1 Tap Cut. Picks an unused informational keyword from `VDOediting-keyword.csv`, researches the top-3 SERP, fetches Pexels images, renders with the `/v5` template, and marks the keyword used. Use when the user types `/blog` or asks for a new blog post.
---

# `/blog` — Blog post generator

Creates a production-ready blog post end-to-end. Output: a new route at `/app/blog/<slug>/page.tsx`, a content file at `/content/blog-<slug>.ts`, downloaded Pexels images under `/public/images/blog/<slug>/`, and updated `references/used-keywords.md`. Must pass `npm run build` before reporting done.

The product is **1 Tap Cut**, a mobile AI video editor in closed beta. The byline is **Fon Rangsanseri, Co-Founder** — a real person. Never invent biography, credentials, or anecdotes for them.

---

## Inputs

- No args → pick the highest-value **unused informational** primary from `VDOediting-keyword.csv`.
- User-supplied keyword (e.g. `/blog how to edit videos on android`) → use that exact phrase. Verify it exists in `VDOediting-keyword.csv` and is not already in `references/used-keywords.md`. If it fails either check, stop and ask before proceeding.

---

## Read these files before doing anything

1. `CLAUDE.md` — project rules (SSG constraints, voice rules, design rules, which files are LEGACY).
2. `on-page-seo.md` — every applicable item must be satisfied.
3. `references/voice.md` — sentence rhythm, banned words, competitor-naming rule, "tells it's AI-written" checklist.
4. `references/humour.md` — dry and deadpan, never puns, one moment per ~500 words, zero near any number.
5. `references/stats.md` — only use product numbers from here. Never round, never invent. Read "Perishable numbers" and "Do not claim".
6. `references/stories.md` — one narrative per post maximum. Don't invent new ones.
7. `references/opinions.md` — one strong opinion per post, backed by a number or a mechanism.
8. `references/used-keywords.md` — what's already taken, plus the near-neighbour warnings.
9. `content/blog-voiced.ts` — reference content **shape** only. It is LEGACY sample copy from a different business; copy the structure, never the prose.
10. `app/v5/page.tsx` — the template you will copy.

**Calibration read (do this, it's the highest-value step):** open `app/blog/content/can-chatgpt-edit-videos.mdx` in the root project and read it end to end. That is the target register. Every draft gets compared back to it.

---

## Step-by-step workflow

### 1. Pick the primary keyword

Read `VDOediting-keyword.csv` and `references/used-keywords.md`.

If no arg:
- Parse the CSV, skip the header row. Column order: `Database,Keyword,Seed keyword,Page,Topic,Page type,Tags,Volume,Keyword Difficulty,CPC (USD),Competitive Density,Number of Results,Intent,SERP Features,Trend,Click potential,Content references,Competitors`.
- Drop any keyword listed as a primary in `used-keywords.md` → "Active primaries".
- Check `used-keywords.md` → "Near-neighbour warnings". If the candidate is listed there, decide which page owns the intent before proceeding — four of the live posts were published against primaries that aren't in the CSV, so the used-list check alone won't catch every collision.
- Drop `Commercial`-intent rows — those are the landing-page inventory, handled by `/landing`. We want `Informational`.
- Rank remaining rows by `Volume / (KD + 1)` descending. Prefer keywords with SERP feature `People Also Ask` listed (means we can mine PAA questions for FAQs).
- Propose the top candidate in one line: `Picking "<keyword>" (vol <V>, KD <KD>). Any objection?`
  - Auto mode: proceed immediately after announcing; do not wait unless the user has expressed a preference earlier in the session.

If arg supplied:
- Confirm it exists in the CSV (case-insensitive exact phrase match against the `Keyword` column).
- Confirm it is not in `used-keywords.md` and not a near-neighbour of a live post.
- If either check fails → stop and ask.

### 2. Build the keyword cluster

Scan the CSV for **same-intent** secondaries (would someone searching this phrase want to land on the same page?). Max 5.

- Use CSV matches first (mark `✓ CSV`).
- Fill remaining slots with invented secondaries from typical People-Also-Ask / Related-Searches patterns (mark `(invented)`). Keep to the same-intent rule.
- Watch for the CSV's near-duplicate rows (e.g. `best iphone video editing app` / `best iphone video editing apps`) — cluster them, don't treat them as separate targets.

### 3. Research the top-3 SERP

Use `WebSearch` with the primary keyword. From results:

- Pick the **top 3 organic results** (skip ads, featured snippets that don't link to a post, and non-blog domains — forums, Reddit, YouTube, directories).
- `WebFetch` each. Extract:
  - **Format**: list / tutorial / guide / comparison / news.
  - **Word count** (rough, from body text). Target within 20% of the median — take the number from this research, don't assume a range.
  - **Every H2 topic they cover** — combined union across the three.
  - **Questions in any FAQ / People-Also-Ask blocks**.
- Add **1–2 topics** none of the top-3 cover but a reader of the primary would value. Taking ground they missed is the whole point.
- **Check their dates.** This category moves fast — a competitor page describing Sora as a live product, or quoting 2025 pricing, is an angle in itself.
- Note the dominant format → our post matches it.

### 4. Verify every external number

Before drafting, resolve every figure the post will contain:

- **Product numbers** → `references/stats.md`. If it isn't there, the sentence doesn't ship.
- **Competitor pricing, limits, and free-tier terms** → perishable. `WebFetch` the vendor's own pricing page today, link it, and stamp the check month in the post body ("verified August 2026"). If you can't verify it, leave it out — do not reuse the July 2026 figures in `stats.md` without re-checking.
- **Platform and API facts** (shutdowns, deprecations, format specs) → link the primary source, not a news aggregator.
- **Anything about our traction** → see `stats.md` → "Do not claim". There are no user counts, ratings, or results. The homepage testimonials are placeholder copy and must never be cited.

### 5. Sanity-check voice and scope

Re-read `references/voice.md` sections "Tells that it's AI-written", "Words we never use", and "Naming competitors". Keep that list beside you while drafting.

Plan:
- One narrative from `stories.md` (most topical — do not invent).
- One opinion from `opinions.md` (most topical — backed by a number).
- At least one **"don't use 1 Tap Cut for this"** moment — biggest voice tell.
- At least one place where a **competitor is named as the right answer**, in the body, without hedging. If the post's comparison table has no row we lose, the table is fake.
- Humour: dry, deadpan, self-aimed. **No puns.** Two or three moments in a 1,500-word post, none within two sentences of a number.
- No exclamation marks. No emoji. None of the banned words.

### 6. Fetch Pexels images

Images come from Pexels via `scripts/fetch-pexels.mjs`. API key: `PEXEL_API` in `.env`.

For each post you need:
- `hero` — landscape, matches the post topic.
- One image per H2 heading in the body — slug key matches the slugified H2 text exactly.
- `frequently-asked` — generic "question" image.
- `join-the-waitlist` — generic "phone / creator filming" image for the closing CTA section.

Edit `scripts/fetch-pexels.mjs`: add a new entry under the `posts` object, keyed by slug. Each section key is the slugified H2 heading; each value is `{ query, alt }`. Follow the existing entries as the pattern. Queries that work in this category: `video editing timeline`, `smartphone filming`, `creator vlogging`, `laptop editing`, `camera roll phone`.

Run:
```
node scripts/fetch-pexels.mjs
```

This writes images to `/public/images/blog/<slug>/` and appends attribution to `/content/pexels.json`. Verify every expected key is present, files exist, and there are no `✗` lines. If any failed, make the query more concrete (single nouns work best) and rerun — the script is idempotent per section key.

### 7. Write the content file

Create `/content/blog-<slug>.ts`. Follow `content/blog-voiced.ts` for **shape only**:

```ts
export const <camelCaseSlug>Post = {
  slug: "<kebab-slug>",
  title: "<50–60 char title, primary keyword near start>",
  description: "<150–160 char meta description, primary keyword + benefit + soft CTA>",
  publishedAt: "<today YYYY-MM-DD>",
  readingMinutes: <rough number>,
  primaryKeyword: "<primary>",
  keywordCluster: ["<primary>", "<secondary 1>", "<secondary 2>", ...],
  author: {
    name: "Fon Rangsanseri",
    role: "Co-Founder, 1 Tap Cut",
    url: "https://www.instagram.com/1tapcut/",
    bio: "<one or two sentences: builds 1 Tap Cut, writes the blog personally, edits their own footage. Nothing beyond what references/voice.md establishes — do not invent credentials, years, or history.>",
  },
  heroImageAlt: "<matches Pexels hero alt>",
  images: { /* mirror pexels.json keys for this slug */ },
  tldr: "<2–4 sentences answering the query directly, reads well on its own>",
  body: `...markdown...`.trim(),
  faqs: [ /* 6–8 entries from People-Also-Ask, schema-ready */ ],
};
```

Body rules (compiled from `voice.md`, `humour.md`, `on-page-seo.md`):

- **Opener**: the answer, immediately. Fragments allowed. No wind-up, no "whether you're a…".
- **Direct answer** in the first paragraph. Primary keyword in the first 100 words.
- **TL;DR** callout (rendered separately — 2–4 sentences).
- **H2 hierarchy** mirrors the top-3 SERP consensus plus 1–2 novel sections. Each H2 is a statement that tells you what the section concludes, not a label.
- **Length** within 20% of the median SERP word count from step 3.
- **One narrative**, **one strong opinion**, **one "don't use us for this"** moment, **one competitor named as the right answer**.
- **Tables** for pricing, feature splits, and timelines. Real numbers only, each with its plan or its source date attached.
- **3–5 internal links**. Real targets: other `/blog/<slug>/` posts, `/services/<slug>/` landing pages. Cross-links to the root site (`/pricing`, `/tools/content-script-generator`) where relevant. Descriptive anchor text.
- **2–3 external links** to primary sources — vendor pricing pages, OpenAI/Adobe/Blackmagic documentation, platform spec pages. `target="_blank" rel="noopener nofollow"`. Not aggregators, not listicles.
- **FAQ section**, 6–8 entries from People-Also-Ask and the top-3's FAQ blocks. Keep answers flat — they get harvested into schema and read out of context, so no humour here.
- **Closing CTA section**: one sentence, waitlist-first. The app is in **closed beta** — say so. There is no phone number, no demo, no sales team. Never "get started today".

After drafting, re-read `voice.md` → "Tells that it's AI-written". Delete any paragraph that matches.

### 8. Create the route

Copy `app/v5/page.tsx` to `app/blog/<slug>/page.tsx`. Change:

- Import the new content file instead of `voicedPost`.
- Rename the `images` lookup key to the new slug.
- `metadata.title.absolute` — post-specific, 50–60 chars.
- `metadata.description` — post-specific.
- `metadata.alternates.canonical` — `"/blog/<slug>/"`.
- `metadata.openGraph.url` — `"/blog/<slug>/"`.
- OG images path — `/images/blog/<slug>/hero.jpg`.
- `toc` array — rebuild from the new H2 structure (IDs must match what `BlogBody` emits via `slugifyHeading`).
- `BlogJsonLd` `slug` prop — `"blog/<slug>"`.
- Breadcrumbs `url` values — `"/blog/<slug>/"`.
- The "While you're here" internal-links card — swap in 2–3 relevant internal targets.
- The "Further reading" external links — swap in 2 relevant primary sources.
- **The v5 template carries the legacy sample business's CTA block, including a phone number and click-to-call.** Replace it entirely with the waitlist CTA. Grep the generated file for `tel:` and for anything from `lib/business.ts` before moving on — none of it belongs in a 1 Tap Cut post.

No SSG-breaking patterns (`cookies()`, `headers()`, `searchParams`, `dynamic = 'force-dynamic'`).

### 9. Update the blog index

Edit `app/blog/page.tsx`. Add an entry pointing at `/blog/<slug>/`, pulling `title`, `description`, `author.name`, `publishedAt`, `readingMinutes` and the hero image from the new content file.

### 10. Mark the keyword used

Append a section to `references/used-keywords.md` under "Active primaries", following the existing format:

- Primary keyword + CSV source line (volume, KD, intent).
- `/blog/<slug>/` as the page, plus the published date.
- Secondary cluster table with `✓ CSV` or `(invented)` tags.
- If the post creates a new collision risk, add a row to "Near-neighbour warnings".

### 11. Verify

```
npm run build
```

All routes must show `○ (Static)`. No build errors. Then confirm in the generated `out/blog/<slug>/index.html` (or via `npm run dev`):

- H1 renders with the primary keyword.
- JSON-LD present for `Article`, `BreadcrumbList`, `FAQPage`, `Person`.
- Hero and all H2 section images load.
- TOC anchors jump correctly.
- **No `tel:` link, no legacy business name, no phone number** anywhere in the output.
- Every competitor number in the post carries a source link and a check date.
- No "AI tells" in the prose — run the checklist one more time.

If anything fails, fix and rebuild. Don't say "done" until the build is green and the page reads in voice.

---

## Output summary (report at the end, ~10 lines)

- Primary keyword + volume/KD.
- Cluster (5 secondaries).
- Top-3 SERP competitors + dominant format + median word count.
- Slug + URL: `/blog/<slug>/`.
- Which narrative and which opinion were used.
- Any competitor numbers included, and the date they were verified.
- Files created/changed.
- Build status.

---

## Anti-patterns (do not do any of these)

- **Don't** invent a keyword that isn't in `VDOediting-keyword.csv`.
- **Don't** reuse a primary from `used-keywords.md`, or pick a near-neighbour of a live post without resolving the collision.
- **Don't** skip `on-page-seo.md` items.
- **Don't** write without reading `voice.md` + `humour.md` + the calibration post first.
- **Don't** invent stats, narratives, or opinions. Pull only from the reference files.
- **Don't** publish a competitor price without a link and a check date.
- **Don't** invent anything about Fon — no age, no years of experience, no origin story, no credentials.
- **Don't** claim user counts, ratings, or results, and never cite the homepage testimonials.
- **Don't** write a comparison table where 1 Tap Cut wins every row.
- **Don't** use puns, exclamation marks, emoji, or any word on the "never use" list in `voice.md`.
- **Don't** leave a phone number, `tel:` link, or legacy business detail in a generated page.
- **Don't** imply the app is generally available. It's in closed beta.
- **Don't** ship without running `npm run build`.
- **Don't** mutate `/app/v5/page.tsx` or `/content/blog-voiced.ts` — copy, don't edit.
- **Don't** introduce runtime APIs or any SSG-breaking pattern.
