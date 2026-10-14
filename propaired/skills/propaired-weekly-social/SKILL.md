---
name: propaired-weekly-social
description: Produce a week of on-brand social posts from the realtor's own listings, sales and market, scheduled across the week, with one approval per post. Use for "my posts for this week", "social content", "what should I post", "I haven't posted in weeks", "just listed post", "just sold post", "market update post", "schedule my week", or a Monday content routine. Nothing publishes until the realtor approves each post. The cadence comes from the realtor or their assistant's own scheduler, not from Propaired.
interface:
  displayName: "Weekly Social"
  shortDescription: "A week of on-brand posts from real activity"
  defaultPrompt: "Draft my posts for this week."
toolset: social_studio
capabilities:
  steps:
    - getMyContext
    - listConnectedSocialAccounts
    - getSocialQueueOverview
    - getMarketingContentContext
    - getRealtorBusinessSummary
    - researchMarketingTopic
    - listMyListingEvents
    - planListingContentPost
    - planMarketContentPost
    - prepareMarketContentProposal
    - submitMarketContentProposal
    - draftSocialCrossPost
    - queueSocialPostPublishApproval
    - activateSocialContentSeries
---

# A week of posts

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

Social consistency is the thing realtors know they should do and do not. The win is three to five
posts ready on Monday, each from something real that happened in their business.

## See what is already there

- `listConnectedSocialAccounts` — where they can actually post. If nothing is connected, say that
  first; drafting posts for accounts that do not exist wastes their time.
- `getSocialQueueOverview` — what is already drafted or scheduled. **Resume what is there before
  writing anything new.** A duplicate week is the most common failure of this routine.

## Find the material

Real events beat generic content every time:

- `listMyListingEvents` — the realtor's own last seven days: coming soon, just listed, sold. This is
  the best source and should drive most of the week. It reports which kinds of event it cannot see:
  a price change has no date in Propaired, so an empty week is **not** evidence their prices held,
  and a listing that has just gone under contract shows up under its original list date.
- `getMarketingContentContext` — their brand, voice, areas and positioning.
- `getRealtorBusinessSummary` — milestones worth mentioning.
- `researchMarketingTopic` — for a market or advice post, from governed sources only.

If the week has no listing events, lean on market and advice posts rather than inventing activity.
Never post about a listing that is not theirs, and never imply a sale they did not make.

## Draft three to five, mixed

`references/post-mix.md` has the mix. Draft with whichever fits:

- `planListingContentPost` for a specific listing, `planMarketContentPost` for a market or advice
  post — these use Propaired's own content pipeline and the realtor's voice.
- `prepareMarketContentProposal` then `submitMarketContentProposal`, or `draftSocialCrossPost`, when
  you are writing the copy yourself and putting it into Propaired.

All of these save as drafts. None of them posts anything.

Show the realtor the set, briefly — one line each, what it is about and which day it would go. Not
the full copy of five posts in one message; offer the full text of any one they want to read.

## Schedule, one approval at a time

`queueSocialPostPublishApproval` with the post and the time it should go. This **does not publish**.
It returns an approval binding that exact caption, media, schedule and set of destinations, so what
they approve is what goes out.

- One approval per post. Do not batch a week into one decision.
- Tell them how many are waiting and hand over the link from the card.
- Poll `resumeCapability({ approvalId })` per approval id. Each publish runs once.
- An approval expires after **90 minutes**. A week's worth will often go stale while they are out
  showing homes — when that happens, queue it again rather than reporting a failure.
- Summarise at the end: what got scheduled, for when, and what is still waiting.

For a repeating series rather than one week, `activateSocialContentSeries` also needs their
approval and carries its own checks.

## Content rules

- **The brokerage name appears on every post.** This is a Utah advertising requirement, not a style
  preference.
- No compensation or commission language.
- Fair Housing: write about the home and the area. Never about who should live there, and never a
  proxy for it — no "perfect for a young family", no "safe neighbourhood", no "good schools for your
  kids" framed as who belongs.
- No client's name, photo or details without the realtor confirming the client agreed.
- Only facts from a tool result: price, status, beds, days on market, sale figures. If a number is
  not in one, leave it out.
- If an image was generated or materially altered by AI, it is disclosed.

## Cadence

Propaired does not run this on a timer. The realtor asks, or their own assistant's scheduler asks on
their behalf. If they want it every Monday, tell them to set that up on their side — and offer to
make the routine short enough that it is a two-minute conversation.
