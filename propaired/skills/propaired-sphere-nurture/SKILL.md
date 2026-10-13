---
name: propaired-sphere-nurture
description: Keep the realtor's past clients and database warm, and spot who is likely to move. Use for "who should I reach out to", "my past clients", "my book of business", "who has equity", "who's likely to sell", "set up follow-up for…", "add them to my nurture", "my database has gone cold", "anniversary", "equity update", or when the realtor wants recurring touches instead of one-off messages. Reads past sales and per-home equity, then sets up or enrols the right follow-up — with the realtor approving anything that starts sending.
interface:
  displayName: "Sphere and Past Clients"
  shortDescription: "Keep past clients warm and spot likely sellers"
  defaultPrompt: "Who in my database should I reach out to?"
toolset: front_office
capabilities:
  steps:
    - getMyContext
    - getBookOfBusinessSummary
    - listBookOfBusiness
    - requestBookOfBusinessValuation
    - linkBookOfBusinessClient
    - createClientFromPastSale
    - createDripSequenceDraft
    - setUpDefaultRealtorNurture
    - enrollClientInDripSequence
    - draftClientEmail
    - queueClientEmailSendApproval
---

# Keep the database warm

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

Most of a realtor's next year is already in their phone. This routine finds who to talk to and gets
a repeatable touch running.

## Who to reach out to

- `getBookOfBusinessSummary` — the shape of their past business: how many homes, how much equity
  sits in them, what has changed.
- `listBookOfBusiness` — the homes themselves, with per-home equity. Filter rather than listing
  everything.
- `requestBookOfBusinessValuation` — ask for a fresh value on a specific home when the one on file
  is stale and the realtor is about to use the number.
- `linkBookOfBusinessClient` — connect a past sale to a client record so the history and the
  follow-up live in one place.
- `createClientFromPastSale` — bring a past buyer or seller into the CRM as a client when they are
  not there yet.

Rank by the reason to call, not alphabetically: years in the home, equity available, a life event
the realtor mentioned, an anniversary, a neighbour's sale that changes their number. Give the
realtor the reason in the same breath as the name — that reason is the opening line of the call.

## Set up the follow-up

- `createDripSequenceDraft` builds a sequence. It saves as a draft and sends nothing.
- `setUpDefaultRealtorNurture` sets up their standard nurture when they have none.
- `enrollClientInDripSequence` **puts a real person into a sending sequence, so it needs the
  realtor's approval.** It returns an approval card rather than enrolling. Tell them who would be
  enrolled, into what, and what the first message says. Poll `resumeCapability({ approvalId })` for
  their decision; the approved enrolment runs once, and the approval expires after 90 minutes.

Never enrol someone because it seemed like a good idea. Enrol who the realtor named.

Consent is not yours to assume. If a person has not agreed to marketing contact, Propaired will
refuse the enrolment — report that plainly rather than looking for another way to reach them.

## One-off touches

For a single message to one person, draft it with `draftClientEmail` and queue it with
`queueClientEmailSendApproval` — the same approval contract. A personal note about something real
beats a sequence, and for a past client it usually is the right call.

Log the outreach with `propaired-crm-notes` so the next conversation knows it happened.

## Content rules

Equity and value talk is where this goes wrong:

- Use the number from a tool result, with its date, and say it is an estimate. Never round it up into
  a promise.
- No compensation or commission language.
- Nothing about the client in anything public.
- Fair Housing applies to farming and neighbourhood messaging as much as to a listing: write about
  homes and market conditions, never about who lives somewhere or who should.
