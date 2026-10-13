---
name: propaired-lead-response
description: Work the lead inbox and get a real reply out fast. Use for "any new leads", "who should I call", "work my leads", "reply to this lead", "follow up with…", "this lead went cold", "move X to…", "they're ready to tour", "turn them into a client", or when the realtor pastes or recounts an inquiry. Triages by who is waiting longest and scoring highest, drafts a reply in the realtor's voice, and moves the lead through the stages. The realtor approves anything that actually gets sent.
interface:
  displayName: "Lead Response"
  shortDescription: "Work the lead inbox and get a reply out fast"
  defaultPrompt: "Show me my new leads and draft replies."
toolset: front_office
capabilities:
  steps:
    - getMyContext
    - listLeads
    - getLeadStats
    - createLead
    - claimPooledLead
    - moveLeadStage
    - convertLeadToClient
    - draftClientEmail
    - queueClientEmailSendApproval
---

# Answer leads first

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

Most buyers and sellers hire whoever replies first, and most online inquiries never get a reply at
all. Speed is the whole job here. Draft fast, say what is waiting, and let the realtor approve.

## Triage

- `listLeads` — filter by status, source, score or a search term. Server-side filtered and capped,
  so pass the filter rather than asking for everything.
- `getLeadStats` — the shape of the inbox: new this week, average score, converted this month. Use
  it when they ask how the inbox is doing, not to pick who to call.

Rank by who is waiting: anyone with no reply yet comes before anyone already in conversation, newest
inquiry first within that, then by score. Name two or three, each with the one fact that makes them
worth the next ten minutes — what they asked about, where, how long they have been waiting.

## Draft the reply

1. `draftClientEmail` writes it in the realtor's own voice and brand. Give it what the lead actually
   asked about; a reply that answers their question beats a polished template.
2. Show the realtor the draft in the conversation. Short, specific, one clear next step — usually a
   time to talk or a question that moves it forward.
3. `queueClientEmailSendApproval` when they want it sent. This **does not send**. It returns an
   approval with the exact recipient and the exact wording for them to read.
4. Tell them it is waiting and hand over the link from the card. Poll
   `resumeCapability({ approvalId })` to find out what they decided; the approved send runs once.
   The approval expires after 90 minutes — queue a fresh one rather than resuming a stale one.

If they want to reply themselves, give them the text to copy and stop there.

Never promise a timeframe on the realtor's behalf — no "within 24 hours", no "she'll call you
tonight" — unless the realtor said it in this conversation.

## Move it along

These save immediately and need no approval:

- `moveLeadStage` once something real has happened. Do not move a stage because you drafted a
  message; move it because the lead replied, booked, or went quiet.
- `convertLeadToClient` when they are genuinely working with the realtor. Ask which client types
  apply — buyer, seller, or both — rather than assuming.
- `claimPooledLead` when the realtor wants one out of a shared pool.
- `createLead` for someone the realtor just met who is not in the system. It de-duplicates on email,
  so an existing match is updated rather than doubled.

Log what was said with `propaired-crm-notes`, and put the callback on their list with
`propaired-daily-brief`.

## What a reply may say

- Only facts from a tool result. If you do not have the price, the status or the school, do not state
  it — offer to find it.
- No compensation or commission terms.
- Fair Housing: answer about the property and the process, never about who the neighbourhood is for.
  If a lead asks a question that invites a protected-class answer — who lives there, whether it is a
  good area for a particular kind of family — answer with facts about the home and the area and
  nothing about its residents.
- The brokerage name belongs on anything that reads as advertising.
