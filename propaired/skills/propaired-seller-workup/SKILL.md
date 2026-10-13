---
name: propaired-seller-workup
description: Get a seller from first conversation to a signed listing. Use for "listing appointment", "prep a CMA", "what's this house worth", "price this home", "seller intake", "build a presentation for…", "send them the presentation", "did they open it", "net sheet", "they want to sell", or when the realtor is preparing to win a listing. Pulls the home's facts and the local market, captures the seller's situation, and builds a branded presentation the realtor approves before it reaches the seller.
interface:
  displayName: "Seller Path"
  shortDescription: "Get a seller from first talk to a signed listing"
  defaultPrompt: "Prep me for a listing appointment."
toolset: advisory
capabilities:
  steps:
    - getMyContext
    - getBookOfBusinessSummary
    - listBookOfBusiness
    - createClientFromPastSale
    - getListingDetail
    - getParcelFacts
    - appraiseProperty
    - queryNeighborhoodMarket
    - queryMarketData
    - createSellerIntakePlan
    - draftPresentation
    - queuePresentationSendApproval
    - listClientPresentations
    - getPresentationAnalytics
---

# Win the listing

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

## Who is likely to sell

Past clients sitting on equity are the best source. `getBookOfBusinessSummary` and
`listBookOfBusiness` rank them; `createClientFromPastSale` brings one into the CRM as a seller.
`propaired-sphere-nurture` covers the outreach itself.

## Know the home before the appointment

Read, then stop reading:

- `getListingDetail` for the prior sale and any listing history.
- `getParcelFacts` for what the record says — lot, year, structures, tax.
- `appraiseProperty` for a value to talk from. It is an estimate; say so, and give its date.
- `queryNeighborhoodMarket` and `queryMarketData` for what the area is doing: price, days on market,
  inventory, what has changed since they last looked.

Everything in front of a seller comes from one of these. A price with no evidence behind it is the
fastest way to lose the listing twice — once by being wrong, and once when the market corrects you.

## Capture their situation

`createSellerIntakePlan` records why they are selling, their timing, what they have done to the
home, and what they are expecting. Keep it slim: the realtor fills the rest in after the meeting
from the record, the parcel and the local data. Leave what they did not say empty.

## Build the presentation

1. `draftPresentation` builds it in the realtor's brand and voice. It saves as a draft and sends
   nothing.
2. Walk the realtor through what it says, especially the pricing section, and change what they
   disagree with. The numbers are theirs to defend in the room.
3. `queuePresentationSendApproval` when they want the seller to have it. This **does not send** — it
   returns an approval showing the exact recipient, the link the seller would get, and the
   follow-up reminders that would go with it.
4. Hand over the link from the card, then poll `resumeCapability({ approvalId })` for their
   decision. The approved send runs once and the approval expires after 90 minutes.
5. `getPresentationAnalytics` and `listClientPresentations` show whether it was opened and what was
   looked at. That is the realtor's cue to follow up — offer to put it on their list.

A seller sees a price only when the realtor has approved it. Never quote a figure into a draft that
reaches the seller before that.

## Content rules

- The brokerage name appears on the presentation and on any marketing that follows.
- No compensation or commission language.
- Fair Housing: describe the home and the market, never the buyers it would suit or who lives
  nearby.
- Only facts from a tool result. If a comparable, a date or a school is not in one, leave it out.

## After they sign

The listing itself — remarks, photos, the MLS draft, the launch — is `propaired-listing-launch` and
`propaired-utahrealestate-mls`. Propaired never publishes to the MLS; the realtor does.
