---
name: propaired-social-series
description: Set up and run a repeating social presence rather than one post at a time. Use for "post every week", "set up a weekly series", "pause my posts", "skip next week", "what's scheduled", "change my recurring posts", "test what it would post", "make a template for this", or anything about posts that keep going on their own. Builds the series, shows the realtor exactly what it would publish, and never lets it start or publish without their approval.
interface:
  displayName: "Social Series"
  shortDescription: "A repeating social presence that keeps running"
  defaultPrompt: "Set up a weekly post series for me."
toolset: social_studio
capabilities:
  steps:
    - getMyContext
    - listConnectedSocialAccounts
    - getSocialQueueOverview
    - listSocialContentSeries
    - getSocialContentSeries
    - getMarketingContentContext
    - listSocialTemplates
    - inspectSocialTemplate
    - createSocialContentSeries
    - updateSocialContentSeries
    - testSocialContentSeries
    - prepareSocialContentSeriesActivation
    - activateSocialContentSeries
    - pauseSocialContentSeries
    - skipSocialContentSeriesOccurrence
    - cancelSocialContentSeries
    - createSocialContentRun
    - getSocialContentRun
    - renderSocialContentRun
    - saveSocialContentDraft
    - reviewSocialContentProposal
    - prepareSocialContentDelivery
    - publishSocialContentArtifact
    - bindSocialTemplateContract
    - prepareCanvaBrief
    - refreshCanvaDesignMetadata
    - synchronizeCanvaExport
    - resumeCapability
---

# Keep the posts coming

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

A series posts on its own, on a schedule, after the realtor has approved it. That is the whole
point and also the whole risk: once it is running, it publishes without anyone looking first. Be
correspondingly careful setting one up.

For a single post this week, use `propaired-weekly-social` instead.

## Before anything else

- `listConnectedSocialAccounts` — where they can actually post. If nothing is connected, say that
  first and stop; everything else here is wasted.
- `getSocialQueueOverview` — what is already scheduled, so you do not stack a new series on top
  of posts they have forgotten about.
- `listSocialContentSeries` and `getSocialContentSeries` — what is already running.
- `getMarketingContentContext` — their voice, areas and positioning.

## Build it, then show them what it would do

`createSocialContentSeries` defines the schedule, the topic and the look. `updateSocialContentSeries`
changes one that already exists. Both save immediately and neither posts anything.

Before you offer to turn it on:

1. `testSocialContentSeries` produces what the next occurrence would actually be. Show them that,
   not a description of it.
2. `prepareSocialContentSeriesActivation` reports what switching it on commits to — the cadence,
   the accounts, how many posts.

Then `activateSocialContentSeries` queues the approval. Say plainly what they are agreeing to:
"this posts to Instagram and Facebook every Tuesday morning until you stop it."

Hand over the link in the card and poll `resumeCapability` with the approval id for what they
decided. A series is not running until a resume says so — and because it then posts on its own,
telling a realtor it is live when it is not is the worst thing you can get wrong here. If they
decline, stop.

## Stopping it

- `pauseSocialContentSeries` — stop it for now, keep the setup.
- `skipSocialContentSeriesOccurrence` — skip one, keep the rest.
- `cancelSocialContentSeries` — end it.

All three take effect immediately and need no approval. Stopping something is never gated: if a
realtor asks you to stop their posts, stop them and confirm, do not queue anything.

## Individual runs

A run is one occurrence. `createSocialContentRun`, `getSocialContentRun` and
`renderSocialContentRun` produce and render it; `saveSocialContentDraft` keeps it;
`reviewSocialContentProposal` works through changes; `prepareSocialContentDelivery` resolves
exactly where it would go.

`publishSocialContentArtifact` is public, so it queues an approval.

## Templates and designs

`listSocialTemplates` and `inspectSocialTemplate` for what is available,
`bindSocialTemplateContract` to attach one to a series. For Canva: `prepareCanvaBrief`,
`refreshCanvaDesignMetadata`, and `synchronizeCanvaExport` to pull a finished design back — that
last one changes what the series will post, so it waits for the realtor.

## What may be said

Everything in `propaired-governance` rule 5 applies to every post the series will ever publish,
not just the one you are looking at: the brokerage name, no commission talk, Fair Housing, no
client details. A series multiplies a mistake by its schedule, so hold the template to the same
standard as a single post.

Never set up a series whose text asserts a number that will be stale by the third occurrence.
Describe the research to repeat, not this month's figure.

## When something is unavailable

If `getAgentPlaybook` reports a step is not available on this connection, say which it is and
stop. On a read-only connection you can show them what is running and what is scheduled; say once
that you cannot change it and what they would need to do.
