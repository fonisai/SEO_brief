# Stories & Recurring Narratives

> The reusable narratives our posts are built on. Draw from these when a post needs something more than a fact list.
> **Every one of these is verifiable.** That's the point — the byline is a real person, so the stories have to be real too.
> Don't invent new ones. If none of these fits the topic, write the post without one.

---

## 1. The Sora wind-down

Until spring 2026 the honest answer to "can ChatGPT make videos" was yes — Sora lived inside the ChatGPT surface. OpenAI then wound it down in stages:

| Date | What happened |
|---|---|
| 24 March 2026 | Wind-down announced, API developers notified |
| 26 April 2026 | Sora web and app experiences discontinued |
| 24 September 2026 | `sora-2`, `sora-2-pro` and the Videos API shut down |

OpenAI's deprecation notice named **no replacement model**. Reporting at the time attributed it to economics — video generation costs far more per output than text, and compute moved to coding and enterprise products.

**Use this when:** writing about what AI can and can't do with video, about ChatGPT workflows, or about any "AI video generator" keyword. It's also the cleanest example of why unsourced, undated content in this category goes wrong — most pages ranking for these terms are still describing a product that no longer exists.

**Already used in:** `/blog/can-chatgpt-edit-videos`. Re-use the dates freely; don't re-tell the whole thing in a post where it isn't the subject.

---

## 2. The paper-edit ceiling

Hand a language model a **timestamped** transcript and it will do genuinely useful work: pick the segments worth keeping, order them into something coherent, and explain why each earns its place. It's the single best use of an LLM on long footage.

Then it stops. It has no timeline, no access to your media, no timecode, and no render pipeline. It produces a **plan**. Something else has to perform it.

The ceiling is the whole story: the model is good at the language half of editing and structurally incapable of the other half.

**Use this when:** writing about AI editing workflows, transcript-driven editing, Descript, or any "can [AI tool] edit video" keyword.

---

## 3. Footage with no transcript

Twenty clips from a weekend. No dialogue. Half of them shaky. This is what most people actually shoot, and every transcript-based AI workflow has **no input at all** for it.

Nothing in the paper-edit route addresses the real work here: finding the usable seconds inside each clip, cutting the dead air, matching pace to music, exporting vertical.

There's a second half to it — the admin tax. On a 40-minute interview, the export-transcript-paste-read-translate-timecodes loop is worth paying. On a 45-second clip, it's several minutes of overhead around an edit you could have done by hand in two.

**Use this when:** writing about mobile editing, travel vlogs, phone footage, or why long-form AI workflows don't transfer to short-form.

---

## 4. The billing-model trap

Per-minute-of-source pricing sounds fair and behaves badly. A 60-minute upload costs 60 credits **regardless of how many clips come out of it** — so the exact footage you most want automated help with is the footage that costs the most to process. Uploading an hour to find three good clips means paying for 57 minutes you threw away.

The point isn't that any one tool is bad. It's that **the billing model, not the feature list, decides which tool you're still using in six months** — and it's the thing nobody compares before signing up.

**Use this when:** writing about pricing, comparisons, "cheapest AI video editor", or long-to-short workflows.

**Verified:** July 2026, in `/blog/opusclip-vs-submagic-vs-descript-comparison`. Re-check the numbers before reusing them — see `stats.md` → Perishable numbers.

---

## 5. The watermark tax

Every free tier in this category applies a watermark. Every paid plan removes it. That, not the export cap, is the actual paywall — a watermarked clip is not postable, so a free plan with three exports and a watermark has an effective limit of zero.

Worth saying plainly because we do it too. Starter is $0 with 3 exports a month, 1080p, **and a 1 Tap Cut watermark**. Same trade as everyone else's.

**Use this when:** writing about free tools, "no watermark" keywords (`free video editing software no watermark`, vol 1,900 in the CSV), or pricing generally.

---

## 6. Fon's own footage

<!-- TODO(Fon): this slot is for one or two real anecdotes from you. Nobody else should fill it in.
     What would make it usable:
       - A specific edit that took absurdly long. What was it, how long, what were you cutting in?
       - The moment you decided to build this instead of continuing to edit by hand.
       - A time a tool — ours or someone else's — got something wrong in a way that taught you something.
     Each needs one concrete detail: a duration, a place, a piece of gear, a number.
     Until this is filled in, posts run with narratives 1–5 or with none. -->

---

## How to use these

- **One narrative per post, maximum.** More than one reads as padding.
- **Don't recite them.** Take the bones — the dates, the mechanism, the specific number — and fit them to the post.
- **Always carry one concrete detail** — a date, a duration, a price, a file limit. The detail is what makes it read as real, because it is.
- **Re-verify anything perishable** before reusing it. Narratives 4 and 5 rest on competitor pricing; see `stats.md`.
- **Never invent a new one.** No composite creators, no "a user told me", no hypothetical framed as history. A post with no story is fine. A post with a fabricated one undoes the byline.
