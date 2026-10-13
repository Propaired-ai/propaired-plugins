---
name: propaired-buyer-workup
description: Run a buyer from first consult to offer. Use for "set up a buyer", "intake for…", "find homes for my client", "what should I show them", "recommend homes for…", "book a showing", "schedule a tour", "what did they think of…", "showing feedback", "they're ready to offer", or any property search that names a client. Captures what the buyer wants, searches with their criteria rather than generic filters, books showings for the realtor's approval, and gathers feedback after the tour.
interface:
  displayName: "Buyer Path"
  shortDescription: "Run a buyer from consult to offer"
  defaultPrompt: "Find homes for my buyer."
toolset: advisory
capabilities:
  steps:
    - getMyContext
    - createBuyerIntakePlan
    - searchForClient
    - searchProperties
    - recommendForClient
    - saveSearchForClient
    - getListingDetail
    - getParcelFacts
    - queryNeighborhoodMarket
    - queryMarketData
    - appraiseProperty
    - createShowing
    - getViewingReport
    - getShowingSummary
    - proposeOffer
    - updateDealStatus
  prohibited:
    - name: getShowingChecklist
      reason: "Filters by client_id, so it returns nothing for a realtor. Use getViewingReport."
---

# Work a buyer

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

## Always search as the client, never as the public

When a client is named — "find homes for Sarah", "something for my buyers in Draper" — use
`searchForClient`. It applies the buyer's own saved criteria and attaches the result to their record,
so the next person to look at that client sees what was shown. `searchProperties` is the generic
public search and loses all of that; use it only when no client is involved.

`recommendForClient` ranks against what that buyer has actually engaged with, rather than a filter
they typed once. Reach for it when the realtor asks what to show, not what exists.

Both save to the client's record immediately and need no approval.

Whichever you use, say **why** each home suits this buyer, from their criteria: "this one because
it's the only three-bed under budget inside the Alta boundary". A list of addresses is something the
realtor could have got themselves.

## Consult and intake

`createBuyerIntakePlan` captures needs, budget, timeline and how the process will work, so the
realtor stops repeating the same explanation. Run it from what the buyer actually said, and leave
blanks blank rather than filling them with assumptions — a wrong budget on file is worse than a
missing one.

`saveSearchForClient` keeps the criteria so new matches reach them without the realtor re-running a
search.

## Showings

`createShowing` **opens a transaction and marks the home as being toured, so it needs the realtor's
approval.** It returns an approval card rather than booking. Tell the realtor which home, which
buyer and when, hand over the link from the card, then poll `resumeCapability({ approvalId })` for
their decision. The booking runs once; the approval expires after 90 minutes.

Before a tour, `getViewingReport` gives the checklist and due diligence for the home together. Use
it, not `getShowingChecklist` — that one returns nothing for a realtor.

After a tour:

- `getShowingSummary` for everything this buyer has seen, which is what a "where are we" conversation
  needs.
- Log the feedback with `propaired-crm-notes` while the realtor still remembers it. What they
  disliked is more useful than what they liked, and it should change the next search.

`propaired-meeting-prep` briefs the realtor before the consult or the tour.

## Offers

`proposeOffer` and `updateDealStatus` change a transaction, so both need the realtor's approval and
return an approval card. Before suggesting a number, read the comparable evidence —
`getListingDetail`, `getParcelFacts`, `queryNeighborhoodMarket`, `queryMarketData`,
`appraiseProperty` — and show the realtor what it is based on. Never put a figure in front of them
that did not come from a tool result.

Propaired does not write the contract. The agreement, the forms and the signatures stay with the
realtor and their brokerage.

## What to be careful about

- A buyer agreement belongs in place before touring. If the realtor has not mentioned one, ask
  rather than booking around it.
- Pre-approval is a fact, not an assumption. Record it when the realtor says so.
- Fair Housing: match homes to stated criteria — beds, budget, commute, schools by name. Never
  steer by who lives in an area, and do not answer "is this a good neighbourhood for us" with
  anything about the people there.
