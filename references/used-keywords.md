# Used Keywords

> Primary keywords already targeted by 1 Tap Cut content.
> **Rule:** before picking a primary for a new page, check this file. Never reuse a primary — that causes keyword cannibalisation.

**Active keyword export:** `VDOediting-keyword.csv` (69 rows, US database, seed `video editing`).

---

## Keyword-selection rule

1. **Primary must come from `VDOediting-keyword.csv`.** Never invent a primary.
2. **Check this file first** — never reuse a primary, and check the **Near-neighbour warnings** section below before picking anything close to a live post.
3. **Split by intent:**
   - `Informational` → blog post (`/blog/<slug>`)
   - `Commercial` → landing page. There are **12 commercial rows** in the CSV; they are the entire landing-page inventory.
4. **Secondary / cluster keywords: CSV first, invent only what's missing.**
   - Scan the CSV for same-intent variations of the primary (mark ✓ CSV).
   - Fill remaining slots from typical People-Also-Ask / Related-Searches patterns (mark `(invented)`).
5. **Same-intent test** for every secondary: would someone searching this phrase want to land on the same page as someone searching the primary? If no, it belongs in a different cluster.

---

## Active primaries

### 1. `can chatgpt edit videos` → `/blog/can-chatgpt-edit-videos`

- **Primary source:** ✓ CSV (vol 590, KD 29, Informational)
- **Published:** 2026-08-02
- **Cluster:**

| Secondary keyword | Source |
|---|---|
| can chat gpt edit videos | ✓ CSV (vol 260, KD 28) |
| what video editing software do youtubers use | ✓ CSV (vol 880, KD 30) |
| best ai video editing tools 2026 | ✓ CSV (vol 480, KD 9) — *see warning below* |
| how to organize files when editing a video | ✓ CSV (vol 260, KD 20) |
| can chatgpt summarise a youtube video | `(invented)` |

---

### 2. `opusclip vs submagic vs descript` → `/blog/opusclip-vs-submagic-vs-descript-comparison`

- **Primary source:** `(not in CSV)` — comparison keyword chosen outside the export
- **Published:** 2026-07-25. Pricing verified July 2026.
- **Cluster:**

| Secondary keyword | Source |
|---|---|
| cheapest ai video editor | `(invented)` |
| does submagic have a free plan | `(invented)` |
| does descript have a free plan | `(invented)` |
| ai video editor for iphone and android | `(invented)` |
| best ai video editor for podcasts | `(invented)` |

---

### 3. `how to remove silence from video automatically` → `/blog/how-to-remove-silence-from-video-automatically`

- **Primary source:** `(not in CSV)`
- **Published:** 2026-07-24
- **Cluster:** not recorded at publication. Rebuild from the CSV before writing anything adjacent.

---

### 4. `best ai video editor for travel vlogs` → `/blog/best-ai-video-editor-for-travel-vlogs-2026`

- **Primary source:** `(not in CSV)`
- **Published:** 2026-07-24
- **Cluster:** not recorded at publication. **High cannibalisation risk** — see warnings.

---

### 5. `turn long footage into short form clips` → `/blog/turn-long-footage-into-short-form-clips-automatically`

- **Primary source:** `(not in CSV)`
- **Published:** 2026-07-24
- **Cluster:** not recorded at publication.

---

## Near-neighbour warnings

Four of the five live posts were published against primaries that aren't in the CSV, so the usual "is it in the used list" check won't protect them. These CSV rows are **close enough to a live post to cannibalise it** — don't select one as a primary without deciding which page owns the intent.

| CSV keyword | Vol / KD | Collides with |
|---|---|---|
| `best ai video editing tools 2026` | 480 / 9 | `/blog/best-ai-video-editor-for-travel-vlogs-2026`, and already used as a secondary on the ChatGPT post |
| `best ai video editing software 2026` | 390 / 17 | Same — these two CSV rows are near-duplicates of each other as well |
| `how to edit out sound in a video` | 1,000 / 27 | `/blog/how-to-remove-silence-from-video-automatically` — different intent (removing an audio track vs cutting silence), but the SERPs overlap. Check before writing. |

---

## Landing-page inventory — the 12 commercial rows

Reserved for landing pages, not blog posts. None used yet.

| Keyword | Vol | KD | CPC |
|---|---|---|---|
| video editing software for beginners | 3,600 | 26 | $1.86 |
| best video editing software for beginners | 2,400 | 27 | $2.55 |
| best video editing software for beginners 2026 | 880 | 25 | $0.00 |
| best ai video editing tools 2026 | 480 | 9 | $0.00 |
| best video editing apps for iphone | 480 | 24 | $1.40 |
| best video editing program for beginners | 480 | 26 | $2.25 |
| best iphone video editing app | 320 | 27 | $1.20 |
| affordable video editing software | 260 | 28 | $2.28 |
| best video editing app for ipad | 260 | 23 | $1.15 |
| low cost video editing software | 260 | 28 | $2.31 |
| best iphone video editing apps | 210 | 26 | $1.20 |
| best video editing software ipad | 210 | 30 | $0.83 |

Note the near-duplicates (`best iphone video editing app` / `best iphone video editing apps`, `best video editing app for ipad` / `best video editing software ipad`). Cluster these, don't build separate pages.

---

## Workflow for the next page

1. Open `VDOediting-keyword.csv`.
2. Filter by intent — `Informational` for a blog post, `Commercial` for a landing page.
3. Skip any primary in "Active primaries" above.
4. Check "Near-neighbour warnings" — if the candidate is listed, decide which page owns the intent before proceeding.
5. Rank the remainder: blog posts by `Volume / (KD + 1)`, landing pages by CPC then `Volume / (KD + 1)`.
6. Scan the CSV for same-intent secondaries — use them first (mark ✓ CSV), fill the rest with invented ones.
7. Add the new section to this file **before** writing the page.
8. Ship, then fill in the published date and the route.
