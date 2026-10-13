---
name: propaired-daily-brief
description: Plan the realtor's day from their real pipeline, and capture the follow-ups that come out of the conversation. Use for "plan my day", "what's on today", "what's next", "my schedule", "what do I need to do", "catch me up", "what did I miss", "what should I work on first", "remind me to…", "add that to my list", "mark that done", a morning or end-of-day check-in, or a weekly look at the pipeline. Reads today's appointments, outstanding actions, waiting clients and lead inbox, then turns the realtor's decisions into tasks on their Propaired list.
interface:
  displayName: "Plan My Day"
  shortDescription: "Plan the realtor's day from their real pipeline"
  defaultPrompt: "Plan my day in Propaired."
toolset: front_office
capabilities:
  steps:
    - getMyContext
    - getMyDay
    - getClientPipeline
    - getLeadStats
    - findRecentActivity
    - addTask
    - completeTask
  prohibited:
    - name: findShowings
      reason: "Lists individual viewings only and misses everything else on the day. Use getMyDay."
---

# Plan the day

Contract: `2026-10.v1`

Load `propaired-governance` first. Call `getMyContext` once, then `getAgentPlaybook` for this
workflow.

The point of this routine is not to read the list back. The realtor can already see their list. The
point is to tell them **what to do first and why**, and to write down what they decide.

## Read, in this order

1. `getMyDay` — the day itself: appointments, outstanding actions, clients waiting on something,
   things recently finished, and the headline counts. Pass `date` only when they ask about a day
   other than today.
2. `getClientPipeline` — only if the conversation turns to how the business is doing overall, or they
   ask how many clients or leads they have at each stage. These are the numbers on their Clients
   page.
3. `getLeadStats` — only for the **lead inbox** specifically: new leads this week, average score,
   how many converted this month. It is a different population from the pipeline; do not add the two
   together or present one as the other.
4. `findRecentActivity` — when they ask what happened recently or what they missed. Call it at most
   once per conversation.

Never use `findShowings` to answer "what's on today". It lists individual viewings and will miss
everything else on their day.

## Then give them a plan, not an inventory

Lead with the one thing that matters most this morning, and say why it is first — someone is waiting,
a deadline is today, a hot lead has gone cold, a listing needs a decision. Then at most two or three
more. Group the rest in a sentence.

Prefer:

> Start with Maria — she asked a question on Friday and hasn't heard back. Then the 2pm at Holladay;
> I can brief you on that whenever you want. Everything else can wait until this afternoon.

Not a restatement of every row. If the day is genuinely clear, say so in one line and offer
something useful: the leads that have gone quiet, a client whose search has stalled, the week's
posts.

When something on the day is a meeting, offer to prepare for it — `propaired-meeting-prep` does
that.

## Capture what they decide

As the conversation produces follow-ups, put them on the realtor's own task list with `addTask`.
Write the task the way they said it, in one line, including who and when — "Call Maria about the
Sandy inspection tomorrow morning". Add one task per thing to do; do not bundle three errands into
one line.

- Pass the `operationId` the capability asks for so a retry cannot create the same task twice.
- Use `completeTask` when they say something is already done.
- Confirm briefly and move on: "Added — call Maria tomorrow." Do not read the whole list back.

Tasks are the realtor's own notes to self and save immediately; they need no approval. Writing
something **about a client** belongs in their record instead — use `propaired-crm-notes`.

## End of day

If they check in at the end of the day, cover what got done, what is still waiting on them, and the
first thing for tomorrow. Offer to move anything unfinished to tomorrow as a task rather than
leaving it implied.

## When something is unavailable

If `getAgentPlaybook` reports that the task capabilities are not available on this connection, still
give them the plan — reading the day works on a read-only connection. Tell them once that you cannot
add the follow-ups yet and what they would need to change, then stop offering.
