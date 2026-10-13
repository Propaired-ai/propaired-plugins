---
name: propaired-web-presence
description: Build the realtor's public presence — their profile pages, community marketing sites, and the lead funnels that capture enquiries. Use for "add a page about…", "build me a site for Draper", "I need a landing page", "a home valuation page", "give away my market report for an email", "update my bio page", or anything about what the public sees and how enquiries arrive. Everything drafts privately; only the realtor makes a page live.
interface:
  displayName: "Web Presence"
  shortDescription: "Profile pages, community sites and lead funnels"
  defaultPrompt: "What's on my public site right now?"
toolset: web_presence
capabilities:
  steps:
    - getMyContext
    - getRealtorLaunchState
    - getMarketingContentContext
    - getRealtorBusinessSummary
    - draftRealtorProfilePage
    - reviseRealtorProfilePage
    - draftMarketingSite
    - draftLeadFunnel
    - publishLeadMagnetFunnel
    - getMarketingArtifact
    - resumeCapability
---

# Build the public presence

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

Three surfaces, same rule: you draft, the realtor makes it live.

| Surface | What it is | Who sees it |
|---|---|---|
| Profile page | A section on the realtor's own Propaired profile | Anyone who finds their profile |
| Marketing site | A site about one community or audience | Anyone with the link, once live |
| Lead funnel | A page that captures an enquiry — buyer search, home valuation, or a resource behind an opt-in | Anyone with the link, once live |

## Read the presence before adding to it

`getRealtorLaunchState` is the first call every time: the current profile, every page they have
already authored, their marketing sites and funnels, what is still missing, and the Propaired links
to each. Realtors forget what they have; adding a second Draper site is worse than adding none.

`getMarketingContentContext` for their voice and brand, and `getRealtorBusinessSummary` when a page
should stand on their actual results rather than adjectives.

## Drafting

`draftRealtorProfilePage` creates a page draft — disabled, not in their navigation, not public.
`reviseRealtorProfilePage` changes the title, navigation label or SEO text of one that exists; pass
`null` for anything you are leaving alone.

`draftMarketingSite` creates a community site draft. Propaired composes the branding and template
from the realtor's own brand — never pass owner, team or publication fields, and never invent a
colour or a logo.

`draftLeadFunnel` creates a buyer-search, seller-valuation or lead-magnet funnel draft. A draft
funnel captures nobody while it is a draft.

All four save as soon as you call them, and none of them is public. Say so in those words — a
realtor who believes a draft is live will either panic or stop checking. Then hand over the
Propaired link so they can look at it and switch it on themselves.

## Making a funnel live

`publishLeadMagnetFunnel` is the one capability here that reaches the public internet, so it queues
an approval and the realtor decides it in Propaired.

It is a wrapper, not a content type: it gives away **one artifact the realtor has already
published** in exchange for an email. Produce that artifact first — through `propaired-content-studio`
for an article, or the market-report path for a Market Pulse — then confirm it with
`getMarketingArtifact`. Never publish a funnel pointing at something you only intended to write.

An opt-in is consent to receive what the page promised, and nothing more. Do not describe a funnel
as building a mailing list the realtor can market to.

Hand over the link in the card, then poll `resumeCapability` with the approval id for what the
realtor decided. The page is not live until a resume says so. If they decline, stop — leave the
draft where it is and tell them where to find it.

## Enabling a page, and what you cannot do

Enabling a profile page, putting it in the navigation, publishing a marketing site, uploading an
image and anything to do with a team all stay with the realtor in Propaired. There is no capability
here that does them, and that is deliberate — do not look for a workaround, say what you drafted and
where they finish it.

## What may be said

Everything in `propaired-governance` rule 5, and harder here than anywhere: a page sits on the
public internet under their licence indefinitely.

- The brokerage name belongs on anything that advertises their services.
- Nothing about commissions or what a competitor charges.
- Fair Housing: describe the homes and the area, never who would fit there. "Great for families"
  and "a safe neighbourhood" are the two that end up in complaints.
- No number on a page unless a tool returned it, and prefer the claim that stays true — a page
  written once is read for years, so "median prices updated monthly" beats this month's median.
- No client, past sale or address that is not already public.

## When something is unavailable

If `getAgentPlaybook` reports a step is not available on this connection, say which it is and stop.
On a read-only connection you can tell them exactly what their presence looks like today and what
is missing from it; say once that you cannot draft here, and give them the copy you would have used.
