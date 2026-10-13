---
name: propaired-listing-launch
description: Take a signed listing from agreement to live and marketed. Use for "launch this listing", "we just signed", "get this listing live", "what's left on this listing", "coming soon", "just listed set", "marketing for my new listing", "tell my sphere about it", or when a realtor wants the whole post-signing sequence run rather than a single asset. Walks the open items, hands the MLS draft to the browser routine, then produces the just-listed set and the sphere email — with the realtor approving everything that goes out.
interface:
  displayName: "Listing Launch"
  shortDescription: "Take a signed listing to live and marketed"
  defaultPrompt: "Launch my new listing."
toolset: social_studio
capabilities:
  steps:
    - getMyContext
    - getListingDetail
    - getListingWorkspace
    - planListingContentPost
    - suggestListingAudienceSegment
    - getSocialQueueOverview
    - queueSocialPostPublishApproval
    - draftClientEmail
    - queueClientEmailSendApproval
    - addTransactionMlsAlert
---

# Launch a listing

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

Seven to fourteen days and about six vendors sit between a signature and a fully marketed listing,
and no single tool owns it. This routine owns the sequence.

## 1. See what is open

`getListingWorkspace` for the seller's listing: the facts resolved so far, what still needs the
realtor, and what is blocking the MLS draft.

Lead with the critical path — the thing that, if it slips, moves the live date. Usually photos, the
seller's signature on the listing form, or a required MLS field nobody can answer yet.

## 2. Get the content drafted

Work the drafting capabilities the workspace offers: public remarks, agent remarks, photo captions.
These save as drafts in the realtor's voice and brand, and the realtor edits before anything is
used.

Remarks rules are not negotiable and the MLS enforces some of them:

- No compensation or commission language.
- Fair Housing: the property, never the people.
- Never copied from another listing.
- Public remarks read best at 255 characters or fewer.

## 3. Hand off the MLS draft

`propaired-utahrealestate-mls` fills the UtahRealEstate draft from the approved package in the
realtor's own browser and stops at Save as Draft. **The realtor publishes.** Propaired never does,
and neither do you.

If this host cannot drive their browser, say so and give them the listing package to work from.

## 4. Read the live listing back

Once they have published, the listing reaches Propaired through the MLS feed. Confirm it with
`getListingDetail` before marketing anything — marketing a listing that is not live yet is a Clear
Cooperation problem, not just a tidiness one.

If they are marketing it as Coming Soon rather than Active, that is a different and time-limited
status. Say which one the material is written for.

## 5. The just-listed set

1. `planListingContentPost` for the listing posts.
2. `getSocialQueueOverview` to avoid duplicating what is already queued.
3. `queueSocialPostPublishApproval` per post, with the time it should go. Each **returns an approval**
   binding the exact caption, media, schedule and destinations — nothing publishes first.
4. Poll `resumeCapability({ approvalId })` per approval. Each publish runs once, and an approval
   expires after 90 minutes.

`propaired-weekly-social` covers the mix if the realtor wants the listing spread across a week
rather than announced once.

## 6. Tell the sphere

1. `suggestListingAudienceSegment` for who in their database this listing actually suits — buyers
   whose criteria it matches, neighbours, past clients in the area.
2. Draft the email with `draftClientEmail`, then `queueClientEmailSendApproval`. Same contract: it
   returns an approval showing the exact recipients and wording, and sends nothing until they
   approve.

The brokerage name goes on every piece of this, the posts and the email both.

## 7. Close the loop on the transaction

`addTransactionMlsAlert` so the listing step on the seller's deal is tracked against the live MLS
record rather than someone remembering to tick it.

Then tell the realtor what is still open, in one short list. Offer to put the remaining items on
their task list — `propaired-daily-brief` does that — and to write up anything the seller was told,
which `propaired-crm-notes` does.

## Not yet Propaired's job

Vendor booking — photography, staging, cleaning, inspection — and the seller's own calendar are not
wired up yet. Surface them as open items the realtor handles, and do not imply a booking was made.
