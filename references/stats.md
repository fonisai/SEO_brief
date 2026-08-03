# Stats & Real Numbers

> Canonical numbers for 1 Tap Cut. Use these exactly. Don't round them, don't invent new ones.
> When a post needs a number about our product, it comes from this file. When it needs a number about anyone else's, see **Perishable numbers** below.
>
> Every figure here traces to a file in this repo. The source column is not decoration — if you change a price on the site, change it here in the same commit, or the blog starts lying.

## The product

- **Name:** 1 Tap Cut (two spaces, capital T, capital C — `1 Tap Cut`, never "1TapCut" in prose)
- **What it is:** a mobile AI video editor for iPhone and Android
- **What it does:** turns raw phone footage into a finished vertical cut without a timeline
- **Site:** https://www.1tapcut.com
- **Instagram:** https://www.instagram.com/1tapcut/
- **Byline on all content:** Fon Rangsanseri, Co-Founder, 1 Tap Cut

**Source:** `app/lib/seo.ts`, `app/blog/posts.ts`

### The four features — use these names exactly

| Feature | What it does |
|---|---|
| **Find shorts** | Finds the hook-worthy moments in a long video, crops to vertical, outputs ready-to-post clips |
| **Silence removal** | Cuts silent gaps, pauses and filler words |
| **Auto captions** | Caption styles, pre-categorised by look |
| **Auto vlog** | Raw footage in, finished vlog out |

**Source:** `app/components/sections.tsx`

Do not describe a fifth feature. Do not describe a roadmap item as if it ships.

## Status — we are in closed beta

- **Closed beta**, invites rolling out **weekly**
- Entry is the **waitlist**
- Waitlist offer: **a free week of Pro**, plus **20% off the first year** if you subscribe
- Joining reserves your **username and email**

**Source:** `app/components/extra-sections.tsx`, `app/pricing/PricingContent.tsx`

Every post that mentions availability says "closed beta". Never write as if the app is generally available.

## Pricing

Monthly price first, annual equivalent second. Annual is billed as one payment.

| Plan | Monthly | Annual | Billed annually |
|---|---|---|---|
| **Starter** | **$0** | $0 | — |
| **Pro** | **$15/mo** | **$12/mo** | **$144/yr** |
| **Studio** | **$39/mo** | **$30/mo** | **$360/yr** |

**Source:** `app/pricing/PricingContent.tsx`

### What's in each plan

**Starter — $0**
- 3 video exports per month
- Up to 1080p
- Standard generation speed
- **1 Tap Cut watermark on clips**
- Standard caption templates
- **10-minute limit per raw clip**

**Pro — $15/mo, $12/mo annually**
- Unlimited exports
- Up to 4K Ultra HD
- **No watermarks**
- Fast priority AI queue
- Custom fonts and caption styles
- Priority email support
- **Uploads up to 2 hours long**

**Studio — $39/mo, $30/mo annually**
- Everything in Pro
- Up to 5 team members
- Collaborative workspace
- Dedicated brand presets kit
- API access — **marked "soon", not shipped**
- 24/7 dedicated account manager

**Source:** `app/pricing/PricingContent.tsx`, `app/components/extra-sections.tsx`

## Limits and speed

- **Render time:** most reels render in **under 30 seconds**, even from an hour of raw footage
- **Upload length:** **2 hours** on Pro and Studio; **10 minutes per raw clip** on Starter
- **Processing:** on-device for short clips, cloud for longer ones; video chunks processed in parallel
- **Cancellation:** upgrade, downgrade or cancel any time; Pro features run to the end of the billing cycle

**Source:** `app/components/extra-sections.tsx`, `app/pricing/PricingContent.tsx`

## Privacy — say this precisely, it's a differentiator

- **Short clips never leave the phone.** The AI runs entirely on-device.
- **Longer projects are processed in the cloud, encrypted**, and the **files are deleted within 24 hours**.
- **We never train on user videos.**

**Source:** `app/components/extra-sections.tsx`

Do not soften this and do not overstate it. "Nothing ever leaves your phone" is false — it's true for short clips only.

## Formats and export

- Every export is auto-formatted for **9:16, 1:1 and 16:9**
- One tap produces a version for each — **no re-rendering**
- Music: licensed in-app library, or bring a track from the phone; **the AI re-syncs every cut to the new beat**
- Editing is optional — every clip, transition and beat is editable, but the default path is one tap

**Source:** `app/components/extra-sections.tsx`

## Perishable numbers — competitor pricing and limits

**Nothing about another company's pricing is canonical in this file.** It changes quarterly and a stale price is the fastest way to lose a reader who is actively comparing.

**The rule:** verify it today, link the source, and stamp the check date in the post. If you can't verify it, leave it out. "Pricing was X as of July 2026" is publishable. "Pricing is X" is not.

The pattern to copy is `app/blog/content/opusclip-vs-submagic-vs-descript-comparison.mdx` — its title, description and body all carry the verification month.

### Last verified July 2026 — re-check before reuse

| Tool | What we published | Re-verify |
|---|---|---|
| **OpusClip** | Starter from $15/mo. Bills **per minute of source video** — a 60-minute upload costs 60 credits regardless of clips produced | Yes |
| **Submagic** | $12/mo **only on annual**. Free tier: 3 videos/mo, watermark, 1m30s cap, 200MB file limit. Videos over 5 min need Business or the Magic Clips add-on | Yes |
| **Descript** | Free plan: 60 media minutes/mo, 720p export with watermark, one-time 100 AI credits | Yes |
| **CapCut** | Free, template-driven, the short-form default | Yes |
| **All four free tiers** | Every one applies a watermark; every paid plan removes it | Yes |

Platform facts that change more slowly, but still get a source link when used:

- **1 Tap Cut** runs natively on iOS and Android. **OpusClip and Submagic** are browser tools. **Descript** is desktop software for Mac and Windows.
- **Sora** was wound down by OpenAI: announced 24 March 2026, web and app discontinued 26 April 2026, `sora-2` / `sora-2-pro` / Videos API off 24 September 2026, no replacement named. See `stories.md`.

## Do not claim — none of these numbers exist

We are a closed-beta product. We have no public traction figures, and inventing them is the fastest way to lose the trust the rest of the voice is built on.

Never write, in any form:

- **User counts** — no "10,000 creators", no "thousands of videos edited"
- **Review scores or star ratings** — we have none published
- **Revenue, funding, growth rates, retention**
- **"Trusted by"** anything
- **Awards, press mentions, or partnerships**
- **Performance results for users** — no "creators see 3x more views". We have not measured this and neither has anyone else.
- **Accuracy percentages** for silence detection, captions, or clip-finding, unless someone has actually run the test and written down the method

### The homepage testimonials are placeholder copy

`app/components/extra-sections.tsx` contains testimonials attributed to "Sarah Jenkins", "Marcus Chen" and "Elena Rostova". **These are placeholders.** They are not real customers, they are not canonical, and they must never be quoted, cited, paraphrased, or counted as social proof in any post or landing page.

If a page genuinely needs a trust signal, use a verifiable one: the privacy model, the flat pricing, the beta status stated plainly.

## How to use these numbers in content

- **Always specific, never rounded.** "$144 a year", not "about $150".
- **Attach every limit to its plan.** "10-minute clips" is misleading; "10-minute clips on the free Starter plan" is true.
- **Quote both prices when quoting one.** "$15 a month, or $12 if you pay annually" — readers comparing tools are comparing annual rates.
- **State the beta.** Any sentence implying the reader can download it right now needs "closed beta" nearby.
- **When comparing to another tool, use their verified numbers or none.** A comparison where we win every row is a tell, not a table — see `voice.md`.
- **If a number isn't in this file and you can't source it, the sentence doesn't ship.**
