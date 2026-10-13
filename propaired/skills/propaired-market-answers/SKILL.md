---
name: propaired-market-answers
description: Answer market and area questions with Propaired's own data instead of a guess. Use for "how's the market", "average price in…", "days on market", "inventory", "is it a buyer's or seller's market", "compare two areas", "what are rents", "what's this area like", "schools near…", "what's nearby", "top agents in…", "price per square foot", or any question a client is about to ask the realtor about a place. Says where each number came from and when, and keeps paid research behind an explicit ask.
interface:
  displayName: "Market Answers"
  shortDescription: "Answer market and area questions with real data"
  defaultPrompt: "How is the market in my area?"
toolset: advisory
capabilities:
  steps:
    - getMyContext
    - queryMarketData
    - queryNeighborhoodMarket
    - queryRentalIntelligence
    - searchProperties
    - searchForClient
    - getListingDetail
    - getParcelFacts
    - appraiseProperty
    - lookupPlaceKnowledge
    - queryKnowledgeGraph
    - findGoodSchoolsNearby
    - queryNearestAmenity
    - searchNearPlace
    - webLookup
    - askGenie
---

# Answer with data

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once.

Realtors get asked these questions in the car, at the open house, and on the phone. The answer has
to be right, current, and attributable — because they are about to repeat it to a client.

## Pick the right source

| Question | Use |
|---|---|
| Market statistics for a city or county — price, days on market, inventory, price per square foot, trend, top agents | `queryMarketData` |
| The same kind of numbers for a neighbourhood or a small area | `queryNeighborhoodMarket` |
| What a home or an area rents for | `queryRentalIntelligence` |
| What is actually for sale right now | `searchProperties`, or `searchForClient` when a client is named |
| A specific home's facts, history and condition | `getListingDetail`, `getParcelFacts` |
| An estimated value for one address | `appraiseProperty` |
| What a place, school, community or builder is like | `lookupPlaceKnowledge`, `queryKnowledgeGraph` |
| Schools near somewhere, by name and rating | `findGoodSchoolsNearby` |
| What is near a property — shops, transit, trails | `queryNearestAmenity`, `searchNearPlace` |
| Something outside Propaired's data entirely | `webLookup` |

The distinction that matters most: **"what homes are in Sandy" is a search; "what is the average
price in Sandy" is market data.** Running a search and averaging the results is not the same
number and must not be presented as one.

Area statistics and a specific property's facts are also different things. A missing rent estimate
for one address does not become the area average, and an area average never becomes a valuation.

## `askGenie` costs money

Call it only when the realtor asks for it by name, or agrees to your suggestion to use it. Do not
reach for it to re-examine evidence already on screen, and do not use it as a fallback when a
cheaper capability returned nothing — say what was missing instead.

## Answer like someone who will be quoted

- Lead with the number and the period it covers: "Median sold in Draper was $612,000 in September,
  up 3% on last year."
- Name the source and the date for anything a client will hear. Pass through citations when the
  capability returns them.
- Say what the number does **not** cover. A city median is not a street, and a condo and a
  four-bedroom are not the same market.
- If the data is thin or the area is too small to be meaningful, say so. "Only six sales closed in
  that subdivision this year, so the median is noisy" is a better answer than a confident figure.
- Never fill a gap from memory. If a capability returned nothing, the honest answer is that
  Propaired does not have it, plus what you can get instead.

## What not to say

- No advice on whether to buy, sell or invest as a financial decision. Give the realtor the numbers
  and let them advise their client.
- Fair Housing: answer about homes, prices, schools by name and amenities. Never characterise who
  lives in an area, and never answer "is this a good area for people like us". If a client asks that,
  give the realtor the factual material — schools, commute, amenities, price — and flag that the
  question itself cannot be answered in those terms.
- No compensation or commission talk.

Worth keeping? Offer to save the realtor's own take on an area with `propaired-knowledge-base` —
their read on a neighbourhood is theirs, and it is worth more than a number next time.
