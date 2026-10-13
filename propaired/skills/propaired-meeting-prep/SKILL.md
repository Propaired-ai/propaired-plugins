---
name: propaired-meeting-prep
description: Brief the realtor before a client meeting so they walk in knowing where things stand. Use for "brief me on…", "prep me for…", "I'm meeting X in an hour", "what do I need to know before…", "catch me up on this client", "what's the latest with…", a buyer consult, a listing appointment or presentation, a showing or tour, an open house, or a check-in call. Pulls the client's situation, what they have been doing, what was said last, the property and the local market, and turns it into what to say and what to ask.
interface:
  displayName: "Meeting Prep"
  shortDescription: "Brief the realtor before a client meeting"
  defaultPrompt: "Brief me on my next meeting."
toolset: front_office
capabilities:
  steps:
    - getMyContext
    - resolveClientReference
    - searchClients
    - getMyDay
    - getClient360Snapshot
    - getClientOverview
    - getClientJourneySummary
    - findRecentActivity
    - getViewingReport
    - getShowingSummary
    - getListingDetail
    - getParcelFacts
    - queryNeighborhoodMarket
    - queryMarketData
    - appraiseProperty
    - getPresentationAnalytics
  prohibited:
    - name: getShowingChecklist
      reason: "Filters by client_id, so it returns nothing for a realtor. Use getViewingReport."
---

# Prepare for a meeting

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

A briefing is read-only. Nothing here changes a record or contacts anyone.

## Find out what the meeting is

Propaired does not yet see the realtor's calendar, so anchor the briefing on something explicit:

- A client they name — resolve them with `resolveClientReference`, or `searchClients` when the name
  is partial. If two clients could match, ask which one rather than guessing.
- An item on their day — `getMyDay` lists today's appointments; use the client or listing attached to
  the one they mean.
- A property they name — resolve it to a listing before reading anything about it.

If they just say "my 2pm" and the day has only one appointment, use it and say which one you used.

## Read what matters for this kind of meeting

Start with the client, every time:

- `getClient360Snapshot` — the standing picture: where they are in their journey, what they want,
  what is open, what has changed. This is the backbone of the briefing.
- `getClientOverview` — budget, preferences, contact details and the actionable items.
- `getClientJourneySummary` — how their search has actually been going, in narrative form. Use it
  when the meeting is about progress or about resetting expectations.
- `findRecentActivity` — what has happened lately, including what the realtor last said to them.

Then add what the meeting needs:

| Meeting | Also read |
|---|---|
| Buyer consult | `getClientJourneySummary`; `queryNeighborhoodMarket` for the areas they are shopping |
| Showing or tour | `getViewingReport` for the property — the checklist and due diligence together. Never `getShowingChecklist`; it returns nothing for a realtor. `getShowingSummary` for everything they have already seen |
| Listing appointment | `getListingDetail` and `getParcelFacts` for the home; `queryNeighborhoodMarket` and `queryMarketData` for pricing context; `appraiseProperty` when they want a value to talk from |
| Progress or price check-in | `getShowingSummary`, `getPresentationAnalytics` if a presentation was sent, `queryMarketData` for what has changed since |

Only read what the meeting needs. Each call costs time the realtor is about to spend walking into
the room.

## Write the briefing to be read in a lift

Four short parts, in this order:

1. **Where this stands** — two sentences. Who they are, what they want, how long this has been
   going on.
2. **Since you last spoke** — only genuine changes: a new question, a price change, homes they saved,
   a viewing, silence where there used to be replies.
3. **Bring up** — two or three things the realtor should raise, each with the fact behind it.
4. **Expect to be asked** — the one or two questions this client is likely to lead with, with the
   answer ready.

Hard rules:

- Every fact comes from a tool result. If something is not there, leave it out — do not fill the gap
  with a plausible number, a school rating, or a guess at motivation.
- Attribute anything that drives advice: "three of the four homes they saved are in Draper", not
  "they seem to like Draper".
- If the picture is thin — a new lead, a quiet client — say so plainly and make the briefing about
  what to find out rather than inventing history.
- Say nothing about other clients.

Offer at the end to add the follow-ups to their list, which `propaired-daily-brief` does, and to
write up the meeting afterwards, which `propaired-crm-notes` does.
